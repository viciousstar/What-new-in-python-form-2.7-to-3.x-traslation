#Python 2.7 的新特性
作者:|A.M. Kuchling (amk at amk.ca)
------------------------------------
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
