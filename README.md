
推荐一个文本解析开源工具：Superpower，方便我们解析文本，比如解析日志文件、构建自己的编程语言还是其他需要精确解析和错误报告的场景。


# 01 项目简介


Superpower 的核心功能是将字符序列作为输入，并生成一个数据结构，以便程序更容易分析、操作或转换。这可以是简单的数字、数据格式中的字段列表，或者是某种编程语言的抽象语法树。


Superpower 允许以声明式风格编写解析器，**并在遇到无效输入时提供精确和信息丰富的错误报告。**


Superpower 在构建时特别注重性能。通过减少回溯、避免分配和间接调度，从而用于极高的性能。


# 02 使用方法


**1、安装依赖**



```
dotnet add package Superpower

```

**2、解析连续大写 'A' 字符的简单文本解析器**



```
var parseA = Character.EqualTo('A').AtLeastOnce();

```

**3、构建复杂的解析器**



```
//解析器：由一个字母开头，后面可以跟任意数量的字母、数字或下划线
TextParser<string> identifier =
    // 使用LINQ查询表达式来构建解析器
    from first in Character.Letter  // 第一个字符必须是字母。
    // 后续字符可以是字母、数字或下划线，且可以出现多次（Many()表示0次或多次）。
    from rest in Character.LetterOrDigit.Or(Character.EqualTo('_')).Many()
    // 将第一个字符和后续字符组合成一个字符串。
    select first + new string(rest);

// 使用上面定义的identifier解析器来解析字符串"abc123"。
var id = identifier.Parse("abc123");

//验证解析结果是否与预期值"abc123"相等
Assert.Equal("abc123", id);

```

**4、除了逐个字符使用输入字符的文本解析器外，Superpower还支持令牌解析器。**



```
// 一个简单的算术表达式："1 * (2 + 3)"。
var expression = "1 * (2 + 3)";

// 1. 使用一个算术表达式分词器（ArithmeticExpressionTokenizer）来分词。

var tokenizer = new ArithmeticExpressionTokenizer();
var tokenList = tokenizer.Tokenize(expression); // 分词后，tokenList将包含表达式中的各个token。

// 2. 使用一个算术表达式解析器（ArithmeticExpressionParser）来解析分词后的token列表。
var parser = ArithmeticExpressionParser.Lambda; // parser built with combinators
var expressionTree = parser.Parse(tokenList); // 解析后，expressionTree将是一个表示表达式的AST。

// 使用解析结果（即AST）
// Compile方法可能是一个将AST转换为一个可执行函数（或委托）的方法。
// 这个函数接受没有参数并返回表达式的结果。
var eval = expressionTree.Compile();


```

5、遇到无效输入时提供精确和信息丰富的错误报告



```
ArithmeticExpressionParser.Lambda.Parse(
    // 对字符串"1 + * 3"进行分词，得到一个token序列。
    // 这个序列应该包含数字1的token、加号(+)的token、星号(*)的token和数字3的token。
    new ArithmeticExpressionTokenizer().Tokenize("1 + * 3")
);

// 解析器在解析过程中遇到了一个语法错误，并报告了错误信息。
// -> Syntax error (line 1, column 5): unexpected operator `*`, expected expression.

```

# 03 项目示例与应用


Superpower 提供了多个示例，包括 JSON 解析器、ISO\-8601 日期时间解析器等。


具体见：[https://github.com/datalust/superpower/tree/dev/sample](https://github.com)


**另外\*\*\*\*Superpower被用于多个实际项目中，例如：**


**Serilog.Expressions，日志事件解析扩展：**


[https://github.com/serilog/serilog\-expressions](https://github.com)


**seqcli，纯文本日志解析：**


[https://github.com/datalust/seqcli](https://github.com)


**PromQL.Parser，Prometheus查询语言的解析器：**


[https://github.com/djluck/PromQL.Parser](https://github.com)


# 04 项目地址


[https://github.com/datalust/superpower](https://github.com)


**更多开源项目：** [https://github.com/bianchenglequ/NetCodeTop](https://github.com):[westworld加速器](https://xbsj9.com)


\- End \-


推荐阅读


[2个零基础入门框架教程！](https://github.com)


[推荐一个Star超过2K的.Net轻量级的CMS开源项目](https://github.com)


[Pidgin：一个轻量级、快速且灵活的 C\# 解析库](https://github.com)


[Atata：一个基于 Selenium的C\#自动化测试Web框架](https://github.com)


[mongo\-csharp\-driver：MongoDB官方的C\#客户端驱动程序！](https://github.com)


