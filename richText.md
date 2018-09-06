# richText

富文本格式（Rich Text Format）即RTF格式，又称多文本格式，是由微软公司开发的跨平台文档格式。大多数的文字处理软件都能读取和保存RTF文档。

### RTF文档示例

    {\rtf1
    Hello!\par
    This is some {\b bold} text.\par
    }

在文字处理软件中将显示为如下效果：

    Hello!
    This is some bold text.

反斜线符号`（\）`标志着RTF控制代码开始。代码`\par`表示开始新的一行，代码`\b`将文本以粗体显示。花括号`{`和`}`定义一个群组。上述例子中使用了一个群组来限制代码`\b`的作用范围。合法的RTF文档是一个以代码`\rtf`开始的群组。