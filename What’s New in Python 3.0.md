#Python 3.0 的新特性
作者:|Guido van Rossum
-----|-------------------------------
这篇文章介绍了相比Python2.6，Python 3.0的新特性。 3.0，也被称为 Python 3000 或者 Py3K，是第一个故意不向前兼容的Python发行版。这里有很多与以前经典版本不同的改变，并且对所有的Python用户都很重要。无论如何，消化掉这些改变之后，你会发现Python并不是所有的都改变了，我们修正了很多众所周知的烦人的部分和不讨人喜欢的旧东西。

这篇文章不打算提供完整的新特性信息，但是提供一个方便的概览。为了得到更多的信息，你应该参考Python3.0的文档，或者更多的文中指向的PEPs。如果你想完整的了解某一个版本的实现设计原理，PEPs通常比常规文档能提供跟多的细节。但是PEPs通常不会保持更新一旦一个特性被完全实现之后。

因为这篇文章的时间限制，他没有很完善。通常对于一个新版本来说，在源文件中的Misc/NEWs文件会提供更详细完善的信息，包含了每一个细小的变化。

##公共障碍部分

这部分列出了如果你过去使用Python2.5最能妨碍你转到Python3的部分。

###Print 是一个函数了

print语句被【print（）】函数替代了，用参数替代了最特殊的老print语法（【PEP 3105】）。例如：
```python
Old: print "The answer is", 2*2
New: print("The answer is", 2*2)

Old: print x,           # Trailing comma suppresses newline
New: print(x, end=" ")  # Appends a space instead of a newline

Old: print              # Prints a newline
New: print()            # You must call the function!

Old: print >>sys.stderr, "fatal error"
New: print("fatal error", file=sys.stderr)

Old: print (x, y)       # prints repr((x, y))
New: print((x, y))      # Not the same as print(x, y)!
```
你也可以自定义各项之间的额分隔符，比如：
```python
print("There are <", 2**32, "> possibilities!", sep="")
```
将会显示出：
```
There are <4294967296> possibilities!
```
注意：

【print（）】函数不支持pirnt语句的“softspace”特性。举例来说，在python 2.x中，print "A\n", "B"会显示"A\nB\n"但是在Python 3.0中，print "A\n", "B"会显示"A\n B\n"。
刚开始，你会发现你总是会输入print x。要花费一些时间来习惯键入print（x）。
使用 2to3 工具可以自动转换。【存疑】

##试图和迭代器代替列表

一些熟悉的APIs将不再返回列表：
- dict methods dict.keys(), dict.items() and dict.values() return “views” instead of lists. For example, this no longer works: k = d.keys(); k.sort(). Use k = sorted(d) instead (this works in Python 2.5 too and is just as efficient).
- Also, the dict.iterkeys(), dict.iteritems() and dict.itervalues() methods are no longer supported.
- map()和filter（）返回迭代器，如果你真的需要一个列表并且输入列表都是等长的，一个很好的解决方式是用list（）包装map（）比如 list（map（）），但是一个更好的方案是使用列表解析（特别是原来的代码使用了lambda表达式），或者重写代码以便不再需要列表。一个特别的忠告是map（）长深了函数副作用，正确的代替是使用常规的for循环。（因为创建的一个列表会被浪费）。
如果输入序列不是等长的，map（）将会停止在最短的序列上。为了完整兼容Python 2.x，也可以用【itertools.zip_longest()】包装序列，比如map（func， *sequences）来构成list(map(func, itertools.zip_longest(*sequences))).

- range（）现在和xrange（）有一样的行为了，除了他和任意值一块工作，xrange（）不存在。

- zip（）现在也返回迭代器。

##排序比较
Python 3.0简化了排序比较规则：
- 当使用（<, <=, >=, >）这些比较符号是如果比较对象没有一个有意义的自然顺序就会抛出一个类型异常。因此，像 1 < '', 0 > None或者len <= len这种表达式是没有效的。None < None 也会抛出类型错误而不是返回False。一个显然的推论是排序一个多类型的列表将不再有意义，只有所有的元素都可以互相比较才能进行排序。。注意这没有应用到==和！=操作符上：不可比较对象总是可以比较是否相等。

- builtin.sorted()和list.sort()不在接受提供比较函数的cmp参数。使用key参数代替。注意，key和reverse参数现在是“keyword-only”。【存疑】

- cmp函数是过去的事情了，__cmp__()方法不在收到支持，使用__it__()来排序，__eq__() __hash__()【存疑】，其余的富比较也被需要【存疑】。（如果你真的需要cmp（）函数，你应该使用(a > b) - (a < b)来代替cmp（a, b)。）

##整数
- 【PEP 0237】：本质上，long被重命名为int。这样，只有一种内置整数类型，为int；但是他的行为大部分和long类型相似。

- 【PEP 0238】：像 1/2这种表达式返回一个浮点。使用1//2会得到截断行为。（后者的语法已经存在很多年了，至少从Python 2.2.）

- repr（）函数在long上不在有后缀L，所以无条件的strips那个字符将会删除最后一个数字。（用 str（）代替）。【不懂】

- 八进制数字不在是0702这种格式；而是0o720代替

##Text Vs. Data Instead Of Unicode Vs. 8-bit【存疑】

所有你了解的二进制数据和Unicode字符都已经改变了。

- Python 3.0使用了text和data的概念代替了Unicode和8-bit字符串。所有的text都是Unicode；然而编码过的Unicode被二进制数据代替。str类型保存文本，bytes类型保存二进制数据。最大的不同是人在在2.x中试图混淆text和data的行为都会在3.0中引发TypeError，在Python2.x中如果你用八位的string包含7-bit的字节，你的程序会正常工作，但是你将得到UnicodeDecoderError如果你包含了非ASCII值。这些行为给人们带来了很多不便。

- 理论上，作为这种改变的后果，几乎所有使用Unicode，编码和二进制数据的程序都需要改变。这些改变是好的，因为在Python2.x中编码和解码问题带来了大量的bug。在Python2.x中你需要的做的准备是，在所有未编码的文本上使用unicode，为所有编码的二进制数据使用str。这样2to3工具会为你做最大的工作。

- 你不能再使用u''语法为Unicode字符串。然而，你可以使用b''语法为二进制数据。

- 因为str和bytes不能再混淆，你必须明确的在他们之间转换。使用str.encode()来将str转换成bytes，使用bytes.decode()来bytes转变成str。你也可以使用bytes(s, encoding=...) 和 str(b, encoding=...)来代替。

- bytes想str一样是不可以修改的。这里有一个分离的可修改的缓冲二进制数据类型，bytearray。几乎所以bytes的API仍旧使用bytearray。可修改的API以collections.MutableSequence为基础。

- All backslashes in raw string literals are interpreted literally. This means that '\U' and '\u' escapes in raw strings are not treated specially. For example, r'\u20ac' is a string of 6 characters in Python 3.0, whereas in 2.6, ur'\u20ac' was the single “euro” character. (Of course, this change only affects raw string literals; the euro character is '\u20ac' in Python 3.0.)

- The built-in basestring abstract type was removed. Use str instead. The str and bytes types don’t have functionality enough in common to warrant a shared base class. The 2to3 tool (see below) replaces every occurrence of basestring with str.

- 打开文本文件通常使用一种编码将内存中的字符串和硬盘上的字节对应起来。二进制文件（用b模式打开）总是使用字节。这意味着如果一个文件不是用正确的编码打开，I/O将会报错而不是简单的显示错误的数据。这意味着Unix用户甚至需要自己置顶打开文件的编码。当有一些平台独立默认编码时候，在Unixy平台上可以设置LANG环境变量（有时也是一些其他的特殊平台本地相关的环境变量。）在很多情况下，但不是所有，默认的编码是UTF-8；你从来都不应嘎以来这种默认。所有的需要读写的应用如果需要写入非ADCII字符就应该有方法来覆盖这种编码设置。所以我们不再需要使用编码流codecs模块。

- The initial values of sys.stdin, sys.stdout and sys.stderr are now unicode-only text files (i.e., they are instances of io.TextIOBase). To read and write bytes data with these streams, you need to use their io.TextIOBase.buffer attribute.

- Filenames are passed to and returned from APIs as (Unicode) strings. This can present platform-specific problems because on some platforms filenames are arbitrary byte strings. (On the other hand, on Windows filenames are natively stored as Unicode.) As a work-around, most APIs (e.g. open() and many functions in the os module) that take filenames accept bytes objects as well as strings, and a few APIs have a way to ask for a bytes return value. Thus, os.listdir() returns a list of bytes instances if the argument is a bytes instance, and os.getcwdb() returns the current working directory as a bytes instance. Note that when os.listdir() returns a list of strings, filenames that cannot be decoded properly are omitted rather than raising UnicodeError.

- Some system APIs like os.environ and sys.argv can also present problems when the bytes made available by the system is not interpretable using the default encoding. Setting the LANG variable and rerunning the program is probably the best approach.

- PEP 3138: The repr() of a string no longer escapes non-ASCII characters. It still escapes control characters and code points with non-printable status in the Unicode standard, however.
- PEP 3120: The default source encoding is now UTF-8.
- PEP 3131: Non-ASCII letters are now allowed in identifiers. (However, the standard library remains ASCII-only with the exception of contributor names in comments.)
- The StringIO and cStringIO modules are gone. Instead, import the io module and use io.StringIO or io.BytesIO for text and data respectively.
- See also the Unicode HOWTO, which was updated for Python 3.0.
