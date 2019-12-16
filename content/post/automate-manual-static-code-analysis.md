---
title: "Automate Manual Static Code Analysis"
tags: [ "Security Testing", "0-day", "SAST", "Python"]
date: 2018-05-08
cover: "/media/CommandMyUnknownLord.gif"
---


#### **Antlr: Automate Manual SAST Activity**

I came across this wonderful which can understand any grammar and can be very helpful for people who do lot of manual source code analysis. This unlike the common grepping allows you to find specifics by programming it in many languages.
Just to showcase the power of tool, I will be using antlr in python to find uninitialized varaibles in java code base.
So before I get started you need to download the latest copy of antlr jar and install python library. Also inorder to feed the grammar to antlr download the .g4 aka grammar definition file for the target language.

```bash
wget http://www.antlr.org/download/antlr-4.7.1-complete.jar
wget https://raw.githubusercontent.com/antlr/grammars-v4/master/java/JavaLexer.g4
wget https://raw.githubusercontent.com/antlr/grammars-v4/master/java/JavaParser.g4
java -jar ./antlr-4.7.1-complete.jar -Dlanguage=Python3 ./JavaLexer.g4
java -jar ./antlr-4.7.1-complete.jar -Dlanguage=Python3 ./JavaParser.g4
pip install antlr4-python3-runtime

```


Here is my simple parser to fetch declared but not uninitialized variables in python using antlr.

```Python
import sys
from antlr4 import *
from JavaLexer import JavaLexer
from JavaParser import JavaParser
from JavaParserListener import JavaParserListener

class enterImportDeclarationPrinter(JavaParserListener):     
    def enterImportDeclaration(self, ctx):       
        print('Found Import')
        print(ctx.getText())


class enterVariableDeclaratorsPrinter(JavaParserListener):
    def enterVariableDeclarators(self,ctx):
        varx = ctx.getText().split(",")
        for newvar in varx:
            if "=" not in newvar:
                print("Variable declared but not initialized: ",newvar)


def main(argv):
    input = FileStream(argv[1])
    lexer = JavaLexer(input)
    stream = CommonTokenStream(lexer)
    parser = JavaParser(stream)
    tree = parser.compilationUnit()
    printer = enterVariableDeclaratorsPrinter()
    walker = ParseTreeWalker()
    walker.walk(printer, tree)



if __name__ == '__main__':
    main(sys.argv)

```


Now it's time to run the script on the source file.

```bash
python3 sastUninit.py Sample.java Variable declared but not initialized:  apple
Variable declared but not initialized:  mango
Variable declared but not initialized:  hehe
Variable declared but not initialized:  hoho
.
.
.

```
