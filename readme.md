# Tetr.io规则观察总结 By MrZ_26

本文档中的`【XX】`是Mod的格式，详细内容见Mod章节

## 开始

该大房间不存在公共的“一局”的概念，所有的人随开随打，不需要等待

## 爬塔

此模式的背景题材为攀登 `天穹之塔 (Zenith Tower)`，最终分数称为`高度`（Altitude）  
每达到一定的高度时会进入下一层

| 层数 | 高度范围 | 名称 | 原文 |
| - | - | - | :-: |
| 一层 | 0m - 50m | 初始大厅 | Hall of Beginnings |
| 二层 | 50m - 150m | 酒店 | The Hotel |
| 三层 | 150m - 300m | 赌场 | The Casino |
| 四层 | 300m - 450m | 竞技场 | The Arena |
| 五层 | 450m - 650m | 博物馆 | The Museum |
| 六层 | 650m - 850m | 废弃办公楼 | Abandoned Offices |
| 七层 | 850m - 1100m | 实验室 | The Laboratory |
| 八层 | 1100m - 1350m | 核心 | The Core |
| 九层 | 1350m - 1650m | 污染区 | Corruption |
| 十层 | 1650m+ | 神之境 | Platform of The Gods |

## 推进器

`推进器等级`（下称`等级`）决定了爬塔的速度，所有增加高度的行为都受到这个倍率的加成，升级推进器需要经验

一级时倍率为×0.25，每升一级倍率增加×0.25（达到×2.75时变成白色不再看得出区别，实际无上限）

### 经验

| 动作 | 高度 | 经验 | 备注 |
| :--: | :--: | :--: | :-- |
| 自然增加 | 每秒 1m | 自然流失，详见`经验流失`章节 | 会`卡层`，距下一层6m~1m时逐渐变为0 |
| 击杀 | 15m | | 【专家+】时 8m |
| 发送 | `行数`m | `行数+0.05`经验 | |
| 抵消 | | `行数*50%+0.05`经验 | 【专家(+)】【双倍+】【双人+】时视为消0行 |
| 消行 | | `min(行数,2)+0.05`经验 | 【专家(+)】时***不触发***，【全旋+】时非spin均视为消1行 |
| `越层` | 3m | | 判定条件是目前高度(算上还未释放的临时高度)距下一层在2m内时***触发***发送/抵消/消行之一 |

> 上述高度增加除了`越层`外都受`等级`影响，例如开局时`等级`为1，倍率为×0.25，每4秒增加1m  
> `抵消惩罚`（计算公式详见后文）会让抵消获得的经验减少。当`抵消惩罚`超过`连续抵消扣经验阈值`后，每多出1会让公式中的50%减少0.5%，最低5%。这个阈值在开了【双倍(+)】时是40，否则开了【全旋(+)】时是10，否则是25

获得高度时，新增的高度会先存入一个临时变量，每帧释放10%，最大10m

推进器升级所需经验为`4*等级`，当经验达到升级所需经验时lv+1并扣除所需经验值

### 跳级

每次升级的同时，若还有大量剩余经验（发生在爆了大几十B2B产生巨量攻击时）仍然超过升级所需经验，则额外升`经验/所需经验`级，**且不扣除这些多余的经验**

    也就是能出现这种情况：
    某一帧 断B2B发出超大spike获得巨量经验
    下一帧 升级+跳级
    下一帧 还能升一次级

### 经验流失

经验会随时间自然流失，每帧减少 `mul*(等级^2+等级)/3600` 点  
其中的mul为：单人正常=3，开【专家(+)】或【双人+】时=5；双人时=3+专家人数

当经验小于0时候降级，经验变为上一级的满经验

推进器升级时会获得一个5秒的`经验流失保护`效果，防止升级后只剩一点点经验瞬间流失完导致立刻降级

`升级疲劳`机制：每次升级时，上一行中的`5秒不流失`会减少1秒（直至1秒），所以反复升降`等级`时这个效果就会越来越弱。  
想恢复的话需要在一次升级动作（而非降级）后紧接着达到50%（经验条中间的斜坡顶部），之后再触发此效果就会有5秒了

以下列出一些算好的数据方便查阅：

| `等级` | 升级所需经验 | 降级秒数 | 每秒流失 | 【专家(+)】降级秒数 | 【专家(+)】流失速率 |
| :--: | :-: | :---: | :--: | :---: | :----: |
|  1   | 4   | 40.00 |  0.1 | 24.00 |  0.17 |
|  2   | 8   | 26.67 |  0.3 | 16.00 |  0.50 |
|  3   | 12  | 20.00 |  0.6 | 12.00 |  1.00 |
|  4   | 16  | 16.00 |  1.0 |  9.60 |  1.67 |
|  5   | 20  | 13.33 |  1.5 |  8.00 |  2.50 |
|  6   | 24  | 11.43 |  2.1 |  6.86 |  3.50 |
|  7   | 28  | 10.00 |  2.8 |  6.00 |  4.67 |
|  8   | 32  | 8.89  |  3.6 |  5.33 |  6.00 |
|  9   | 36  | 8.00  |  4.5 |  4.80 |  7.50 |
|  10  | 40  | 7.27  |  5.5 |  4.36 |  9.17 |
|  11  | 44  | 6.67  |  6.6 |  4.00 | 11.00 |
|  12  | 48  | 6.15  |  7.8 |  3.69 | 13.00 |
|  13  | 52  | 5.71  |  9.1 |  3.43 | 15.17 |
|  14  | 56  | 5.33  | 10.5 |  3.20 | 17.50 |
|  15  | 60  | 5.00  | 12.0 |  3.00 | 20.00 |
|  16  | 64  | 4.71  | 13.6 |  2.82 | 22.67 |
|  17  | 68  | 4.44  | 15.3 |  2.67 | 25.50 |
|  18  | 72  | 4.21  | 17.1 |  2.53 | 28.50 |
|  19  | 76  | 4.00  | 19.0 |  2.40 | 31.67 |
|  20  | 80  | 3.81  | 21.0 |  2.29 | 35.00 |
|  21  | 84  | 3.64  | 23.1 |  2.18 | 38.50 |
|  22  | 88  | 3.48  | 25.3 |  2.09 | 42.17 |
|  23  | 92  | 3.33  | 27.6 |  2.00 | 46.00 |
|  24  | 96  | 3.20  | 30.0 |  1.92 | 50.00 |
|  25  | 100 | 3.08  | 32.5 |  1.85 | 54.17 |
|  26  | 104 | 2.96  | 35.1 |  1.78 | 58.50 |

### 外观

推进器信息显示在场地的下方，此处用文字简述一下外观：

| `等级` | 倍率 | 颜色 | 图案 | 速通模式备注 |
| :-: | :--: | :-: | - | - |
|  1  | 0.25 | 无 | 一个进度条 | |
|  2  | 0.50 | 红 | 下面加一个三角形 | |
|  3  | 0.75 | 橙 | 三角形长出翅膀 | |
|  4  | 1.00 | 黄绿 | 翅膀变大 | |
|  5  | 1.25 | 蓝 | 翅膀变大加底座 | |
|  6  | 1.50 | 紫 | 翅膀伸出进度条长度 | |
|  7  | 1.75 | 亮橙 | 翅膀更多细节 | 不退出速通模式的最低`等级` |
|  8  | 2.00 | 青绿 | 进度条上面加一对平行四边形 | 1/2层到此触发 |
|  9  | 2.25 | 青蓝 | 两对平行四边形 | 3/4层到此触发 |
| 10  | 2.50 | 亮紫 | 三对平行四边形 | 5层到此触发 |
| 11  | 2.75 | 白 | 加一对小三角 | |

### 速通模式（Hyperspeed）

当玩家在前五层时`等级`就达到8/8/9/9/10时就会进入速通模式  
会出现花里胡哨的致敬Bejeweled Twist的动画，和专属速通音乐

屏幕顶部出现大字显示当前局时间，底部列出完成前九层花的时间和离个人纪录的分段差异  
达到十层时完成速通模式获得一个隐藏成就，或者`等级`掉到6级时会退出速通模式

开启任何mod时也会禁用速通

## 攻击对象

本模式无法手动指定改变攻击对象，不过玩家的状态会影响自身被攻击的概率：`受击权重`  
这个值越大越容易被其他人攻击，**初始值为3**

注意，`受击权重`不是唯一的影响被别人攻击的因素，玩家的攻击对象由服务器全权决定，只能说这个值应该是主要因素之一

其他可能的增加被其他人选中概率的因素：

- 推进器等级高
- 和其他人高度差较小

### 危险时临时减免

`简单模式`限定：每0.25秒刷新一次，如果“处于危险中”（详细条件没看懂，当成到顶版面闪红警告吧，不会差很多），`受击权重`会临时减1.5，脱离危险后1.5会加回来

### 随时间增加 （7分钟封顶翻一倍）

当时间达到3/5/7分钟时`受击权重`+1

`受击权重`随时间/被快速打满/处于危险状态中的变化：

| 时间段 | 权重 | 打满时 | 危险时 |
| - | :-: | :-: | :-: |
| 0~2分钟 | 3 | -100% | -50% |
| 3~4分钟 | 4 | -75% | -75% |
| 5~6分钟 | 5 | -66% | -60% |
| 7分钟后 | 6 | -50% | -25% |

## 攻击、全旋、B2B

普通消1、2、3行攻击为0、1、2

`简单模式`限定：0连击普通消一+1攻击，这让散消一也有1的攻击效率了，特定情况可以考虑利用

消4攻击为4，属于`特殊消除`（增加B2B计数，用于`蓄力攻击`，详见后一节）

此模式使用`All-Mini+`规则，是`全旋`（All-Spin）的一个亚种，除了T外其他所有块的Spin也都计`特殊消除`  
其中只有T块通过三角判定才算普通Spin，拥有`2*消行数`的基础伤害，不满足三角或者非T块满足不可移动判定都计为Mini，基础伤害和普通消除一致，为`消行数-1`

全消攻击为3，但同时全消计为两次`特殊消除`（和Spin独立计算）（TL中略有不同，5攻击但不加也不断B2B）

连续`特殊消除`从第二个开始会累计B2B计数，同时+1伤害

小数取舍规则采用RNG，计算过程保留小数，最后把小数部分作为概率决定是否向上取整，例如1.2最后有20%概率变成2，80%概率变成1

### 蓄力攻击（Surge Attack）

断B2B时会产生一个超大攻击，伤害为`B2B数-3`（或者说连续特殊消除数-4）（TL中略有不同，几B2B就打几，不-3）

> 这是qp中一个很重要的机制，特定情况下很需要使用

### 多段攻击（Windup Attack）

如果收到了单块发送的8点及以上的攻击，这个攻击会被拆分成多段接收并带有预警提示，防止由于各种原因导致受击倍率增加后容易暴毙

拆分规则：把攻击拆成若干个4和剩余部分，最多四段（装不下的全都放在最后一段），例如14攻击会被拆成4+4+4+2

预警方式：收到此种攻击时先开始播放警告动画和提示音  
提示音的数量对应本轮攻击总共被拆成了几节（例如响3声对应4+4+n，也就是9~12的攻击）  
1s后然后攻击开始进入缓冲条，每一段间隔0.5s

如果开启了【双倍】，上述和攻击数量有关的值都翻倍（近似，个别情况会±1）

## 其他攻击相关

### 连续抵消

**此机制基本可以认为是由于定式[机械之心](https://bilibili.com/opus/997806608970940469)的出现而添加的，用于防止无脑定式高速循环，只要能抵消全部垃圾就可以杀穿大量项目**

1. 有一个`连续抵消`变量，抵消N行垃圾 **+N**  
1. 全消时 **+5**  
1. 【机心专杀】记录S/Z spin消行时用的是S还是Z，如果连着六次都相同则 **+2**  
1. 【机心专杀】如果I块竖着放在场地边两列没有消行，那么检查同侧边两列的另一列相邻四格，如果都是I块的青色格子则 **+2**  
1. 消除灰色格子（垃圾行）时立刻 **减为0**  
1. 攻击缓冲释放时，场地里每冒出一行垃圾 **-3**  
1. I块spin消行时 **-2**  
1. 消四时 **-3** ，记录列数，如果和前两次（不重复记录，只留新的）都不一样则再 **-4**  
1. S/Z spin消行时和I块消四一样，只是换成旋转中心列数，都不一样时 **-2**  
1. J/L spin消行时和I块消四一样，只是换成旋转中心列数，都不一样时 **-2**  
1. 每30秒没有做减少`连续抵消`的动作时 **+5**  
1. 每75块没有做减少`连续抵消`的动作时 **+5**  

> 注1：该值不会被降低到0以下  
> 注2：唉机心专杀都整出来了，在此宣布机心胜利！Garbo你在干什么啊Garbo（  
> 注3：上述所有条件都已经被机心的拓展版本攻克，虽然要多学一些变化

当`连续抵消`达到一定值后，原本的7-bag序列会被修改，向每包添加额外方块：

- 20: O块
- 30: L/J随机一块
- 40: S/Z随机一块，初次达到时会再加一个i5（5格长条，一局仅一次）
- 50: L/J随机一块
- 50: T/I随机一块
- 60: S/Z随机一块

还会缩短收到的垃圾行数量，详见`受击数量计算`章节

### 受击保护

有一个`受击保护`变量，受到攻击时增加和红条等量的值（经过了各种惩罚/保护调整的值，不是原始攻击数，除了【双倍+】的三倍不计），直到**18**封顶

`受击保护`内的值会降低`垃圾混乱度`，不过此机制会被以下原因关闭：【专家+】中游戏时间达到6分钟、【混乱(+)】

如果`受击保护`>0，则会在距离“最后受击时间”一段时间后减少1，并刷新“最后受击时间”为当前时间，释放间隔如下表：

| 层数 | 释放间隔（秒） | 【专家+】时 |
| :-: | :-: | :-: |
| 1  | 4.8 | 1.0 |
| 2  | 3.9 | 0.9 |
| 3  | 2.1 | 0.8 |
| 4  | 1.4 | 0.7 |
| 5  | 1.3 | 0.6 |
| 6  | 0.9 | 0.5 |
| 7  | 0.6 | 0.4 |
| 8  | 0.4 | 0.3 |
| 9  | 0.3 | 0.2 |
| 10 | 0.2 | 0.1 |

### 受击数量计算

受到攻击时会根据各种因素调整数量，最终在攻击缓冲中出现红条，具体流程如下：

#### 1.高度惩罚

计算`临界高度`=`max(min(攻击者高度,3500), min(高度-1000,2000))`

如果高度大于`临界高度`，攻击乘`100% + 0.4% * (高度-临界高度)^2/(高度+临界高度)`

#### 2.队友惩罚

如果开了【双人(+)】而且队友死了，攻击乘`150%`

#### 3.抵消惩罚（定式惩罚）

先取`惩罚系数`：  
默认0.001  
有【双倍】没【全旋】：0.0003  
有【全旋】没【双倍】：0.002

攻击加`惩罚系数 * 连续抵消 ^ 2`，不过增加量不会超过：

- 基础上限：看楼层，前七层为 **层数+3**，八层后无限
- 如果正在windup：和6取较小值
- 如果有【双倍】则乘2

#### 4.老带新保护

如果开了【双人(+)】且通过发送/击杀提供的高度贡献占比小于20%，攻击乘`55%+贡献度%`

#### 5.受击保护保护

如果`受击保护`超过了8，攻击乘`100% - 5%*(受击保护-8)`

#### 结算

攻击乘`受击倍率`，取整（qp中是以小数作为权重随机），生成红条

## 垃圾混乱度

垃圾行洞位置连续的概率。该值越大，每一行洞位置越容易都不同

TETR.IO中垃圾行的混乱度由两个数字决定：  
“同一段垃圾中每一行有X%的概率重抽位置”，“不同段垃圾行有Y%的概率重抽位置”

在TL中X=0%，Y=100%，也就是同段垃圾一定在同列，不同段几乎一定不同列（重抽到同一个位置就不变，10%）  
而在qp2中这两个数就不这么极端了，所以你会感觉qp2中垃圾行洞的位置似乎和几段垃圾行关系不大

在qp2中，默认Y=2.5*X，也就是打入垃圾行的间断点处有更大的概率（2.5倍）不在同一列

本文档中的`垃圾混乱度`就是这个X，会受到各种因素影响

| 因素 | 影响值 |
| :-: | :-: |
| 层数 | `层数*3%`，【专家(+)】时`层数*5%` |
| 【混乱(+)】 | +25% (100%) |
| 【全旋+】 | +30% |
| 【专家+】疲劳11分钟的`全散`效果 | =100%（前面的计算可超100%，此效果覆盖） |
| `受击保护`（最终生成时计算） | 每1点`受击保护`让Y减少3.75%，X减少1.5% |

> 受击保护满18点时，Y减少67.5%，X减少27%
> 真的没写反，这里挺奇怪的，可能需要拉个表格观察一下各种因素导致的混乱度变化
> 吃了`全散`效果后两个值都被设为100%时反而在间断处更容易在同一列，很神奇

## 垃圾易挖度

决定了垃圾行洞位置的选取倾向。该值可正可负：正数越大，洞就会选得越难挖，同理，负得越多越好挖

该值为0时（TL等模式的通常情况），垃圾行洞的位置均等随机选取。如果同时开启了`垃圾行不连续`（messiness_nosame，如自定义房间设置），则会避开上一次选取的列

在qp2中，该值默认的公式为`33-楼层数*3`，越到后面越简单，疑似是用来抵消混乱度增加导致难度增加过快的  
开启【专家(+)】时，该值-33，也就是开局时均等随机  
开启【混乱(+)】时，该值-25  
开启【双倍+】时，该值全程锁定为50

> 最坏的情况是【专家】【混乱】十层后的 -55
> 最简单的是【双倍+】的 50

详细的选择垃圾行洞位置流程：

1. 计算每一列的`挖掘难度分`
    1. 如果该列是最高列（之一）且没有空格，该列`挖掘难度分`为0
    1. 找到该列最低的空格，找到场地最高点的高度
    1. `挖掘难度分 = 空格到场地顶的高度差 + 5*空格到最浅垃圾行洞的水平距离`
1. 根据`挖掘难度分`给十列进行排序，分低的也就是好挖的列在前（相同的值随机排）（曾经分高的在前，完全反了！唉谁写的代码）
1. 根据`垃圾易挖度`计算概率分布，公式为`weight[i] = 10 + 垃圾易挖度 * (1 - i / 4.5)`，负数视为0
    1. 你可以在这个[Desmos文件](https://www.desmos.com/calculator/yfzabziltb)里观察一下图像
    1. 图像的X轴为列编号（左侧第0列最好挖，右侧第9列最难挖），Y轴为权重，用左侧滑块调整favor的值
1. 根据上一步计算出的权重随机选择一列作为垃圾行的洞的位置

## 垃圾集中

有一个`垃圾集中`开关，在`垃圾混乱度`<=15%且5层以内开启

开启时，垃圾行洞位置一定不会在场地两旁的两列，也就是一定在3~8列

## 垃圾行等待时间

收到攻击时垃圾行会在缓冲条中等待一段时间再进入可触发状态，此时落块不消行就会上涨：黄色中空 → 红色中空 → 红色实心（可触发）  
等待时间由楼层和Mod决定，具体值如下表：

| 层数 |  普通（非专家）   |  【专家(+)】   | 【混乱+/双倍+/双洞+】 | 其他【Mod+】 |
| :-: | :----------: | :---------: | :-------------: | :-----: |
|  1  |  5.0秒 (300f)  | 2.2秒 (132f) |   **2.5秒**    |**2.5秒**|
|  2  |  4.5秒 (270f)  | 2.0秒 (120f) |   **2.5秒**    |**2.5秒**|
|  3  |  4.0秒 (240f)  | 1.8秒 (108f) |   **2.5秒**    |**2.5秒**|
|  4  |  3.5秒 (210f)  | 1.6秒  (96f) |   **2.5秒**    |**2.5秒**|
|  5  |  3.0秒 (180f)  | 1.4秒  (84f) |   **2.5秒**    |**2.5秒**|
|**6**|**2.5秒 (150f)**| 1.2秒  (72f) |   **2.5秒**    |**2.5秒**|
|  7  |  2.0秒 (120f)  | 1.0秒  (60f) |   **2.5秒**    | `2.0秒` |
|  8  |  1.5秒  (90f)  | 0.8秒  (48f) |   **2.5秒**    | `1.5秒` |
|  9  |  1.0秒  (60f)  | 0.6秒  (36f) |   **2.5秒**    | `1.0秒` |
| 10  |  0.5秒  (30f)  | 0.4秒  (24f) |   **2.5秒**    | `0.5秒` |

> 由于垃圾行要切换两次才进入激活状态，故代码中的数据是上表中的一半

## 疲劳时间

为了防止一局游戏过长，从8分钟开始每分钟会获得一个负面效果

| 时间 | 负面效果 | 文本 | 原始文本 |
| --- | --- | --- | --- |
|  8:00 | +2行实心垃圾 | 疲劳开始侵蚀… | FATIGUE SETS IN… +2 PERMANENT LINES |
|  9:00 | +25%受击倍率 | 你的身体变得虚弱… | YOUR BODY GROWS WEAK… receive 25% more garbage |
| 10:00 | +3行实心垃圾(共5) | 所有感官混为一团… | ALL SENSES BLUR TOGETHER… +3 PERMANENT LINES |
| 11:00 | +25%受击倍率 | 你的意识开始消逝… | YOUR CONSCIOUSNESS FADES… receive 25% more garbage |
| 12:00 | +5行实心垃圾(共10) | 结束了。 | THIS IS THE END. +5 PERMANENT LINES |

> 【专家+】时疲劳效果不同，详见后文

## Mod

Mod是在游戏开始前可选的主动增加游戏难度的方式，基本只有坏处没有好处，但是开启Mod（或特定的Mod组合）后爬到特定高度可以获取成就

Mod总共有9个，每个都对应一个特殊效果可以独立开关，在背景设定中呈现为塔罗牌，下文会在每个Mod的名称后注明，章节最后也有一个表格用于速查

本文档中的`简单模式`指没开【专家(+)】也没开【Mod+】

### 专家 （皇帝 The Emperor）

> A display of power for those willing to bear its burden.  
> 为那些愿意承受其重负的人而准备的力量的展现。

- 更难获得高度和经验
- `垃圾混乱度`增加
- `垃圾易挖度`降低
- 移除垃圾上涨动画，瞬间出现（和TL一样）
- 移除“处于危险中”减少`受击权重`的机制

### 无暂存 （节制 Temperance）

> Use each piece as they come and embrace the natural flow of stacking.  
> 跟随序列的流动，拥抱自然的堆叠。

- 禁用暂存

### 混乱 （命运之轮 Wheel of Fortune）

> The only constant in life is change.  
> 生活中永恒不变的就是变化。

- `垃圾混乱度`增加
- `垃圾易挖度`降低

### 重力 （塔 The Tower）

> What will you do when it all comes crumbling down?  
> 大厦将倾时，你会如何应对？

- 重力显著增加
- 十层的锁定延迟表（单位 帧）： 30, 29, 28, 27, 26, 24, 22, 20, 18, 16

### 双倍 （力量 Strength）

> Match great obstacles with greater determination.  
> 面对巨大的障碍，拿出更大的决心。

- `受击倍率`和`抵消倍率`都变为2倍

> 可以大致认为是从场地底部升起的垃圾行数量翻倍
> 由于只是让垃圾行的数量直接增加，所以变相地降低了垃圾混乱度，所以此Mod可以加快前期爬楼进度，不过后期威胁太大所以好不好得看具体在打什么项目

### 双洞 （恶魔 The Devil）

> Redefine your limits or succumb to his chains.  
> 突破你的极限，或是屈服于他的枷锁。

- 每行垃圾行可能会有两个洞

### 隐形 （隐士 The Hermit）

> When the outside world fails you, trust the voice within to light a path.  
> 若外界的喧嚣让你失去信心，内心的声音会为你指引方向。

- 放置方块会隐形
- 每5秒会全场闪烁一次方便挖开

### 全旋 （魔法师 The Magician）

> Inspiration is nothing short of magic.  
> 涌现的灵感无异于魔法。

- 非T块的Spin都能像T块一样提供消行数*2的基础伤害了（Spin判定规则见开头）
- 每当进行消除时（准确说其实是锁定一块时如有冒出文本，不消行Spin也能触发），如果左上角提示文本完全一致，那么立刻出现一行特殊的实心垃圾行作为惩罚，上面随机位置写了一个倒计时数字为`当前层数+5`，当玩家做未被惩罚的消除时-1，归零时变为一行普通的单洞垃圾行，洞位置和数字位置一致

> 唯一一个玩家水平足够后能提供较大正面效果的Mod

### 双人 （恋人 The Lovers）

> Love, and resign yourself to the fate of another.  
> 去爱吧，与另一人休戚与共。

- 会员玩家可以邀请其他人和自己两个人一起玩此模式
- 对于每个人自己来说，大多数对外的数值都会被砍半，例如发送攻击和累计经验等
- 两个人都死了后游戏结束，但一个人死后另一个人可以做任务复活队友

会出现的任务如下表：

| 难度标记 | 内部id | 目标数值 | 内容 | 外显文本 | 标签类型（？） | 有特定Mod时不出现 |
| - | - | - | - | - | - | - |
| F | combo              | 3   | 3连击（四连消） | Perform a 3-Combo | 2 | |
| F | double             | 2   | 2 x 消二 | Clear 2 Doubles | 2 | |
| F | quad               | 1   | 消四 | Clear a Quad | 1 | |
| F | lines              | 6   | 消6行 | Clear 6 Lines | 1 | |
| F | osingle            | 1   | O块消一 | Clear a Single\nusing an O-Piece | 3 | |
| F | odouble            | 1   | O块消二 | Clear a Double\nusing an O-Piece | 3 | |
| F | szdouble           | 1   | S/Z消二 | Clear a Double\nusing an S or Z-Piece | 3 | |
| F | ljtriple           | 1   | L/J消三 | Clear a Triple\nusing an L or J-Piece | 3 | |
| F | iholdlines         | 3   | 暂存着I 消三行 | Clear 3 lines\nwhile holding an I-Piece | 3 | 【无暂存】 |
| F | hold               | 8   | 暂存8下 | Use Hold 8 times | 2 | 【无暂存】 |
| F | rotate             | 20  | 转20下 | Rotate 20 times | 2 | |
| F | singleconsecutive  | 2   | 2连消一 | Clear 2 Singles in a row | 3 | |
| E | spin               | 1   | 任意spin | Perform any Spin | 2 | |
| E | tspinsingle        | 1   | T旋消一 | Clear a T-Spin Single | 2 | |
| E | tspindouble        | 1   | T旋消二 | Clear a T-Spin Double | 2 | |
| E | szspin             | 1   | S/Z旋消除 | Clear an S/Z-Spin | 1 | |
| E | ljspin             | 1   | L/J旋消除 | Clear an L/J-Spin | 1 | |
| E | combo              | 5   | 5连击（六连消） | Perform a 5-Combo | 2 | |
| E | iflat              | 2   | 2 x 横放I消一 | Clear 2 Lines using\nhorizontal I-Pieces | 3 | |
| E | pieces             | 20  | 放20块 | Place 20 pieces | 2 | |
| E | attack             | 6   | 送出6行攻击 | Send 6 Attack | 1 | |
| E | placeoconsecutive  | 2   | 连放两块O | Place 2 O-Pieces\nin a row | 3 | |
| E | norotateclockwise  | 12  | 只用逆时针转 连放12块（其他旋转时重来） | Place 12 pieces while only\nrotating counterclockwise | 4 | |
| E | singlenocombo      | 6   | 6 x 消一 不连击（二连消时重来） | Clear 6 Singles without\nstarting a combo | 3 | |
| D | double             | 4   | 4 x 消二 | Clear 4 Doubles | 2 | |
| D | spam               | 3   | 不转不移 连放3块 | Place 3 pieces in a row\nwithout moving or rotating | 4 | |
| D | noclear            | 14  | 不消行 连放14块 | Place 14 pieces in a row\nwithout clearing any lines | 4 | |
| D | szdouble           | 2   | 2 x S/Z消二 | Clear 2 Doubles\nusing S or Z-Pieces | 3 | |
| D | ljtriple           | 2   | 2 x L/J消三 | Clear 2 Triples\nusing L or J-Pieces | 3 | |
| D | ispinclear         | 1   | I旋消除 | Clear an I-Spin | 1 | |
| D | upperhalfquad      | 1   | 在上半场消四（I块最低格至少第10行） | Clear a Quad in the\nupper half of the board | 4 | |
| D | rotate             | 80  | 转80下 | Rotate 80 times | 2 | |
| D | quadcombo          | 1   | 连击消四（至少第三连消） | Clear a Quad\nwhile on a 2+-Combo | 4 | |
| D | szsingle           | 2   | S/Z块 2连消一 | Clear 2 Singles in a row\nusing S or Z-Pieces | 4 | |
| D | combonohold        | 3   | 不暂存 3连击（四连消） | Perform a 3-Combo\nwithout using Hold | 3 | |
| D | noclearspin        | 3   | 3 x 不消除的旋 | Perform 3 Spins\nthat don't clear any lines | 4 | |
| D | szljspin           | 2   | 2 x S/Z/J/L旋 | Perform 2\nS/Z/L/J-Spins | 3 | |
| C | tspintriple        | 1   | T旋消三 | Clear a T-Spin Triple | 2 | |
| C | nohold             | 25  | 不暂存 连放25块 | Place 25 pieces in a row\nwithout using Hold | 4 | 【无暂存】 |
| C | triple             | 3   | 3 x 消三 | Clear 3 Triples | 2 | |
| C | b2b                | 4   | B2B达到4 | Reach B2B x4 | 1 | |
| C | quadbuckets        | 2   | 2 x 不同列的消四 | Clear a Quad in\n2 different columns | 3 | |
| C | holdconsecutive    | 12  | 一直暂存 连放12块 | Use Hold on\n12 pieces in a row | 3 | 【无暂存】 |
| C | softdrop           | 10  | 一直按软降 连放10块 | Place 10 pieces without\nreleasing Soft Drop | 4 | |
| C | top3rows           | 3   | 最高堆到至少第18行 保持3秒 | Have part of your stack in\nthe top 3 rows for 3 seconds | 4 | |
| C | linesnoti          | 10  | 消10行 不用T和I（用T/I消除时重来） | Clear 10 Lines without\nclearing with T or I-pieces | 4 | |
| C | szspintriple       | 1   | S/Z旋消三 | Clear an S/Z-Spin Triple | 2 | |
| C | odoubleconsecutive | 2   | 2连O块消二 | Clear 2 Doubles consecutively\nusing two O-Pieces | 4 | |
| C | tspinminiclear     | 4   | 4 x T旋mini消 | Clear 4 T-Spin Minis | 2 | |
| C | attack             | 14  | 送出14行攻击 | Send 14 Attack | 1 | |
| C | doublespiece       | 3   | 3 x 消二 只用一种块（其他块消二时重来） | Clear 3 Doubles\nwith the same type of piece | 4 | |
| C | ljgarbage          | 1   | L/J旋挖垃圾行 | Clear Garbage\nusing a L/J-Spin | 3 | |
| C | szgarbage          | 1   | S/Z旋挖垃圾行 | Clear Garbage\nusing a S/Z-Spin | 3 | |
| C | columnopiece       | 3   | 在第一列放三个O块 | Place 3 O-Pieces\nin column 1 | 3 | |
| C | spinclear          | 2   | 一轮连击包含2次旋消 | Clear 2 Spins\nin one combo | 3 | |
| C | iclearspam         | 1   | 不动I块消一 | Clear a Single with an I-Piece\nwithout moving or rotating | 4 | |
| C | holddas            | 6   | 保持充着DAS 连放6块（任一移动键状态变化时重来） | Place 6 Pieces\nwithout releasing DAS | 3 | |
| B | oclear             | 6   | 消6行 用O块 | Clear 6 Lines\nusing O-Pieces | 3 | |
| B | spinbuckets        | 3   | 三个不同块旋消 | Clear Spin-Clears\nwith 3 different pieces | 3 | |
| B | quad               | 4   | 4 x 消四 | Clear 4 Quads | 1 | |
| B | spam               | 5   | 不转不移 连放5块 | Place 5 pieces in a row\nwithout moving or rotating | 4 | |
| B | ljspintriple       | 1   | L/J旋消三 | Clear an L/J-Spin Triple | 2 | |
| B | quadconsecutive    | 2   | 2连消四 | Clear 2 Quads in a row | 2 | |
| B | singlesonly        | 8   | 8 x 消一 无暂存 （消二三四/暂存时重来） | Clear 8 Singles without doing\nother clears or using Hold | 4 | |
| B | nogarbage          | 4   | 场内无垃圾 保持4秒 | Have no Garbage Lines on\nyour board for 4 seconds | 4 | 【双人+】 |
| B | rotate             | 300 | 转300下 | Rotate 300 times | 2 | |
| B | nocancel           | 8   | 不抵消垃圾 保持8秒 | Don't cancel any\ngarbage for 8 seconds | 3 | |
| B | tspindoubleup      | 1   | T旋消二，但朝上 | Clear a T-Spin Double\nwith the Piece pointing up | 4 | |
| B | oclearspam         | 1   | 不动O块消二 | Clear a Double with an O-Piece\nwithout moving or rotating | 4 | |
| B | tnorotate          | 3   | 放三个不旋转的T （旋转T块时重来） | Place 3 T-Pieces\nwithout rotating any | 3 | |
| B | tspincombo         | 1   | 连击T旋消二（至少第三连消） | Clear a T-Spin Double\nwhile on a 2+-Combo | 3 | |
| A | combo              | 7   | 7连击（8连消） | Perform a 7-Combo | 2 | |
| A | ispindouble        | 1   | I旋消二 | Clear an I-Spin Double | 2 | |
| A | szspinconsecutive  | 2   | 2连S/Z旋消二 | Clear two S/Z-Spin\nDoubles consecutively | 3 | |
| A | ljspinconsecutive  | 2   | 2连L/J旋消二 | Clear two L/J-Spin\nDoubles consecutively | 3 | |
| A | colorclear         | 1   | 色彩消除 | Perform a Color Clear | 2 | |
| A | lines              | 40  | 消40行 | Clear 40 Lines | 1 | |
| A | combospin          | 4   | 一轮连击包含4次旋消 | Clear 4 Spins\nin one Combo | 3 | |
| A | tspindtcolumn      | 1   | T旋消二/三，但旋转中心在边列 | Clear a T-Spin Double/Triple\ncentered in column 1 or 10 | 3 | |
| X(特殊) | ospinconsecutive | 2 | 2连O旋消二 | Clear two O-Spin Mini\nDoubles consecutively | 3 | |

有一个`复活累积难度`变量，复活时增加（前五层+1，六至九层+2，10层+3）

需要复活时计算复活难度分=`层数+复活累积难度`，然后根据分数从下表选取任务方案，最后从上表里随机选取任务并打乱顺序：

1. F
2. F F
3. F F F
4. F F E
5. F E E
6. E E E
7. E E D
8. E D D
9. D D D
10. D D C
11. D C C
12. C C C
13. C C B
14. C B B
15. B B B
16. B B A
17. A B A (上限，并且此时固定顺序ABA不打乱)

## Mod+

每个Mod都有对应的强化版本，需要带着对应的Mod累计爬3万米解锁（开了多个可以同时累计）
【双人+】特殊，见对应小节

所有的Mod+都只能单独开启游玩，不能再叠加其他Mod

### 尽快解锁

如果你想尽快解锁所有Mod+的话建议同时开几个，以下给出一个参考方案（假设你可以畅玩所有的单Mod且能拿到几个f10）：

第一轮：`【混乱】【重力】【双洞】【全旋】`四个一起开然后靠allpin猛猛输出，尽量不要让垃圾行进来，刷满3万米。如果不怕`【隐形】`就也可以带上，否则之后要单独打

第二轮：然后分别打`【专家】`、`【无暂存】`、`【隐形】`，有能力也可以合起来打（虽然更难但进度双倍了，可以计算一下效率再决定），如果只打一个的话就推荐带上`【双倍】`，尽量多吃垃圾然后用消四挖开就可以爬得快很多（同时尽量打出攻击而不是抵消，奖励更多，但哪怕抵消也是双倍的量，所以推荐开双倍）

另外如果有些成就项目没怎么打过的话也可以顺便打一打，比如`【专家】【无暂存】【双洞】`，就可以把第一轮四个mod中的`【双洞】`拿走放到第二轮里，刷得更有意义一些

### 一些之后会出现的效果说明

1. `垃圾行保护`（仅在部分Mod中使用）：
场地内的每一行`非永久垃圾行`（包含可消除的***灰色***格子的行）会使`受击权重`减少0.5，至多减2.5（由于初始权重为3，所以这几个模式开局很少被打），每0.25秒刷新一次

### 专家+ （暴君 The Tyrant）

> Fear, oppression, and limitless ambition.  
> 恐惧、压迫和无限的野心。

- 在原有的所有限制基础上，
- 击杀基础奖励从15m减少为8m
- 推进器不再随着时间增加高度，必须通过输出和击杀才能获得高度
- `受击保护`释放更快（详见`受击保护`相关章节）
- 添加下坠机制，高度会随时间下降，不过会被每层的地板拦住不降层
- 在每一层停留的时间超过60秒时，每秒永久增加0.5%受击倍率（例如累计生效200秒后收到攻击全都双倍）
- 疲劳机制变得更严苛

|     层数     |  下坠速度 | 对应`等级`和spm |
| :--------: | :-----: | :---------: |
|  1 (0m)    | 0.6 m/s | 3级 & 48spm |
|  2 (50m)   | 0.8 m/s | 4级 & 48spm |
|  3 (150m)  | 1.1 m/s | 4级 & 66spm |
|  4 (300m)  | 1.5 m/s | 5级 & 72spm |
|  5 (450m)  | 2.0 m/s | 5级 & 96spm |
|  6 (650m)  | 2.6 m/s | 6级 & 104spm |
|  7 (850m)  | 3.3 m/s | 6级 & 132spm |
|  8 (1100m) | 4.1 m/s | 6级 & 164spm |
|  9 (1350m) | 5.0 m/s | 7级 & 171spm |
| 10 (1650m) | 6.0 m/s | 7级 & 206spm |

> 下坠速度公式是 `(层数^2+层数+10)/20`
> `等级`和spm仅供大致参考，实际游玩中抵消不计入spm所以基本做不到，五六层后基本必须靠三位数的蓄力攻击才能继续往上爬
> 游玩时下落看起来有加速度，只是视觉效果，实际还是匀速的

| 时间 | 负面效果 | 文本 | 原始文本 |
| --- | --- | --- | --- |
|  6:00 | 垃圾行更混乱(禁用`受击保护`效果) | 你的权力开始流逝… | YOUR POWER SLIPS… garbage received becomes messier |
|  7:00 | +25%受击倍率 | 流言开始大肆传播… | WHISPERS OF DISCONTENT SPREAD… receive 25% more garbage |
|  8:00 | +3行实心垃圾 | 示威人群涌上街头… | PROTESTERS LINE THE STREETS… +3 PERMANENT LINES |
|  9:00 | +25%受击倍率 | 昔日同盟背叛了你… | YOUR CLOSEST ALLIES DEFECT… receive 25% more garbage |
| 10:00 | +5行实心垃圾(共8) | 偏执遮蔽了你的理性… | PARANOIA CLOUDS YOUR JUDGEMENT… +5 PERMANENT LINES |
| 11:00 | 垃圾行彻底混乱(获得`全散`效果) | 革命拉开了帷幕… | THE REVOLUTION HAS BEGUN… garbage received becomes much messier |
| 12:00 | +12行实心垃圾(共20) | 时代的终结。 | THE END OF AN ERA. +12 PERMANENT LINES |

### 无暂存+ （禁欲 Asceticism）

> A detachment from even that which is moderate.  
> 对哪怕是中庸之物的疏离。

- 禁用暂存
- 1预览
- 没有影子
- 纯随机序列
- Spin全都计为Mini（基础攻击为`消行数-1`）
- 垃圾行都变成2宽的样式

### 混乱+ （铅骰 Loaded Dice）

> In a rigged game, your mind is the only fair advantage.  
> 在被操纵的游戏中，你的头脑是唯一合理的优势。

- `垃圾混乱度`增加
- 消行延迟从0增加到1.15秒
- 初始场地变成固定的六个圈的图案
- 启用`垃圾行保护`

```
开局图案：
..........
.XXX..XXX.
.X.X..X.X.
.XXX..XXX.
..........
.XXX..XXX.
.X.X..X.X.
.XXX..XXX.
..........
.XXX..XXX.
.X.X..X.X.
.XXX..XXX.
..........
```

### 重力+ （自由落体 Freefall）

> The ground you stood on never existed in the first place.  
> 你所站立的地面从始至终都不存在。

- 从一开局起就20G（十层的锁定延迟：24，22，20，18，16，15，14，13，12，11）

### 双倍+ （顽抗 Last Stand）

> Strength isn't necessary for those with nothing to lose.  
> 对那些一无所有的人而言，力量并非必需。

- 受击倍率变为3倍（注意：`抵消倍率`不变）
- 14高场地
- 垃圾行孔洞位置有两个预告
- `垃圾易挖度`锁定在一个很高的值，且永远开启`垃圾集中`

### 双洞+ （天谴 Damnation）

> Neither the freedom of life or peace of death.  
> 生无自由，死无宁静。

- 初始场地变成12行棋盘垃圾行
- 垃圾行变成每行随机3~4个灰格的混沌垃圾行
- Spin判定规则改为`T-spin+`
- 无法攻击（包括抵消），除非是在：消除垃圾行时，或者带有“BLIGHTED”效果时
- BLIGHTED：消除垃圾行之后获得，持有时下一次消除攻击*1.75，如果没有消除垃圾时buff消失并+1攻击
- 场地内固定有4行垃圾，挖了瞬间出
- 启用`垃圾行保护`（呃…不是固定有4行吗）

### 隐形+ （流亡者 The Exile）

> Never underestimate blind faith.  
> 永远不要低估盲目的信仰。

- 放置方块会隐形（但场地不再闪烁）
- 垃圾行只能看到最浅层的三行
- 开局会收系统3行垃圾

### 全旋+ （邪术师 The Warlock）

> Into realms beyond heaven and earth.  
> 踏入超脱天地之境界。

- 所有非Spin消除称为“Void”，无攻击
- 更苛刻的连续相同消除惩罚：只记录Spin不同行数（包括0）或者Void的动作，只要连续两个动作相同就罚20行直接死亡
- 到达B2B x 4之后，任何Spin（包括不消行的）+2攻击，而且不消行也能增加b2b计数
- 开局会收系统10行垃圾
- `垃圾混乱度`增加
- 启用`垃圾行保护`

## 特殊Mod

### 双人+ （流血之心 Bleeding Hearts）

> Even as we bleed, we keep holding on...  
> 即便已伤痕累累，我们仍紧紧相拥…

2025年情人节活动限定，一周内所有人免费游玩（也包括正位），无需解锁

- 攻击时（送出/抵消都算）有Backfire，优先给队友，如果已经死了就打给自己（参数是zen模式中的unclear 0.5x，无预警无等待瞬间打入场地）
- 任一人死了的时候高度会被暂时锁定，直到其复活
- 锁定最大等级为4（通常无限制）
- 特殊的疲劳流程

|  时间  | 效果 | 文本 | 原始文本 |
| ----- | --- | --- | --- |
|  1:00 | `垃圾混乱度`=5% | 两人的关系陷入了停滞… | THE RELATIONSHIP STAGNATES…  garbage becomes a bit messier |
|  1:30 | `垃圾混乱度`=15% | 不安感愈发强烈… | INSECURITIES GROW STRONGER…  garbage becomes messier |
|  2:00 | `垃圾混乱度`=30% | {队友}感到被忽视… | {PARTNER} FEELS NEGLECTED…  garbage becomes much messier |
|  2:30 | `垃圾混乱度`=20% | {自己}的道歉被接受了…？ | {SELF} SUCCESSFULLY APOLOGIZES…?  garbage becomes a bit cleaner |
|  3:00 | `垃圾混乱度`=0% | 一切回归正轨…！ | THINGS ARE BACK TO HOW THEY SHOULD BE…!  garbage becomes much cleaner |
|  3:30 | `垃圾混乱度`=10% | 未说出口的话是如此沉重… | THE WEIGHT OF WORDS UNSPOKEN…  garbage becomes messier |
|  4:00 | `垃圾混乱度`=25% | “你就不能好好听我说吗？” | "WHY CAN'T YOU JUST LISTEN TO ME?"  garbage becomes much messier |
|  4:30 | 复活难度+3级 | “这都是你的错。” | "THIS IS ALL YOUR FAULT."  revive difficulty increased |
|  5:00 | `垃圾混乱度`=10% | {队友}道了同样的歉… | {PARTNER} MAKES THE SAME PROMISE AGAIN…  garbage becomes cleaner |
|  5:30 | +4行实心垃圾 | “这次不一样了。” | "THIS TIME WILL BE DIFFERENT."  +4 PERMANENT GARBAGE |
|  6:00 | `垃圾混乱度`=30% | 有些习惯是无法改变的… | SOME HABITS CAN'T BE BROKEN…  garbage becomes much messier |
|  6:30 | `垃圾混乱度`=40% | 所有的信任都消逝无踪… | ALL TRUST HAS WITHERED AWAY…  garbage becomes messier |
|  7:00 | `垃圾混乱度`=50% | {自己}发出了最后通牒… | {SELF} SETS AN ULTIMATUM…  garbage becomes messier |
|  7:30 | `垃圾混乱度`=60% | {队友}思索着ta白费的努力。… | {PARTNER} CONTEMPLATES THEIR WASTED EFFORT…  garbage becomes messier |
|  8:00 | +25%受击倍率 | 最后一次痛苦的争吵… | ONE LAST PAINFUL ARGUMENT…  receive 25% more garbage |
| `8:30`| “禁用复活” | 永别了。 | GOODBYE.  you can no longer revive |
| `9:30`| `垃圾混乱度`=20% | “想念你” | "I MISS YOU"  garbage becomes much cleaner |
| 10:00 | `垃圾混乱度`=10% | 如果…？ | WHAT IF…?  garbage becomes a bit cleaner |
| 10:30 | +12行实心垃圾(共16) | … | …  +12 PERMANENT LINES |

> 该玩法中`垃圾混乱度`只根据疲劳流程和时间决定，原有的机制全都不管了
> 获得“禁用复活”buff后，复活任务组将固定变成那一条做不了的X级任务

### 五连 （愚者 The Fool）

> A journey of one thousand six hundred fifty meters starts with a single step.  
> 一千六百五十米的旅程始于第一步。

2025年愚人节活动限定，一周内免费游玩

- 五连块
- ASC-DX旋转系统（思路完全同ASC，但范围从±2扩展到±3）
- Garbo亲手绘制的黑白草稿背景
- 如果也带着【双人】，复活队友时任务时会变成一个“消4*复活难度分”
- 打开所有mod（除了【双人】）时mod组合名称是“WHY”

ASC-DX的踢墙表（顺时针和0←→2的180°，逆时针和1←→3的180°是镜像对称）：
```
35 34 33 __ __ __ __
28 27 26 25 __ __ __
22 20 19 18 31 __ __
_9 _7 _1 _0 16 24 __
10 _8 _3 _2 17 29 36
13 11 _5 _4 21 30 37
15 14 12 _6 23 32 38
```

### 五连+ （愚蠢的徒劳 A Fool's Errand）

> You'll never escape who you are.  
> 你永远无法摆脱你的本性。

同【五连】为2025年愚人节活动，需要带【五连】爬2500m解锁

- 【五连】的基础上改成六连块

## 塔罗牌简要总结

| 正位 | 名称 | 效果简述 | 逆位 | 名称 | 效果简述（包含正位效果） |
| ---: | :--- | :---: | ---: | :--- | :---: |
| The Emperor | 皇帝 | 减小容错+更难爬高 | The Tyrant | 暴君 | 极难爬高+会下坠+超级疲劳 |
| Temperance | 节制 | 无暂存 | Asceticism | 禁欲 | 1预览+随机+2宽垃圾 |
| Wheel of Fortune | 命运之轮 | 垃圾更乱 | Loaded Dice | 铅骰 | 1s消行延迟+图案开局 |
| The Tower | 塔 | 高重力+后期降锁延 | Freefall | 自由落体 | 20G低锁延 |
| Strength | 力量 | 双倍垃圾+双倍抵消 | Last Stand | 顽抗 | 三倍垃圾+正常抵消+矮场地 |
| The Devil | 恶魔 | 双洞垃圾 | Damnation | 天谴 | 废墟垃圾行+挖掘才能攻击+棋盘开局 |
| The Hermit | 隐士 | 隐形+5秒闪烁 | The Exile | 流亡者 | 不再闪烁+垃圾看不全 |
| The Magician | 魔法师 | 不mini+惩罚重复 | The Warlock | 邪术师 | 即死惩罚+非旋无攻击+散垃圾开局 |
| The Lovers | 恋人 | 双人 | Bleeding Hearts | 流血之心 | （活动）优先打给队友的Backfire |

## 一些备注

所有疲劳机制中`+X%受击`会因为【双倍(+)】对应地翻2(3)倍，相当于乘算

如果你能看懂javascript代码：[技术信息补充](/技术信息补充.js)

【广告】体验过后觉得这堆Mod都太难了玩不了？逆位解锁太麻烦不想刷？  
如果你已经对所有mod都有基本的了解，那来看看这个新游戏！隆重介绍……  
[***天穹点点乐 Zenith Clicker***](https://github.com/MrZ626/ZenithClicker)

## 鸣谢

感谢 ThTsOd 提供格式化代码和部分解读

感谢进行测试、指出问题、帮助修改的群友们
