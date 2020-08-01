# 本地latex编译环境配置

1）选择vscode为开发环境，下载LaTeX-Workshop插件  

2）快捷键默认ctrl+alt在mac键盘不能使用，应替换为comand+option  

3）LaTeX-Workshop默认使用latexmk编译工具，但是本机mac没有安装，所以从网上下载mactex安装包来得到latexmk、xelatex等工具。安装mactex后，这些工具位于`/Library/TeX/texbin`目录下，无需设置$PATH变量一般都可以找到可执行文件。下载mactex尝试了mactex官网和清华镜像站，但是都太慢，最后在上海交大站下载（https://mirrors.sjtug.sjtu.edu.cn/ctan/systems/mac/mactex/MacTeX.pkg）  

4）latexmk、lualatex、pdflatex等工具无法对中文内容编译，所以需要xelatex编译工具，需要进行配置，具体包括以下两个配置项：  
在latex-workshop.latex.tools中添加xelatex工具，然后在latex-workshop.latex.recipes中注册工具：  
```json
{
    ......
    "latex-workshop.latex.recipes": [
        ......
        // 添加xelatex recipes
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ]
        },
        ......
    ],
    "latex-workshop.latex.tools": [
        
        ......
        // 添加xelate工具
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ],
            "env": {}
        },
        ......
    ]
}
```

配置后重启vscode后就可以在LaTeX-Workshop扩展命令工具集中看到`Recipe:xelatex`，利用这个工具编译就可以成功得到PDF文件。  

5）latex-workshop.latex.recipe.default配置项设置为`lastUsed`  

在编译一次后，通过command+S便可在保存时候反复编译。

6) 字体设置

使用xeCJK包直接指定系统字体，自己安装了思源宋体，通过`fc-list | grep 'Source'`找到自己的字体名称，填入即可。  
```latex
\usepackage{xeCJK}
\setCJKmainfont{Source Han Serif SC Light}
```