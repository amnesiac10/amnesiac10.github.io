12种不宜使用的Javascript语法
http://www.ruanyifeng.com/blog/2010/01/12_javascript_syntax_structures_you_should_not_use.html

Goto 命令

通常认为，在 AutoHotkey 中唯一适用 Goto 命令的情况是在一个函数中存在多个返回点且在返回时必须进行一些清理的情况。类似这样的情况很少见（批处理中则很常见），保留此命令主要是为了兼容 AutoItv2。如果随意使用通常会让脚本结构混乱不容易阅读且难以维护，所以不鼓励使用 Goto。考虑使用 Else、区块、Break 和 Continue 来代替 Goto。
