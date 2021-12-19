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

- 控制序列/命令/标记：`\` 开头、第一个非字母结束的内容，不会被显示。
  - 大小写敏感。
  - 参数：放在紧跟其后的花括号中。
  - 可选参数：放在紧跟其后的方括号中。
- 宏包：一系列控制序列的合集。

## 序文 preamble

在 `\begin {document}` 之前的内容称为序文，不会在文档中显示出来。

### 文档类型

```latex
\documentclass[12pt, letterpaper]{article}
```

- 字体大小，默认为 `10pt`。
- 纸张类型：`letterpaper/a4paper/legalpaper`。

### 引入包 usepackage

#### 文档编码

```latex
\usepackage[utf8]{inputenc}
```

#### 常见的包

- 图像：`graphicx`。
- 数学公式：`amsmath`。

### 元信息

- 标题：`\title{First document}`
- 作者：`\author{Hubert Farnsworth}`
- 致谢：`\thanks{funded by the Overleaf team}`
- 日期：`\date{February 2014}`。
  - 可以使用 `\today` 命令，编译时显示为当前日期。

在正文中使用 `\maketitle` 输出这些信息。

## 正文

### 章节/子标题

- 标题前会自动生成编号。
  - 用星号 `*` 禁止使用自动编号，如 `\section*{title}`。

- 按照层级有如下顺序：

  - `\part{title} `（只在 book 中可用）

  - `\chapter{title}`（只在 report 中可用）

  - `\section{title}`

  - `\subsection{title}`

  - `\subsubsection{title}`

  - `\paragraph{title}`

  - `\subparagraph{title}`
- 目录：`\tableofcontents` 命令自动生成目录。

### 文章摘要

在正文中。以特殊格式显示在文档顶部。

```latex
\begin{abstract}
This is a simple paragraph at the beginning of the
document. A brief introduction about the main subject.
\end{abstract}
```

### 正文段落

- 另起一段：两段之间要隔开一行，即两个换行符新起一段。
- 段内强制换行：`\\` 或者 `\newline`。
- 无缩进：`\noindent`。

#### 文字格式

- 粗体：`\textbf{TEXT}`。
- 斜体：`\textit{TEXT}`。
- 下划线：`\underline{TEXT}`。

### 数学公式

需要 `amsmath` 包。

#### inline 内联模式

- 界定符：美元符号 `$` 中，和 Markdown 中一样。
- 其他界定符：`\( ... \)` 或者 `\begin{math} ... \end{math}`。

#### display 陈列模式

- 界定符：`\[ ... \]`，`\begin{displaymath} ... \end{displaymath}` 或 `\begin{equation} ... \end{equation}`。
  - 双美元 `$$` 不建议使用。

### 图片

- 需要引入包 `graphicx`。
- 指定位置：`\graphicspath{ {images/} }` 表示所有图片都存在 `images` 目录下。
- 显示图片：`\includegraphics{universe}` 显示目录下 `universe.png`（或者其他拓展名）的图片。也可以带上拓展名。

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

### 代码高亮

需要使用 `minted` 包。

在自己部署的 Overleaf 中，要先 apt 安装 pygments，再在 `/usr/local/texlive/2021/texmf.cnf` 中添加行 `shell_escape = t` 并重启容器，才能开启代码高亮。

#### 可选参数

- 边框 `frame`：`leftline/topline/bottomline/single/lines`，`lines` 是代码块上下分别加上分隔线。
- 框架分隔 `framesep`。
- 行高 `baselinestretch`。
- 行号显示 `linenos`。

#### 代码段

```latex
\begin{minted}{cpp}
inline int read(){
    return 0;
}
\end{minted}

\begin{minted}[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
linenos
]{cpp}
inline int read(){
    return 0;
}
\end{minted}
```

#### 单行代码

```latex
\mint{html}|<h2>Something <b>here</b></h2>|
```

#### 文件引入代码

```latex
\inputminted{octave}{test.cpp}
```

### 浮动体

#### 图片浮动体

```latex
\begin{figure}[h]
    \centering
    \includegraphics[width=0.25\textwidth]{mesh}
    \caption{a nice plot}
    \label{fig:mesh1}
\end{figure}
```

- 方括号中的可选参数是浮动体的位置，`h/t/b/p` 分别是 `here/top/bottom/float page`。
- 图片下方的标题：`\caption{title}`。
- 标记供后文引用：`\label{fig:mesh1}` 会生成一个编号。
- 在后文中引用：`\ref{fig:mesh1}` 会显示为该图片的编号。

#### 表格浮动体

```latex
\begin{table}[h!]
    \centering
    \begin{tabular}{|c c c c|}
       Col1 & Col2 & Col3 \\
       1 & 6 & 8 \\
       2 & 7 & 7
    \end{tabular}
    \caption{Table to test captions and labels}
    \label{table:data}
\end{table}

\ref{table:data}
```

#### 代码块浮动体

```latex
\begin{listing}[!ht]
	\inputminted{octave}{BitXorMatrix.m}
	\caption{Example from external file}
	\label{listing:code1}
\end{listing}

\ref{listing:code1}
```

