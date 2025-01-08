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

`推进器等级`（下称`rank`，代码中命名如此）决定了爬塔的速度，所有增加高度的行为都受到这个倍率的加成，升级推进器需要经验（下称xp）。

一级时倍率为×0.25，每升一级倍率增加×0.25（达到×2.75时变成白色不再看得出区别，实际无上限）

### 获得高度&经验，与升级

增加高度和xp的途径如下表：

| 动作 | 效果 |
| - | - |
| 随时间每帧增加 | 每秒1m，但在距离下一层6m~1m时以此法速率会从×1均匀变为×0，要用其他方式增加高度到达下一层 |
| 击杀 | 15m |
| 发送攻击 | 每行 1m 和 1.05xp |
| 抵消垃圾行 | （非专家模式）0.55xp |
| 消行 | （非专家模式）一行1.05xp 两行及以上2.05xp |

> 注意所有的高度增加都受 `rank` 影响，都要 `*rank/4`，例如开局时倍率为×0.25，每4秒增加1m

推进器升级所需xp为 `4*rank`，当xp达到所需xp时lv+1并扣除所需xp的值。

同时在升级时会获得一个 `xp在5秒内不自然流失` 的效果（详见两节后的xp流失段落），防止升级后只剩一点点xp刚好自然流失导致立刻降级。

同时还有一个`升级疲劳`机制：每次升级时，上一行中的`5秒内不自然流失`会减少1秒（至少留1秒），反复降级升级时这个效果就会越来越弱。想恢复的话需要在一次升级后紧接着达到50%（经验条中间段的斜坡顶），效果重置回5秒。

### 跳级

每次升级时，若xp仍然超过升级所需xp，则额外升 `xp/所需xp` 级，**且不扣除xp**

也就是能出现这种情况：

    某一帧 断B2B发出超大spike获得巨量xp
    下一帧 连跳几级
    下一帧 还能升一次级

### xp流失

推进器xp会随时间流失，每帧减少 `e*(rank^2+rank)/3600` 点。

其中e和专家模式有关，具体如下：

| 模式 | e的值 |
| - | - |
| 普通模式 | 3 |
| 专家模式 | 5 |
| DUO | 3+专家人数 |

当xp小于0时候降级，xp变为上一级的满xp

以下列出一些算好的数据方便查阅：

| rank | 升级所需xp | 降级所需秒数 | 每秒流失速率 |
| - | - | - | - |
| 1 | 4 | 40.00 | 0.1 |
| 2 | 8 | 26.67 | 0.3 |
| 3 | 12 | 20.00 | 0.6 |
| 4 | 16 | 16.00 | 1 |
| 5 | 20 | 13.33 | 1.5 |
| 6 | 24 | 11.43 | 2.1 |
| 7 | 28 | 10.00 | 2.8 |
| 8 | 32 | 8.89 | 3.6 |
| 9 | 36 | 8.00 | 4.5 |
| 10 | 40 | 7.27 | 5.5 |
| 11 | 44 | 6.67 | 6.6 |
| 12 | 48 | 6.15 | 7.8 |
| 13 | 52 | 5.71 | 9.1 |
| 14 | 56 | 5.33 | 10.5 |
| 15 | 60 | 5.00 | 12 |
| 16 | 64 | 4.71 | 13.6 |
| 17 | 68 | 4.44 | 15.3 |
| 18 | 72 | 4.21 | 17.1 |
| 19 | 76 | 4.00 | 19 |
| 20 | 80 | 3.81 | 21 |
| 21 | 84 | 3.64 | 23.1 |
| 22 | 88 | 3.48 | 25.3 |
| 23 | 92 | 3.33 | 27.6 |
| 24 | 96 | 3.20 | 30 |
| 25 | 100 | 3.08 | 32.5 |
| 26 | 104 | 2.96 | 35.1 |
| rank | 升级所需xp | 降级所需秒数 | 每秒流失速率 |
| 专家1 | 4 | 24.00 | 0.167 |
| 专家2 | 8 | 16.00 | 0.5 |
| 专家3 | 12 | 12.00 | 1 |
| 专家4 | 16 | 9.60 | 1.667 |
| 专家5 | 20 | 8.00 | 2.5 |
| 专家6 | 24 | 6.86 | 3.5 |
| 专家7 | 28 | 6.00 | 4.667 |
| 专家8 | 32 | 5.33 | 6 |
| 专家9 | 36 | 4.80 | 7.5 |
| 专家10 | 40 | 4.36 | 9.167 |
| 专家11 | 44 | 4.00 | 11 |
| 专家12 | 48 | 3.69 | 13 |
| 专家13 | 52 | 3.43 | 15.167 |
| 专家14 | 56 | 3.20 | 17.5 |
| 专家15 | 60 | 3.00 | 20 |
| 专家16 | 64 | 2.82 | 22.667 |
| 专家17 | 68 | 2.67 | 25.5 |
| 专家18 | 72 | 2.53 | 28.5 |
| 专家19 | 76 | 2.40 | 31.667 |
| 专家20 | 80 | 2.29 | 35 |
| 专家21 | 84 | 2.18 | 38.5 |
| 专家22 | 88 | 2.09 | 42.167 |
| 专家23 | 92 | 2.00 | 46 |
| 专家24 | 96 | 1.92 | 50 |
| 专家25 | 100 | 1.85 | 54.167 |
| 专家26 | 104 | 1.78 | 58.5 |

### 外观

推进器信息显示在场地的下方，此处用文字简述一下外观：

| rank | 倍率 | 颜色 | 图案 | 速通模式备注 |
| - | - | - | - | - |
| 1 | 0.25 | 无 | 一个进度条 | |
| 2 | 0.5 | 红 | 下面加一个三角形 | |
| 3 | 0.75 | 橙 | 三角形长出翅膀 | |
| 4 | 1 | 黄绿 | 翅膀变大 | |
| 5 | 1.25 | 蓝 | 翅膀变大加底座 | |
| 6 | 1.5 | 紫 | 翅膀伸出进度条长度 | |
| 7 | 1.75 | 亮橙 | 翅膀更多细节 | 不退出速通模式的最低rank |
| 8 | 2 | 青绿 | 进度条上面加一对平行四边形 | 1/2层到此触发 |
| 9 | 2.25 | 青蓝 | 两对平行四边形 | 3/4层到此触发 |
| 10 | 2.5 | 亮紫 | 三对平行四边形 | 5层到此触发 |
| 11 | 2.75 | 白 | 加一对小三角 | |

### 速通模式（Hyperspeed）

当玩家在前五层时rank就达到8/8/9/9/10时就会进入速通模式

会出现花里胡哨的致敬Bejeweled Twist的动画，和专属速通音乐

屏幕顶部出现大字显示当前局时间，底部列出完成前九层花的时间和离个人纪录的分段差异

达到十层时完成速通模式获得一个隐藏成就，或者rank掉到6级时会退出速通模式

## 攻击对象

本模式无法手动指定改变攻击对象，不过玩家的状态会影响自身被攻击的概率

玩家有一个`受击权重`参数，这个值越大越容易被攻击，初始值为3

### 随时间增加

当时间达到3/5/7分钟时`受击权重`+1

### 随“是否危险”变化

非专家模式下，每0.25秒刷新一次，如果”处于危险中“（详细条件没看懂，当成到顶版面闪红警告吧，不会差很多），`受击权重`会临时减1.5，脱离危险后1.5会加回来

### 转移到受击缓冲（没有净变化）

有一个`受击缓冲`，被攻击时，行数*0.25的`受击权重`会被转移到缓冲槽内（最多装到3，否则不转移），稍后再变回来

`受击缓冲`装满时（达到3），受击倍率会下降25%（继续被打都会少吃25%垃圾）

`受击缓冲`内的值会降低垃圾混乱度，装满时可让`messiness_change -45%`和`messiness_inner -18%`（线性）

`受击权重`释放间隔（第N层用第N个数，单位秒） 4.8， 3.9， 2.1， 1.4， 1.3， 0.9， 0.6， 0.4， 0.3， 0.2

如果`受击缓冲`内有值，每固定间隔（取自上边这个表）将`受击缓冲`内0.25的值转移回`受击权重`，也就是说层数越高，系统会允许其他人以更高的频率打入垃圾行

## 攻击、Allspin、B2B

普通消1、2、3行攻击为0、1、2 （注：非专家模式0连击普通消1会+1攻击，这让散消1有1的攻击效率了，特定情况可以考虑利用）

消4攻击为4，属于`特殊消除`

此模式使用All-Mini规则，所有的块的spin都是`特殊消除`，可以增加B2B计数 （其中T使用三角判定，其他块使用不可移动判定且只会计为Mini，伤害计算同普通消除一致，消123打012）

全消攻击为3，但同时全消计为两次`特殊消除`（和spin独立计算）（和新TL少量不同，新TL中为5攻击但不加也不断B2B）

从第二个`特殊消除`开始会累计B2B计数，同时提供1的额外伤害 （不再像老TL一样慢慢增加）

不过仍然有连续B2B数计算，且有一个然后断B2B时会产生一个超大攻击（B2B数-3，或者说连续特殊消除数-4）

所有伤害保留小数计算，最后小数部分按照数值概率向上取整，例如1.2最后有20%概率变成2，80%概率变成1

## 垃圾行

本模式的垃圾行和攻击分节状况无关，只考虑数量

具体的位置计算同zen模式内打开cheese timer后的效果，对于每一行垃圾有一定概率改变位置

`专家`和`混乱垃圾行`mod都会增加混乱度

`受击缓冲`的值会降低混乱度，详见攻击对象章节

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

### 专家 （塔罗牌：皇帝 Emperor）

各方面都变难一些：

1. 垃圾行瞬间出现，而不是一行行上涨
1. 增加垃圾混乱度
1. 移除“处于危险中”减少受击权重的机制

### 无暂存 （塔罗牌：节制 Temperance）

禁用暂存

### 混乱垃圾行 （塔罗牌：命运之轮 Wheel of Fortune）

垃圾混乱度显著增加

### 高重力 （塔罗牌：塔 The Tower）

重力显著增加

### 不稳定垃圾行 （塔罗牌：力量 Strength）

从场地底部升起的垃圾行数量翻倍（内部的机制其实是收到攻击数翻倍，且抵消倍率也翻倍）

这个Mod在前期可能可以一定程度加速玩家的进度，但是后期的威胁大于好处，所以基本还是一个负面mod（除非在打速通之类的项目，需要单独讨论）

### 双洞垃圾行 （塔罗牌：恶魔 The Devil）

每一行垃圾行都有可能会有两个洞

### 隐形 （塔罗牌：隐士 The Hermit）

玩家放下的方块会隐形，不过每5秒会全场地闪烁一次

### All-Spin （塔罗牌：魔法师 The Magician）

（见开头的spin判定规则）将spin伤害计算升级为非T块必定不mini，也能像T块一样提供消行数*2的输出

但是代价是每当玩家消除时（准确来说是锁定一块时场地左上角冒出的文本，除了消除还有无消行spin），如果提示文本完全一致，那么场地底部会出现一行特殊的实心垃圾行作为惩罚，上面随机位置写了一个倒计时数字为`当前层数+5`，当玩家做一个没有惩罚的消除时-1，归零时变为一行随机位置的单洞普通垃圾行，洞和数字的位置一致

这是唯一一个玩家水平足够后能提供很大正面效果而的mod

### 双人 （塔罗牌：恋人 The Lovers）

会员玩家可以邀请其他人和自己两个人一起玩此模式，两个人都死了后游戏结束

两个人发送出去给别人的伤害数值减半

一个人死了后另一个人可以做任务复活队友，任务分为ABCDEF六个等级，内容如下

| 难度等级 | 内部id | 数值 | 内容 | 外显文本 | 标签类型（？） | 有特定mod时不出现 |
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

其中F~A级的任务分别对应1~6的难度分，复活时任务的总复活难度分=`层数+已复活次数*2`（尽量平分为三个整数），具体表如下：

1. F×1
1. F×2
1. F×3
1. F×2 E×1
1. F×1 E×2
1. E×3
1. E×2 D×1
1. E×1 D×2
1. D×3
1. D×2 C×1
1. D×1 C×2
1. C×3
1. C×2 B×1
1. C×1 B×2
1. B×3
1. B×2 A×1
1. B×1 A×2
1. A×3 (上限)

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
