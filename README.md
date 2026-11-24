# Hermite 多项式离线计算器

Hermite 基计算工具：支持把已知多项式或离散数据点投影到埃尔米特正交基上进行近似拟合，也支持按课堂白板的方阵配置做严格 Hermite 插值。提供 Jupyter Notebook 与内嵌 MathJax 的单文件 HTML，**完全离线运行，无外部请求**。

## 功能特性

- **双约定可选**：物理学家版 $H_n$（权 $f(x)=e^{-x^2}$）与概率学家版 $He_n$（权 $\varphi(x)=\frac{1}{\sqrt{2\pi}}e^{-x^2/2}$，标准正态密度），两基满足 $He_n(x)=2^{-n/2}H_n\!\left(\frac{x}{\sqrt2}\right)$
- **模式一：已知 $h(x)$**——输入阶数与各阶系数，精确有理运算，输出分数形式的埃尔米特展开系数、Rodrigues 基系数向量
- **模式二：未知 $h(x)$**——输入或随机生成数据点（2–20000 点），按梯形数值积分（笔记公式 III）逐阶估计 $\alpha_n$，还原完整多项式；长残差表区域滚动显示，长计算提供进度反馈
- **模式三：严格 Hermite 插值**——输入 $m$ 个互异节点后自动取 $B_0,\ldots,B_{m-1}$ 共 $m$ 个基函数，组装 $m\times m$ 节点矩阵 $\mathbf H$ 并求解 $\mathbf H\boldsymbol\alpha=\mathbf y$；输出 $\mathbf H$、$\boldsymbol\alpha$、节点残差、Hermite 形式多项式与插值图。界面保护上限为 120 点，大规模点云近似仍建议使用模式二
- **三级阶数提示**：点数充足无提示；每参数不足 2 点显示黄色警告；参数多于点数（欠定）显示红色提示，仍完整出结果。课堂演示界面向 $N\le20$，更高阶会给出限制提示
- **系数与误差诊断**：同列对齐显示一阶差分 $\Delta\alpha_n=\alpha_n-\alpha_{n-1}$、稳定性增量 $\Delta_n$、平方损失和 $\mathrm{SSE}=\sum_i(y_i-F(x_i))^2$ 与均方误差 $\mathrm{MSE}=\mathrm{SSE}/m$
- 规范 LaTeX 排版，分数上下堆叠；超大/超小数字自动转科学计数法

## 文件说明

| 文件 | 说明 |
|---|---|
| `Hermite离线计算器.ipynb` | 中文版 Notebook（7 个 Cell，含模式三与离线自检） |
| `Hermite_Calculator_EN.ipynb` | English version notebook (7 cells, including Mode 3 and self-check) |
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

模式三不做积分近似。对 $m$ 个互异节点，令 $H_{kj}=B_j(x_k)$，其中行对应节点、列对应基函数，则直接求解方阵：

$$\mathbf H\boldsymbol\alpha=\mathbf y,\qquad \widehat\varphi(x)=\sum_{j=0}^{m-1}\alpha_jB_j(x)$$

因此模式三的结果在数值精度内严格通过全部输入节点；模式二是用户选定阶数下的最小二乘/投影型近似，不保证穿过每个点。

Rodrigues 记号：

$$
\frac{f^{(n)}}{f}=(-1)^n H_n,\qquad \frac{\varphi^{(n)}}{\varphi}=(-1)^n He_n
$$
