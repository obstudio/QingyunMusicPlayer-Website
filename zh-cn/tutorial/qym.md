# **QYM 编写指南**


## QYM 格式简介

QYM 格式是一种集记谱、播放功能为一体的新型音乐文件格式。QYM 格式以简谱符号为基础，同时融入了五线谱中的各种特殊符号，使得它既记法简单、容易编写，又功能强大、能支持五线谱和简谱中几乎所有的符号。

QYM 格式是一种文本化的音乐格式，只需要使用任何一款文本编辑器（如记事本）就能高效地编写音乐。同时，你也可以十分方便地将 QYM 格式渲染为五线谱和简谱，方便大家阅读。

如果你对简谱或五线谱有一定的了解，你将十分容易使用 QYM 来编写音乐。如果你没有使用过简谱或五线谱，这个由浅入深的指南也能让你很快明白编写 QYM 文件的方法。

> 青云播放器中另一种与 QYM 类似的格式是 QYS。QYM 和 QYS 中的符号大多是相同的，对于不同之处，本文档会给出说明。QYS 在简谱和五线谱的基础上，增加了函数、全局变量、子音轨等一些新的概念，使得音乐编写更加自由。与 QYS 相比，QYM 格式则更加严谨，更多地遵照五线谱和简谱中对各符号的规定。要全面了解 QYS 格式，请参见 [《QYS 编写手册》](/zh-cn/tutorial/qys)。


## 目录

+ [音符](#音符)
+ [连音](#连音)
+ [节奏](#节奏)
+ [调号](#调号)
+ [实例1](#实例1)
+ [多声部](#多声部)
+ [和弦](#和弦)
+ [反复](#反复)
+ [装饰音](#装饰音)
+ [实例2](#实例2)
+ [功能支持情况](#功能支持情况)


## 音符

音符的记法与简谱中音符的符号基本一致。

### 音高

`1`、`2`、`3`、`4`、`5`、`6`、`7` 分别代表唱名 do、re、mi、fa、sol、la、si。

例：输入南京地铁提示音（mi、do、re、sol）

	3125

音符后的 `'` 和 `,` 分别代表升高和降低一个八度。

例：输入一个音阶

	12345671'

音符前的 `#` 和 `b` 分别代表升高和降低一个半音。

例：输入mi、fa、#sol、la

	34#56

注意：
1. `'` 和 `,` 的作用相互抵消，所以在一个音符后最多只应出现 `'` 和 `,` 中的一种符号（但可以有多个）。同样的情况也适用于 `#` 和 `b`。

> QYS 中 `#`、`b` 与 `'`、`,` 一样，都记在音符后。

### 时值

音符的默认时值为一个四分音符的时值。

音符后的 `-` 代表音符时值延长一个四分音符的时值。

例：输入两个二分音符和一个全音符

	1-5-3---

音符后的 `_` 代表音符时值缩短一半。

例：输入一个八分音符和两个十六分音符

	5_5__6__

音符后的一个附点符号 `.` 会将音符时值变为原来的 1.5 倍，两个附点符号（双附点符号）	`..` 会将音符时值变为原来的 1.75 倍。一般说来，音符后的 n 附点符号将音符时值变为原来的 (2-2^(-n)) 倍。

例：输入一个附点四分音符和一个八分音符

	1.3_

注意：
1. `-` 和 `_` 不应混用，即一个音符后最多只应有 `-` 和 `_` 中的一种。若要表示类似 2.25 个四分音符时值这样的时值，请将其拆成两个音符，并用 [连音线](#连音线) 连接;
2. 附点符号应该记在所有 `-` 或 `_` 之后，以免时值计算上出现混乱。

### 休止符和打击乐

`0` 代表休止符。

例：输入两个休止符

	2300

打击乐没有音高，使用 `x` 来表示。打击乐需要在 [乐器](#乐器) 中事先指定才能使用。

注意：
1. 依照简谱的规范，多拍的休止请使用多个休止符（如 `0000`）表示，而不要用类似 `0---` 这样的符号表示。


## 连音

连音线和连音符是两种不同的符号，这里将其一起介绍，以示区别。

### 连音线

连音线分为延音线和圆滑线。延音线用在音高相同的两个音符之间，表示将这两个音符时值相加作为一个音符演奏；圆滑线用在音高不同的两个音符之间，表示演奏连贯。不管是延音线还是圆滑线，统一用音符之间的 `^` 表示，程序会自动区分。对于多个音符之间的连音线，需要在每两个音符之间都加上连音线 `^`。

例：输入两个音符之间的连音线

	5.^4_3^5

### 连音符

连音符表示在一定的时值中，连续均等地演奏若干个音符。如三连音符号 `(3)` 会将该符号之后的 3 个音符的时值变为原有时值的 2/3 倍，这样在原本只能演奏两个音符的时值内，就能连续均等地演奏 3 个音符。一般说来，n 连音符号 `(n)` 会将该符号之后的 n 个音符的时值变为原有时值的 (2^Floor(Log2(n)))/n 倍（其中 Floor() 表示向下取整，Log2() 表示以 2 为底的对数）。

例：输入三连音

	3_.1__(3)5_5_5_

注意：
1. 连音符表示连续均等地演奏若干个音符，所以 n 连音符号后的 n 个音符原时值应相同。

> QYS 中，连音符用 `(n~)` 表示。


## 节奏

### 小节

`|` 为小节线，表示两小节的分隔

例：输入四个小节上行三度及下行二度音程

	1-3-|2-4-|3-5-|4-6-|

`||` 为终止线，表示乐曲的终止。

例：输入一个四个小节的乐曲

	1155|665-|4433|221-||

### 拍号

`<x/y>` 表示乐谱为 *x*/*y* 拍（即一个 *y* 分音符为一拍，每个小节有 *x* 拍）。

例：输入3/4拍

	<3/4>

### 速度

`<speed>` 表示每分钟演奏 *speed* 拍。

例：规定速度为每分钟 100 拍

	<100>


## 调号

调号用 `<1=X>` 表示，其中 *X* 表示该调主音（即 do）的绝对音高。*X* 与主音绝对音高的对应关系请参见下表：

<table>
	<tr>
		<th><i>X</i></th>
		<th>主音</th>
		<th>与 C 大调相差半音数</th>
	</tr>
	<tr>
		<td>C</td>
		<td>c1</td>
		<td>0</td>
	</tr>
	<tr>
		<td>G</td>
		<td>g1</td>
		<td>7</td>
	</tr>
	<tr>
		<td>D</td>
		<td>d1</td>
		<td>2</td>
	</tr>
	<tr>
		<td>A</td>
		<td>a1</td>
		<td>9</td>
	</tr>
	<tr>
		<td>E</td>
		<td>e1</td>
		<td>4</td>
	</tr>
	<tr>
		<td>B</td>
		<td>b</td>
		<td>-1</td>
	</tr>
	<tr>
		<td>#F</td>
		<td>升f1</td>
		<td>6</td>
	</tr>
	<tr>
		<td>#C</td>
		<td>升c1</td>
		<td>1</td>
	</tr>
	<tr>
		<td>F</td>
		<td>f1</td>
		<td>5</td>
	</tr>
	<tr>
		<td>bB</td>
		<td>降b</td>
		<td>-2</td>
	</tr>
	<tr>
		<td>bE</td>
		<td>降e1</td>
		<td>3</td>
	</tr>
	<tr>
		<td>bA</td>
		<td>降a1</td>
		<td>8</td>
	</tr>
	<tr>
		<td>bD</td>
		<td>降d1</td>
		<td>1</td>
	</tr>
	<tr>
		<td>bG</td>
		<td>降g1</td>
		<td>6</td>
	</tr>
	<tr>
		<td>bC</td>
		<td>降c1</td>
		<td>-1</td>
	</tr>
</table>

若要表示更高或更低的主音，可以在 *X* 后加 `'` 和 `,`，如 `<1=A,>` 表示主音为 a。

例：输入G大调

	<1=G>

注意：
1. <1=bB> 和 <1=B> 表示主音是小字组的降 B 和 B，而不是小字一组的。


## 实例1

### 注释

在文件的开头，可以用 `//` 表示注释，记录音乐的基本信息。

### 实例

以下是 QYM 格式的音乐《东方红》：

	//东方红
	//陕北民歌
	<1=F><2/4><90>55_^6_|2-|11_^6,_|2-|55|6_^1'_6_5_|11_^6,_|2-|52|17,_^6,_|5,5|23_2_|11_^6,_|2_3_2_1_|2_^1_7,_^6,_|5,-^|5,0||

如果你发现你还有一些符号不能理解，请 [单击这里](#音符) 再认真阅读一下之前的内容。


## 多声部

### 多声部表示

多声部之间以换行符分隔，所有的行都会同时开始播放。

例：一段含有两个声部的音乐

	35_5_^|5_5_4|5,4_4_^|4_4_3||
	13_3_^|3_3_2|5,2_2_^|2_2_1||

### 初始化行

一首音乐中的调号、拍号和速度，在各个声部中一般是一样的。因此，允许在所有声部之前增加一个仅含有调号、拍号和速度符号的初始化行，初始化行中的符号对以下的所有声部都有效。

例：在所有声部之前增加初始化行

	<1=G><2/4><132>
	35_5_^|5_5_4|5,4_4_^|4_4_3||
	13_3_^|3_3_2|5,2_2_^|2_2_1||

程序会自动判断初始化行是否存在，若初始化行不存在，程序将采用默认值 C 大调、4/4 拍、速度为 90（在没有初始化行的情况下，调号、拍号和速度可以在声部中修改，如 [实例1](#实例) 中的情况）。

### 乐器

对于每一个声部，可以使用 `{instrument}` 指定该声部的乐器。若没有指定，默认为 Piano（钢琴）。*instrument* 的所有可能的取值如下：

一般乐器：Accordion, Agogo, AltoSax, Applause, Atmosphere, Bagpipe, Bandoneon, Banjo, BaritoneSax, Bass, BassAndLead, Bassoon, Bird, BlownBottle, Bowed, BrassSection, Breath, Brightness, BrightPiano, Calliope, Celesta, Cello, Charang,Chiff, Choir, Clarinet, Clavi, Contrabass, Crystal, DrawbarOrgan, Dulcimer, Echoes, ElectricBass, ElectricGrandPiano, ElectricGuitar, ElectricPiano, ElectricPiano2, EnglishHorn, Fiddle, Fifths, Flute, FrenchHorn, FretlessBass, FretNoise, Glockenspiel, Goblins, Guitar, GuitarDistorted, GuitarHarmonics, GuitarMuted, GuitarOverdriven, Gunshot, Halo, Harmonica, Harp, Harpsichord, Helicopter, HonkyTonkPiano, JazzGuitar, Kalimba, Koto, Marimba, MelodicTom, Metallic, MusicBox, MutedTrumpet, NewAge, Oboe, Ocarina, OrchestraHit, Organ, PanFlute, PercussiveOrgan, Piano, Piccolo, PickedBass, PizzicatoStrings, Polysynth, Rain, Recorder, ReedOrgan, ReverseCymbal, RockOrgan, Sawtooth, SciFi, Seashore, Shakuhachi, Shamisen, Shanai, Sitar, SlapBass, SlapBass2, SopranoSax, Soundtrack, Square, Steeldrums, SteelGuitar, Strings, Strings2, Sweep, SynthBass, SynthBass2, SynthBrass, SynthBrass2, SynthDrum, SynthStrings ,SynthStrings2, SynthVoice, Taiko, Telephone, TenorSax, Timpani, Tinklebell, TremoloStrings, Trombone, Trumpet, Tuba, TubularBells, Vibraphone, Viola, Violin, Voice, VoiceAahs, VoiceOohs, Warm, Whistle, Woodblock, Xylophone

打击乐器：BassDrum, BassDrum2, BellTree, Cabasa, Castanets, ChineseCymbal, Clap, Claves, Cowbell, CrashCymbal, CrashCymbal2, ElectricSnare, GuiroLong, GuiroShort, HighAgogo, HighBongo, HighCongaMute, HighCongaOpen, HighFloorTom, HighTimbale, HighTom, HighWoodblock, HiHatClosed, HiHatOpen, HiHatPedal, JingleBell, LowAgogo, LowBongo, LowConga, LowFloorTom, LowTimbale, LowTom, LowWoodblock, Maracas, MetronomeBell, MetronomeClick, MidTom, MidTom2, MuteCuica, MuteSurdo, MuteTriangle, OpenCuica, OpenSurdo, OpenTriangle, RideBell, RideCymbal, RideCymbal2, ScratchPull, ScratchPush, Shaker, SideStick, Slap, Snare, SplashCymbal, SquareClick, Sticks, Tambourine, Vibraslap, WhistleLong, WhistleShort

例：指定乐器为 ElectricGuitar（电吉他）

	{ElectricGuitar}5,|3--5_^6_|33_^2_1.3_|2_.^3__2_1_6,^6,__^1__^2__^1__|2--||

### 音量

每个声部的音量默认均为 100%。音量符号 `<volume%>` 可以将该声部的音量更改为 *volume*%。

例：将音量改为 50%

	{ElectricPiano}<50%>3|1.^2_35|6.1'_76|5--||

对于多种乐器演奏同样的音乐的情况，可以用逗号隔开，一起记录。如 `{ElectricPiano,ElectricGuitar}<50%,25%>` 表示这个声部由电子琴和电吉他一起演奏，音量分别为 50% 和 25%。

注意：
1. 若一个声部指定了多个乐器，但音量只有一个，那么这个声部所有的乐器的音量都会设置为指定的音量。除此之外，不允许其它乐器个数与音量个数不一致的情况。

### 段落

大多数音乐较长，不便显示。段落功能解决了这个问题。

一个段落由任意行（也可以没有）注释、0 或 1 个初始化行、若干个声部行组成。两个段落之间由一个空行分隔。在所有段落之前，可以有任意行的音乐注释，之后用一个空行与第一个段落分开。一般的段落格式如下：

	//音乐注释 1
	//音乐注释 2
	//……
	//音乐注释 n
	　
	//段落 1 注释 1
	//段落 1 注释 2
	//……
	//段落 1 注释 m
	<段落 1 初始化>
	段落 1 声部 1 ||
	段落 1 声部 2 ||
	……
	段落 1 声部 p ||
	　
	//段落 2 注释 1
	//段落 2 注释 2
	//……
	//段落 2 注释 k
	<段落 2 初始化>
	段落 2 声部 1 ||
	段落 2 声部 2 ||
	……
	段落 2 声部 q ||
	　
	……
	　
	……

由于方便编辑和显示，即使是单声部的音乐，也建议用段落的形式记录。如 [实例1](#实例) 一般会写成这样：

	//东方红
	//陕北民歌
	<1=F><2/4><90>
	55_^6_|2-|11_^6,_|2-|55|6_^1'_6_5_|11_^6,_|2-|
	　
	52|17,_^6,_|5,5|23_2_|11_^6,_|2_3_2_1_|2_^1_7,_^6,_|5,-^|5,0||

不同段落的声部数也可以不一致，也就是说，如果一个声部在之后的段落才开始出现，之前的段落中完全可以不写该声部。


## 和弦

有待完善。


## 反复

### 段落反复符号

段落反复符号 `||:` 和 `:||` 分别标记反复的结束和开始。

### 反复跳跃符号

`[n.]`


### Coda 符号

`||*` `*||`

## 装饰音

### 前倚音

`(abc^)`

### 震音

`(n-)` `(n=)`

### 滑音

`a~b`


## 功能支持情况

青云播放器现有 QuickBasic、Wolfram Language（Mathematica）和 JavaScript 三个版本，由于部分语言存在一定限制，各功能的支持情况略有不同（“√”表示完全支持，“〇”表示有一定限制，“×”表示不支持）：

<table>
	<tr>
		<th>功能</th>
		<th>QuickBasic</th>
		<th>Wolfram Language</th>
		<th>JavaScript</th>
	</tr>
	<tr>
		<td>基本音符</td>
		<td>√</td>
		<td>√</td>
		<td></td>
	</tr>
	<tr>
		<td>段落反复</td>
		<td>√</td>
		<td>√</td>
		<td></td>
	</tr>
</table>