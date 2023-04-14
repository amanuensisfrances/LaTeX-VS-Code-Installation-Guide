# *LaTeX VS Code Installation Guide*
A guide for installing LaTeX on Windows and using Visual Studio Code with the LaTeX Workshop extension as a TeX editor.

## 1. Download and install the following:
  - [`Strawberry Perl`](https://strawberryperl.com/releases.html)
  - [`MikTeX`](https://miktex.org/download) (or [`TeX Live`](https://tug.org/texlive/windows.html))
  - [Visual Studio Code](https://code.visualstudio.com/Download)
  - [Sumatra PDF](https://www.sumatrapdfreader.org/download-free-pdf-viewer) (or [any external PDF viewer that handles live updating](https://superuser.com/q/599442))

## 2. Add the binaries of `Strawberry Perl`, `MikTeX`, and VS Code to Path in your System Environment Variables.

Press `Win+S` and type "Edit the system environment variables", click open or press enter, then go to `Environment Variables -> Path -> New` to add the directories of the binaries where the default should be similar to something as follows:

  `C:\Strawberry\c\bin`

  `C:\Strawberry\perl\site\bin`

  `C:\Strawberry\perl\bin`

  `C:\Program Files\MiKTeX\miktex\bin\x64`

  `C:\Users\<USERPROFILE>\AppData\Local\Programs\Microsoft VS Code\bin`

If you've installed `TeX Live` as your TeX distribution, you can skip this step as the installer takes care of this on Windows.

## 3. Open `MikTeX` (or `TeX Live`), check for updates, update the packages, and install the [`latexmk`](https://ctan.org/pkg/latexmk) package using the package manager.

## 4. Open VS Code, click on `Extensions` or press `Ctrl+Shift+X`, search and install the following extensions:

  [Code Runner by Jun Han](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)

  [LaTeX Workshop by James Yu](https://github.com/James-Yu/LaTeX-Workshop/wiki/Install)

## 5. Open VS Code's `settings.json` and paste the following:
```
// in file "C:\Users\<USERPROFILE>\AppData\Roaming\Code\User\settings.json"

  "latex-workshop.view.pdf.viewer": "external", // open PDF with an external viewer (e.g., SumatraPDF)
  "latex-workshop.view.pdf.external.synctex.command": "C:/Users/<USERPROFILE>/AppData/Local/SumatraPDF/SumatraPDF.exe", // or change this directory to your preferred external PDF viewer
  "latex-workshop.view.pdf.external.synctex.args": [
    "-forward-search",
    "%TEX%",
    "%LINE%",
    "-reuse-instance",
    "-inverse-search",
    "\"C:\\Users\\<USERPROFILE>\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"C:\\Users\\<USERPROFILE>\\AppData\\Local\\Programs\\Microsoft VS Code\\resources\\app\\out\\cli.js\" --ms-enable-electron-run-as-node -r -g \"%f:%l\"",
    "%PDF%"
  ],
  "latex-workshop.synctex.synctexjs.enabled": true, // enable using a built-in synctex function (Ctrl+Alt+J and Ctrl+LeftClick)
  "latex-workshop.synctex.afterBuild.enabled": true, // execute forward synctex at cursor position after compiling LaTeX project
  "latex-workshop.latex.autoBuild.run": "never",
  "latex-workshop.latex.recipe.default": "<FOO>LaTeX Recipe", // replace the placeholder <FOO> with either "pdf", "Xe", or "Lua" to respectively use pdfTeX, XeTeX, or LuaTeX as the TeX engine
  "latex-workshop.latex.recipes": [
    {
      "name": "pdfLaTeX Recipe",
      "tools": [
        "pdflatex",
        "clean-temporary-files" // optional
      ]
    },
    {
      "name": "XeLaTeX Recipe",
      "tools": [
        "xelatex",
        "clean-temporary-files" // optional
      ]
    },
    {
      "name": "LuaLaTeX Recipe",
      "tools": [
        "lualatex",
        "clean-temporary-files" // optional
      ]
    }
  ],
  "latex-workshop.latex.tools": [
    {
      "name": "pdflatex",
      "command": "latexmk",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-pdflatex",
        "-outdir=%DIR%",
        "%DOC_EXT%"
      ]
    },
    {
      "name": "xelatex",
      "command": "latexmk",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-xelatex",
        "-outdir=%DIR%",
        "%DOC_EXT%"
      ]
    },
    {
      "name": "lualatex",
      "command": "latexmk",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-lualatex",
        "-outdir=%DIR%",
        "%DOC_EXT%"
      ]
    },
    {
      "name": "clean-temporary-files",
      "command": "latexmk",
      "args": ["-c"]
    }
  ],
```
Make sure to replace every instance of `<USERPROFILE>` to your user profile and replace `<FOO>` with either `pdf`, `Xe`, or `Lua` depending on your choice of TeX engine and the document being compiled. You can also change the value of `"latex-workshop.view.pdf.external.synctex.command"` to be the directory of your preferred external PDF viewer if you're not using Sumatra PDF.

## 6. Create a LaTeX folder, a LaTeX document, and set VS Code as your default TeX editor
Open `File Explorer` and create a folder for your LaTeX documents, e.g., `D:\<LaTeX-folder>` and create a new text document with the `.tex` filename extension, e.g., `D:\<LaTeX-folder>\<document-name>.tex` (make sure to toggle "File name extensions" in the `View` section at the top of `File Explorer`). Right click on the `<document-name>.tex` and click `Open with -> Choose another app -> Visual Studio Code` and toggle the "Always use this app to open `.tex` files" then click "OK" to set VS Code as your default TeX editor.

## 7. Try to compile a simple LaTeX document similar to something as follows:

```
% in file "D:\<LaTeX-folder>\<document-name>.tex"
  \documentclass[12pt, a4paper]{article}

  \title{Title}
  \author{Author}
  \date{\today}

  \begin{document}
  \maketitle
  Hello, World!
  \end{document}
```

If you've succesfully compiled the LaTeX document, congratulations! You've installed LaTeX using `MikTeX` (or `TeX Live`) as the TeX distribution on Windows and you should be able to use VS Code as a TeX editor with the help of `Strawberry Perl` + `latexmk` + `Code Runner` + `LaTeX Workshop`.

\*The only thing left I would recommend is to install themes (e.g., [`Material Darker`](https://marketplace.visualstudio.com/items?itemName=divyanshu013.vscode-material-darker)), useful keybindings (e.g., [`Emacs Friendly Keymap`](https://marketplace.visualstudio.com/items?itemName=lfs.vscode-emacs-friendly)), and colored indentation extensions (e.g., [`indent-rainbow`](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)).

\*To italicize and bold your markup code, you can also add the following to your VS Code's `settings.json` file:

```
// in file "C:\Users\<USERPROFILE>\AppData\Roaming\Code\User\settings.json"
  "editor.tokenColorCustomizations": {
    "[<VS-CODE-THEME]": {// replace <VS-CODE-THEME> with your preferred VS Code theme 
      "textMateRules": [
      {
        "name": "Markup - Italic",
        "scope": [
          "markup.italic"
        ],
        "settings": {
          "fontStyle": "italic",
        }
      },
      {
        "name": "Markup - Bold-Italic",
        "scope": [
          "markup.bold markup.italic",
          "markup.italic markup.bold",
          "markup.quote markup.bold",
          "markup.bold markup.italic string",
          "markup.italic markup.bold string",
          "markup.quote markup.bold string"
        ],
        "settings": {
          "fontStyle": "bold italic",
        }
      },
      {
        "name": "Markup - Quote",
        "scope": [
          "markup.quote"
        ],
        "settings": {
          "fontStyle": "italic",
        }
      },
      ]
    }
  }
```
