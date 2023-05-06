# English-Chinese-Dictionary
A English to Chinese Dictionary Integration

整合一份英英/英汉词典资源，包含单词、音标、英英释义、英汉释义、例句等内容。可用于本地词典、背单词软件等的开发。在此记录资源来源及字段含义。

### 0. 参考来源

1. GitHub项目 <a href="https://github.com/skywind3000/ECDICT">ECDICT</a>, 收集了数十万单词的词典数据库，2.4k Star。
2. CSDN <a href="https://u25th-engineer.blog.csdn.net/article/details/105901529?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param">爬取并处理150708个英语单词的例句</a>，博主自行爬取了单词的注音、释义、例句。


### 1. 整理及合并
ECDICT项目内容非常的丰富，后续的资料整合都是以该项目的资料作为基础。该项目最近更新时间已超过一年，其中部分数据字段如detail，audio等后续暂未添加，因此暂时删去。该数据库中的音标经对照基本为英式音标，同时没有例句资源。

<a href="https://u25th-engineer.blog.csdn.net/article/details/105901529?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param">爬取并处理150708个英语单词的例句</a>文章中，博主自行爬取了单词的注音、释义、例句，并发布了.sql文件。该数据库中的音标（含英、美）及释义字段合并为了同一个字段，使用的时候可能有所不便。通过模糊匹配提取出了ECDICT中缺乏的美式音标，并基于单词进行关联。例句资源也同样进行关联。另，为了便于导出为csv等文件格式使用，所有空值使用文本"nullTag"代替。

最终数据库包含以下字段：
|  字段名  | 字段说明  |
|  ----  | ----  |
| wordID  | 单词ID，主键 |
| wordContent  | 单词 |
| phonetic_EN  | 英式英标 |
| phonetic_US  | 美式英标 |
| definition  | 英英释义 |
| translation  | 英汉释义 |
| wordTags  | 单词标记（四六级/雅思等） |
| wordExchanges  | 时态复数等词形变换 |
| bncLevel  | 英国国家语料库词频顺序（1为频率最高） |
| frqLevel  | 当代语料库词频顺序（1为频率最高） |
| collinsLevel  | 柯林斯星级（1-5星，5星为频率最高） |
| oxfordLevel  | 是否牛津三千核心词汇 |
| exampleSentences  | 例句 |

### 2. 字段详情

* wordID
   
  单词ID，主键，1-770611，无空值，无重复值
* wordTags
  
  有效字段条数：14942。14942词包含任一标签，其余为nullTag。属于多个标签则空格分隔。
  
        e.g. abandon
        gk cet4 cet6 
    |  标签明细  | 说明  |
    |  ----  | ----  |
    |  zk  | 中考词汇  |
    |  gk  | 高考词汇  |
    |  ky  | 考研词汇  |
    |  cet4  | 四级词汇  |
    |  cet6  | 六级词汇  |
    |  ielts  | 雅思词汇  |
    |  toefl  | 托福词汇  |
    |  gre  | GRE词汇  |

* wordExchanges

  有效字段条数：96290。96290词包含至少一个词形变化。
  
        e.g. perceive
        d:perceived/p:perceived/3:perceives/i:perceiving
    |类型	|说明
    |  ----  | ----  |
    |p	|过去式（did）
    |d	|过去分词（done）
    |i	|现在分词（doing）
    |3	|第三人称单数（does）
    |r	|形容词比较级（-er）
    |t	|形容词最高级（-est）
    |s	|名词复数形式
    |0	|Lemma（词根），如 perceived 的 Lemma 是 perceive
    |1|	Lemma 的变换形式，比如 s 代表 apples 是其 lemma 的复数形式

* bncLevel

    有效字段条数：45443。British National Corpus，英国国家语料库词频顺序，1-45443，1为使用频率最高的单词(the)。其余单词此字段值为0。

    > 参考资料： 
    > 
    >http://www.kilgarriff.co.uk/bnc-readme.html
    >http://www.natcorp.ox.ac.uk/using/index.xml?ID=freq
    >    
    >百度到最常见的bnc15000词与本数据库提供的有所出入，但是大致的词频趋势是一致的

* frqLevel

    有效字段条数：42231。数据基于Corpus of Contemporary American English (COCA)，当代美国英语语料库，1-42231，1为使用频率最高的单词(the)。其余单词此字段值为0。

    >    参考资料： 
    >
    >    https://www.wordfrequency.info/files/entries.pdf
    >
    >    本数据库中的数据与此来源完全一致

    >    英国国家语料库词频顺序与当代美国英语语料库顺序区别：引自ECDICT源项目
    >
    >BNC 词频统计的是最近几百年的历史各类英文资料，而当代语料库只统计了最近 20 年的，为什么两者都要提供呢？
    >
    >很简单，quay（码头）这个词在当代语料库里拍两万以外，你可能觉得是个没必要掌握的生僻词，而 BNC 里面却排在第 8906 名，基本算是一个高频词，为啥呢？可以想象过去航海还是一个重要的交通工具，所以以往的各类文字资料对这个词提的比较多，你要看懂 19 世纪即以前的各类名著，你会发现 BNC 的词频很管用。
    >
    >而你要阅读各类现代杂志，当代语料库的作用就体现出来了，比如 Taliban（塔利班），在 BNC 词频里基本就没收录（没进前 20 万词汇），而在当代语料库里，它已经冒到 6089 号了，高频中的高频。
    >
    >BNC 较为全面和传统，针对性学习能帮助你阅读各类国外帝王将相的文学名著，当代语料库较为现代和实时，以和科技紧密相关。所以两者搭配，干活不累。


* collinsLevel

    有效字段条数：13633。数据基于Collins COBUILD Advanced Learner's English-Chinese Dictionary，柯林斯词典。分为1-5级，级别5为最常使用，级别1为较少使用。共13633词有级别，其余单词此字段值为0。

    >    参考资料： 
    >
    >    https://inli.neocities.org/Collins%20COBUILD%20English%20Dictionary%20FREQUENT%20WORD%20LIST.pdf
    >
    >    2010版柯林斯星级词汇，本数据库中的数据与此来源不完全一致，但大致相同

* oxfordLevel

    有效字段条数：3461。数据基于Oxford English Corpus，是一份从包含20亿词的牛津英语语料库（Oxford English Corpus）中精选而出的英语学习者必备常用 3000 词表。若为核心词汇则为1，3461词，其余单词此字段值为0。

    >    参考资料： 
    >
    >    https://www.oxfordlearnersdictionaries.com/wordlists/oxford3000-5000

* exampleSentences
  
  有效字段条数：95675。爬取自必应词典，共95675词有例句，每条例句包含中英对照，用`<br>`隔开

        e.g. abandon
        Later, as a result of working reason and the matter that oneself study, abandon for a long time without time.<br>后来，由于工作的原因和自己学习的原因，没有时间放弃很长时间了。<br>Through repeated failures a man learns to persist in the pursuit of worthy goals and to abandon goals that are unworthy of him.<br>通过不断的失败，真正的男人会学会如何坚持追求真正有价值的东西，放弃没有价值的追求。
    
### 3. 提取特定词典

完整数据库导出为csv后270MB+，为了使用方便，也提取几个较小的词典。

|  文件名  | 说明  |
|  ----  | ----  |
|  Complete_Dictionary.7z  | 完整词典的压缩包，解压后约270M  |
|  Common_Words.csv  | 常用词（有单词标记/属于柯林斯星级词/属于牛津核心词/BNC频率前10000/当代美国英语语料频率前10000），共2w条  |
|  CET_Words.csv  | tag包含cet4/cet6的单词 ~~（待上传）~~  |
|  IELTS_Words.csv  | tag包含ielts的单词 ~~（待上传）~~  |
|  Complete_Dictionary_1Sentence.csv  | 全数据集，但仅包含1条例句（待上传）  |
|  Complete_Dictionary_3Sentences.csv  | 全数据集，但仅包含3条例句（待上传）  |
