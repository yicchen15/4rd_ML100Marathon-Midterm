# 4rd_ML100Marathon-Midterm
 
## 機器學習百日馬拉松期中考 - Enron Fraud Dataset 安隆公司詐欺案資料集

安隆公司曾是一間能源公司，2001 年破產前是世界上最大的電力、天然氣及電信公司之一。擁有上千億資產的公司於 2002 年竟然在短短幾周內宣告破產，才揭露其財報在多年以來均是造假的醜聞。在本資料集中你將會扮演偵探的角色，透過高層經理人內部的 mail 來往的情報以及薪資、股票等財務特徵，訓練出一個機器學習模型來幫忙你找到可疑的詐欺犯罪者是誰! 我們已經先幫你找到幾位犯罪者 (Person-of-Interest, poi) 與清白的員工，請利用這些訓練資料來訓練屬於自己的詐欺犯機器學習模型吧!

### 特徵說明

有關財務的特徵: ['salary', 'deferral_payments', 'total_payments', 'loan_advances', 'bonus', 'restricted_stock_deferred', 'deferred_income', 'total_stock_value', 'expenses', 'exercised_stock_options', 'other', 'long_term_incentive', 'restricted_stock', 'director_fees'] (單位皆為美元)。更詳細的特徵說明請參考 enron61702insiderpay.pdf 的最後一頁

有關 email 的特徵: ['to_messages', 'email_address', 'from_poi_to_this_person', 'from_messages', 'from_this_person_to_poi', 'shared_receipt_with_poi'] (除了 email_address，其餘皆為次數)

嫌疑人的標記，也就是我們常用的 **y**。POI label: [‘poi’] (boolean, represented as integer)

我們也建議你對既有特徵進行一些特徵工程如 rescale, transform ，也試著發揮想像力與創意，建立一些可以幫助找到嫌疑犯的特徵，增進模型的預測能力。

========================================================

#### 步驟：
1. 數據清洗/前處理
* 缺失值處理: 填補 0、中位數、平均值、眾數，缺失資料比例過高則捨去該欄位
* 離群值處理/去偏態 (未用到)
* 特徵轉換(編碼): 0/1 (用在是否有email)
* 特徵縮放：標準化、MinMax
* 特徵選擇(未用到)

2. 模型選擇：
* Logistic Regression: 基本評估指標
* Lasso: LR+L1
* Ridge: LR+L2
* Gradient Boosting Regressor :
 透過集成-Boosting，由後面生成的樹修正前面的樹預測不好/錯誤的地方。
* Random Forest Regressor: 
 具高解釋性的模型，目標使訊息增益最大化，透過集成-Bagging將多棵樹模型結果組合在一起
* xgboost: 
 在gradient boosting方法加 L2 Regression
* Blending: 
 將不同模型的預測值加權合成，權重和為1。單一個別模型效果好，且模型差異大，能有更好表現。

3. 數據切分：4份訓練、1份驗證。

4. 評估指標：競賽使用AUC(Area Under Curve) 
預測結果分別有 True Positive、False Negative、False Positive、True Negative 四種情況，
可藉由Confusion Matrix 得到以下幾種指標：
> Precision = TP/(TP+FP)、 Recall = TP/(TP+FN) 

ROC曲線的橫坐標為false positive rate(FPR)，縱坐標為true positive rate(TPR)，
> FPR = FP/(FP+TN)、TPR = TP/(TP+FN)

AUC為ROC曲線下面積，其值屆於0~1之間，表示分類器正確判斷陽性樣本的機率高於陰性樣本之機率，就是AUC值越大的分類器其正確率越高。

5.上傳結果(AUC):
* Logistic Regression: 0.67857
* Lasso: 0.67857
* Ridge: 0.73571
* Gradient Boosting Regressor: 0.69285
* Random Forest Regressor: 0.76785
* xgboost: 0.75000
* Blending
  - rf-xgbst-ridge: 0.79285
  - lasso-ridge-rf: 0.80714
  - lr+gdbt+rf： 0.77142

6. 結論：
在未經過特徵篩選，僅做其他數據前處理(缺失值、特徵縮放、文字運用獨熱編碼)，再透過Blending模型，AUC可拿到0.8左右的結果。
若要再提高分數，可運用相關係數、Lasso、SelectKBest套件等方式進行特徵篩選；此外，適當去除離群值也可能有幫助。
模型的參數調整，能夠選定表現較好的模型，嘗試調整參數比較差異。
