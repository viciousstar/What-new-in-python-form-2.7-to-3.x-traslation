#Python 2.7 的新特性
作者:|A.M. Kuchling (amk at amk.ca)
-----|-------------------------------
这篇文章介绍了在2010年七月三号发布的python2.7的新特性。

数值处理在很多方面得到了提高，包括浮点数和[Decimal](https://docs.python.org/3/library/decimal.html#decimal.Decimal)类。
标准库中还添加了很多有用的改进，包括[unittest](https://docs.python.org/3/library/unittest.html#module-unittest)模块，
用于命令行选择解析的【argparse】模块，在【collections】模块中的方便的【OrderedDict】和【Counter】类，以及很多其他的提升。

Python 2.7是计划发布的最后一个2.x版本，所以我们致力于使他成为一个长期的好的发行版本。
为了与python3对接，2.7也包含了一些3.x版本的新特性。

这篇文章不打算提供完整的新特性信息，但是提供一个方便的概览。
为了得到更多的信息，你应该参考Python2.7的文档 https://docs.python.org/ 
。如果你想了解我们设计和实现的原理，你应该参考特定版本的PEP，这些信息在issue里讨论https://bugs.python.org/ 。
只要可能，Python的新特性就会与bug/patch【存疑】的改变相连。

##Python 2.x 的未来
随着Python维护者将重心转移到位Python 3.x开发新特新，Python 2.7 是最后的主要的2.x系列版本【存疑】。
这意味着虽然python 2 会继续修复bug，支持新硬件和新的操作系统，但是将不会增加完整的语言和标准库方面的新特性。

然而，尽管这里有很多可以直接从自动安全迁移到python3的共同部分，
但是也有很多改变（注意Unicode处理）需要仔细的思考，并且最好通过鲁棒【存疑】回归单元测试进行有效的迁移。

这意味这Python 2.7 仍然会存在很长的一段时间，为没有准备好接入Python 3的产品系统【存疑】提供稳定的受支持的平台。
完整的Python 2.7 系列的生存周期详见【PEP 373】.

2.7作为长期的支持版本有一些重要的后果：
- 如上所说，想起其余的2.x版本，2.7有更长的维护周期。2.7当当前仍旧被核心开发团队所支持（接受安全更新和bug修复）到2020年（在他的初始版本发布十年后，相比其他典型的18-24个月的支持周期【存疑】）
- 随着Python标准库的成长，有效使用Python Package Index变得更重要了，我们增加了大量的第三方包来完成不同的任务，一些可用的包
还包括一些来自Python 3的模块和特性并且已经适配了Python 2，当然还增加了一些工具和库使我们更容易的迁移到Python 3.
【Python Packaging User Guide】提供了从Python Package Index下载安装软件的指导。
- 发布新的包到Python Package Index是一个很好的提升Python 2 的方法【存疑】，但是在某些情况下这是不必要的，特别是与网络安全有关的的时候。这些特殊情况下，不能合适的发布新的或者更新包到PyPi，Python增强提案可能会直接将这些特性加到Python 2 标准库中。所有的这种增加，在维护版本发行的地方，都会在【New Features Added to Python 2.7 Maintenance Releases】下方提及。

对于那些想从Python 2迁移到Python 3或者是想同时支持Python 2或者Python 3的项目，这里有很多工具和指导帮助选择一个合适的方法和管理这些技术细节。建议从【Porting Python 2 Code to Python 3】指导开始。

##处理过时警告的改变
对于Python 2.7， 做出了一个决策，静默那些只有开发者默认感兴趣的警告。【DeprecationWarning】和他的衍生作品开始忽视这种警告除非被要求开启，从而防止用户看到这些呗应用出发的警告。这个改变也在Python 3.2分支中做出（在 stdlib-sig中讨论并且在 【issue 7319】中做出决定。）

在以前的版本中，DeprecationWarning消息是默认开启的，为Python开发者提供一个简单的指示在他们的代码可能在Python未来版本不好用的地方。

*******************************【todo】*******************************

是不相关者担心他们的应用是否好用。

你也可以开启这个选项。

unittest模块会自动开启这个选项。

*******************************【todo】********************************

##Python 3.1 特性（会在3.1中翻译，不在这里重复了，如果想观看，请自行查阅）
*******************************【todo】*******************************

*******************************【todo】********************************

##PEP 372： 添加排序字典到collections
普通的Python字典是以随意的顺序迭代键值对。这些年来，很多作者写了一些可替代的实现来记住键值对插入字典的顺序。在这些基础上，2.7
在【collections】模块中引入了一个新的【OrderedDict】类。

*******************************【todo】*******************************

详细API

*******************************【todo】********************************

OrderedDict是如何工作的？他保留了一个双向链表，当新的键值被插入时将它扩展到链表。同时另一个字典有键到他们链表节点的映射，所以删除不需要移动整个链表，所以他的复杂度仍未O（1）.
下面的几个标准库支持使用了排序字典
- ConfigParser模块默认使用他们，这就意味着配置文件可以读，修改，并且以原来的顺序写会。
- 【collections.namedtuple()】的【_asdict()】方法现在返回一个排序字典和底层的元组目录相同。
- 【json】模块的【JSONDecoder】类构造器扩展了一个 object_paris_hook。 参数来允许排序字典实例被解码器构造。一些第三方工具像【PyYANML】也支持了这些特性。

###See also：
【PEP 372】-添加排序字典到collections

PEP written by Armin Ronacher and Raymond Hettinger; implemented by Raymond Hettinger.
 
*******************************【todo】*******************************
##PEP 378：千位分隔符的格式化说明符【存疑】
##PEP 389：The argparse Module for Parsing Command Lines
##PEP 391：Dictionary-Based Configuration For Logging
*******************************【todo】********************************

##PEP 3106：字典视图
字典方法【keys（），values（），items()】在Python 3.x中是不同的。他们返回一个称作view的对象而不是完全的具体的列表。
在Python 2.7中是不可能改变key（），values（），items（）的返回值的，因为太多的代码会出错。与Python 3中不同的的是有了新的名字，viwekeys(),viewvalues(),viewitems()。
```python
>>> d = dict((i*10, chr(65+i)) for i in range(26))
>>> d
{0: 'A', 130: 'N', 10: 'B', 140: 'O', 20: ..., 250: 'Z'}
>>> d.viewkeys()
dict_keys([0, 130, 10, 140, 20, 150, 30, ..., 250])
```
Views也可以被迭代，但是key和item views的行为类似sets。```&```操作符执行交集，```|```操作执行并集：
```python
>>> d1 = dict((i*10, chr(65+i)) for i in range(26))
>>> d2 = dict((i**.5, i) for i in range(1000))
>>> d1.viewkeys() & d2.viewkeys()
set([0.0, 10.0, 20.0, 30.0])
>>> d1.viewkeys() | range(0, 30)
set([0, 1, 130, 3, 4, 5, 6, ..., 120, 250])
```
view会跟踪字典，当字典的内容改变时它的内容也会改变：
```python
>>> vk = d.viewkeys()
>>> vk
dict_keys([0, 130, 10, ..., 250])
>>> d[260] = '&'
>>> vk
dict_keys([0, 130, 260, 10, ..., 250])
```
然而，你不能在迭代view是增加或者删除keys：
```python
>>> for k in vk:
...     d[k*2] = k
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
RuntimeError: dictionary changed size during iteration
```
你可以使用view methods在Python 2.x中，2to3 转换会将它们变成标准的keys(), values(), items()方法。

###See also
PEP 3106 - Revamping dict.keys(), .values() and .items()
PEP written by Guido van Rossum. Backported to 2.7 by Alexandre Vassalotti; issue 1967.

*******************************【todo】*******************************

##PEP 3137： memoryview 对象

*******************************【todo】********************************

##其他的语言改变
在Python语言核心中其余一些小的改变如下：
- set语法向后接入Python 3.x。大括号被用来产生可变set；set 和字典区分开了通过不包含冒号和值。```{}```继续代表空字典；使用```set()```产生空set。
```python
>>> {1, 2, 3, 4, 5}
set([1, 2, 3, 4, 5])
>>> set() # empty set
set([])
>>> {}    # empty dict
{}
```
Backported by Alexandre Vassalotti; 【issue 2335】.

- 字典和set解析是另一个先后接入Python 3.x的特性，通过在sets和字典上使用列表解析语法生成列表或者生成器。
Backported by Alexandre Vassalotti; 【issue 2333】.

- 【with】语法现在在一条语句中使用多文本管理。文本管理从左到右把每一当作是新的【with】语句处理，这相当于：
```python
with A() as a, B() as b:
    ... suite of statements ...
```
等同于：
```python
with A() as a:
    with B() as b:
        ... suite of statements ...
```
contextlib.nested()函数提供同样的功能，所以这不再是不要的，而且是不提倡的了。
(Proposed in https://codereview.appspot.com/53094; implemented by Georg Brandl.)


*******************************【todo】*******************************

need to do more

*******************************【todo】********************************

##优化
一些性能提升加了进来：
- 增加了一个新的操作符用来执行with语句初始化操作，查看 【__enter__()】 和 【__exit__()】方法。（Contributed by Benjamin Peterson.）

- 在一个常见使用模式下垃圾收集器表现的更好：当很多对象被申请而没有释放时。在以前会在垃圾收集上花费二次时间，但是现在当对象数量在堆上增长的时候全部垃圾收集的数量减少了【存疑】。新的逻辑只有在中代被收集了十次并且剩余的对象数量超过最老一代的10%以上才会执行完全的垃圾收集。
【此段不太懂，什么是完全的垃圾收集，是在分代收集之下的吗】

- 垃圾收集尽量避免追踪不能形成循环引用的简单容器。在Python 2.7中，这在地点和元组仅仅包含原子类型（例如整数，字符串等）。如果一个字典包含的都是仅仅包含原子类型的元组，也不会被跟踪。这减少了垃圾收集的额代价通过这种减少是收集器检查和跟踪的对象数量。 (Contributed by Antoine Pitrou; issue 4688.)

- 长整型现在在内部可以以2**15或者2**30为基数存储，基数在创建的时候确定。以前总是以2**15为基数存储。使用2**30为基数存储能在64为机器上带来明显的性能提升，但是这个结果在32为机器上是相反的【存疑】。因此，默认在64位机器上使用2**30，在32位机器上使用2**15为基数存储。在Unix，有一个新的配置选项 --enable-big-digits 可以覆盖默认配置。

Apart from the performance improvements this change should be invisible to end users, with one exception: for testing and debugging purposes there’s a new structseq sys.long_info that provides information about the internal format, giving the number of bits per digit and the size in bytes of the C type used to store each digit:

```python
>>> import sys
>>> sys.long_info
sys.long_info(bits_per_digit=30, sizeof_digit=4)
```
(Contributed by Mark Dickinson; issue 4258.)
另一些改变使得长整数更少字节：在32位操作系统上少两字节，在64位操作系统上少6字节(Contributed by Mark Dickinson; issue 5260.)

- 长整数除法通过收紧内部循环变得更快，使用移位来代替乘法，修正了迭代。各种不同的测试显示在长整数除法和取模运算中提升了50%到150%的性能提升 (Contributed by Mark Dickinson; issue 5512.)。按位操作也显著变快了 (initial patch by Gregory Smith; issue 1087418)。

- ```%```为左值应该是字符串的检查实现和特例他【存疑】。这提升了那些经常使用%的应用的额-3%的性能，例如 templating库。(Implemented by Collin Winter; issue 5176.)

- List comprehensions with an if condition are compiled into faster bytecode. (Patch by Antoine Pitrou, back-ported to 2.7 by Jeffrey Yasskin; issue 4715.)

- Converting an integer or long integer to a decimal string was made faster by special-casing base 10 instead of using a generalized conversion function that supports arbitrary bases. (Patch by Gawain Bolton; issue 6713.)

- The split(), replace(), rindex(), rpartition(), and rsplit() methods of string-like types (strings, Unicode strings, and bytearray objects) now use a fast reverse-search algorithm instead of a character-by-character scan. This is sometimes faster by a factor of 10. (Added by Florent Xicluna; issue 7462 and issue 7622.)

- The pickle and cPickle modules now automatically intern the strings used for attribute names, reducing memory usage of the objects resulting from unpickling. (Contributed by Jake McGuire; issue 5084.)

- The cPickle module now special-cases dictionaries, nearly halving the time required to pickle them. (Contributed by Collin Winter; issue 5670.)

*******************************【todo】*******************************

more need to do

*******************************【todo】********************************
