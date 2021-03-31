# Nishika_中古マンション価格予測

## 内容  
テーマは国内の中古マンションの価格推定  
データ内容  
https://www.nishika.com/competitions/11/data

## 評価方法  
 * 目的変数は、取引価格（総額）について常用対数をとった、「取引価格（総額）_log」  
 * 予測精度の評価はMAE
 * テストデータの約50%により暫定スコアが計算され、残りの約50%により最終スコアが計算される

## 最終順位    
Public：20位、Private：22位

## フォルダ構成  
input:データ格納フォルダ  
notebook：前処理から予測までのnotebookを格納

## 方針  
過去のコンペのSolutionを参考に、以下の様なフローで実施。  
参考：Probspace Re:不動産取引価格予測コンペ  
https://oregin-ai.hatenablog.com/entry/2020/08/11/200829  

![image](https://user-images.githubusercontent.com/46860245/113106230-7e9fe300-923d-11eb-8731-f3094e122029.png)

## 追加データ  
 * 国土交通省平成30年地価公示 https://www.mlit.go.jp/totikensangyo/H30kouji_index.html  
 * 都道府県・市区町村別統計表（国勢調査） https://www.e-stat.go.jp/stat-search/files?page=1&layout=datalist&toukei=00200521&tstat=000001049104&cycle=0&tclass1=000001049105&collect_area=200&result_page=1&tclass2val=0
 * 公示地価（国交省） https://nlftp.mlit.go.jp/ksj/old/datalist/old_KsjTmplt-L01.html

## 特徴量  
基本的な集計などを実施。  
前処理に関しては、Baselineを参考に実施。  
https://www.nishika.com/competitions/11/topics/79

 * CountEncoding
 * OneHotEncoding
 * OrdinalEncoding
 * aggregation("count", "sum", "mean", "max", "min")

## model  
1層目は二つのモデルを使用（seed averagingは使用せず）
 * LightGBM(kfold=5)
 * CatBoost(kfold=5)

2層目：DNN

## 反省点
 * GPUを使用できなかったので、学習に時間がかかる中、明確な方針を持って特徴量作成ができなかった。
 * trainデータが大きいためにメモリとの戦いだった。。。（ダウンサンプリングなどを実施してもよかった）
 * 期間が長い中で、実験方法含めてないことが多かった。
 * DNNの結果がずれるのが、不明でロス（seedは固定していたのに）
