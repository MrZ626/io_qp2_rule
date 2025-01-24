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

| 动作 | 高度 | 经验 | 特殊 |
| :--: | :--: | :--: | :--: |
| 自然增加 | 每秒 1m | `自然流失`，见后文 | 会`卡层`，距下一层6m~1m时逐渐变为0 |
| 击杀 | 15m | | 【专家+】时 8m |
| 发送 | `行数`m | `行数+0.05`xp | |
| 抵消 | | `行数/2+0.05`xp | 【专家(+)】或【双倍+】时视为0行，仅 0.05xp |
| 消行 | | `min(行数,2)+0.05`xp | 【专家(+)】时***不触发*** |
| `卡层`时触发前三条 | 3m | | 此项不受`rank`影响，实际判定是距下一层2m内时 |

> 上述高度增加除了卡层的+3m外都受 `rank` 影响，具体来说是倍率为`rank/4`，例如开局时`rank`为1，倍率为×0.25，每4秒增加1m

获得高度时，新增的高度会先存入一个临时变量，每帧释放10%，最大10m

推进器升级所需xp为 `4*rank`，当xp达到升级所需xp时lv+1并扣除所需xp的值
同时在升级时会获得一个 `xp在5秒内不自然流失` 的效果（详见两节后的xp流失段落），防止升级后只剩一点点xp刚好自然流失导致立刻降级

另外还有一个`升级疲劳`机制：每次升级时，上一行中的`5秒内不自然流失`会减少1秒（直至1秒），反复升降rank时这个效果就会越来越弱。想恢复的话需要在一次升级后紧接着达到50%（经验条中间段的斜坡顶），效果重置回5秒

### 跳级

每次升级的同时，若还有大量剩余xp（发生在爆了大几十B2B产生巨量攻击时）仍然超过升级所需xp，则额外升 `xp/所需xp` 级，**且不扣除这些多余的xp**

也就是能出现这种情况：

    某一帧 断B2B发出超大spike获得巨量xp
    下一帧 升级+跳级
    下一帧 还能升一次级

### xp流失

推进器xp会随时间流失，每帧减少 `mul*(rank^2+rank)/3600` 点  
其中mul和【专家(+)】有关，没开=3，开了=5，双人时=3+专家人数

当xp小于0时候降级，xp变为上一级的满xp

以下列出一些算好的数据方便查阅：

| `rank` | 升级所需xp | 降级秒数 | 每秒流失 | 【专家(+)】降级秒数 | 【专家(+)】流失速率 |
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

| `rank` | 倍率 | 颜色 | 图案 | 速通模式备注 |
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

本模式无法手动指定改变攻击对象，不过玩家的状态会影响自身被攻击的概率：`受击权重`
这个值越大越容易被其他人攻击，**初始值为3**

另外据观察，大多数情况锁定的都是高度相似的人，具体计算方式不明

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

## 受击保护

有一个`受击保护`变量，受到攻击时，增加行数等量的值（【双倍+】的三倍不计），直到**18**封顶

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

## 攻击、全旋、B2B

普通消1、2、3行攻击为0、1、2

`简单模式`限定：0连击普通消一+1攻击，这让散消一也有1的攻击效率了，特定情况可以考虑利用

消4攻击为4，属于`特殊消除`（增加B2B计数，用于`蓄力攻击`，详见后一节）

此模式使用All-Mini规则，是`全旋`（All-Spin）的一个亚种，除了T外其他所有块的Spin也都计`特殊消除`
其中只有T块通过三角判定才算普通Spin，拥有`2*消行数`的基础伤害，不满足三角或者非T块满足不可移动判定都计为Mini，基础伤害和普通消除一致，为`消行数-1`

全消攻击为3，但同时全消计为两次`特殊消除`（和Spin独立计算）（TL中略有不同，5攻击但不加也不断B2B）

连续`特殊消除`从第二个开始会累计B2B计数，同时+1伤害

所有伤害保留小数计算，最后小数部分按照数值概率向上取整，例如1.2最后有20%概率变成2，80%概率变成1（此规则不确定是否还在使用）

### 蓄力攻击（Surge Attack）

断B2B时会产生一个超大攻击，伤害为`B2B数-3`（或者说连续特殊消除数-4）（TL中略有不同，几B2B就打几，不-3）

> 这是qp中一个很重要的机制，特定情况下很需要使用

### 多段攻击（Windup Attack）

如果收到了单块发送的8点及以上的攻击（不包括Surge Attack），这个攻击会被拆分成多段接收并带有预警提示

> 这个触发条件很可能不准确，需要研究和补充

拆分方式：把攻击拆成若干个4和剩余部分（【双倍】时改为8）

预警方式：收到此种攻击时提前1s开始播放!和!!等动画警告并带有提示音，提示音的数量对应本轮攻击总共被拆成了几节（例如响4声对应4+4+4+n，也就是13~16的攻击）
1s后攻击开始进入缓冲条，每一段间隔0.5s

## 垃圾混乱度

TETR.IO中垃圾行的混乱度由两个数字决定：
“同一段垃圾中每一行有X%的概率不在同一列”，“不同段垃圾行有Y%的概率不在同一列”

在TL中X=0%，Y=100%，也就是同段垃圾一定在同列，不同段一定不同列
而在qp2中这两个数就不这么极端了，所以你会感觉qp2中垃圾行洞的位置似乎和几段垃圾行关系不大

在qp2中，默认Y=2.5*X，也就是打入垃圾行的间断点处有更大的概率（2.5倍）不在同一列

本文档中称这个X为`垃圾混乱度`，会受到各种因素影响，具体的变动如下表：

| 因素 | 影响值 |
| :-: | :-: |
| 层数 | `层数*3%`，【专家(+)】时`层数*5%` |
| 【混乱(+)】 | +25% (100%) |
| 【全旋+】 | +30% |
| 【专家+】疲劳6分钟的`全散`效果 | =100%（前面的计算可超100%，此效果覆盖） |
| `受击保护`（最终生成时计算） | 每1点`受击保护`让Y减少3.75%，X减少1.5% |

> 受击保护满18点时，Y减少67.5%，X减少27%  
> 真的没写反，这里挺奇怪的，可能需要拉个表格观察一下各种因素导致的混乱度变化  
> 吃了`全散`效果后两个值都被设为100%时反而在间断处更容易在同一列，很神奇

## 垃圾行等待时间

被攻击时垃圾行会在缓冲条中等待一段时间然后进入可触发状态：开始黄色中空，然后红色中空，然后红色实心，此时落块不消行就会上涨
垃圾行经历这两次切换的等待时间只由楼层和专家模式决定，具体值见下表（等差数列）：

| 层数 |  普通（非专家）   |  【专家(+)】   | 【混乱+】【双倍+】【双洞+】 | 其他【Mod+】 |
| :-: | :----------: | :---------: | :----: | :-----: |
|  1  | 2.50秒 (150f) | 1.1秒 (66f) | 同普通6层 | 同普通6层 |
|  2  | 2.25秒 (135f) | 1.0秒 (60f) | 同普通6层 | 同普通6层 |
|  3  | 2.00秒 (120f) | 0.9秒 (54f) | 同普通6层 | 同普通6层 |
|  4  | 1.75秒 (105f) | 0.8秒 (48f) | 同普通6层 | 同普通6层 |
|  5  | 1.50秒 (90f)  | 0.7秒 (42f) | 同普通6层 | 同普通6层 |
|  6  | 1.25秒 (75f)  | 0.6秒 (36f) | 同普通6层 | 同普通6层 |
|  7  | 1.00秒 (60f)  | 0.5秒 (30f) | 同普通6层 | 同普通`7`层 |
|  8  | 0.75秒 (45f)  | 0.4秒 (24f) | 同普通6层 | 同普通`8`层 |
|  9  | 0.50秒 (30f)  | 0.3秒 (18f) | 同普通6层 | 同普通`9`层 |
| 10  | 0.25秒 (15f)  | 0.2秒 (12f) | 同普通6层 | 同普通`10`层 |

> 由于垃圾行要切换两次才进入激活状态，所以实际等待时间是这个表的两倍

## 疲劳时间

为了防止一局游戏过长，8分钟时每分钟会多一个负面效果，总共五个：

| 时间 | 负面效果 | 文本 | 原始文本 |
| --- | --- | --- | --- |
| 8分钟  | +2行实心垃圾 | 疲劳开始侵蚀… | FATIGUE SETS IN… +2 PERMANENT LINES |
| 9分钟  | +25%受击 | 你的身体变得虚弱… | YOUR BODY GROWS WEAK… receive 25% more garbage |
| 10分钟 | +3行实心垃圾(共5) | 所有感官混为一团… | ALL SENSES BLUR TOGETHER… +3 PERMANENT LINES |
| 11分钟 | +25%受击 | 你的意识开始消逝… | YOUR CONSCIOUSNESS FADES… receive 25% more garbage |
| 12分钟 | +5行实心垃圾(共10) | 结束了。 | THIS IS THE END. +5 PERMANENT LINES |

> 【专家+】时疲劳效果不同，详见后文

## Mod

Mod是在游戏开始前可选的主动增加游戏难度的方式，基本只有坏处没有好处，但是开启Mod（或特定的Mod组合）后爬到特定高度可以获取成就

Mod总共有9个，每个都对应一个特殊效果可以独立开关，在背景设定中呈现为塔罗牌，下文会在每个Mod的名称后注明，章节最后也有一个表格用于速查

本文档中定义没开专家Mod或任意Mod+时为`简单模式`

本文档中所有的Mod以【XX】格式标注

### 专家 （皇帝 The Emperor）

各方面都变难一些：

1. `垃圾混乱度`增加
1. 移除垃圾上涨动画，瞬间出现（和TL一样）
1. 移除“处于危险中”减少`受击权重`的机制

### 无暂存 （节制 Temperance）

禁用暂存

### 混乱 （命运之轮 Wheel of Fortune）

`垃圾混乱度`增加

### 重力 （塔 The Tower）

重力显著增加
十层的锁定延迟表（单位 帧）： 30, 29, 28, 27, 26, 24, 22, 20, 18, 16

### 双倍 （力量 Strength）

受击倍率和抵消倍率都变为2倍

> 可以大致认为是从场地底部升起的垃圾行数量翻倍
> 由于只是让垃圾行的数量直接增加，所以变相地降低了垃圾混乱度，所以此Mod可以加快前期爬楼进度，不过后期威胁太大所以好不好得看具体在打什么项目

### 双洞 （恶魔 The Devil）

每行垃圾行可能会有两个洞

### 隐形 （隐士 The Hermit）

放置方块会隐形
每5秒会全场闪烁一次方便挖开

### 全旋 （魔法师 The Magician）

（Spin判定规则见开头）将非T块的Spin从必定Mini改为必定不Mini，会像T块一样提供消行数*2的基础伤害

但代价是每当玩家消除时（准确说其实是锁定一块时如果场地左上角有冒出文本。不消行Spin也会触发），如果提示文本完全一致，那么场地底部会出现一行特殊的实心垃圾行作为惩罚，上面随机位置写了一个倒计时数字为`当前层数+5`，当玩家做一个没有惩罚的消除时-1，归零时变为一行随机位置的单洞普通垃圾行，洞和数字的位置一致

> 唯一一个玩家水平足够后能提供较大正面效果的Mod

### 双人 （恋人 The Lovers）

会员玩家可以邀请其他人和自己两个人一起玩此模式
对于其中一个人来说，大多数对外的数值都会被砍半，例如发送攻击和累计推进器经验等

两个人都死了后游戏结束，但一个人死后另一个人可以做任务复活队友，任务分为ABCDEF六个等级，内容如下：

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

每个Mod都有对应的强化版本（除【双人】），需要带着对应的Mod累计爬3万米解锁（开了多个可以同时累计）

    如果你想尽快解锁所有Mod+的话建议同时开几个，以下给出一个参考方案（假设你可以畅玩所有的单Mod且能拿到几个f10）：
    `【重力】【混乱】【双洞】【全旋】`四个一起开然后靠allpin猛猛输出，尽量不要让垃圾行进来，刷满3万米。如果你能用循环定式无脑玩Blitz模式不怕`【隐形】`就可以带，否则之后要单独打
    然后打`【隐形】【双倍】`和`【无暂存】【双倍】`，注意尽量多吃垃圾然后用消四挖开会爬得更快（因为抵消垃圾行会亏），同时尝试带和不带`【专家】`计算一下总效率（高度/耗时，带专家就乘2）决定要不要顺便刷专家，能带尽量带，否则之后要单独打
    最后是`【专家】【双倍】`，单独打是因为Mod会大幅降低爬的效率，放最后是因为前面可能顺便打了

所有的Mod+都只能单独开启游玩，不能再叠加其他Mod

之后会出现的效果说明：

1. `开局保护`：仅会在开局有垃圾行的Mod中出现，的清理掉场地内最后5行垃圾行时，每一行会让`受击权重`增加0.5，总共2.5（具有此条目的Mod估计是会改低这个初始值，然后通过这种方式逐渐恢复正常）

### 专家+ （暴君 The Tyrant）

> Fear, oppression, and limitless ambition.
> 恐惧、压迫和无限的野心。

在原有的种种限制上，

推进器不再随着时间增加高度，必须通过输出和击杀才能获得高度

下坠：高度会随时间下降，不过会被每层的地板拦住不降层：

| 层数 | 下坠速度 | 对应rank和spm |
| :-: | :---: | :---------: |
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
> rank和spm仅供大致参考，实际游玩中抵消不计入spm所以基本做不到，五六层后基本必须靠三位数的蓄力攻击才能继续往上爬
> 游玩时下落看起来有加速度，只是视觉效果，实际还是匀速的

疲劳机制变得更严苛：

| 时间 | 负面效果 | 文本 | 原始文本 |
| --- | --- | --- | --- |
| 6分钟 | 垃圾行更混乱(禁用`受击保护`效果) | 你的权力开始流逝… | YOUR POWER SLIPS… garbage received becomes messier |
| 7分钟 | +25%受击 | 流言开始大肆传播… | WHISPERS OF DISCONTENT SPREAD… receive 25% more garbage |
| 8分钟 | +3行实心垃圾 | 示威人群涌上街头… | PROTESTERS LINE THE STREETS… +3 PERMANENT LINES |
| 9分钟 | +25%受击 | 昔日同盟背叛了你… | YOUR CLOSEST ALLIES DEFECT… receive 25% more garbage |
| 10分钟 | +5行实心垃圾(共8) | 偏执遮蔽了你的理性… | PARANOIA CLOUDS YOUR JUDGEMENT… +5 PERMANENT LINES |
| 11分钟 | 垃圾行彻底混乱(获得`全散`效果) | 革命拉开了帷幕… | THE REVOLUTION HAS BEGUN… garbage received becomes much messier |
| 12分钟 | +12行实心垃圾(共20) | 时代的终结。 | THE END OF AN ERA. +12 PERMANENT LINES |

在每一层停留的时间超过60秒时，每秒永久增加0.5%受击倍率（例如累计生效200秒后收到攻击全都双倍）

`受击保护`释放更快（详见`受击保护`相关章节）

击杀基础奖励从15m减少为8m

### 无暂存+ （禁欲 Asceticism）

> A detachment from even that which is moderate.
> 对哪怕是中庸之物的疏离。

在原有的无暂存基础上，
1预览
没有影子
纯随机序列（目测，应该是）
Spin全都计为Mini（基础攻击为`消行数-1`）
垃圾行都变成2宽的样式

### 混乱+ （铅骰 Loaded Dice）

> In a rigged game, your mind is the only fair advantage.
> 在被操纵的游戏中，你的头脑是唯一合理的优势。

`垃圾混乱度`增加
消行延迟从0增加到1秒
初始场地变成固定的六个圈的图案：

    ..........
    .XXX..XXX.
    .X.X..X.X.
    .XXX..XXX.
    ..........
    摞三层，互相和墙都不接触，最后就像骰子的六点/麻将的六筒一样
有`开局保护`

### 重力+ （自由落体 Freefall）

> The ground you stood on never existed in the first place.
> 你所站立的地面从始至终都不存在。

从一开局起就20G
十层的锁定延迟表： 24, 22, 20, 18, 16, 15, 14, 13, 12, 11

### 双倍+ （顽抗 Last Stand）

> Strength isn't necessary for those with nothing to lose.
> 对那些一无所有的人而言，力量并非必需。

受击倍率变为3倍（注意：抵消倍率不变）
14高场地
垃圾行孔洞位置有两个预告
锁定garbagefavor的值为50（具体算法不明，应该是因此变得特别整齐）

### 双洞+ （天谴 Damnation）

> No more second chances.
> 这是你最后的机会。

初始场地变成12行棋盘垃圾行
垃圾行变成每行随机3~4个灰格的混沌垃圾行
不能抵消垃圾行（不过收到的也很少）
有`开局保护`

### 隐形+ （放逐 The Exile）

> Never underestimate blind faith.
> 永远不要低估盲目的信仰。

在原有的隐形基础上，
场地不再闪烁
垃圾行只能看到最上面的三行
开局会收系统3行垃圾

### 全旋+ （邪术师 The Warlock）

> Into realms beyond heaven and earth.
> 踏入超脱天地之境界。

在原有的连续相同消除惩罚基础上，
惩罚改为20行实心垃圾行（立刻死亡）
同时非Spin消除全部强制视为Single（消二接消四 计为两个Single，死）
开局会收系统10行垃圾
`垃圾混乱度`增加
有`开局保护`

## 塔罗牌简要总结

| 正位 | 名称 | 效果简述 | 逆位 | 名称 | 效果简述（包含正位效果） |
| ---: | :--- | :---: | ---: | :--- | :---: |
| The Emperor | 皇帝 | 减小容错+更难爬高 | The Tyrant | 暴君 | 极难爬高+会下坠+超级疲劳 |
| Temperance | 节制 | 无暂存 | Asceticism | 禁欲 | 1预览+随机+2宽垃圾 |
| Wheel of Fortune | 命运之轮 | 垃圾更乱 | Loaded Dice | 铅骰 | 1s消行延迟+图案开局 |
| The Tower | 塔 | 高重力+后期降锁延 | Freefall | 自由落体 | 20G低锁延 |
| Strength | 力量 | 双倍垃圾+双倍抵消 | Last Stand | 顽抗 | 三倍垃圾+正常抵消+矮场地 |
| The Devil | 恶魔 | 双洞垃圾 | Damnation | 天谴 | 废墟垃圾行+棋盘开局 |
| The Hermit | 隐士 | 隐形+5秒闪烁 | The Exile | 放逐 | 不再闪烁+垃圾看不全 |
| The Magician | 魔法师 | 不mini+惩罚重复 | The Warlock | 邪术师 | 即死惩罚+非旋算消一+散垃圾开局 |
| The Lovers | 恋人 | 双人 | | | |

## 一些备注

所有疲劳机制中`+X%受击`会因为【双倍(+)】对应地翻2(3)倍，也就是相当于乘算

## 一些随手贴着不知道会不会有用的技术信息补充

如果你想搜索代码，以下是部分变量名：

1. `受击权重` targetingfactor
1. `受击保护` targetinggrace
1. `各种Mod` zenith_[mod名]
1. `逆位Mod` [mod名]_reverse
1. `间断洞位改变概率` messiness_change
1. `连续洞位改变概率` messiness_inner
1. `垃圾行的？？？` garbagefavor
1. `垃圾行等待时间` garbagephase

```js
    /*
        代码来自2025.01.19的版本，估计是ts编译后的js所以有一定混淆，以下代码为修改过的参考用代码
        删去了一些无关紧要（嗯嗯嗯 欸嘿）的内容，添加了各种注释
        “MOD_”是为了方便阅读的简写，在原代码为“this.S.setoptions.zenith_”，
        或者“"xxxxxx_reversed" === this.S.setoptions.zenith_mods[0]”
    */

    // 一些常数表
    FloorDistance = [0, 50, 150, 300, 450, 650, 850, 1100, 1350, 1650, 1 / 0];
    GravityBumps = [0, .48, .3, .3, .3, .3, .3, .3, .3, .3, .3];
    GravLockDelay = [0, 30, 29, 28, 27, 26, 24, 22, 20, 18, 16];
    GravRevLockDelay = [0, 24, 22, 20, 18, 16, 15, 14, 13, 12, 11];
    SpeedrunReq = [7, 8, 8, 9, 9, 10, 0, 0, 0, 0, 0]; // 第一个元素存的是不掉出去的等级
    TargetingGrace = [0, 4.8, 3.9, 2.1, 1.4, 1.3, .9, .6, .4, .3, .2];
    TargetingGraceRevEx = [0, 1, .9, .8, .7, .6, .5, .4, .3, .2, .1];
    RevNoHoldHoleSideChangeChance = [.1, .1, .15, .2, .25, .3, .35, .4, .45, .5, .55];
    GetSpeedCap(frame) {
        const t = this.FloorDistance.find((t => frame < t)) - frame;
        return Math.max(0, Math.min(1, t / 5 - .2))
    }

    // 主循环
    Loop() {
        const frame = this.self.esm.frame;
        let rank = Math.floor(this.S.stats.zenith.rank);
        const height0 = this.S.stats.zenith.altitude; // 记录用于卡楼层的高度

        // 经验流失
        if (frame >= this.S.zenith.rank_locked_until) {
            let leakSpeed = ...; // 单人:普通3专家5  双人:3+专家人数
            this.S.zenith.climb_pts -= leakSpeed * (rank ** 2 + rank) / 3600 // climb_pts是当前xp
        }

        const nextRankXP = 4 * rank;
        const storedXP = 4 * (rank - 1);
        if (this.S.zenith.climb_pts < 0)
            // 降级
            if (rank <= 1)
                // 不会掉到0级以下
                this.S.zenith.climb_pts = 0;
            else {
                // 恢复计算总xp
                this.S.zenith.climb_pts += storedXP;
                this.S.zenith.last_rank_change_was_promote = false;
                rank--;
            }
        else if (this.S.zenith.climb_pts >= nextRankXP) 
            // 清空xp升1级
            this.S.zenith.climb_pts -= nextRankXP; 
            this.S.zenith.last_rank_change_was_promote = true;
            this.S.zenith.rank_locked_until = frame + Math.max(60, 60 * (5 - this.S.zenith.promotion_fatigue));
            this.S.zenith.promotion_fatigue++;
            rank++;
        }

        // xp在5秒内不自然流失效果
        if (this.S.zenith.last_rank_change_was_promote && this.S.zenith.climb_pts >= 2 * (rank - 1))
            this.S.zenith.promotion_fatigue = 0;

        // 跳级！如果升级后还有大量剩余的话
        this.S.stats.zenith.rank = rank + this.S.zenith.climb_pts / (4 * rank);

        // 一些统计
        this.S.stats.zenith.peakrank = Math.max(this.S.stats.zenith.rank, this.S.stats.zenith.peakrank);
        this.S.stats.zenith.avgrankpts += this.S.stats.zenith.rank;

        const o = this.S.stats.zenith.altitude;
        const floor = me.GetFloorLevel(o);

        if (MOD_expertRev) {
            // 【专家+】的下坠
            this.S.stats.zenith.altitude = Math.max(me.FloorDistance[floor - 1], o - .05 * (floor ** 2 + floor + 10) / 60);
        } else {
            // 推进器随时间爬升
            this.S.stats.zenith.altitude += .25 * rank / 60 * me.GetSpeedCap(o);
        }

        // 平滑的高度变化
        if (this.S.zenith.bonusremaining > 0)
            if (this.S.zenith.bonusremaining <= .05) {
                this.S.stats.zenith.altitude += this.S.zenith.bonusremaining;
                this.S.zenith.bonusremaining = 0;
            }
            else {
                const delta = Math.min(10, .1 * this.S.zenith.bonusremaining);
                this.S.stats.zenith.altitude += delta;
                this.S.zenith.bonusremaining -= delta;
            }

        // 不让用“推进器随时间爬升”途径上楼
        this.S.setoptions.zenith_tutorial && this.S.stats.zenith.altitude >= 50 &&
        this.S.zenith.tutorial.stage > 0 && this.S.zenith.tutorial.stage < 5 && (
            this.S.stats.zenith.altitude = Math.min(49.99, height0);
            this.S.zenith.bonusremaining = 0;
        )

        // 【重力(+)】
        floor !== this.S.stats.zenith.floor && (
            MOD_gravity ? (this.S.g += me.GravityBumps[floor], this.S.setoptions.locktime = me.GLockDelay[floor]) : MOD_gravityRev && (this.S.g = 20, this.S.setoptions.locktime = me.GRLockDelay[floor]), this.S.zenith.lastfloorchange = frame, 1 === floor ? this.S.glock = 240 : this.S.stats.zenith.splits[floor - 2] = Math.round(this.self.lm.GetGameTime())
        )
        this.S.stats.zenith.floor = floor

        // 【专家+】的超时惩罚
        if (MOD_expertRev && frame - this.S.zenith.lastfloorchange > 3600)
            this.S.setoptions.receivemultiplier += .005 / 60;

        // 不知道啥玩意
        if (this.S.TEMP_zenith_apm_cycle) {
            if (this.S.TEMP_zenith_apm_cycle += this.S.TEMP_zenith_apm / 3600 / 2.5 * (.75 + .5 * this.S.rngex.nextFloat()), this.S.TEMP_zenith_apm_cycle >= 1) {
                this.S.TEMP_zenith_apm_cycle--;
                this.self.atm.FightLines(this.S.rngex.nextFloat() >= .5 ? 4 : 1);
            }
        }

        // 某些机制的即死，估计是给【全旋+】用的
        if (this.self.atm.GetPendingGarbageCount() >= this.S.TEMP_zenith_instakill_at)
            this.self.gom.GameOver("garbagesmash");

        // 受击权重在3/5/7分钟增加
        if (frame===10800 || frame===18000 || frame===25200)
            this.S.stats.zenith.targetingfactor++;

        // 释放受击保护
        let r = 60 * (MOD_expertRev ? TargetingGrace : TargetingGraceRevEx)[floor];
        if (this.S.stats.zenith.targetinggrace > 0 && frame >= this.S.lastatktime + r) {
            this.S.stats.zenith.targetinggrace--;
            this.S.lastatktime = frame;
        }

        // 刷新混乱度
        const messy = (MOD_expert ? .05 : .03) * floor;
        if (MOD_messy) messy += .25;
        if (MOD_messyRev) messy += 1;
        if (MOD_allspinRev) messy += .3;
        this.S.setoptions.messiness_inner = messy;
        this.S.setoptions.messiness_change = 2.5 * messy;
        if (this.S.zenith.maxmessy) {
            this.S.setoptions.messiness_change = 1;
            this.S.setoptions.messiness_inner = 1;
        }

        // 垃圾行favor？？？暂时不知道具体怎么用的
        this.S.setoptions.garbagefavor = MOD_volatileRev ? 50 : (MOD_expert ? 0 : 33) - 3 * floor - (MOD_messy ? 25 : 0);

        // 垃圾行等待时间
        this.S.setoptions.garbagephase = MOD_expert ? 66 - 6 * floor : 165 - 15 * floor;
        if (MOD_anyRev && !MOD_expert)
            this.S.setoptions.garbagephase = (MOD_messyRev || MOD_volatileRev || MOD_doubleholeRev) ? 75 : [75, 75, 75, 75, 75, 75, 75, 60, 45, 30, 15][floor];

        // 随着消除垃圾行逐渐关闭开局保护
        if (frame % 15 == 0 && (MOD_messyRev || MOD_doubleholeRev || MOD_allspinRev)) {
            const line = this.self.bm.CountGarbageLinesNoPerma();
            if (line !== this.S.zenith.garbagerowcount) {
                const t = Math.max(0, 2.5 - .5 * this.S.zenith.garbagerowcount);
                const n = Math.max(0, 2.5 - .5 * line);
                this.S.stats.zenith.targetingfactor += n - t;
                this.S.zenith.garbagerowcount = line;
            }
        }
    }

    // 一些事件
    AwardKill() { // 击杀
        this.GiveBonus(.25 * Math.floor(this.S.stats.zenith.rank) * (MOD_expertRev ? 8 : 15))
    }
    AwardLines(amount, giveHeight = true, giveXP = true) { // 非专家消行(false,true) / 抵消(false,true) / 发送(true,true)
        // 加高度（受mod等影响，见推进器章节表格）
        let dh = .25 * Math.floor(this.S.stats.zenith.rank) * amount * (giveHeight ? 1 : 0);

        // 卡层时+3m
        const heightToNextFloor = me.FloorDistance.find((e => this.S.stats.zenith.altitude < e)) - this.S.stats.zenith.altitude - dh - this.S.zenith.bonusremaining;
        if (heightToNextFloor >= 0 && heightToNextFloor <= 2) dh += 3;

        this.GiveBonus(dh);

        // 消行获取经验
        this.GiveClimbPts((amount + .05) * (giveXP ? 1 : 0));
    }
    AwardHasBeenAttacked(amount) { // 被攻击
        const spaceRemain = Math.min(18 - this.S.stats.zenith.targetinggrace, amount);
        if (spaceRemain > 0) this.S.stats.zenith.targetinggrace += spaceRemain;
    }
    GiveBonus(amount) { // 获取高度（各种途径）
        if (this.S.setoptions.zenith_tutorial && this.S.zenith.tutorial.stage > 0 && this.S.zenith.tutorial.stage < 5)
            amount *= me.GetSpeedCap(this.S.stats.zenith.altitude);
        this.S.zenith.bonusremaining += amount;
        if (this._bonusExpires < this.self.esm.frame) this._bonusCount = 0;
        this._bonusCount += amount;
        this._bonusExpires = this.self.esm.frame + 60;
        this.S.zenith.bonusfromally += amount;
    }
```
