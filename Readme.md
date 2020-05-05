## How to auto replace spaces by non-breaking spaces
- In VScode open `View` > `Replace`
- Fill the fields like this:
    - **Search**: `\b([aiouksvz])_` ❗❗REPLACE `"_"` by SPACE ❗❗
    - **Replace**: `$1~`
    - **Files to include**: `*.tex`

## Latex setup on 

Repository is using `lualatexmk` library **instead of** `latex` / `xetex` / `xelatex`. 

For generating bibiliography is used `biber`.
```latex
\usepackage[backend=biber]{biblatex}
```

### Steps to install awesome workflow with autocomplete & autogeneration with live preview:
1. Install `mactex-no-gui`
    ```sh
    $ brew cask install mactex-no-gui
    ```
2. Update packages (tlmgr is package manager for latex plugins)
    ```sh
    $ sudo tlmgr update --self && sudo tlmgr update --all
    ```

3. Install VS Code extension latex-workshop
4. Paste this config to .settings in VS code (after other's VS code settings)
    ```JSON
    {
        "latex-workshop.latex.recipes": [
            {
                "name": "biber -> lualatexmk",
                "tools": [
                    "biber",
                    "lualatexmk",
                ]
            }
        ],
        "latex-workshop.latex.tools": [
            {
                "name": "lualatexmk",
                "command": "latexmk",
                "args": [
                    "--shell-escape",
                    "-synctex=1",
                    "-interaction=nonstopmode",
                    "-file-line-error",
                    "-lualatex",
                    "-outdir=%OUTDIR%",
                    "%DOC%"
                ],
                "env": {
                    "PATH": "/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/TeX/texbin"
                }
            },
            {
                "name": "xelatex",
                "command": "xelatex",
                "args": [
                    "-synctex=1",
                    "-interaction=nonstopmode",
                    "-file-line-error",
                    "%DOC%"
                ],
                "env": {
                    "PATH": "/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/TeX/texbin"
                }
            },
            {
                "name": "biber",
                "command": "biber",
                "args": [
                    "%DOCFILE%"
                ],
                "env": {
                    "PATH": "/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/TeX/texbin"
                }
            },
        ],        
    }
    ```

5. For the first compile may be needed to run in terminal (`main` is the name of root document e.g. `main.tex`)
    ```sh
    $ xelatext main --shell-escape
    ```

6. This repository uses package `minted`, which requires to be installed at least `python 3.7` and packages `pygments` and `jsx-lexer` (for ES6 syntax). 
    ```sh
    $ brew install python
    $ easy_install pip
    $ pip install Pygments
    $ pip install jsx-lexer
    ```

7. Change something and save the file. A PDF should be generated. If not, run CMD + SHIFT + P and select command `Build with recipe` > `biber lulatexmk`.


### Troubleshooting

Problems with generating bibliography references try running (`main` is the name without ext of main document file)
```sh
$ biber main
```

Settings contains hardcoded variable `PATH` of `.env` file. Maybe in your case you can try removing whole setting of env object, so only thing left would be `"env": {}`

DO IT LIKE THIS FOR ALL COMMANDS (focus on `env` prop)
```JSON
    ...
    {
        "name": "biber",
        "command": "biber",
        "args": [
            "%DOCFILE%"
        ],
        "env": {}
    },

```