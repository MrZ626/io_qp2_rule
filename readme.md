# Tetr.io规则观察总结 By MrZ_26

## 开始

该大房间不存在公共的“一局”的概念，所有的人随开随打，不需要等待

## 爬塔

此模式的背景题材为攀登 `天顶之塔 (Zenith Tower)`，最终分数称为`高度`（Altitude）

达到一定的高度后会进入下一层，每一层的高度范围如下：

| 层数 | 高度范围 | 名称 | 译名 |
| - | - | - | - |
| 一层 | 0m - 50m | Hall Of Beginnings | 初始大厅 |
| 二层 | 50m - 150m | The Hotel | 酒店 |
| 三层 | 150m - 300m | The Casino | 赌场 |
| 四层 | 300m - 450m | The Arena | 竞技场 |
| 五层 | 450m - 650m | The Museum | 博物馆 |
| 六层 | 650m - 850m | Abandoned Offices | 废弃办公楼 |
| 七层 | 850m - 1100m | The Laboratory | 实验室 |
| 八层 | 1100m - 1350m | The Core | 核心 |
| 九层 | 1350m - 1650m | Corruption | 污染区 |
| 十层 | 1650m+ | Platform Of The Gods | 神之境 |

## 推进器 （本节主要数据来源：ThTsOd）

`推进器等级`（下称`rank`，代码中命名如此）决定了爬塔的速度，所有增加高度的行为都受到这个倍率的加成，升级推进器需要经验（下称xp）

一级时倍率为×0.25，每升一级倍率增加×0.25（达到×2.75时变成白色不再看得出区别，实际无上限）

### 获得高度&经验，与升级

增加高度和xp的途径如下表：

| 动作 | 效果 |
| - | - |
| 随时间每帧增加 | 每秒1m，但在距离下一层6m~1m时以此法速率会从×1均匀变为×0，要用其他方式增加高度到达下一层 |
| 击杀 | 15m （专家+模式 8m） |
| 发送攻击 | 每行 1m 和 1.05xp |
| 抵消垃圾行（非专家模式） | 0.55xp |
| 消行（非专家模式） | 一行1.05xp 两行及以上2.05xp |

> 注意所有的高度增加都受 `rank` 影响，具体来说是倍率为`rank/4`，例如开局时`rank`为1，倍率为×0.25，每4秒增加1m

推进器升级所需xp为 `4*rank`，当xp达到升级所需xp时lv+1并扣除所需xp的值

同时在升级时会获得一个 `xp在5秒内不自然流失` 的效果（详见两节后的xp流失段落），防止升级后只剩一点点xp刚好自然流失导致立刻降级

同时还有一个`升级疲劳`机制：每次升级时，上一行中的`5秒内不自然流失`会减少1秒（直至1秒），反复升降rank时这个效果就会越来越弱。想恢复的话需要在一次升级后紧接着达到50%（经验条中间段的斜坡顶），效果重置回5秒

### 跳级

每次升级时，若xp仍然超过升级所需xp，则额外升 `xp/所需xp` 级，**且不扣除xp**

也就是能出现这种情况：

    某一帧 断B2B发出超大spike获得巨量xp
    下一帧 连跳几级
    下一帧 还能升一次级

### xp流失

推进器xp会随时间流失，每帧减少 `e*(rank^2+rank)/3600` 点

其中e和专家模式有关，具体如下：

| 模式 | e的值 |
| - | - |
| 普通模式 | 3 |
| 专家模式 | 5 |
| DUO | 3+专家人数 |

当xp小于0时候降级，xp变为上一级的满xp

以下列出一些算好的数据方便查阅：

| rank | 升级所需xp | 降级所需秒数 | 每秒流失速率 |
| :--: | :-: | :---: | :--: |
|  1   | 4   | 40.00 |  0.1 |
|  2   | 8   | 26.67 |  0.3 |
|  3   | 12  | 20.00 |  0.6 |
|  4   | 16  | 16.00 |  1.0 |
|  5   | 20  | 13.33 |  1.5 |
|  6   | 24  | 11.43 |  2.1 |
|  7   | 28  | 10.00 |  2.8 |
|  8   | 32  | 8.89  |  3.6 |
|  9   | 36  | 8.00  |  4.5 |
|  10  | 40  | 7.27  |  5.5 |
|  11  | 44  | 6.67  |  6.6 |
|  12  | 48  | 6.15  |  7.8 |
|  13  | 52  | 5.71  |  9.1 |
|  14  | 56  | 5.33  | 10.5 |
|  15  | 60  | 5.00  | 12.0 |
|  16  | 64  | 4.71  | 13.6 |
|  17  | 68  | 4.44  | 15.3 |
|  18  | 72  | 4.21  | 17.1 |
|  19  | 76  | 4.00  | 19.0 |
|  20  | 80  | 3.81  | 21.0 |
|  21  | 84  | 3.64  | 23.1 |
|  22  | 88  | 3.48  | 25.3 |
|  23  | 92  | 3.33  | 27.6 |
|  24  | 96  | 3.20  | 30.0 |
|  25  | 100 | 3.08  | 32.5 |
|  26  | 104 | 2.96  | 35.1 |
| rank | 升级所需xp | 降级所需秒数 | 每秒流失速率 |
| 专家 1  |   4 | 24.00 |  0.167 |
| 专家 2  |   8 | 16.00 |  0.500 |
| 专家 3  |  12 | 12.00 |  1.000 |
| 专家 4  |  16 |  9.60 |  1.667 |
| 专家 5  |  20 |  8.00 |  2.500 |
| 专家 6  |  24 |  6.86 |  3.500 |
| 专家 7  |  28 |  6.00 |  4.667 |
| 专家 8  |  32 |  5.33 |  6.000 |
| 专家 9  |  36 |  4.80 |  7.500 |
| 专家 10 |  40 |  4.36 |  9.167 |
| 专家 11 |  44 |  4.00 | 11.000 |
| 专家 12 |  48 |  3.69 | 13.000 |
| 专家 13 |  52 |  3.43 | 15.167 |
| 专家 14 |  56 |  3.20 | 17.500 |
| 专家 15 |  60 |  3.00 | 20.000 |
| 专家 16 |  64 |  2.82 | 22.667 |
| 专家 17 |  68 |  2.67 | 25.500 |
| 专家 18 |  72 |  2.53 | 28.500 |
| 专家 19 |  76 |  2.40 | 31.667 |
| 专家 20 |  80 |  2.29 | 35.000 |
| 专家 21 |  84 |  2.18 | 38.500 |
| 专家 22 |  88 |  2.09 | 42.167 |
| 专家 23 |  92 |  2.00 | 46.000 |
| 专家 24 |  96 |  1.92 | 50.000 |
| 专家 25 | 100 |  1.85 | 54.167 |
| 专家 26 | 104 |  1.78 | 58.500 |

### 外观

推进器信息显示在场地的下方，此处用文字简述一下外观：

| rank | 倍率 | 颜色 | 图案 | 速通模式备注 |
| :-: | :--: | :-: | - | - |
|  1  | 0.25 | 无 | 一个进度条 | |
|  2  | 0.50 | 红 | 下面加一个三角形 | |
|  3  | 0.75 | 橙 | 三角形长出翅膀 | |
|  4  | 1.00 | 黄绿 | 翅膀变大 | |
|  5  | 1.25 | 蓝 | 翅膀变大加底座 | |
|  6  | 1.50 | 紫 | 翅膀伸出进度条长度 | |
|  7  | 1.75 | 亮橙 | 翅膀更多细节 | 不退出速通模式的最低rank |
|  8  | 2.00 | 青绿 | 进度条上面加一对平行四边形 | 1/2层到此触发 |
|  9  | 2.25 | 青蓝 | 两对平行四边形 | 3/4层到此触发 |
| 10  | 2.50 | 亮紫 | 三对平行四边形 | 5层到此触发 |
| 11  | 2.75 | 白 | 加一对小三角 | |

### 速通模式（Hyperspeed）

当玩家在前五层时rank就达到8/8/9/9/10时就会进入速通模式

会出现花里胡哨的致敬Bejeweled Twist的动画，和专属速通音乐

屏幕顶部出现大字显示当前局时间，底部列出完成前九层花的时间和离个人纪录的分段差异

达到十层时完成速通模式获得一个隐藏成就，或者rank掉到6级时会退出速通模式

## 攻击对象

本模式无法手动指定改变攻击对象，不过玩家的状态会影响自身被攻击的概率

玩家有一个`受击权重`参数，这个值越大越容易被攻击，**初始值为3**

### 随时间增加 （7分钟封顶翻一倍）

当时间达到3/5/7分钟时`受击权重`+1

### 危险时临时减免 （没有净变化）

仅限非专家模式，每0.25秒刷新一次，如果“处于危险中”（详细条件没看懂，当成到顶版面闪红警告吧，不会差很多），`受击权重`会临时减1.5，脱离危险后1.5会加回来

### 转移到受击缓冲 （没有净变化）

有一个`受击缓冲`，被攻击时，行数*0.25的`受击权重`会被转移到缓冲槽内（最多装到3，否则不转移），稍后再变回来

`受击缓冲`装满时（达到3），受击倍率会下降25%（继续被打都会少吃25%垃圾）

`受击缓冲`内的值会（线性）降低垃圾混乱度，装满时`不同段切换概率 -45%`和`同段切换概率 -18%`

如果`受击缓冲`内有值，每隔一定时间（由楼层决定，见下一小节的表）将`受击缓冲`内0.25的值转移回`受击权重`，也就是说层数越高，系统会允许其他人以更高的频率打入垃圾行

### 相关数据表

`受击权重`随时间/被快速打满/处于危险状态中的变化：

| 时间段 | 权重 | 打满时 | 危险时 |
| ----- | :-: | :-: | :-: |
| 0~2分钟 | 3 | -100% | -50% |
| 3~4分钟 | 4 | -75% | -75% |
| 5~6分钟 | 5 | -66% | -60% |
| 7分钟后 | 6 | -50% | -25% |

`受击权重`释放间隔：

| 楼层数 | 释放间隔（秒） | 最大稳定接收APM |
| :---: | :---: | :---: |
|   1   |  4.8  |  12.5  |
|   2   |  3.9  |  15.4  |
|   3   |  2.1  |  28.6  |
|   4   |  1.4  |  42.9  |
|   5   |  1.3  |  46.1  |
|   6   |  0.9  |  66.7  |
|   7   |  0.6  |  100  |
|   8   |  0.4  |  150  |
|   9   |  0.3  |  200  |
|   10  |  0.2  |  300  |

> 注意上表中的`最大稳定接收APM`并不是实战数据，不能拿来和平时说的APM比较，参考价值很小

## 攻击、All-spin、B2B

普通消1、2、3行攻击为0、1、2 （注：非专家模式0连击普通消1会+1攻击，这让散消1有1的攻击效率了，特定情况可以考虑利用）

消4攻击为4，属于`特殊消除`

此模式使用All-Mini规则，所有的块的spin都是`特殊消除`，可以增加B2B计数 （其中只有T块通过三角判定才算普通spin，不满足三角或者其他块使用不可移动判定只会计为Mini，伤害计算同普通消除一致，消123打012）

全消攻击为3，但同时全消计为两次`特殊消除`（和spin独立计算）（和新TL少量不同，新TL中为5攻击但不加也不断B2B）

从第二个`特殊消除`开始会累计B2B计数，同时提供1的额外伤害 （不再像老TL一样慢慢增加）

不过仍然有连续B2B数计算，且有一个然后断B2B时会产生一个超大攻击（B2B数-3，或者说连续特殊消除数-4）

所有伤害保留小数计算，最后小数部分按照数值概率向上取整，例如1.2最后有20%概率变成2，80%概率变成1

## 垃圾行洞位置

TETR.IO中垃圾行的混乱度由两个数字决定：  
“同一段垃圾中每一行有X%的概率在同一列”，“不同段垃圾行有Y%的概率在同一列”

在TL中X=100%，Y=0%，也就是同段垃圾一定在同列，不同段一定不同列  
而在qp2中这两个数就不这么极端了，所以你会感觉qp2中垃圾行洞的位置似乎和几段垃圾行关系不大

`专家`和`混乱垃圾行`Mod都会增加混乱度

`受击缓冲`的值会降低混乱度，详见攻击对象章节

> 注：代码中变量名为 messiness_change 和 messiness_inner

## 垃圾行等待时间

被攻击时垃圾行会在缓冲条中等待一段时间然后进入可触发状态：开始黄色中空，然后红色中空，然后红色实心，此时落块不消行就会上涨

垃圾行经历这两次切换的等待时间只由楼层和专家模式决定，具体值见下表（等差数列）：

| 楼层数 |    非专家     |     专家     |
| :-: | :----------: | :---------: |
|  1  | 2.50秒 (150f) | 1.1秒 (66f) |
|  2  | 2.25秒 (135f) | 1.0秒 (60f) |
|  3  | 2.00秒 (120f) | 0.9秒 (54f) |
|  4  | 1.75秒 (105f) | 0.8秒 (48f) |
|  5  | 1.50秒 (90f)  | 0.7秒 (42f) |
|  6  | 1.25秒 (75f)  | 0.6秒 (36f) |
|  7  | 1.00秒 (60f)  | 0.5秒 (30f) |
|  8  | 0.75秒 (45f)  | 0.4秒 (24f) |
|  9  | 0.50秒 (30f)  | 0.3秒 (18f) |
| 10  | 0.25秒 (15f)  | 0.2秒 (12f) |

> 注1：由于垃圾行要切换两次才进入激活状态，所以实际等待时间是这个表的两倍  
> 注2：代码中变量名为 garbagephase

## 疲劳时间

为了防止一局游戏过长，8分钟时每分钟会多一个负面效果，总共五个：

| 时间 | 负面效果 | 文本 | 原始文本 |
| --- | --- | --- | --- |
| 8分钟  | +2行实心垃圾 | 疲劳开始侵蚀… | FATIGUE SETS IN… +2 PERMANENT LINES |
| 9分钟  | +25%受击 | 你的身体变得虚弱… | YOUR BODY GROWS WEAK… receive 25% more garbage |
| 10分钟 | +3行实心垃圾 | 所有感官混为一团… | ALL SENSES BLUR TOGETHER… +3 PERMANENT LINES |
| 11分钟 | +25%受击 | 你的意识开始消逝… | YOUR CONSCIOUSNESS FADES… receive 25% more garbage |
| 12分钟 | +5行实心垃圾 | 结束了。 | THIS IS THE END. +5 PERMANENT LINES |

## Mod

Mod是在游戏开始前可选的主动增加游戏难度的方式，基本只有坏处没有好处，但是开启Mod（或特定的Mod组合）后爬到特定高度可以获取成就

Mod总共有9个，每个都对应一个特殊效果可以独立开关

Mod的设定是塔罗牌，下文会在每个Mod的名称后写出

### 专家 （皇帝 Emperor）

各方面都变难一些：

1. 垃圾行瞬间出现，而不是一行行上涨
1. 增加垃圾混乱度
1. 移除“处于危险中”减少受击权重的机制

### 无暂存 （节制 Temperance）

禁用暂存

### 混乱垃圾行 （命运之轮 Wheel of Fortune）

垃圾混乱度显著增加

### 高重力 （塔 The Tower）

重力显著增加

### 不稳定垃圾行 （力量 Strength）

从场地底部升起的垃圾行数量翻倍（内部的机制其实是收到攻击数翻倍，且抵消倍率也翻倍）

这个Mod在前期可能可以一定程度加速玩家的进度，但是后期的威胁大于好处，所以基本还是一个负面Mod（除非在打速通之类的项目，需要单独讨论）

### 双洞垃圾行 （恶魔 The Devil）

每一行垃圾行都有可能会有两个洞

### 隐形 （隐士 The Hermit）

玩家放下的方块会隐形，不过每5秒会全场地闪烁一次

### All-Spin （魔法师 The Magician）

（spin判定规则见开头）将非T块的spin从必定mini改为必定不mini，会像T块一样提供消行数*2的基础伤害

但代价是每当玩家消除时（准确说其实是锁定一块时如果场地左上角有冒出文本。不消行spin也会触发），如果提示文本完全一致，那么场地底部会出现一行特殊的实心垃圾行作为惩罚，上面随机位置写了一个倒计时数字为`当前层数+5`，当玩家做一个没有惩罚的消除时-1，归零时变为一行随机位置的单洞普通垃圾行，洞和数字的位置一致

这是唯一一个玩家水平足够后能提供较大正面效果的Mod

### 双人 （恋人 The Lovers）

会员玩家可以邀请其他人和自己两个人一起玩此模式，两个人都死了后游戏结束

两个人发送出去给别人的伤害数值减半

一个人死了后另一个人可以做任务复活队友，任务分为ABCDEF六个等级，内容如下

| 难度等级 | 内部id | 数值 | 内容 | 外显文本 | 标签类型（？） | 有特定Mod时不出现 |
| - | - | - | - | - | - | - |
| F | combo             | 3   | 3连击（四连消） | Perform a 3-Combo | 2 | |
| F | double            | 2   | 2 x 消二 | Clear 2 Doubles | 2 | |
| F | quad              | 1   | 消四 | Clear a Quad | 1 | |
| F | lines             | 6   | 消6行 | Clear 6 Lines | 1 | |
| F | odouble           | 1   | O块消二 | Clear a Double \n using an O-Piece | 3 | |
| F | garbageclear      | 4   | 消4行垃圾 | Clear 4 Garbage Lines | 2 | |
| F | szdouble          | 1   | S/Z消二 | Clear a Double \n using an S or Z-Piece | 3 | |
| F | ljtriple          | 1   | L/J消三 | Clear a Triple \n using an L or J-Piece | 3 | |
| E | tspinmini         | 1   | T旋mini | Perform a T-Spin Mini | 1 | |
| E | tspinsingle       | 1   | T旋消一 | Clear a T-Spin Single | 2 | |
| E | tspindouble       | 1   | T旋消二 | Clear a T-Spin Double | 2 | |
| E | szspin            | 1   | S/Z旋消除 | Clear an S/Z-Spin | 1 | |
| E | ljspin            | 1   | L/J旋消除 | Clear an L/J-Spin | 1 | |
| E | combo             | 5   | 5连击 | Perform a 5-Combo | 2 | |
| E | iflat             | 2   | 2 x 横放I消一 | Clear 2 Lines using \n horizontal I-Pieces | 3 | |
| E | tank              | 4   | 放4行垃圾进场 | Tank 4 Garbage Lines | 2 | |
| E | cancel            | 4   | 抵消4行垃圾 | Cancel 4 Garbage Lines | 2 | |
| D | double            | 4   | 4 x 消二 | Clear 4 Doubles | 2 | |
| D | spam              | 3   | 不转不移 连放3块 | Place 3 pieces in a row \n without moving or rotating | 4 | |
| D | noclear           | 14  | 不消行 连放14块 | Place 14 pieces in a row \n without clearing any lines | 4 | |
| D | send              | 6   | 送出6行攻击 | Send 6 Lines | 1 | |
| D | pieces            | 20  | 放20块 | Place 20 pieces | 2 | |
| D | szdouble          | 2   | 2 x S/Z消二 | Clear 2 Doubles \n using S or Z-Pieces | 3 | |
| D | ljtriple          | 2   | 2 x L/J消三 | Clear 2 Triples \n using L or J-Pieces | 3 | |
| D | ispinclear        | 1   | I旋消除 | Clear an I-Spin | 1 | |
| D | upperhalfquad     | 1   | 在上半场消四（I块最低格至少第10行） | Clear a Quad in the \n upper half of the board | 4 | |
| C | tspintriple       | 1   | T旋消三 | Clear a T-Spin Triple | 2 | |
| C | nohold            | 25  | 不暂存 连放25块 | Place 25 pieces \n without using Hold | 3 | nohold |
| C | triple            | 3   | 3 x 消三 | Clear 3 Triples | 2 | |
| C | b2b               | 4   | 4 x B2B | Reach B2B x4 | 1 | |
| C | quadbuckets       | 2   | 2 x 不同列的消四 | Clear a Quad in \n 2 different columns | 3 | |
| C | holdconsecutive   | 12  | 一直暂存 连放15块 | Use Hold on \n 15 pieces in a row | 3 | nohold |
| C | softdrop          | 10  | 一直按软降 连放10块 | Place 10 pieces without \n releasing Soft Drop | 4 | |
| C | top3rows          | 3   | 最高堆到至少第18行 保持3秒 | Have part of your stack in \n the top 3 rows for 3 seconds | 4 | |
| C | linesnoti         | 10  | 消10行 不用T和I | Clear 10 Lines without \n clearing with T or I-pieces | 4 | |
| C | szspintriple      | 1   | S/Z旋消三 | Clear an S/Z-Spin Triple | 2 | |
| C | odoubleconsecutive| 2   | 2连O块消二 | Clear 2 Doubles consecutively \n using two O-Pieces | 4 | |
| C | tspinminiclear    | 4   | 4 x T旋mini消 | Clear 4 T-Spin Minis | 2 | |
| B | oclear            | 6   | 消6行 用O块 | Clear 6 Lines \n using O-Pieces | 3 | |
| B | spinbuckets       | 3   | 三个不同块旋消除 | Clear Spin-Clears \n with 3 different pieces | 3 | |
| B | quad              | 4   | 4 x 消四 | Clear 4 Quads | 1 | |
| B | spam              | 5   | 不转不移 连放5块 | Place 5 pieces in a row \n without moving or rotating | 4 | |
| B | send              | 18  | 送出18行攻击 | Send 18 Lines | 1 | |
| B | ljspintriple      | 1   | L/J旋消三 | Clear an L/J-Spin Triple | 2 | |
| B | quadconsecutive   | 2   | 2连消四 | Clear 2 Quads in a row | 2 | |
| B | singlesonly       | 8   | 8 x 消一 不消二三四不暂存 | Clear 8 Singles without doing \n other clears or using Hold | 4 | |
| B | nogarbage         | 4   | 场内无垃圾 保持4秒 | Have no Garbage Lines on \n your board for 4 seconds | 4 | |
| B | rotate            | 100 | 转100下 | Rotate 100 times | 2 | |
| B | nocancel          | 8   | 不抵消垃圾 保持8秒 | Don't cancel any \n garbage for 8 seconds | 3 | |
| A | combo             | 7   | 7连击 | Perform a 7-Combo | 2 | |
| A | ispindouble       | 1   | I旋消二 | Clear an I-Spin Double | 2 | |
| A | szspinconsecutive | 2   | 2连S/Z旋消二 | Clear two S/Z-Spin \n Doubles consecutively | 3 | |
| A | ljspinconsecutive | 2   | 2连L/J旋消二 | Clear two L/J-Spin \n Doubles consecutively | 3 | |
| A | colorclear        | 1   | 色彩消除 | Perform a Color Clear | 2 | |
| A | lines             | 40  | 消40行 | Clear 40 Lines | 1 | |

其中F\~A级的任务分别对应1\~6的难度分，复活时任务的总复活难度分=`层数+已复活次数*2`（尽量平分为三个整数），具体表如下：

1. F
1. F F
1. F F F
1. F F E
1. F E E
1. E E E
1. E E D
1. E D D
1. D D D
1. D D C
1. D C C
1. C C C
1. C C B
1. C B B
1. B B B
1. B B A
1. B A A
1. A A A (上限)

## Mod+

每个Mod都有一个强化版本（除了双人），需要带着对应的Mod累计爬3万米解锁（开了多个可以同时累计）

所有的强化Mod都只能单独开启游玩，不能再叠加其他Mod

并且使用强化Mod后，疲劳相关机制也有修改：

| 时间 | 负面效果 | 文本 | 原始文本 |
| --- | --- | --- | --- |
| 6分钟 | 垃圾行更混乱 | 你的权力开始流逝… | YOUR POWER SLIPS… garbage received becomes messier |
| 7分钟 | +25%受击 | 流言开始大肆传播… | WHISPERS OF DISCONTENT SPREAD… receive 25% more garbage |
| 8分钟 | +3行实心垃圾 | 示威人群涌上街头… | PROTESTERS LINE THE STREETS… +3 PERMANENT LINES |
| 9分钟 | +25%受击 | 昔日同盟背叛了你… | YOUR CLOSEST ALLIES DEFECT… receive 25% more garbage |
| 10分钟 | +5行实心垃圾 | 猜疑侵蚀着你的理性… | PARANOIA CLOUDS YOUR JUDGEMENT… +5 PERMANENT LINES |
| 11分钟 | 垃圾行显著混乱 | 革命拉开了帷幕… | THE REVOLUTION HAS BEGUN… garbage received becomes much messier |
| 12分钟 | +12行实心垃圾 | 时代的终结。 | THE END OF AN ERA. +12 PERMANENT LINES |

### 专家+ （暴君 The Tyrant）

在原有的很难爬高的基础上，  
新增下坠加速度机制，更难爬高，打出攻击往上跳一点后如果一会没动就会开始下坠直到落到当前楼层底部（加速度和终端速度和楼层有关）

### 无暂存+ （禁欲 Asceticism）

在原有的无暂存基础上，  
1预览，非7-Bag  
垃圾行变成2宽的样式，稍微好挖一点

### 混乱垃圾行+ （铅骰 Loaded Dice）

在原有的垃圾更混乱基础上，  
消行延迟从0增加到1秒  
初始场地变成固定的六个圈的图案：

    ..........
    .XXX..XXX.
    .X.X..X.X.
    .XXX..XXX.
    ..........
    摞三层，互相和墙都不接触，最后就像骰子的六点/麻将的六筒一样

### 高重力+ （自由落体 Freefall）

从一开局起就20G

### 不稳定垃圾行+ （顽抗 Last Stand）

三倍受击（没有三倍抵消）  
14高场地  
垃圾行孔洞位置有两个next

### 双洞垃圾行+ （天谴 Damnation）

初始场地变成12行棋盘垃圾行  
垃圾行变成每行随机3~4个灰格的混沌垃圾行  
不能抵消垃圾行（不过收到的也很少）

### 隐形+ （放逐 The Exile）

在原有的隐形基础上，  
场地不再闪烁  
垃圾行只能看到最上面的三行  
开局会收系统3行全散垃圾

### All-Spin+ （邪术师 The Warlock）

在原有的连续相同消除惩罚基础上，  
惩罚改为20行实心垃圾行（立刻死亡）  
同时非spin消除全部强制视为single（消二接消四 计为两个single，死）  
开局会收系统10行全散垃圾

## 塔罗牌简要总结

| 正位 | 名称 | 效果简述 | 逆位 | 名称 | 效果简述（包含正位效果） |
| ---: | :--- | :---: | ---: | :--- | :---: |
| Emperor | 皇帝 | 减小容错+更难爬高 | The Tyrant | 暴君 | 极难爬高，会下坠 |
| Temperance | 节制 | 无暂存 | Asceticism | 禁欲 | 1预览+随机+2宽垃圾 |
| Wheel of Fortune | 命运之轮 | 垃圾更乱 | Loaded Dice | 铅骰 | 1s消行延迟+图案开局 |
| The Tower | 塔 | 高重力 | Freefall | 自由落体 | 20G |
| Strength | 力量 | 双倍垃圾+双倍抵消 | Last Stand | 顽抗 | 三倍垃圾+正常抵消+矮场地 |
| The Devil | 恶魔 | 双洞垃圾 | Damnation | 天谴 | 废墟垃圾行+棋盘开局 |
| The Hermit | 隐士 | 隐形+5秒闪烁 | The Exile | 放逐 | 不再闪烁+垃圾看不全 |
| The Magician | 魔法师 | 不mini+惩罚重复 | The Warlock | 邪术师 | 即死惩罚+非旋算消一+散垃圾开局 |
| The Lovers | 恋人 | 双人 | | | |

## 一些随手贴着不知道会不会有用的技术信息补充

`受击权重`:targetingfactor
`受击缓冲`:targetinggrace

```js
    // 一些常数表
    FloorDistance - [0, 50, 150, 300, 450, 650, 850, 1100, 1350, 1650, 1 / 0];
    TargetingGrace - [0, 4.8, 3.9, 2.1, 1.4, 1.3, .9, .6, .4, .3, .2];
    GravityBumps = [0, .48, .3, .3, .3, .3, .3, .3, .3, .3, .3];
    LockTimes = [0, 30, 29, 28, 27, 26, 24, 22, 20, 18, 16];
    SpeedrunReq = [7, 8, 8, 9, 9, 10, 0, 0, 0, 0, 0];
    GetSpeedCap(e) {
        const t = this.FloorDistance.find((t => e < t)) - e;
        return Math.max(0, Math.min(1, t / 5 - .2))
    }

    // 释放受击缓冲
    const cooldownDuration = 60 * se.TargetingGrace[l];
    if (this.S.stats.zenith.targetinggrace > 0 && e >= this.S.lastatktime + cooldownDuration) {
        if (this.S.stats.zenith.targetinggrace >= 3) {
            this.S.setoptions.receivemultiplier += this.S.setoptions.zenith_volatile ? 0.5 : 0.25;
        }
        this.S.stats.zenith.targetingfactor += 0.25;
        this.S.stats.zenith.targetinggrace -= 0.25;
        this.S.lastatktime = e;
    }

    const expertBonus = this.S.setoptions.zenith_expert ? 0.05 : 0.03;
    const messyBonus = this.S.setoptions.zenith_messy ? 0.25 : 0;
    const messinessIncrease = 2.5 * (expertBonus + messyBonus);
    this.S.setoptions.messiness_change = messinessIncrease;
    this.S.setoptions.messiness_inner = expertBonus + messyBonus;
    this.S.setoptions.garbagefavor = (this.S.setoptions.zenith_expert ? 0 : 33) - 3 * l - (this.S.setoptions.zenith_messy ? 25 : 0);
    this.S.setoptions.garbagephase = this.S.setoptions.zenith_expert ? 66 - 6 * l : 165 - 15 * l;
```
