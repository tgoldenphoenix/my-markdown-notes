# Learn LaTeX

## Jargon

`pdfLaTeX` là latex compiler that compile `.tex` into `pdf`

`luaLaTeX` is a more advanced compiler

## Why LaTeX? & Installing

[Why LaTeX?](https://www.youtube.com/watch?v=9eLjt5Lrocw) Youtube video

[install MacTeX](https://www.tug.org/mactex/mactex-download.html)

MacTeX writes a symbolic link `/Library/TeX/texbin/` which indirectly points to the TeX Live binary directory. Configure your GUI programs to use this link. The GUI programs we supply will automatically configure themselves. The `pdflatex` and `latexmk` are stored in here.

On MacOS and Ubuntu, mactek version is stored in `/usr/local/texlive/2023/bin/`. Mấy cái như `latexmk` chứa ở đây. Còn thư mục `/Library/TeX/` chỉ chứa những symbolic links.

[reddit](https://www.reddit.com/r/LaTeX/comments/wqh08a/how_do_i_add_latex_to_my_path_on_linux/) How do I add latex to my PATH on linux? 

[cách fix](https://www.xm1math.net/texmaker/texmakermac.html) lỗi install texmaker on Mac báo lỗi

## Class, Commands, Options, and Packages

The **default formatting** in LaTeX documents is determined by the `class` used by that document. This default look can be changed and more functionalities can be added by means of a `package`. The class file names have the `.cls` extension, the package file names have the **.sty** extension.

A `standalone` is both a package and a class.

LaTeX environments are used to apply specific typesetting effect(s) to a section of your document’s content. An environment starts with `\begin{name}` and ends with `\end{name}`

class: `scrbook` from `KOMA-Script`, `beamer`, moderncv, article (or scrartcl), and scrreprt.

The scrbook class provides several `options` to customize the format of the book.

- `Commands` in LATEX are denoted by the backslash `\` as the first character before their names. 
- The `argument` is filled inside the curly brackets {} following the command.

### Packages

You can build your own commands. Fortunately, many, many people have built their own commands already and made them available to $\text{\LaTeX}$ users in packages. Packages allow us to use extra commands without having to include tons and tons of code in the preamble of a document.

AMS Math packages; AMS stands for "American Mathematical Society"

[parskip](https://www.overleaf.com/learn/latex/Articles/How_to_change_paragraph_spacing_in_LaTeX) to change paragraph spacing in LaTeX

[blindtext](https://ctan.org/pkg/blindtext?lang=en) tạo lorem ipsum để test

[how to](https://tex.stackexchange.com/questions/10868/accessing-mac-mactex-install-folder) Find out path of a package on your computer

## Structure Hierarchy

Một book chia ra nhiều `chapter`. Mỗi `chapter` có nhiều `section`.

```latex
% <preamble before the main document>
\begin{document}
...
\chapter{The Basic Set-up and Structure of a \LaTeX{} Book}
...
\section{Class, Commands, Options, and Packages}
\paragraph{Class}
For each \LaTeX{} document, we need to specify its \textit{class}. Throughout this book, ...
\end{document}
```

---

Latex auto number chapters. However, we can manually override that by invoking the command `\setcounter{chapter}{<number >}`.

To achieve the effect of Chapter 0 at the very beginning, we use `\setcounter{chapter}{-1}` so that the counter increases by 1 to 0 as intended when Chapter 0 is generated. The same can be done for sections.

---

starred ver-sions like `\chapter*{<chapter_name>}`, `\section*{<section_name>}`, \subsection*{<section_name>}, and so on, neither display nor in-crement the numbering/counters.

---

We can create references to chapters/sections by appending the `\label{<label_name>}` after their declarations, and then use them via `\ref{<label_name>}`.

```latex
\chapter{Installing or Preparing \LaTeX{}}
\label{chap:install}
...

\ref{chap:install}
```

---

The `\par` command signals the end of a paragraph and appends a vertical line spacing afterwards

`\\` or `\newline`: These commands end the current line but do not start a new paragraph. The following text belongs to the same logical paragraph, which means no indentation or extra vertical spacing (controlled by `\parskip`) will be applied. They should be used sparingly, often within specific environments like tabular.

## Managing a multi-file LaTeX projects

[OverLeaf](https://www.overleaf.com/learn/latex/Management_in_a_large_project) Management in a large project: `\input`, `\include`

[OverLeaf](https://www.overleaf.com/learn/latex/Multi-file_LaTeX_projects) Multi-file LaTeX projects: `\subfile`, `\standalone`

the University of Oslo's [tutorial](https://github.com/uio-latex/Introduction-to-LaTeX/blob/master/large-documents.md): 

[blog](https://castel.dev/post/lecture-notes-3/) How I manage my LaTeX lecture notes by Gilles Castel

`\input` for TikZ code

## Formatting of Text and Paragraphs

### Text Attributes

To control the local font size for some places, we can use the size commands.

`\tiny`, `\scriptsize`, `footnotesize`, `\small`, `\large`, etc...

```latex
produces \par
{\small Though she be but little} {\LARGE she is fierce} \\ % scope \scriptsize % switch
taken from Shakespeare’s A Midsummer Night’s Dream
\normalsize % back to default ...
```

The double backslash `\\` sign breaks the current line and starts a new line right below. And again, the curly brackets `{}` limit the effect of commands within their scope.

## PGF/TikZ

[PGF/TikZ](https://en.wikipedia.org/wiki/PGF/TikZ) is a pair of languages for producing vector graphics (e.g., technical illustrations and drawings) from a geometric/algebraic description, with standard features including the drawing of points, lines, arrows, paths, circles, ellipses and polygons. PGF is a lower-level language, while TikZ is a set of higher-level macros that use PGF.

## Syntax

[learnlatex.org](https://www.learnlatex.org/en/)

Another decent option, despite the clickbait title, is [Overleaf’s *Learn LaTeX in 30 minutes](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes).

 As using LaTeX is a bit like programming, it’s often called ‘compiling’ your document, although ‘typesetting’ is more accurate.

 pdflatex is an engine, a program.

 A really useful feature in all modern LaTeX editors is SyncTeX: the ability to click on your source and go straight to your PDF, or back the other way.

[here](https://tex.stackexchange.com/questions/9363/how-does-one-insert-a-backslash-or-a-tilde-into-latex) => How does one insert a backslash or a tilde (~) into LaTeX?

Using the `$$` method to initiate and terminate display-math mode in LaTeX documents is nowadays heavily deprecated [but why?](https://tex.stackexchange.com/questions/40492/what-are-the-differences-between-align-equation-and-displaymath). Instead, use

**Vim & LaTeX**

 I believe Vim needs no introduction. vimtex can be seen as a continuation of LaTeX-Box and is probably the best TeX plugin available for Vim at the moment of speaking. Compilation is handled very smoothly through latexmk. Most popular PDF viewers are supported (including some which by themselves cannot do forward searching).

 In LaTeX you separate paragraphs with one or more blank lines. Multiple spaces are treated as a single space.
 
## Handling errors

LaTeX always creates a log of what it is doing; this is a text file ending in `.log`. You can always see the full error messages there, and if you have a problem, expert LaTeX users will often ask for a copy of your log file.

## Hyperlinks

## Template links

## References

[Overleaf template directory](https://www.overleaf.com/gallery)

University of Oslo LaTeX templates and [guides](https://github.com/uio-latex/Introduction-to-LaTeX)

[Hàn Thế Thành](https://tug.org/interviews/thanh.html) is the creator of and still maintains `pdfTeX`.

[The TeXbook](https://visualmatheditor.equatheque.net/doc/texbook.pdf) by DONALD E. KNUTH

[latex tiếng việt](https://www.learnlatex.org/vi/language-01)