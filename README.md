# Micro-Java Compiler

This repository contains a complete compiler for Micro-Java, a simplified subset of the Java language. Developed as a project for a compilers course, it performs lexical, syntax, and semantic analysis, and generates executable bytecode for a custom virtual machine.

## Core Components

The compiler is structured into several distinct phases:

1.  **Lexical Analyzer (Scanner)**: Built using **JFlex**, the scanner reads the source code and converts it into a stream of tokens. The token specification is located in `spec/mjlexer.lex`.

2.  **Syntax Analyzer (Parser)**: Developed with **CUP**, the parser takes the token stream, verifies it against the language's context-free grammar, and builds an Abstract Syntax Tree (AST). The grammar definition, which includes rules for error recovery, can be found in `spec/mjparser_astbuild.cup`.

3.  **Semantic Analyzer**: The `SemanticPass.java` class traverses the AST to perform crucial validation checks, including type consistency, scope resolution (ensuring variables are declared before use), and correct method signatures.

4.  **Bytecode Generator**: Once the AST is semantically validated, the `CodeGenerator.java` visitor traverses it to produce bytecode. This bytecode is compatible with the provided Micro-Java runtime environment (`lib/mj-runtime-1.1.jar`).

## Micro-Java Language Features

The compiler supports a specific subset of Java with some unique extensions:

-   **Data Types**: `int`, `char`, and `bool`.
-   **Declarations**: Global and local variables, constants (`const`), and single-dimensional arrays (`int[]`, `char[]`, etc.).
-   **Methods**: Supports methods with return values and `void` methods. The program's entry point must be a `void main()` method with no parameters.
-   **Statements**:
    -   Assignment (`=`)
    -   Increment (`++`) and Decrement (`--`) on integer variables and array elements.
    -   `read(designator)`: Reads a single `int` or `char` from standard input.
    -   `print(expr)`: Prints the value of an expression to standard output.
    -   `print(expr, width)`: Prints an expression with a specified minimum field width.
    -   `print(array)`: Prints all elements of an array.
-   **Expressions**: Standard arithmetic operators (`+`, `-`, `*`, `/`, `%`).
-   **Array Operations**: Arrays can be instantiated using `new type[size]`.
-   **Custom `range` Function**: A special `range(n)` function is implemented, which returns a new integer array populated with elements from `0` to `n-1`.

## Getting Started

### Prerequisites

-   Java Development Kit (JDK)
-   Apache Ant

### Build and Execution

The project is managed using an Apache Ant `build.xml` file.

1.  **Compile the Compiler:**
    Navigate to the `MJCompiler/` directory and run the following command. This will generate the lexer and parser from the specification files and compile all Java sources.
    ```shell
    ant compile
    ```

2.  **Compile a Micro-Java Program:**
    The main test class `MJParserTest.java` is configured to compile the source file located at `test/test301.mj`. Running this class (e.g., from an IDE or a custom Ant target) will execute the full compilation pipeline and generate the bytecode file `test/program.obj`.

3.  **Run the Bytecode:**
    To execute the compiled Micro-Java program, use the `runObj` Ant target. This target uses the Micro-Java runtime to execute `test/program.obj`. Any required input for the `read()` function should be placed in `test/input.txt`.
    ```shell
    ant runObj
    ```

4.  **Debug the Bytecode:**
    To execute the program in debug mode, which provides a step-by-step view of the virtual machine's state, use the `debug` target:
    ```shell
    ant debug
    ```

## Example Program

The following is an example Micro-Java program from `test/test301.mj` that demonstrates several language features.

```java
// Test301

program test301

	const int nula = 0;
	const int jedan = 1;
	const int pet = 5;

	int niz[], niz2[];
	char nizch[];
	
{
	void main()	
		int bodovi;
		bool bt;
	{
		bodovi = 0;
		bodovi++;
		bodovi = bodovi + jedan;
		bodovi = bodovi * pet;
		bodovi--;
		print(bodovi); 
			
		
		niz = new int[3];
		niz[nula] = jedan;
		niz[1] = 2;
		niz[niz[jedan]] = niz[niz[0]] * 3;
		bodovi = niz[2]/niz[0];
		print(bodovi); 
		print(niz[2]);
		
		
		nizch = new char[3];
		nizch[0] = 'a';
		nizch[jedan] = 'b';
		nizch[pet - 3] = 'c';
		print(nizch[1]);
		print(nizch[jedan * 2]);
			
		
		bodovi = bodovi + (pet * jedan - 1) * bodovi - (3 % 2 + 3 * 2 - 3); 
		print(bodovi);
			
		read(bodovi);
		print(bodovi);

		niz2 = range(bodovi + 1);
		print(niz2);
	}
}
