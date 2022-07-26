# Plang
A Toy programming language named Plang (short for Programming lang) written in C for the purpose of understanding the concepts of language design and implementation. 
The tokenizer scans the tokens of the language and the parser builds the Abstract Syntax Tree. Thereafter, the interpreter recursively walks the AST nodes and performs actions upon them. 

## Quick start
To run a .plang file:
```
$ make
$ ./plang.exe fib.plang
``` 
To start the REPL:
```
$ make
$ ./plang.exe
Welcome to the REPL (Read, Evaluate, Print, Loop) environment
> print 2+2;
( print ( +  2.000000 2.000000 ) )
4.000000
``` 

## Grammar rules
The blocks below define the grammar for Plang.
Terminals are defined between quotes (i.e., "var"). Nonterminals are defined as words starting with an uppercase character.
All capitalized words can be seen as macros. Here, they are left unexpanded to preserve readability. 
Furthermore, a variant of the Baukus-Naur form (BNF) is used for the grammar, which defines the following constructs:
- (*word*)* means 0 or more times *word*
- (*word*)+ means 1 or more times *word*
- (*word*)? means 0 or 1 time *word*
- (*A* | *B*) means choose either *A* or *B*

### Toplevel statements
```ebnf
Program     = (Declaration ";")*
Declaration = VarDecl | 
              Stmt
VarDecl     = "var" Identifier ("=" Expression)?
Stmt        = PrintStmt | 
              Expression |
              IfStmt |
              WhileStmt |
              ForStmt |
              BlockStmt
PrintStmt   = "print" Expression
IfStmt      = "if (" Expression ")" Stmt ("else" Stmt)?
WhileStmt   = "while (" Expression ")" Stmt
ForStmt     = "for (" (VarDecl | Expression | ";") Expression? ";" Expression? ")" Stmt
BlockStmt   = "{" (Declaration ";")* "}"
```

### Expressions
```ebnf
Expression  = Assignment |
              Ternary
Assignment  = Identifier "=" Expression | Logic_or
Logic_or    = Logic_and ("or" Logic_and)*
Logic_and   = Equality ("and" Equality)*
Ternary     = Expression "?" Expression : Expression

Equality    = Comparison (("!=" | "==") Comparison)*
Comparison  = Term (("<" | "<=" | "=>" | ">") Term)*
Term        = Factor (("+" | "-") Factor)*
Factor      = Unary (("*" | "/") Unary)*
Unary       = (("!" | "-") Unary)* |
              Primary
Primary     = Number |
              String |
              Boolean |
              "(" Expression ")" |
              Nil |
              Identifier
```

### Primary types 
```ebnf
Number      = DIGIT+ ("." DIGIT+)?
String      = "\"" (CHAR)* "\""
Boolean     = "true" | "false"
Nil         = "nil" |
Identifier  = ALPHA (ALPHA | DIGIT)*
```

