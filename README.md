# latex-cv-stamp

Auto-stamp your LaTeX CV with a render date and a link to the latest version. One command, zero maintenance.

<img width="987" height="195" alt="CV header showing 'Rendered on 13 May 2026 · Latest version at lxqu.dev/cv' stamp in the top-right corner" src="https://github.com/user-attachments/assets/f333e068-943e-4bc5-ba29-d806ed6fdc6e" />

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

All three packages used below (`textpos`, `xcolor`, `hyperref`) are included in any standard TeX distribution (TeX Live, MiKTeX) — no extra installation needed.

**Step 1** — add to your preamble:

```latex
\usepackage[absolute]{textpos}
\usepackage{xcolor}
\usepackage{hyperref}

\definecolor{datecolor}{HTML}{666666}

\setlength{\TPHorizModule}{1mm}  % textblock coordinates are in mm
\setlength{\TPVertModule}{1mm}
\textblockorigin{0mm}{5mm}       % shift origin 5mm from the top of the page
```

**Step 2** — define the command (replace the URL with your own):

```latex
\newcommand{\lastupdated}{%
  \begin{textblock}{60}(150,0)   % x=150mm from left, y=0mm from origin
    \color{datecolor}\fontsize{8pt}{10pt}\selectfont
    Rendered on \today \\
    Latest version at \href{https://yourlink.com}{yourlink.com}
  \end{textblock}%
}
```

> **No personal domain?** Use a URL shortener with a custom alias — [bit.ly](https://bit.ly), [tinyurl.com](https://tinyurl.com), or similar. Point it at your CV on Google Drive, Dropbox, or any file host. When you upload a new version, update the redirect target. The printed URL never changes.

**Step 3** — call it at the very start of your document body:

```latex
\begin{document}
\lastupdated
% ... rest of your CV
```

See [`example.tex`](example.tex) for a complete minimal document.

---

## How it works

The [`textpos`](https://ctan.org/pkg/textpos) package places an absolutely-positioned block outside the normal text flow. The coordinates `(150, 0)` are in mm (set by the `\TPHorizModule`/`\TPVertModule` lengths), measured from the `\textblockorigin`. `\today` expands to the compile date at build time, so the stamp is always accurate without any manual updates.

`(150, 0)` works for A4 with standard margins; tweak the x-coordinate for Letter or different margin settings. Adjust `\textblockorigin` to control the vertical offset from the top of the page.

> **Note:** `\today` format depends on your engine and locale (e.g. "May 13, 2026" vs "13 May 2026"). Use the [`datetime2`](https://ctan.org/pkg/datetime2) package if you need a consistent format across engines.
