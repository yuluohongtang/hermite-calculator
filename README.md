# Hermite 多项式离线计算器

非线性回归的线性拟合工具：把任意多项式或离散数据点投影到埃尔米特正交基上，输出广义傅里叶系数与完整拟合多项式。提供 Jupyter Notebook 与内嵌 MathJax 的单文件 HTML，**完全离线运行，无外部请求**。

## 功能特性

- **双约定可选**：物理学家版 $H_n$（权 $f(x)=e^{-x^2}$）与概率学家版 $He_n$（权 $\varphi(x)=\dfrac{1}{\sqrt{2\pi}}e^{-x^2/2}$，标准正态密度），两基满足 $He_n(x)=2^{-n/2}H_n\!\left(\dfrac{x}{\sqrt2}\right)$
- **模式一：已知 $h(x)$**——输入阶数与各阶系数，精确有理运算，输出分数形式的埃尔米特展开系数、Rodrigues 基系数向量
- **模式二：未知 $h(x)$**——输入数据点坐标，按梯形数值积分（笔记公式 III）逐阶估计 $\alpha_n$，还原完整多项式
- **三级阶数提示**：点数充足无提示；每参数不足 2 点显示黄色警告；参数多于点数（欠定）显示红色提示，仍完整出结果
- **系数诊断**：同列对齐显示一阶差分 $\Delta\alpha_n=\alpha_n-\alpha_{n-1}$ 与平方损失和 $\mathrm{SSE}=\sum_i(y_i-F(x_i))^2$
- 规范 LaTeX 排版，分数上下堆叠；超大/超小数字自动转科学计数法

## 文件说明

| 文件 | 说明 |
|---|---|
| `Hermite离线计算器.ipynb` | 中文版 Notebook（6 个 Cell，含离线自检） |
| `Hermite_Calculator_EN.ipynb` | English version notebook |
| `Hermite离线计算器.html` | 单文件 HTML，MathJax 已整体内嵌，双击即可离线使用 |

## 使用方法

**HTML（最简单）**：直接双击 `Hermite离线计算器.html`，在浏览器中选择约定与模式，无需安装任何东西、无需联网。

**Notebook**：按 Cell 顺序运行即可。依赖：

```
numpy  scipy  matplotlib  sympy  ipywidgets  nbformat
```

## 数学原理

埃尔米特展开系数（广义傅里叶系数）是函数在正交基上的投影：

$$\alpha_n=\frac{\langle h,P_n\rangle_w}{\langle P_n,P_n\rangle_w},\qquad h(x)=\sum_{n=0}^{N}\alpha_n P_n(x)$$

模式二用梯形法把内积积分替换为节点上的离散求和（公式 III）：

$$\alpha_n\approx\frac{\displaystyle\sum_{i=1}^{m-1}\frac{x_{i+1}-x_i}{2}\left[y_{i+1}P_n(x_{i+1})+y_iP_n(x_i)\right]w_i}{\displaystyle\sum_{i=1}^{m-1}\frac{x_{i+1}-x_i}{2}\left[P_n^2(x_{i+1})+P_n^2(x_i)\right]w_i}$$

Rodrigues 记号：$\dfrac{f^{(n)}}{f}=(-1)^nH_n$，$\dfrac{\varphi^{(n)}}{\varphi}=(-1)^nHe_n$。

## License

MIT
