---
title: Statistics Software
tags:
  - Statistics
  - Tools
---

# Statistics Software

- [IBM SPSS Statistics](https://www.ibm.com/analytics/spss-statistics-software)
- [Comparison of statistical packages (Wikipedia)](https://en.wikipedia.org/wiki/Comparison_of_statistical_packages)
- [Best Free and Open Source Software for Statistical Analysis](https://blog.cometdocs.com/the-best-free-and-open-source-software-for-statistical-analysis)

## R-squared ($R^2$, 决定系数)

$R^2$ 是统计学和机器学习中用于评估回归模型**拟合优度（Goodness of Fit）**的核心指标之一。它反映了在因变量的总体方差中，能由自变量（即你的回归模型）解释的比例。

**主要概念与直观理解：**

- **取值范围**：通常在 **0 到 1** 之间。
  - **$R^2 = 1$**：完美拟合，模型的预测值和真实值完全一致，没有任何误差。
  - **$R^2 = 0$**：模型没有提供任何比“总是预测平均值”更好的信息，自变量和因变量之间毫无线性关系。
  - **$R^2 < 0$**：这种情况通常发生在你选择了不恰当的模型甚至连均值都没拟合好时（此时模型的误差比直接用平均值盲猜的误差还大）。
- **通俗例子**：假设你训练了一个预测房价的模型，最终得到 $R^2 = 0.8$。这表示影响房价波动的因素中，有 **80%** 是由你放进模型里的特征（如面积、地段、房龄等）决定的，剩下的 20% 则是由于未能被模型捕捉到的其他因素（噪音或未收集的变量）所导致。

**数学计算公式：**
$$ R^2 = 1 - \frac{SS*{res}}{SS*{tot}} $$

- **$SS_{res}$ (Residual Sum of Squares, 残差平方和)**：模型预测值与真实值之间的误差平方和。这部分表示**模型未能解释的方差**。
- **$SS_{tot}$ (Total Sum of Squares, 总平方和)**：真实值与整体平均值之差的平方和。这部分表示**数据自身的整体变异度（总方差）**。
- **本质也就是：** $1 - \frac{\text{模型解释不了的部分}}{\text{总的波动部分}}$。

**一句话总结：**
$R^2$ 告诉你模型在多大程度上把混乱的数据“解释”清楚了，分数越高，模型拟合得越好！

## 普通线性回归

- (Ordinary Least Squares, OLS)

## 加权线性回归 {#weighted-linear-regression}

- 加权线性回归 - Weighted linear regression - WLR

$$ \text{Cost} = \sum\_{i=1}^{n} w_i \cdot (y_i - \hat{y}\_i)^2 $$

- 处理异方差性 (Heteroscedasticity)
- 降低异常值 (Outliers) 的影响
- 数据聚合时的权重
