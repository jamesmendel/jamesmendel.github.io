---
layout: projects
categories: project
title: "LaTeX Assignment Templates"
description: "Programmatically generates PDF with cumstomized header and background for better organization."
fontawesome: true
---

When I started taking notes and doing assignments digitally on a tablet with stylus, I wanted an elegant way to keep things organized and pretty.
My note-taking application of choice, Goodnotes for iPad, allows you to import PDFs and draw on them directly. This sounds like a great use of LaTeX!

## More Information
<i class="fas fa-fw fa-code-branch"></i> [[Github Repo](https://github.com/jamesmendel/latex-assignment-template){:target="_blank"}]

## LaTeX Details
The contents of the header and footer are all imported from `details.tex`. **You only need to edit this file if you would like to create your own document.**
The file uses `\newcommand` to create what are essentially variables that can then be accessed in the main file:
```latex
\newcommand{\assignmentNumber}{1}           % Assignment number in sequence -- appears after assignmentType
\newcommand{\dueMonth}{Jan}                 % Month due         eg. Jan
...
```
So, "*Jan*" can be typeset in the document by adding `\dueMonth` at any location.

My favorite background to write on is a grid of subtle dots, so I created them with a simple `for` loop in my `main.tex`:
```latex
\begin{tikzpicture}
    \foreach \xdot in {0,...,34}{
        \foreach \ydot in {0,...,44}{
            \fill[color=dotcolor] (\xdot,\ydot) circle[radius=.85pt];
        } 
    }
\end{tikzpicture}
```
This generates a grid of dots evenly spaced thoughout the background of the page.

I was speaking with my chemistry professor at the time, and he suggested a version with hexagons for use in organic chemistry.
I thought this would be a neat challenge to help learn the `tikzpicture` package, so I went ahead and created the two other files, `hex.tex` and `hex-dots.tex` that create solid hexagons and dots at each vertex respectively. 

## Example
Here is an exmaple of the dots template for an Electromagnetics homework assignement.
![Exmaple of the dots template for an Electromagnetics homework assignement]({{ site.baseurl }}/images/latex-assignment-template.jpg)