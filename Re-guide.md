Re提供了三种构造函数来书写简单的单元:

``` 
Re('az', fmt='cenum') 	相当于 [az]
Re('az', fmt='crange')	相当于 [a-z]
Re('az', fmt='cseq') 	相当于 'az'
```

第二个参数fmt指定第一个参数的格式，分别是character enumeration, character range,character sequence. 这三种构造方式被包装成三个顶级函数, CEnum(), CRange(), CSeq().

更复杂的表达结构由Re对象携带的方法和程序级别的操作符组合而成。他们分别是:

``` 
repeat(m, n)				相当于  {m, n}
|					相当于  |
~					相当于  [^]
+					相当于  CONCAT
```

这些操作符或方法总是返回一个新的Re对象。

Example 1: 匹配单词
``` 
w =  CRange('AZaz09')|CEnum('_') 
word = w.repeat(1, Re.REPEAT_MAX)
```

Example 2: 匹配非单词
``` 
nonword = (~w).least(1)
```
变量w来自于例子1, 上面的least()方法是repeat()的一个wrapper.

Example 3: 匹配vim临时文件名
``` 
r = CSeq('.') + (~CEnum('/')).least(1) + '.sw' + CRange('az')
```
上面第二个部分，linux文件名允许除'/'以外的所有字符.
第三个部分，裸露的'.sw'，在连接到左侧的Re对象时, 会被自动提升为CSeq('sw')类型。


元字符和锚点在Re里以模板的方式提供，通过全局变量ReT来访问。
元字符:
\w				=>		ReT.WORD, 		ReT.w
\W				=>		ReT.NOT_WORD, 	ReT.W
\s				=>		ReT.SPACE, 		ReT.s
\S				=>		ReT.NOT_SPACE, 	ReT.S
.				=>		ReT.DOT, 		ReT.LANY

锚点
\b				=>		ReT.WORD_BOUNDARY,		ReT.b
\B				=>		ReT.NOT_WORD_BOUNDARY,	ReT.B
^				=>		ReT.LINE_BEGIN
$				=>		ReT.LINE_END

Re还通过第二个模板ReT2，提供一些常用的正则单元，例如 ReT2.INT, ReT2.FLOAT

捕获和反向引用
name(_name)
Ref(name)
Re不支持数字引用，只支持名字引用。
Example
接着Example 3, 下面演示获取vim临时文件中间的文件名部分。
r = CSeq('.') + ((~CEnum('/')).least(1)).name('foo') + '.sw' + CRange('az')
m = r.exec('.abc.txt.swa'')
print(r.group('foo'))
name方法生成一个命名捕获组,
在原有基础上，只需要把第二部分增加调用一个name方法。

反向引用前需要


断言
Assert
AssertNot

程序级别的括号使用
程序级别的括号不会保证出现在正则表达式里，




Example 1:
domain_chars =  (CRange('AZaz09') | CEnum('-')).repeat(1, Re.REPEAT_MAX)
print(domain_chars.regex)
[0-9a-zA-Z-]+
首先我们得到了域名允许的字符集，他们是26个大写字母，26个小写字母，和破折号。然后我们让它重复至少1次。并用一个名叫domain_chars的python变量。
观察print输出，你会发现操作符|在这里被优化掉了，否则应该是[0-9a-zA-Z]|-。{1,}也被优化成了*。

Example 2:
protocol = CSeq('http') + CSeq('s').repeat(0,1)
suffix = CSeq('com') | 'me' | 'org' | 'cc'
url =  protocol + '://' + domain_chars + suffix
你可能已经猜到，我们在为匹配网址做一些工作。prefix匹配http://或https://,suffix则匹配四种域名后缀.
上面的表达式中出现了'://'这样的裸字符串，裸字符串s从右侧连接到一个Re对象时，会被自动的提升为CSeq(s).

Example 3: 
url = prefix + domain_chars + '.'
prefix + (domain + '.').repeat(0,1).capture() + (domain + '.').capture()

最后，注意括号里的-没有转义，因为当它不在三个中间时是不需要转义的。当我在编写Re库的时候，我会想到我作为新手时面对这些转义是多么无助，我现在知道这种无助是合理的。

Example 2:
http = CSeq('http') + CEnum('s').repeat(0,1)

Example 3:
r = http + "://' + 

others = CSeq('ftp') | 'file' | 
 + ':' + CSeq('www.')






2 相反，正则表达式的原理不应该是它看上去的那么简单。例如回溯，它不应该是一个高阶知识点，而应该是一个基本知识点，否则使用者根本无法理解(.*)abc为什么能匹配到abc，明明.*直接能吃掉全体字符。

操作符 或
CRange('') | CRange('-')		[]

操作符 连接

操作符 补集

重复 repeat
它们看上去乏味，但如果字符的内容不是az呢? 
Re('^-]', fmt='cenum') 相当于 [\^\-\]]
Re('$[(', fmt='cseq') 相当于 \$\[\(

这样就省心多了。如果用上Re提供的三个顶级函数，它们甚至还有一点儿简洁。
CEnum( '^-]' )	vs.		[\^\-\]]
CSeq( '$[(' )	vs.		\$\[\(


m = r1.exec('hello, world!', 'g')

用Re构造一些简单的pattern.
r1 = Re('09azAZ', fmt='crange')
r2 = Re('-', fmt='cenum')
上面展示了Re的第一个功能，利用构造函数，手写一些基本的模式单元。第一个参数是pattern,第二个参数fmt指示了pattern里所采用的语法(正则表达式不是描述pattern的唯一语法)，crange表示character range,cenum表示字符枚举。

现在我们得到了两个Re对象，它们分别描述了两个字符集。
接下来利用

利用被重载过的程序语言运算符和方法组合出更复杂的pattern.

