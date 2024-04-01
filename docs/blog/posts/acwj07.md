---
draft: true
date: 2024-03-28
categories:
  - C
  - Compiler
authors:
  - whx
tags:
  - C
  - Compiler
---

# acwj Part 7 : 比较操作符

我接下来要添加 IF 语句，但后来我意识到我会最好先添加一些比较运算符。事实证明，这是相当简单，因为它们和现有运算符一样的二进制运算符。

因此，让我们快速看看添加六个比较 `==`, `!=`, `<`, `>`, `<=` and `>=`。

<!-- more -->

## 添加新的 Token 类型

我们将有六个新的 Token ，所以让我们将它们添加到 `defs.h`：

```c title="def.c" linenums="1" hl_lines="6 7"
// Token types
enum {
  T_EOF,
  T_PLUS, T_MINUS,
  T_STAR, T_SLASH,
  T_EQ, T_NE,
  T_LT, T_GT, T_LE, T_GE,
  T_INTLIT, T_SEMI, T_ASSIGN, T_IDENT,
  // Keywords
  T_PRINT, T_INT
};
```

我重新排列了 token 的顺序，以便具有优先级的 token 出现在从低到高的优先级顺序，在没有任何 token 之前优先。

## x86-64 汇编代码生成

在 x86 汇编中，`sete`, `setne`, `setg`, `setl`, `setge`, `setle` 都是 ==条件设置指令== ，它们会根据前一个比较或测试指令的结果来设置目标寄存器的值。这些指令通常在比较或测试指令之后使用。

1. `sete`（Set if Equal）：如果前一个比较或测试指令的结果是相等（即零标志 ZF 设置为 1），则将目标寄存器的值设置为 1，否则设置为 0。

2. `setne`（Set if Not Equal）：如果前一个比较或测试指令的结果是不相等（即零标志 ZF 设置为 0），则将目标寄存器的值设置为 1，否则设置为 0。

3. `setg`（Set if Greater）：如果前一个比较或测试指令的结果是大于（即零标志 ZF 设置为 0，并且符号标志 SF 等于溢出标志 OF），则将目标寄存器的值设置为 1，否则设置为 0。

4. `setl`（Set if Less）：如果前一个比较或测试指令的结果是小于（即符号标志 SF 不等于溢出标志 OF），则将目标寄存器的值设置为 1，否则设置为 0。

5. `setge`（Set if Greater or Equal）：如果前一个比较或测试指令的结果是大于或等于（即符号标志 SF 等于溢出标志 OF），则将目标寄存器的值设置为 1，否则设置为 0。

6. `setle`（Set if Less or Equal）：如果前一个比较或测试指令的结果是小于或等于（即零标志 ZF 设置为 1，或者符号标志 SF 不等于溢出标志 OF），则将目标寄存器的值设置为 1，否则设置为 0。

这些指令通常用于条件分支和循环控制结构中。

## x86-64 寄存器

## 付诸行动（进行测试）

> Premature optimization is the root of all evil (or at least most of it) in programming. --Donald Knuth
> 在编程过程中，过早优化是万恶之源（至少大部分是）。

