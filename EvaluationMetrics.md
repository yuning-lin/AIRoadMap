## 先備知識
### 平均數
* 概念：表達數據的集中趨勢
* 常見的平均數求法：
  
名稱|公式|適用時機|特點
----|----|----|----
算術平均數<br>arithmetic mean|$\frac {\sum_{i=1}^{n}{x_{i}}}{n}$|當數值單位一樣時適用|可以接受正負數及零，但易受極端值影響<br>為常見濃縮變數表徵
幾何平均數<br>geometric mean|$\sqrt[n]{x_{i} \* ... \* x_{n}}$|當數值單位不一樣時適用|不適用於有零、負數的情境<br>也因為乘法性質可以處理不同數值單位，更可以彙總具有乘法或指數關係的數據
調和平均數<br>harmonic mean|$\frac{n}{\sum_{i=1}^{n}{\frac{1}{x_{i}}}}$|當數值為不同度量方法計算的比率時適用|有零時不能使用，極小值的影響比極大值的影響更甚<br>一般可以用在相同距離但速率不同時，平均速率的計算

## 適用於分類模型的指標
*  Confusion Matrix：
  
  預測值\真實值|陽性|陰性
  :----:|----|----
  **陽性**|True Positive（TP）|False Positive（FP）
  **陰性**|False Negative（FN）|True Negative（TN）

* 基於 confusion matrix，常見的計算指標：
  
名稱|公式|特點|範例
----|----|----|----
Accuracy|$\frac{TP+TN}{TP+FP+FN+TN}$|表所有個體被正確預測的比率<br>為最常見指標，但遇到不平衡的資料會失效|EX：偵測垃圾訊息<br>100 封信件裡僅有一封是垃圾信件<br>當全部都判為正常時，accuracy=0.99=\frac{0+99}{0+0+1+99}<br>其實模型沒有起到偵測作用
Specificity|$\frac{TN}{TN+FP}$|表所有實際為陰性且被正確判斷為陰性之比率|
Recall|$\frac{TP}{TP+FN}$|表所有實際為陽性且被正確判斷為陽性的比率<br>考量 FP 及 FN 影響較大者|EX：門禁系統<br>同仁被偵測為外人（FN）<br>只要多刷幾次卡就好，較沒這麼嚴重
Precision|$\frac{TP}{TP+FP}$|表所有被判斷為陽性中實際確為陽性的比率<br>考量 FP 及 FN 影響較大者|EX：門禁系統<br>希望外人被偵測為同仁的機會（FP）越少越好
F1 Score|$\frac{2\*Precision\*Recall}{Precision+Recall}$|綜合考量相同權重的 Precision 及 Recall|即是調和平均數<br>當不同模型的 Precision 及 Recall 此消彼長<br>即可以用綜合考量來評斷
F Measure|$\frac{(1+\beta^{2})\*Precision\*Recall}{(\beta^{2}\*Precision)+Recall}$|綜合考量不同權重的 Precision 及 Recall|當 Precision 和 Recall 一樣重要<br>當 belta=1 即為 F1 Score；<br>較在意 Precision，belta=非負分數<br>當 belta=0 時，F Measure=Precision；<br>較在意 Recall，belta>1<br>當 belta=$\infty$ 時，F Measure=Recall；

* Accuracy Paradox
  * 白話：Accuracy 不能代表模型的好壞
  * 但同樣的，只要是 imbalance data，以上談論的指標都會有 Accuracy Paradox 的問題
  * 例子：100 封信件裡有 99 封是垃圾信件，模型將全部信件都判為垃圾時
    * Accuracy=0.99、Recall=1、Precision=0.99、F1 Score=0.99
    * 雖然 Recall、Precision、F1 Score 都比 Accuracy 可以看到更多資訊，但以上四種指標皆無法解決 Accuracy Paradox 的問題
* 對不平衡資料可能更適合的解法：
  * 透過抽樣方法平衡資料
  * 透過合成樣本平衡資料
  * MCC（Matthews Correlation Coefficient）= $\frac{TP\*TN-FP\*FN}{\sqrt{(TP+FP)\*(TP+FN)\*(TN+FP)\*(TN+FN)}}$
  * ROC Curve（Receiver Operating Characteristic Curve）＆ AUC（Area Under the Curve）

## 適用於迴歸模型的指標
名稱|公式|值域|特點
----|----|----|----
MAE（Mean Absolute Error）|$\frac{\sum_{i=1}^{n}\|y_{i}-\hat{y_{i}}\|}{n}$|\[0,$\infty$)|又稱 L1 Loss Function<br>用絕對值可以避免正負抵銷
MAPE（Mean Absolute Percentage Error）|$\frac{\sum_{i=1}^{n}{\|\frac{y_{i}-\hat{y_{i}}}{y_{i}}\|}}{n}\*100$|\[0,$\infty$)|分母不得為 0，故觀測值有 0 的資料無法算出結果<br>以百分比表示但值可能會超過 100%，與比例無關
MSE（Mean Square Error）|$\frac{\sum_{i=1}^{n}{(y_{i}-\hat{y_{i}})^{2}}}{n}$|\[0,$\infty$)|又稱 L2 Loss Function<br>對於極值相對敏感
RMSE（Root Mean Square Error）|$\sqrt\frac{\sum_{i=1}^{n}{(y_{i}-\hat{y_{i}})^{2}}}{n}$|\[0,$\infty$)|開根號使得值與 y 的單位一致，易於解釋
R Squared|$1-\frac{\sum_{i=1}^{n}{(y_{i}-\hat{y_{i}})^{2}}}{\sum_{i=1}^{n}{(y_{i}-\overline{y_{i}})^{2}}}$|\[0,1\]|值越大表示模型的解釋能力越好<br>但越多變數加入也會導致值越大，故有 Adjusted R-Squared
Adjusted R-Squared|$1-\frac{\frac{\sum_{i=1}^{n}{(y_{i}-\hat{y_{i}})^{2}}}{n-p-1}}{\frac{\sum_{i=1}^{n}{(y_{i}-\overline{y_{i}})^{2}}}{n-1}}$|\[0,1)|n:樣本數、p：變數個數<br>平衡變數增加帶來的影響

## 資料來源
* [部落格：秒懂 Confusion Matrix](https://www.ycc.idv.tw/confusion-matrix.html)
* [部落格：Evaluation Metrics : 分類模型](https://medium.com/ai%E5%8F%8D%E6%96%97%E5%9F%8E/evaluation-metrics-%E5%88%86%E9%A1%9E%E6%A8%A1%E5%9E%8B-ba17ad826599)
* [部落格：不平衡資料的二元分類 1：選擇正確的衡量指標](https://taweihuang.hpd.io/2018/12/28/imbalanced-data-performance-metrics/)
* [部落格：迴歸損失函數1：L1 loss, L2 loss以及Smooth L1 Loss的對比 ](https://www.cnblogs.com/wangguchangqing/p/12021638.html)
