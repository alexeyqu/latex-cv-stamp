# latex-cv-lastupdated

Auto-stamp your LaTeX CV with a render date and a link to the latest version. One command, zero maintenance.

---

## The problem

Recruiters save your CV PDF and open it weeks or months later — they're reading a stale version without knowing it. You send an updated one, they still open the old attachment.

## The trick

Add a small stamp in the top-right corner of every compiled PDF:

```
Rendered on 13 May 2026
Latest version at lxqu.dev/cv
```

`\today` updates automatically on every compile. The link always points to your current CV. Recruiters who find an old PDF can instantly get the fresh one — even from a printout.

---

## Usage

**Step 1** — add to your preamble:

```latex
\usepackage[absolute]{textpos}
\usepackage{xcolor}
\usepackage{hyperref}

\definecolor{datecolor}{HTML}{666666}

\setlength{\TPHorizModule}{1mm}
\setlength{\TPVertModule}{1mm}
\textblockorigin{0mm}{5mm}
```

**Step 2** — define the command (replace the URL with your own):

```latex
\newcommand{\lastupdated}{%
  \begin{textblock}{60}(150,0)
    \color{datecolor}\fontsize{8pt}{10pt}\selectfont
    Rendered on \today \\
    Latest version at \href{https://yourlink.com}{yourlink.com}
  \end{textblock}%
}
```

**Step 3** — call it at the very start of your document body:

```latex
\begin{document}
\lastupdated
% ... rest of your CV
```

See [`example.tex`](example.tex) for a complete minimal document.

---

## No personal domain? No problem.

Use a URL shortener with a custom alias — [bit.ly](https://bit.ly), [tinyurl.com](https://tinyurl.com), or similar. Point it at your CV file on Google Drive, Dropbox, or any file host. When you upload a new version, update the redirect target. The printed URL never changes.

---

## How it works

The [`textpos`](https://ctan.org/pkg/textpos) package places an absolutely-positioned block at `(150mm, 0mm)` from the page origin, outside the normal text flow. `\today` expands to the compile date at build time. Together they produce a stamp that is always accurate and never interferes with your layout.

Adjust the coordinates to taste — `(150, 0)` works for A4 with standard margins; you may need to tweak for Letter or different margin settings.

