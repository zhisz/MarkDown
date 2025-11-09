# keil5设置深色主题



[参考教程](https://blog.csdn.net/coollingomg/article/details/127759860)



- ## 通过对global.prop文件进行主题修改

  1. 首先在安装路径中找到global.prop文件

  自己的Keil安装路径→UV4→global.prop文件

   ![3d29d2d59230370d564125595b7edbe0](D:\Downloads\3d29d2d59230370d564125595b7edbe0.png) 



2. 然后用记事本打开该文件，修改其中的代码。

   

![9189ae63cb26a22489ab61a11447fb2d](D:\Downloads\9189ae63cb26a22489ab61a11447fb2d.png)  

在将修改后的代码保存后，重新打开keil就可以看到已经改好了风格



暗色代码如下，直接复制即可

# properties for all file types
indent.automatic=1
virtual.space=0
view.whitespace=0
view.endofline=0
code.page=936
caretline.visible=1
highlight.matchingbraces=1
print.syntax.coloring=1
use.tab.color=1
create.backup.files=0
auto.load.ext.modfiles=0
save.prj.before.dbg=0
save.files.before.dbg=0
function.scanner.project=1
function.scanner.files=1
function.scanner.modules=1

# properties for c/cpp files
syntax.colouring.cpp=1
use.tab.cpp=0
tabsize.cpp=4
line.margin.visible.cpp=1
fold.cpp=1
monospaced.font.cpp=1

# properties for asm files
syntax.colouring.asm=1
use.tab.asm=0
tabsize.asm=4
line.margin.visible.asm=1
monospaced.font.asm=1

# properties for other files
use.tabs=0
tabsize=4
line.margin.visible.txt=0
monospaced.font.txt=1

# setting for code completion and syntax check
cc.autolist=1
cc.highlightsyntax=1
cc.showparameters=1
cc.triggerlist=1
cc.triggernumchars=3
cc.enter.as.fillup=0
cc.usealpha4inactcode=1
cc.alphavalue=50

# autosave for editor files
autosave=0
autosave.interval=5

# vertical edge at right margin
edge.mode=0
edge.column=80


# Specification for text selection and caret line
selection.fore=#000000
selection.back=#005EB3
caret.fore=#FFFFFF
caret.back=#000000

# Color for vertical edge
edge.colour=#66FAFA

# C/C++ Editor files
template.cpp="#define","#define |";"#if","#if |\r\n\r\n#endif";\\
    "#include","#include ";"Header","// Header:\r\n// File Name: |\r\n// Author:\r\n// Date:\r\n";\\
    "continue","continue;";"do","do\r\n{\r\n\t// TODO: enter the block content here\r\n\t\r\n\t|\r\n} while ();\r\n";\\
    "enum","enum |\r\n{\r\n\t\r\n};\r\n";"for","for(|;;)\r\n{\r\n}";\\
    "fpointer_type","typedef int (* |F)();\r\n";"function","void function(|)\r\n{\r\n\r\n}\r\n";\\
    "if","if (|)";"ifelse","if (|)\r\n{\r\n}\r\nelse\r\n{\r\n}";\\
    "struct","struct | \r\n{\r\n\r\n};\r\n";"switch","switch (|)\r\n{\r\n\tcase:\r\n\t\tbreak;\r\n\tcase:\r\n\t\tbreak;\r\n\tdefault:\r\n\t\tbreak;\r\n}";\\
    "void","void | ();\r\n";"while","while (|)\r\n{\r\n}";\\
    
font.monospace.cpp=Fixedsys
font.acpmonofontname.cpp=Fixedsys
font.acppropfontname.cpp=Fixedsys
style.cpp.32=font:Fixedsys,size:14,fore:#9CDCFE,back:#1E1E1E
style.cpp.4=font:Fixedsys,size:14,fore:#4EC9B0,back:#1E1E1E
style.cpp.10=font:Fixedsys,size:14,fore:#DCDCDC,back:#1E1E1E
style.cpp.1=font:Fixedsys,size:14,fore:#57A64A,back:#1E1E1E
style.cpp.2=font:Fixedsys,size:14,fore:#007F00,back:#1E1E1E
style.cpp.5=font:Fixedsys,size:14,fore:#007ACC,back:#1E1E1E
style.cpp.6=font:Fixedsys,size:14,fore:#FF80FF,back:#1E1E1E
style.cpp.11=font:Fixedsys,size:14,fore:#DCDCDC,back:#1E1E1E
style.cpp.9=font:Fixedsys,size:14,fore:#4EC9B0,back:#1E1E1E
style.cpp.7=font:Fixedsys,size:14,fore:#FF80FF,back:#1E1E1E
style.cpp.34=font:Fixedsys,size:14,fore:#500000,back:#007ACC
style.cpp.35=font:Fixedsys,size:14,fore:#FF0000,back:#1E1E1E
style.cpp.16=font:Fixedsys,size:14,fore:#9CDCFE,back:#1E1E1E
style.cpp.12=font:Fixedsys,size:14,fore:#FF80FF,back:#1E1E1E
style.cpp.86=font:Fixedsys,size:14,fore:#696969,back:#FFFFFF


# Asm Editor files
font.monospace.asm=Courier New
style.asm.32=font:Courier New,size:10,fore:#000000,back:#FFFFFF
style.asm.1=font:Courier New,size:10,fore:#616161,back:#FFFFFF
style.asm.2=font:Courier New,size:10,fore:#FF0000,back:#FFFFFF
style.asm.3=font:Courier New,size:10,fore:#7F007F,back:#FFFFFF
style.asm.4=font:Courier New,size:10,fore:#000000,back:#FFFFFF
style.asm.5=font:Courier New,size:10,fore:#000000,back:#FFFFFF
style.asm.6=font:Courier New,size:10,fore:#0000FF,back:#FFFFFF
style.asm.7=font:Courier New,size:10,fore:#0000FF,back:#FFFFFF
style.asm.9=font:Courier New,size:10,fore:#0000FF,back:#FFFFFF
style.asm.10=font:Courier New,size:10,fore:#0000FF,back:#FFFFFF
style.asm.11=font:Courier New,size:10,fore:#007F00,back:#FFFFFF
style.asm.12=font:Courier New,size:10,fore:#7F007F,back:#FFFFFF
style.asm.8=font:Courier New,size:10,fore:#46AA03,back:#FFFFFF


# Editor Text files
font.monospace.txt=Consolas
style.txt.32=font:Verdana,size:10,fore:#000000,back:#FFFFFF