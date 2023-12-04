#### PCA实战之MINST数据集

#### 1.PCA(主成分分析法)

PCA是一种常见的降维方法，sklearn库提供实现方法，下面介绍一下方法参数：

在Scikit-learn（sklearn）的PCA（主成分分析）类中，有几个常用的参数可以调整以控制PCA的行为。以下是一些常用的PCA类参数：

1. `n_components`：指定要保留的主成分数量或方差的比例。可以设置为整数值，表示要保留的主成分数量，或设置为0到1之间的浮点数，表示要保留的方差比例。默认值为`None`，表示保留所有主成分。
2. `copy`：指定是否在进行PCA操作时创建输入数据的副本。默认值为`True`，表示创建副本。如果数据集很大，可以设置为`False`以节省内存。
3. `whiten`：指定是否对降维后的数据进行白化处理。白化是一种线性变换，它使每个特征的均值为0，方差为1。默认值为`False`，表示不进行白化处理。
4. `svd_solver`：指定SVD（奇异值分解）的求解器。可以选择的值有`auto`、`full`、`arpack`和`randomized`。默认值为`auto`，根据输入数据和`n_components`的大小自动选择求解器。
5. `tol`：指定SVD求解器的停止标准的容忍度。默认值为0.0，表示使用默认容忍度。
6. `iterated_power`：指定幂迭代法的迭代次数。默认值为'auto'，表示根据问题的大小和精度自动选择迭代次数。

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2, copy=True, whiten=False, svd_solver='auto', tol=0.0, iterated_power='auto')
```

#### 2.MINST实战

```python
# 导入包
from sklearn.model_selection import train_test_split
import pandas as pd
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression
```

```python
# 划分数据集
x_train, x_test, y_train, y_test = train_test_split(digits.data, digits.target, test_size=0.2, random_state=0)

# 标准化
scaler = StandardScaler()
x_train_scaled = scaler.fit_transform(x_train)
x_test_scaled = scaler.transform(x_test)
```

```python
# 逻辑回归 - 不使用PCA
lr = LogisticRegression()
lr.fit(x_train_scaled, y_train)
score_without_pca = lr.score(x_test_scaled, y_test)

# 应用PCA
pca = PCA(n_components=0.97) # 保留97%的方差
x_train_pca = pca.fit_transform(x_train_scaled)
x_test_pca = pca.transform(x_test_scaled)

# 逻辑回归 - 使用PCA
lr_pca = LogisticRegression()
lr_pca.fit(x_train_pca, y_train)
score_with_pca = lr_pca.score(x_test_pca, y_test)

print("Accuracy without PCA:", score_without_pca)
print("Accuracy with PCA:", score_with_pca)

# Accuracy without PCA: 0.9611111111111111
# Accuracy with PCA: 0.9722222222222222
```

