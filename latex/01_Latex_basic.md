# 1. Installation

```bash
# Fedora
sudo dnf install texlive texlive-luatex latexmk

```

# 2. Latex build workflow

## Problem:
The traditional 3-step workflow (`latex -> divps -> ps2pdf`) is PostScript-based, good for EPS and legacy workflows.
Direct PDF output (`pdflatex`) is faster, supports modern formats (PNG, JPG, PDF).

## Bash build script

```bash
#!/bin/bash

filename=$1

if [[ -z "$filename" ]]; then  
echo "Please provide filename to build"  
exit 1  
fi  
  
mkdir -p build  
  
pdflatex -interaction=nonstopmode -output-directory=build "$filename.tex"
bibtex build/"$filename"  
pdflatex -interaction=nonstopmode -output-directory=build "$filename.tex"  
pdflatex -interaction=nonstopmode -output-directory=build "$filename.tex"
```

`latexmk`
```bash
#!/bin/bash  
  
filename=$1  
  
if [[ -z "$filename" ]]; then  
echo "Please provide filename to build"  
exit 1  
fi  
  
mkdir -p build  
rm -rf build/*  
  
latexmk -lualatex \  
-interaction=nonstopmode \  
-synctex=1 \  
-file-line-error \  
-output-directory=build \  
"$filename.tex"
```