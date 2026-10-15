# Template: awesome-cv

- **Type:** CV
- **Source extension:** .tex
- **Engine/toolchain:** xelatex
- **Page limit:** 2 pages
- **Fonts:** Roboto, FontAwesome (bundled in fonts/)
- **Class/packages:** awesome-cv.cls, fontawesome.sty, tikz, tcolorbox, geometry, hyperref

## Compile command

    cd <output dir> && rm -f <file>.pdf && mkdir -p build && xelatex -interaction=nonstopmode -output-directory=build <file>.tex && mv build/<file>.pdf ./

## Style rules

- Section headings use `\cvsection{...}` with highlight color
- Main work experience and education use `\begin{cventries} \cventry{<title>}{<company>}{<location>}{<dates>}{\begin{cvitems} \item ... \end{cvitems}} \end{cventries}`
- Skills use `\begin{cvskills} \cvskill{<category>}{<comma-separated skills>} \end{cvskills}`
- Honors/Certifications use `\begin{cvhonors} \cvhonor{<name>}{<issuer>}{<location>}{<year>} \end{cvhonors}`
- Accent color is set via `\colorlet{awesome}{awesome-red}` (also supports: `awesome-emerald`, `awesome-skyblue`, `awesome-orange`, `awesome-nephritis`, `awesome-concrete`, `awesome-darknight`)

## Known pitfalls

- Requires `xelatex` (because of `fontspec` and font file loading via `\fontdir[fonts/]`). Do not compile with `pdflatex`.
- `fonts/` directory must be located in the compile directory or specified in `\fontdir`.
