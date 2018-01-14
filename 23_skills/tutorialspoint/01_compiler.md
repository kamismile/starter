# compiler design

[TOC]

a compiler translates the code written in one language to some other language without changing the meaning of the program. it is also expected that a compiler shoud make the target code efficient and optimized in terms of time and space.



compiler design covers basic translation mechanism and error detection & recovery. it inclues lexical, syntax, and semantic analysis as front end, and code generation and optimization as back-end.

## overview

> a. user writes a program in c language(high-level language).
>
> b. the c compiler, compiles the program and translates it to assembly program (low-level language)
>
> c. an assembler then translates the assembly program into machine code (object)
>
> d. a linker tool is used to link all the parts of the program together for execution (executable machine code)
>
> e. a loader loads all of them into memory and then the program is executed.

### Preprocessor

generally considered as a part of compiler, is a tool that produces input for compilers. it deals with macro-processing, augmentation, file inclusion, language extension, etc

### Interpreter

like a compiler, translates high-level language into low-level machine language. the difference lies in the way tey read the source code or input. a compiler reads the whole source code at onece, creates tokens, checks sematics, generates intermedite code, executes the whole program and may involve many passes.in contrast, an interpreter reads a statement from the input, converts it to an intermediate code, execute it , then takes the next statement in sequence.if an error occurs, an interpreter stops execution and reports it. whereas a computer reads then whole program even if it encouters serveral errors

### Assembler

an assembler translates assembly language programs into machine code. the output of an assembler is called an object file, which contains a combination of machine instructions as well as the data required to place these instructions in memory.



### linker

linker is a computer program that links and merges various object files together in order to make executable file. all these files might have been compiled by separete assemblers. the major task of a linker is to search and locate referenced module/rutines in a program to determine the memory location where these codes will be loaded, making the program instruction to have absolute references.

### loader

loader is a part of operating system and is responsible for loading executable files into memory and execute them. it calculates  the size of a program (instructions and data) and creates memory space for it. it initializes various registers to initiate execution.

### cross-compiler

a compiler that runs on platform (A) and is capable of generating executable code for platform (B) is called a cross-compiler.

### source-to-source compiler

a compiler that takes the source code of one programing language and translates it into the source code of another programming language is called a source-to-source compiler.

## architecture

a compiler can broadly be divided into two phases based on the way they compile.

#### analysis phase

known as the front-end of the compiler, the ananysis phase of the compiler reads the source program, divides it into core parts and then checks for lexical, grammer and syntax errors. the analysis phase generates an intermediate representation of the source program and symbol table, which should be fed to the synthesis phase as input.

#### Synthesis phase

known as the backend of the compiler, the synthesis phase generates the target program with the help of intermediate source code representation and symbol table.

a compiler can have many phases and passes.

- a pass refers to the traversal of a compiler through the entile program.

- phase: a phase of a compiler is a distuinguaishable stage, which takes input from the previous stage, processes and yields output that can be used as input for the next stage. a pass can have more than one phase.




## phase of compiler

  ![phase of compiler](https://www.tutorialspoint.com/compiler_design/images/compiler_phases.jpg)



### lexical analysis

the first phase of scanner works as a text scanner. this phase scans the source code as a stream of characters and converts it into meaningful lexemes. lexical anayzer represents these lexemes in the form of tokens as 

```xml
<token-name, attribute-value>
```

### syntax analysis

the next phase is called the syntax analysis or parsing. it takes the token produced by lexical analysis as input and genertates a parse tree(or syntax tree). in this phase, token arragements are checked against the source code grammer, i.e. the parser checks if the expression made by the tokens is syntactically correct.

### semantic analysis

semantic analysis checks whether the parse tree constructed follows the rules of language. for example, assignment of values is between cimpatible data types, and adding strings to an integer. also, the semantic analyzer keeps track of identifiers, their types and expressions; whether identifiers are declared before use or not etc. the semantic analyzer produces an annotated syntax tree as output.

### Intermediate code generation

after smantic analysis the compiler generates an  interemediate code of the source code for the target machine. it represents a program for some abstract machine. it is in between the high-level language and the machine language.this intermediate code should be generated in such a way that it makes it easier to be translated into the target machine code.

### code optimization

the next phase does code optimization of the intermediate code. optimization can be assumed as somethiing that removes unneccessary code lines, and arrages the sequence of statement

### symbol table

it is a data-structure mantained throughout all the phases of  a compiler. all the identifier's names along with their types are stored here. the symbol table makes it easier for the compiler to quickly search the identifier record and retrieve it. the symbol table is also used for scope management.

## lexical analysis

the first phaser of a compiler. it takes the modified source code from languages preprocessors that are written in the form of sentences. the lexical analyzer breaks these snytaxes into a series of tokens, by removing any whitespace or comments in the source code.































