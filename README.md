# RedGPT: Reference-Enlightened-Dialogue by GPT and for GPT

[[English Version](README_EN.md)]

众所周知，事实正确性是ChatGPT的一大薄弱环节，也是所有试图复现ChatGPT的同行们所面临的重大挑战。想要提升事实正确性，可以标注大量的事实型对话数据（比如人物、科技、医疗、法律、艺术）用于微调GPT模型。为了避免人工标注的昂贵成本，我们提出一种自动生成事实型对话的方法，并公开我们的部分数据。我们公开的第一批数据（RedGPT-Dataset-V1-CN）共包含5万条中文多轮对话。



## 方法介绍


我们的目标是自动生成海量、高质量、事实型多轮对话，用于训练GPT，提升GPT的事实正确性。我们采用如下方法自动生成数据。

1. 采集优质的事实型文档，我们称之为reference，其来源可以是电子书、维基百科、优质垂类网站。文档需要涵盖尽量多的主题，包括但不限于人物、机构、科技、医疗、法律、人文、经济、家居、汽车、出行、美食、时尚、体育、教育、宠物。
2. 利用已有的LLM（例如付费API）生成多轮对话。输入是一篇reference，prompt类似“请根据这篇文章生成多轮问答”。API会输出一段多轮对话（dialogue）。这种方法将原本只适合预训练的文档转化成可供微调的多轮对话。
3. 第2步收集到大量的reference-dialogue二元组。将reference和prompt作为输入，dialogue作为目标，微调一个GPT模型（可以基于LLaMA或BLOOM的预训练基座）。我们将微调出的模型称作*Reference-Enlightened-Dialogue GPT*，缩写*RedGPT*。有了RedGPT，即可基于reference生成多轮对话，获得海量的数据。


外界复现我们的方法时，需要关注2个要点。
1. Reference的质量、广度。Reference内容质量必须高，比如医疗等优质垂类网站的页面、维基百科上非生僻的词条，且需要对网页做清洗。Reference的广度应当大，不能限制在单个垂类或单个网站。
2. 调用已有LLM时需要写prompt，需要仔细尝试各种prompt，使得LLM生成的多轮对话符合预期。


## 数据公开

我们使用上述步骤1和2获得大量多轮对话。我们公开其中部分数据，称之为“RedGPT-Dataset-V1-CN”，包含5万对中文reference-dialogue二元组，其中dialog是基于reference生成的多轮对话。

### 例1：

- Reference：1863年征兵暴动。美国南北战争期间，纽约市工人因对征兵制度不满而举行的四天暴动。法律允许交纳三百美元即可免役，这笔金额仅极少数人能负担。1863年7月11日纽约市点名入伍一开始，大批白人（多为工人）涌上街头，攻打征兵总部，烧毁房屋。在纽约市，白人对于解放后南方来的黑人进入劳工市场，特别是许多雇主雇用黑人破坏罢工，又有嫉恨，于是暴动者时常砸毁无辜的黑人家庭和商店。约一百人死亡（多为暴民）。

- Dialogue：
	- Human: 你知道1863年的征兵暴动吗？
	- Assistant: 是的，那是在美国南北战争期间，纽约市的工人因不满征兵制度而举行的四天暴动。
	- Human: 那么，为什么工人会暴动呢？
	- Assistant: 因为法律允许交纳三百美元即可免役，这笔金额仅极少数人能负担，大多数工人无法承受这样的费用。
	- Human: 那么这场暴动导致了多少损失？
	- Assistant: 暴动者攻打征兵总部，并烧毁了房屋，造成了巨大的财产损失。此外，暴动者还残酷地攻打了一些无辜的黑人家庭和商店，共有约一百人死亡。
	- Human: 为什么暴动者会攻击黑人家庭和商店呢？
	- Assistant: 在纽约市，白人对于解放后南方来的黑人进入劳工市场，特别是许多雇主雇用黑人破坏罢工，有嫉恨，这导致了暴动者时常砸毁无辜的黑人家庭和商店。


### 例2：

- Reference：巴泽雷克冢墓。南西伯利亚早期铁器时代的墓地。位于俄罗斯戈尔诺阿尔泰省的巴泽雷克盆地 。主要包括自南向北联成一线的5 座冢墓。冢墓的发掘为研究阿尔泰早期畜牧部落的体质类型和物质文化提供了珍贵资料。 巴泽雷克冢墓于1927 ～ 1949年间全部发掘 。墓上均有圆形石冢，地下以双层原木构成墓穴。墓室南墙附近放置厚木棺具，棺具表面有禽兽图。室内北面随葬墓主生前遗物 ，墓室外葬有马具和马尸。阿尔泰地区高山酷寒，流入墓中的积水常年冰冻不化，使随葬品中许多有机物得以保存完好 。随葬品有来自中国和西亚的丝织和毛织品，上面饰有极具特色的阿尔泰狮身人面像。还有武器、木器、金银装饰品、乐器和吸食大麻的烟具等。 巴泽雷克冢墓出土的马饰 巴泽雷克冢墓主人为部落首领或贵族，有文身习俗。其妻妾同时殉葬。所有遗体均经过防腐处理。墓主兼具欧罗巴人种和蒙古人种的特征。多数学者认为墓葬的年代在公元前3世纪～前2世纪间。当时处于氏族解体阶段。

- Dialogue：
	- Human: 巴泽雷克冢墓位于哪里？其发掘提供了何种珍贵资料？
	- Assistant: 巴泽雷克冢墓位于俄罗斯戈尔诺阿尔泰省的巴泽雷克盆地。发掘巴泽雷克冢墓为研究阿尔泰早期畜牧部落的体质类型和物质文化提供了珍贵资料。该冢墓发掘于1927~1949年间。
	- Human: 巴泽雷克冢墓的建筑特点是什么？
	- Assistant: 巴泽雷克冢墓上均有圆形石冢，地下以双层原木构成墓穴。墓室南墙附近放置厚木棺具，棺具表面有禽兽图。室内北面随葬墓主生前遗物，室外葬有马具和马尸。
	- Human: 巴泽雷克冢墓出土了哪些随葬品？
	- Assistant: 巴泽雷克冢墓出土了来自中国和西亚的丝织和毛织品，上面饰有极具特色的阿尔泰狮身人面像。还有武器、木器、金银装饰品、乐器和吸食大麻的烟具等。
	- Human: 巴泽雷克冢墓主人的身份是什么？墓葬的年代是什么时候？
	- Assistant: 巴泽雷克冢墓主人为部落首领或贵族，其妻妾同时殉葬。所有遗体均经过防腐处理。墓主兼具欧罗巴人种和蒙古人种的特征。多数学者认为墓葬的年代在公元前3世纪～前2世纪间，那时正处于氏族解体阶段。


### 例3：

- Reference：广义货币是什么？ 货币统一有利于商品交换和物资交流，更具有促进经济发展和国家统一的重要作用。我们最常见的是每个国家只使用一种货币，并且由中央银行发行和控制。可是，也会存在例外，如欧盟国家通用的欧元、西非经济共同体中的法郎等，出现了多个国家使用同一种货币的现象。一个国家可以选择别国的货币作为法定流用货币，如巴拿马把美元作为法定货币。不同国家的货币也可以使用相同的名字，如法国和比利时在使用欧元之前，它们和瑞士的货币都叫法郎。 广义货币是和狭义货币相对应的一个经济学概念，是货币供给的一种形式或者口径，通常以M2来表示，计算方法是交易货币加上定期存款和储蓄存款。由于历史原因，不同国家统计口径和方法也会有多不同，如在美国中常常以M3来表示广义货币。 广义货币的发展历史可以分为几个阶段。 （1）物物交换。人类最早使用的货币出现在物物交换时代，在生产力提高到一定水平后，为了获得自己需要的物资，寻找一种双方都愿意接受的中间物品。这个中间物品就充当了最原始的货币，如牲畜、盐、贝壳等。 （2）金属货币。经过不断地摸索，人们发现了金属最适合充当货币，并最终选择了金银等。这些金属呈块状，使用时先测试其成色，又要称重。为了方便，古希腊、罗马和波斯逐渐开始铸造重量、成色统一的硬币。 （3）纸币。随着经济的发展，人们发现金属货币也会出现种种缺陷，尤其是大额交易中，金属货币的重量和体积令人烦恼。于是，替代金属货币的纸币产生了。最初的纸币是以黄金为基础的，可以与黄金自由兑换，这种货币制度被称为“金本位”。 我国对货币层次的划分是这样的：M0=流通中的现金；M1=M0+银行活期存款；M2=M1+定期存款+证券公司客户保证金。其中M1是狭义货币，M2是广义货币。

- Dialogue：
	- Human: 什么是广义货币？ 
	- Assistant: 广义货币是一种货币供给的形式或口径，通常用M2来表示，计算方法是交易货币加上定期存款和储蓄存款。 
	- Human: 除了使用M2来表示外，还有其他表示广义货币的口径吗？
	- Assistant: 是的，在美国中常常使用M3来表示广义货币，但不同国家统计口径和方法有所不同。 
	- Human: 货币的发展史有哪些阶段？ 
	- Assistant: 货币的发展可以分为三个阶段：物物交换、金属货币、纸币。 
	- Human: 我国如何划分货币的层次？ 
	- Assistant: 我国对货币的层次划分是：M0=流通中的现金；M1=M0+银行活期存款；M2=M1+定期存款+证券公司客户保证金。其中M1是狭义货币，M2是广义货币。




### **注：**

1. 本次公开的数据（V1-CN）只包含中文。对于其他语言，可以采用类似的方法用reference生成dialog，也可以将我们公开的数据翻译成其他语言。我们计划在未来公布英文数据。
2. 本次公开的数据（V1-CN）未经人工校验，无法严格保证正确性、安全性。使用者需要对数据产生的结果负责。



## RedGPT模型

我们使用某些已有的LLM生成了大量的多轮对话（公开了其中5万段）。我们用所有的reference-dialogue二元组微调GPT，将得到的模型称为RedGPT，RedGPT是单任务模型，专门根据reference生成dialogue。我们可能会在未来公开RedGPT，届时同行可以用RedGPT生成多轮对话，最后将多轮对话用于复现ChatGPT。


## FAQ

- 问1：你们爬取了哪些网站或书籍？你们用的是GPT-3.5或GPT-4的API吗？
- 答1：由于潜在的法务风险，我们不便回答这些细节问题。

- 问2：你们的数据V1-CN包含5万条样本，只是你们数据的一部分对吗？可以公开全部吗？
- 答2：我们获取了大量reference-dialogue二元组用于训练RedGPT。我们公开的是全部数据的一个子集，以后会考虑公开全部。

- 问3：有了RedGPT，可以基于网页、电子书生成海量的多轮对话，对吗？你们是否打算公开RedGPT生成的数据？
- 答3：有了RedGPT，任何人都可以低成本生成海量的多轮对话，但前提是爬取和清洗网页、电子书。我们不会公开生成的数据，原因是爬取数据涉及法务风险。

- 问4：你们本次公布的数据只有中文，之后会公开其他语言的数据吗？
- 答4：我们计划公布英文数据，量级也是5万，请大家耐心等待。



## 主要贡献者

1. 杨东杰, djyang.tony@sjtu.edu.cn
2. [袁瑞峰](https://scholar.google.com.hk/citations?user=zPj0R-8AAAAJ&hl=zh-CN), ruifeng.yuan@connect.polyu.hk
3. 范元涛, yuantaofan@bupt.edu.cn
4. 杨逸飞, yifeiyang@sjtu.edu.cn
5. [汪自力](https://commencement.github.io/), ziliwang.do@gmail.com
6. [王树森](http://wangshusen.github.io/)，wssatzju@gmail.com



## 引用

如果你使用我们的方法或数据，请引用本GitHub Repo。

```bibtex
@misc{redgpt,
  author = {Dongjie Yang and Ruifeng Yuan and Yuantao Fan and and Yifei Yang and Zili Wang and Shusen Wang},
  title = {RedGPT: Reference-Enlightened-Dialogue by GPT and for GPT},
  year = {2023},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/ziliwangnlp/RedGPT}},
}
```




