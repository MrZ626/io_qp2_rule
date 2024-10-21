# Tetr.io规则观察总结 By MrZ_26

## 开始

该大房间不存在公共的“一局”的概念，所有的人随开随打，不需要等待

## 爬塔

此模式的题材为爬塔，最终分数称为`高度`（Altitude）

增加`高度`有三个途径：发送垃圾行（抵消不算），击杀敌人（+15），随时间轻微增加

玩家只会和`高度`相近的其他玩家交互

（待验证）但特别大的攻击可能会打给高层玩家

（待验证）越接近下一层，越容易成为别人的攻击目标

达到一定的`高度`后会进入下一层，每一层的`高度`范围如下：

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

`高度`的增加量受“推进器等级”影响，推进器信息显示在屏幕下方，推进器经验的获取途径和高度基本一致，但会随时间减少，级别越高掉得越快

最开始倍率为1，推进器每升1级获得×0.25的倍率，达到×2.75时变成白色不再看得出区别，但无上限

| 级别 | 倍率 | 颜色 | 图案 | 速通模式备注 |
| - | - | - | - | - |
| 1 | 0.25 | 无 | 一个进度条 | |
| 2 | 0.5 | 红 | 下面加一个三角形 | |
| 3 | 0.75 | 橙 | 三角形长出翅膀 | |
| 4 | 1 | 黄绿 | 翅膀变大 | |
| 5 | 1.25 | 蓝 | 翅膀变大加底座 | |
| 6 | 1.5 | 紫 | 翅膀伸出进度条长度 | |
| 7 | 1.75 | 亮橙 | 翅膀更多细节 | 不退出速通模式的最低等级 |
| 8 | 2 | 青绿 | 进度条上面加一对平行四边形 | 1/2层到此触发 |
| 9 | 2.25 | 青蓝 | 两对平行四边形 | 3/4层到此触发 |
| 10 | 2.5 | 亮紫 | 三对平行四边形 | 5层到此触发 |
| 11 | 2.75 | 白 | 加一对小三角 | |

### 推进器经验槽

推进器经验槽由以下方式变化

#### 发送垃圾行

每发送 1 行，获得 1 点

#### 抵消预告垃圾

每抵消 1 行，获得 0.5 点，专家模式下无效

#### 消行

一次消行下，每消 1 行，获得 1 点，最多 2 点，专家模式下无效

每一次，每一种获取经验槽，都会有额外 0.05 点

推进器经验槽每升级所需经验为 4 * 当前等级

推进器经验槽随推进器等级和时间减少，设等级为 t ，每帧减少量为 e * (t ** 2 + t) / 3600 ，其中系数 e 如下

普通模式 e = 3

DUO下一人专家模式 e = 4

专家模式 e = 5

当经验槽 < 0 时触发掉级，获得上一级所需经验

换算表

普通模式

| 级别 | 下一级经验 | 降级所需时间（秒） | 1点经验所需时间 | 每分钟降低速率 |
| - | - | - | - | - |
| 1 | 4 | 40.00 | 10.00 | 6 |
| 2 | 8 | 26.67 | 3.33 | 18 |
| 3 | 12 | 20.00 | 1.67 | 36 |
| 4 | 16 | 16.00 | 1.00 | 60 |
| 5 | 20 | 13.33 | 0.67 | 90 |
| 6 | 24 | 11.43 | 0.48 | 126 |
| 7 | 28 | 10.00 | 0.36 | 168 |
| 8 | 32 | 8.89 | 0.28 | 216 |
| 9 | 36 | 8.00 | 0.22 | 270 |
| 10 | 40 | 7.27 | 0.18 | 330 |
| 11 | 44 | 6.67 | 0.15 | 396 |
| 12 | 48 | 6.15 | 0.13 | 468 |
| 13 | 52 | 5.71 | 0.11 | 546 |
| 14 | 56 | 5.33 | 0.10 | 630 |
| 15 | 60 | 5.00 | 0.08 | 720 |
| 16 | 64 | 4.71 | 0.07 | 816 |
| 17 | 68 | 4.44 | 0.07 | 918 |
| 18 | 72 | 4.21 | 0.06 | 1026 |
| 19 | 76 | 4.00 | 0.05 | 1140 |
| 20 | 80 | 3.81 | 0.05 | 1260 |
| 21 | 84 | 3.64 | 0.04 | 1386 |
| 22 | 88 | 3.48 | 0.04 | 1518 |
| 23 | 92 | 3.33 | 0.04 | 1656 |
| 24 | 96 | 3.20 | 0.03 | 1800 |
| 25 | 100 | 3.08 | 0.03 | 1950 |
| 26 | 104 | 2.96 | 0.03 | 2106 |

专家模式

| 级别 | 下一级经验 | 降级所需时间（秒） | 1点经验所需时间 | 每分钟降低速率 |
| - | - | - | - | - |
| 1 | 4 | 24.00 | 6.00 | 10 |
| 2 | 8 | 16.00 | 2.00 | 30 |
| 3 | 12 | 12.00 | 1.00 | 60 |
| 4 | 16 | 9.60 | 0.60 | 100 |
| 5 | 20 | 8.00 | 0.40 | 150 |
| 6 | 24 | 6.86 | 0.29 | 210 |
| 7 | 28 | 6.00 | 0.21 | 280 |
| 8 | 32 | 5.33 | 0.17 | 360 |
| 9 | 36 | 4.80 | 0.13 | 450 |
| 10 | 40 | 4.36 | 0.11 | 550 |
| 11 | 44 | 4.00 | 0.09 | 660 |
| 12 | 48 | 3.69 | 0.08 | 780 |
| 13 | 52 | 3.43 | 0.07 | 910 |
| 14 | 56 | 3.20 | 0.06 | 1050 |
| 15 | 60 | 3.00 | 0.05 | 1200 |
| 16 | 64 | 2.82 | 0.04 | 1360 |
| 17 | 68 | 2.67 | 0.04 | 1530 |
| 18 | 72 | 2.53 | 0.04 | 1710 |
| 19 | 76 | 2.40 | 0.03 | 1900 |
| 20 | 80 | 2.29 | 0.03 | 2100 |
| 21 | 84 | 2.18 | 0.03 | 2310 |
| 22 | 88 | 2.09 | 0.02 | 2530 |
| 23 | 92 | 2.00 | 0.02 | 2760 |
| 24 | 96 | 1.92 | 0.02 | 3000 |
| 25 | 100 | 1.85 | 0.02 | 3250 |
| 26 | 104 | 1.78 | 0.02 | 3510 |

### 推进器升级

当经验槽大于所需经验时，触发升级，升级扣除所需点数，此时推进器经验不会下降，持续 5 秒

因为掉级而重新升级时，会减少 1 秒，至少持续 1 秒，升级后至少达到上一级一半的经验，将重置到 5 秒

### 推进器跳级

触发一次升级后，设升级后等级为 t，若经验槽剩余点数 > (4 * t)，则额外升级 剩余点数 / (4 * t)，不扣除经验槽点数

会出现 第 N 帧发出大spike，N + 1 帧跳级，N + 2 帧还升级的情况

### 速通模式（Hyperspeed）

当玩家在前五层时推进器等级就达到8/8/9/9/10时就会进入速通模式

会出现花里胡哨的致敬Bejeweled Twist的动画，和专属速通音乐

屏幕顶部出现大字显示当前局时间，底部列出完成前九层花的时间和离个人纪录的分段差异

达到十层时完成速通模式获得一个隐藏成就，或者推进器掉到6级时会退出速通模式

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

全消攻击为3，但同时全消计为两次`特殊消除`（和spin独立计算）（和新TL少量不同，新TL中为5攻击但不加也不断b2b）

从第二个`特殊消除`开始会累计b2b计数，同时提供1的额外伤害 （不再像老TL一样慢慢增加）

不过仍然有连续b2b数计算，且有一个然后断b2b时会产生一个超大攻击（b2b数-3，或者说连续特殊消除数-4）

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

| 难度等级 | 内部id | 数值 | 外显文本 | 标签类型（？） | 被mod屏蔽 |
| - | - | - | - | - | - |
| F | combo             | 3   | Perform a 3-Combo | 2 | |
| F | double            | 2   | Clear 2 Doubles | 2 | |
| F | quad              | 1   | Clear a Quad | 1 | |
| F | lines             | 6   | Clear 6 Lines | 1 | |
| F | odouble           | 1   | Clear a Double \n using an O-Piece | 3 | |
| F | garbageclear      | 4   | Clear 4 Garbage Lines | 2 | |
| F | szdouble          | 1   | Clear a Double \n using an S or Z-Piece | 3 | |
| F | ljtriple          | 1   | Clear a Triple \n using an L or J-Piece | 3 | |
| E | tspinmini         | 1   | Perform a T-Spin Mini | 1 | |
| E | tspinsingle       | 1   | Clear a T-Spin Single | 2 | |
| E | tspindouble       | 1   | Clear a T-Spin Double | 2 | |
| E | szspin            | 1   | Clear an S/Z-Spin | 1 | |
| E | ljspin            | 1   | Clear an L/J-Spin | 1 | |
| E | combo             | 5   | Perform a 5-Combo | 2 | |
| E | iflat             | 2   | Clear 2 Lines using \n horizontal I-Pieces | 3 | |
| E | tank              | 4   | Tank 4 Garbage Lines | 2 | |
| E | cancel            | 4   | Cancel 4 Garbage Lines | 2 | |
| D | double            | 4   | Clear 4 Doubles | 2 | |
| D | spam              | 3   | Place 3 pieces in a row \n without moving or rotating | 4 | |
| D | noclear           | 14  | Place 14 pieces in a row \n without clearing any lines | 4 | |
| D | send              | 6   | Send 6 Lines | 1 | |
| D | pieces            | 20  | Place 20 pieces | 2 | |
| D | szdouble          | 2   | Clear 2 Doubles \n using S or Z-Pieces | 3 | |
| D | ljtriple          | 2   | Clear 2 Triples \n using L or J-Pieces | 3 | |
| D | ispinclear        | 1   | Clear an I-Spin | 1 | |
| D | upperhalfquad     | 1   | Clear a Quad in the \n upper half of the board | 4 | |
| C | tspintriple       | 1   | Clear a T-Spin Triple | 2 | |
| C | nohold            | 25  | Place 25 pieces \n without using Hold | 3 | nohold |
| C | triple            | 3   | Clear 3 Triples | 2 | |
| C | b2b               | 4   | Reach B2B x4 | 1 | |
| C | quadbuckets       | 2   | Clear a Quad in \n 2 different columns | 3 | |
| C | holdconsecutive   | 12  | Use Hold on \n 15 pieces in a row | 3 | nohold |
| C | softdrop          | 10  | Place 10 pieces without \n releasing Soft Drop | 4 | |
| C | top3rows          | 3   | Have part of your stack in \n the top 3 rows for 3 seconds | 4 | |
| C | linesnoti         | 10  | Clear 10 Lines without \n clearing with T or I-pieces | 4 | |
| C | szspintriple      | 1   | Clear an S/Z-Spin Triple | 2 | |
| C | odoubleconsecutive| 2   | Clear 2 Doubles consecutively \n using two O-Pieces | 4 | |
| C | tspinminiclear    | 4   | Clear 4 T-Spin Minis | 2 | |
| B | oclear            | 6   | Clear 6 Lines \n using O-Pieces | 3 | |
| B | spinbuckets       | 3   | Clear Spin-Clears \n with 3 different pieces | 3 | |
| B | quad              | 4   | Clear 4 Quads | 1 | |
| B | spam              | 5   | Place 5 pieces in a row \n without moving or rotating | 4 | |
| B | send              | 18  | Send 18 Lines | 1 | |
| B | ljspintriple      | 1   | Clear an L/J-Spin Triple | 2 | |
| B | quadconsecutive   | 2   | Clear 2 Quads in a row | 2 | |
| B | singlesonly       | 8   | Clear 8 Singles without doing \n other clears or using Hold | 4 | |
| B | nogarbage         | 4   | Have no Garbage Lines on \n your board for 4 seconds | 4 | |
| B | rotate            | 100 | Rotate 100 times | 2 | |
| B | nocancel          | 8   | Don't cancel any \n garbage for 8 seconds | 3 | |
| A | combo             | 7   | Perform a 7-Combo | 2 | |
| A | ispindouble       | 1   | Clear an I-Spin Double | 2 | |
| A | szspinconsecutive | 2   | Clear two S/Z-Spin \n Doubles consecutively | 3 | |
| A | ljspinconsecutive | 2   | Clear two L/J-Spin \n Doubles consecutively | 3 | |
| A | colorclear        | 1   | Perform a Color Clear | 2 | |
| A | lines             | 40  | Clear 40 Lines | 1 | |

复活时要做什么样的任务取决于`复活难度分=层数+已复活次数*2`

| 难度分 | 任务内容 |
| --- | --- |
|  1 | F×1     |
|  2 | F×2     |
|  3 | F×3     |
|  4 | F×2 E×1 |
|  5 | F×1 E×2 |
|  6 | E×3     |
|  7 | E×2 D×1 |
|  8 | E×1 D×2 |
|  9 | D×3     |
| 10 | D×2 C×1 |
| 11 | D×1 C×2 |
| 12 | C×3     |
| 13 | C×2 B×1 |
| 14 | C×1 B×2 |
| 15 | B×3     |
| 16 | B×2 A×1 |
| 17 | B×1 A×2 |
| 18 | A×3     |

## 一些随手贴着不知道会不会有用的技术信息补充

`受击权重`:targetingfactor
`受击缓冲`:targetinggrace

```js
    // 一些常数表
    FloorDistance = [0, 50, 150, 300, 450, 650, 850, 1100, 1350, 1650, 1 / 0];
    TargetingGrace = [0, 4.8, 3.9, 2.1, 1.4, 1.3, .9, .6, .4, .3, .2];
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
