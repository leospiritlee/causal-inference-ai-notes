## 序：预测问看到什么，决策问做了什么

一个推荐模型可以很准确地预测「看到这个特征的用户会点击」，却无法回答「如果我们给他发这张券，他会不会因为这张券而点击」。前者是关联，后者是干预。把前者当后者用，系统会在最关键的环节出错：你改变的正是不再满足原有相关性的那个环节。这本专题精读写的是从预测走向决策时，必须补上的那一层约束。

因果推断不替换机器学习，而是在流水线之外增加三件事：用图显式写下数据生成假设；用识别理论判断某个 `causal effect` 能否从现有数据唯一确定；在可识别的前提下给出带不确定性的估计。少掉第一步，特征筛选会删掉对调整必要但对预测无用的变量；少掉第二步，模型会把不可识别的量偷偷换成某个可计算的关联量；少掉第三步，一个点估计会被当成无需检验的结论。

**分工**：与《统计学习方法精读》不同，那本讲预测方法与泛化；与《机器学习系统精读》不同，那本讲训练与服务系统；与《推理模型精读》不同，那本讲回答那一刻多花的算力。这本只讲一件事：当问题从「看到什么」变成「做了什么」，哪些结论有资格被称为因果，哪些只是更精确的错觉。

## 因果决策全景图

```text
+------------------------------------------------------------------+
| CAUSAL INFERENCE FOR AI: FROM ASSOCIATION TO INTERVENTION         |
|                                                                  |
| [1] question -> [2] graph -> [3] identify -> [4] estimate         |
| [5] IV/DID -> [6] matching -> [7] heterogeneity -> [8] DML        |
| [9] discovery -> [10] libraries -> [11] refute -> [12] governance |
|                                                                  |
| prediction: P(Y|X)   decision: P(Y|do(T))   refute before ship    |
+------------------------------------------------------------------+
```

这不是一张库调用图，而是「一个决策问题从提问到上线，新出现的约束依次落在哪里」：先分清问题是在问关联还是干预，再把假设画成图，再判断能不能识别，最后才轮到估计器。越往后，代价越不是统计精度，而是假设治理与上线门禁。

## 十二关速查表

| 关 | 主题 | 一句话 |
|---|---|---|
| 1 | 关联与干预 | `causal inference` 问的是做了什么会怎样，不是看到什么会怎样 |
| 2 | 因果图 | `confounding` 要阻断，`collider` 别条件化，`mediation` 分直接/间接 |
| 3 | 识别 | `back-door criterion` 与 `front-door criterion` 是充分条件，不是万能钥匙 |
| 4 | 潜在结果 | 同一单位只能看到一个 `potential outcome`，另一个靠假设补全 |
| 5 | 准实验 | `instrumental variable` 与 `difference-in-differences` 各有不可省的假设 |
| 6 | 可比对照 | `propensity score matching` 造的是平衡，不是随机化本身 |
| 7 | 异质性 | `conditional average treatment effect` 比平均效应更难估 |
| 8 | 正交化 | `double machine learning` 用 `cross-fitting` 隔离正则偏差 |
| 9 | 发现与表示 | `causal discovery` 没有干预只能给出等价类 |
| 10 | 工程库 | `DoWhy` 管流程，`EconML` 管估计，`CausalML` 管增益落地 |
| 11 | 反驳 | 没有真值标签，靠 `sensitivity analysis` 与负对照守门 |
| 12 | 边界 | 不可识别就别换模型，先改问题或设计实验 |

## 三层读法：语言、识别、估计

`structural causal model`、`directed acyclic graph`、`d-separation`、`potential outcome`、`counterfactual` 是语言层，用来把假设写成可讨论的对象。`identifiability`、`back-door criterion`、`front-door criterion`、`instrumental variable`、`unconfoundedness`、`overlap`、`parallel trends` 是识别层，回答「这个量能不能从数据里算出来」。`propensity score`、`inverse probability weighting`、`matching`、`causal forest`、`generalized random forest`、`double machine learning`、`cross-fitting`、`doubly robust` 是估计层，回答「算出来时怎么少偏一点」。

三层不能互相替代。估计层再精巧，也救不回识别层不成立的问题；图上画得漂亮，也不等于估计器在你的数据上可信。综合报告口径建议按「先写图与识别路径、再选估计器、最后做反驳」的顺序推进；每一步都用自己的数据做诊断，不要照搬别人的效应值。

## 一个可验收的出口

读完十二关，出口标准不是记住某个算法名，而是能对着一个真实决策回答四句话：目标效应写成什么 `causal estimate`，在图与数据条件下是否可识别；你依赖的是哪组假设，随机化、条件可忽略、工具变量还是平行趋势；估计之后做了哪些反驳检验，`covariate balance`、`placebo test`、`negative control` 有没有过；这个结论上线后如何监测与回溯。

对做工程的人还要多一句：如果第二句答不出来，后面所有置信区间都只是更精确地描述偏差。这一句答不出来，前面三句答得再完整也算没过。

## 语料边界与口径

一手论文逐字留存，用于机制与术语的精确写法：DoWhy、Towards Causal Representation Learning、Causal Effect Inference with Deep Latent-Variable Models、Heterogeneous Treatment Effects using Random Forests、Generalized Random Forests、Counterfactual Fairness、CausalVAE、CausalML、Causal Discovery from Heterogeneous/Nonstationary Data、Pessimistic Uplift Modeling、Boosting for uplift modeling、Matching Methods for Causal Inference、Synthetic Difference in Differences、The Do-Calculus Revisited、Graphical models for mediation analysis、Deep Instrumental Variables、Introduction to Pearl's Do-Calculus、Generalized Optimal Matching Methods。官方文档给出接入形态：EconML、CausalML、DoWhy 的仓库与文档，以及两份高校课程资料。

综合报告只补工程判断，比例、成本与趋势一律标注「综合报告口径」。十二关的顺序、场景与速查表属于「延伸，非语料原文」，不加行内代码标记；行内代码只用语料中确实出现的标识符。本册不构成法律或医疗建议。