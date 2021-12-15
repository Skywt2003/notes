# $\LaTeX$ 学习笔记

[toc]

## 基础模板

```latex
\documentclass{article}
\usepackage[utf8]{inputenc}

\title{StudyLatex}
\author{SkyWT}
\date{December 2021}

\usepackage{natbib}
\usepackage{graphicx}

\begin{document}

\maketitle

\tableofcontents

\section{Introduction}
There is a theory which states that if ever anyone discovers exactly what the Universe is for and why it is here, it will instantly disappear and be replaced by something even more bizarre and inexplicable.
There is another theory which states that this has already happened.

\begin{figure}[h!]
\centering
\includegraphics[scale=1.7]{universe}
\caption{The Universe}
\label{fig:universe}
\end{figure}

\section{Conclusion}
``I always thought something was fundamentally wrong with the universe'' \citep{adams1995hitchhiker}

\bibliographystyle{plain}
\bibliography{references}
\end{document}
```

## 基础概念

- 控制序列/命令/标记：`\` 开头、第一个非字母结束的内容，不会被显示
  - 大小写敏感
  - 参数：放在紧跟其后的花括号中
  - 可选参数：放在方括号中
- 宏包：一系列控制序列的合集。

## 序文 preamble

在 `\begin {document}` 之前的内容称为序文，不会在文档中显示出来。

### 文档类型

```latex
\documentclass[12pt, letterpaper]{article}
```

- 字体大小，默认为 `10pt`。
- 纸张类型：`letterpaper/a4paper/legalpaper`

### 引入包 usepackage

#### 文档编码

```latex
\usepackage[utf8]{inputenc}
```

#### 常见的包

- 图像：`graphicx`
- 数学公式：`amsmath`

### 元信息

- **标题：** `\title{First document}`
- **作者：** `\author{Hubert Farnsworth}`
- **致谢：** `\thanks{funded by the Overleaf team}`
- **日期：** `\date{February 2014}`
  - ‘可以使用 `\today` 命令，编译时使用当前日期

在正文中使用 `\maketitle` 输出这些信息。

## 正文

### 子标题

- 用星号 `*` 禁止使用自动编号，如 `\section*{title}`

- 按照层级有如下顺序：

  - `\part{title} `（只在 book 中可用）

  - `\chapter{title}`（只在 report 中可用）

  - `\section{title}`

  - `\subsection{title}`

  - `\subsubsection{title}`

  - `\paragraph{title}`

  - `\subparagraph{title}`

- 目录：`\tableofcontents` 命令自动生成目录。

### 摘要

在正文中。以特殊格式显示在文档顶部。

```latex
\begin{abstract}
This is a simple paragraph at the beginning of the
document. A brief introduction about the main subject.
\end{abstract}
```

### 段落

- 另起一段：两段之间要隔开一行，即两个换行符新起一段。
- 段内强制换行：`\\` 或者 `\newline`。需要 `parskip` 包。

#### 文字格式

- **粗体：** `\textbf{TEXT}`
- **斜体：** `\textit{TEXT}`
- **下划线：** `\underline{TEXT}`

### 图像

- 需要引入包 `graphicx`。

- 指定位置：`\graphicspath{ {images/} }` 表示所有图片都存在 `images` 目录下。

- 显示图片：`\includegraphics{universe}` 显示目录下 `universe.png`（或者其他拓展名）的图片。也可以带上拓展名。

- 浮动体：带标签等内容的图片

  ```latex
  \begin{figure}[h]
      \centering
      \includegraphics[width=0.25\textwidth]{mesh}
      \caption{a nice plot}
      \label{fig:mesh1}
  \end{figure}
  ```

  - 方括号中的可选参数是插图的位置，`h/t/b/p` 分别是 `here/top/bottom/float page`。
  - 图片下方的标题：`\caption`
  - 标记图片供后文引用：`\label{fig:mesh1}`
  - 在后文中引用：`\ref{fig:mesh1}`

### 列表

#### 无序列表

```latex
\begin{itemize}
  \item Item Text 1
  \item Item Text 2
\end{itemize}
```

#### 有序列表

```latex
\begin{enumerate}
  \item Item Text 1
  \item Item Text 2
\end{enumerate}
```

### 表格

```latex
\begin{center}
\begin{tabular}{ |c|c|c| }
 cell1 & cell2 & cell3 \\
 cell4 & cell5 & cell6 \\
 cell7 & cell8 & cell9
\end{tabular}
\end{center}
```

- `&` 表示分列，`tabular` 参数中 `l/c/r` 分别表示左对齐/居中/右对齐。

- 垂直边框：由 `tabular` 中的 `|c|c|c|` 定义

- 水平边框：行间使用 `\hline` 命令。可以使用多次。

- 带标签等内容的表格

  ```latex
  \begin{table}[h!]
  \centering
  \begin{tabular}{|c c c c|}
   \hline
   Col1 & Col2 & Col2 & Col3 \\ [0.5ex]
   \hline
   1 & 6 & 87837 & 787 \\
   2 & 7 & 78 & 5415 \\
   3 & 545 & 778 & 7507 \\
   4 & 545 & 18744 & 7560 \\
   5 & 88 & 788 & 6344 \\ [1ex]
   \hline
  \end{tabular}
  \caption{Table to test captions and labels}
  \label{table:data}
  \end{table}
  ```

  - 和图像的基本用法一样。

### 数学公式

需要 `amsmath` 包。

#### inline 内联模式

- 界定符：在美元符号 `$` 中，和 Markdown 中一样。
- 其他界定符：`\( ... \)` 或者 `\begin{math} ... \end{math}`。

#### display 陈列模式

- 界定符：`\[ ... \]`，`\begin{displaymath} ... \end{displaymath}` 或 `\begin{equation} ... \end{equation}`
  - 双美元 `$$` 不建议使用

## 版面设置

