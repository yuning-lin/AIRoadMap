## Decision Tree

* 安裝套件
```python
pip install pydotplus
pip install graphviz
pip install sklearn
```
* 可視化
```python
import pydotplus
from IPython.display import Image
from sklearn.externals.six import StringIO
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, export_graphviz

y = df.target
X = df.drop(['target'], axis=1)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0)
model = DecisionTreeClassifier(random_state=0, splitter='random') # splitter='random', 一樣的資料每次跑會有一樣的模型
model.fit(X_train, y_train)

dot_data = StringIO()
export_graphviz(model, out_file=dot_data,
                filled=True, rounded=True, impurity=False,
                special_characters=True, max_depth=3,
                feature_names=X_train.columns.tolist(),
                class_names=model.classes_)
graph = pydotplus.graph_from_dot_data(dot_data.getvalue())
graph.write_png(f'tree.png')
Image(graph.create_png())
```
* 錯誤訊息：跑到 `graph.write_png(f'tree.png')` 會出現錯誤訊息
```python
pydotplus.graphviz.InvocationException: GraphViz's executables not found
```
* 解決辦法
  1. 前往[官網](http://www.graphviz.org/download/) 下載系統對應（Windows/Mac/Linux）的 .zip
  2. 解壓縮後將 ./Graphviz/bin 路徑加入環境變數，如下即可使用
  ```python
  import os
  os.environ['PATH'] += os.pathsep + 'D:/user_name/Downloads/Graphviz/bin'
  ```
## 參考資料
* [Blog：Visualizing Decision Trees with Python (Scikit-learn, Graphviz, Matplotlib)](https://towardsdatascience.com/visualizing-decision-trees-with-python-scikit-learn-graphviz-matplotlib-1c50b4aa68dc)
* [Blog：Decision Tree In Python](https://towardsdatascience.com/decision-tree-in-python-b433ae57fb93)
* [DataCamp：Decision Tree Classification in Python Tutorial](https://www.datacamp.com/tutorial/decision-tree-classification-python)
