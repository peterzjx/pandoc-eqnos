`pandoc-eqnos` fork for html5+mathjax equation numbering.
=========================================================

Based on original version 2.5.0. Visit https://github.com/tomduck/pandoc-eqnos for the original `pandoc-eqnos`.

**Note** Experimental version. Only intended for markdown to html5+mathjax usage with no future plan to support other formats. May subject to change.

## Features and Usage
Identical to the original `pandoc-eqnos`. If you need more than html5+mathjax, consider rename this package to coexist with the original `pandoc-eqnos`.

### Native mathjax numbering
Due to the complex alignment issue of multiple labels within a `math-display` equation, the numbering will be based on the native `mathjax` `tag` feature instead of the html-css approach. Turn off the automatic numbering in `mathjax` to avoid conflict.

**Note**: By default `pandoc` changes the multiline environment names to `aligned` or `gathered` when converted from `latex`, which do not support `tag`. Use `align` or `gather` instead.

### Multiple labels within multiline environment
Use `sublabel` attribute for individual sub-labels within one math display environment. Separate the sublabels by comma (`,`) with no space. Each sublabel must also conform with the naming convention by starting with `eq:`.

```
$$\begin{align}
2x - 5y &= 8 \\
3x + 9y &= -12
\end{align}$$ {#eq: sublabel=eq:eq3,eq:eq4}
```
By default, the sublabels will be numbered (see below for the detailed incremental strategy) and can be referenced (i.e. `-@eq:eq3`) but will *not* display any numbering in the equation.

To display the corresponding numbering, the equation body should contain the same number of `\label{..}` as the number of the sublabels. The actual label names do not matter, the numbering will be based on the order of appearance.

#### References
Each sublabel will be referenceble, whose html target points to an empty `<span>` with the sublabel as id. Note that this element is not actually a children element of the `math-display`.

#### Number incremental strategy
If the `id` of the main equation is absent (i.e. to be exactly `#eq:`), the environment is considered anonymous and each sublabel will have its own number, while the main equation will not be referenceable. Otherwise, all sublabels will display a shared major numerical index and each will have a minor alphabetical index. Both the main label and sublabels are referenceable.

#### Tags
If attribute `tag` is set for the environment, the numbering of all the sublabels inside will be skipped although can still be referenced (TODO: the displayed ref link will have incorrect numbering). The numbering of sublabels will not be displayed. To add individual tags for the subequations, avoid using `tag` attribute and use the native `\tag{}` latex command.

## Demo
```
Numbered equation

$$x^2 \label{eq1}$$ {#eq:eq1}

-@eq:eq1

Tagged equation, unnumbered

$$\textrm{tag}$$ {#eq:eq2 tag=EQ2}

-@eq:eq2

Unnumbered equation

$$\begin{align}
2x - 5y &= 8 \\
3x + 9y &= -12\end{align}$$

Anonymous subequation
$$\begin{align}
2x - 5y &= 8 \label{eq3}\\
3x + 9y &= -12 \label{eq4}\end{align}$$ {#eq: sublabel=eq:eq3,eq:eq4}

-@eq:eq3
-@eq:eq4

Named subequation
$$\begin{align}
2x - 5y &= 8 \label{dummy5}\\
3x + 9y &= -12 \label{dummy6}\end{align}$$ {#eq:eq56 sublabel=eq:eq5,eq:eq6}

-@eq:eq56
-@eq:eq5
-@eq:eq6

Anonymous subequation with main tag
$$\begin{align}
2x - 5y &= 8 \label{dummy7}\\
3x + 9y &= -12 \label{dummy8}\end{align}$$ {#eq: sublabel=eq:eq7,eq:eq8 tag=EQ78}

-@eq:eq7
-@eq:eq8

Named subequation with main tag
$$\begin{align}
2x - 5y &= 8 \label{dummy9}\\
3x + 9y &= -12 \label{dummy10}\end{align}$$ {#eq:eq910 sublabel=eq:eq9,eq:eq10 tag=EQ910}

-@eq:eq910
-@eq:eq9
-@eq:eq10

Individual tags
$$
\begin{align}
2x - 5y &= 8 \tag{5x}\\
3x + 9y &= -12 \tag{5y}
\end{align}$$ {#eq:eq11 sublabel=eq:eq11x,eq:eq11y}

-@eq:eq11
-@eq:eq11x
-@eq:eq11y
```
Renders as

![](https://github.com/peterzjx/pandoc-eqnos/blob/master/1.png?raw=true)
