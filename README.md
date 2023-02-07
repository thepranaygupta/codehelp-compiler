CodeHelp Compiler
===================

- The library functions as an interface for the compilers on your system.
- It offers APIs to launch child processes and run programs.
- It comes equipped with type support.
- It is compatible with both asynchronous programming using `async/await` and `promises`.

## Supported Languages 

- C
- C++
- Java
- JavaScript(Node.js env)
- Python


## Prerequisites

The following must be installed on your machine and its location must be added to your PATH. 

| Language | Software |
|---------|:-------:|
|C | gcc |
|C++ | gcc |
|Java | jdk |
|Python | python |
|JavaScript(in node.js environment) | node.js |

The source files for programs are stored in a folder named 
`.codehelp-compiler` located in the home directory. Ensure that you have the necessary permissions for this folder.

## Installation

You can install it by using `npm` like below.

```shell
npm install codehelp-compiler --save
```
oe by using `yarn` like below.

```shell
yarn add codehelp-compiler
```

## Usage

It consists of 5 modules, each dedicated to a specific language.

```javascript
const {c, cpp, node, python, java} = require('codehelp-compiler');
```
The modules each have two functions:

## 1. `runFile` 

This function allows you to run a file and requires the file path as an argument. Options and a callback function can be provided as optional arguments.
## 2. `runSource`
This function enables the direct execution of source code stored in a string. The source code must be provided as an argument, with options and a callback function as optional inputs.

## Examples:-

### - Running a C++ source code file

```js
let resultPromise = cpp.runFile('C:\\example.cpp', { stdin:'3\n2 '});
resultPromise
    .then(result => {
        console.log(result); //result object
    })
    .catch(err => {
        console.log(err);
    });
```

### - Running a python source code string

```js
const sourcecode = `print("Hello World!")`;
let resultPromise = python.runSource(sourcecode);
resultPromise
    .then(result => {
        console.log(result);
    })
    .catch(err => {
        console.log(err);
    });
```

### - Using Callbacks

You can pass a callback by including it as:
- an optional argument in case of `runFile`
    ```javascript
    cpp.runFile('C:\\example.cpp', { stdin:'3\n2 '}, (err, result) => {
        if(err){
            console.log(err);
        }
        else{
            console.log(result);
        }
    });
    ```
- without an option
    ```javascript
    cpp.runFile('E:\\abcd.cpp', (err, result) => {
        if(err){
            console.log(err);
        }
        else{
            console.log(result);
        }
    });
    ```

### - Providing path

```javascript
// Providing custom path in java
java.runFile('C:\\Main.java',{
    compilationPath: '<path to javac>',
    executionPath: '<path to java>'
},(err,result)=>console.log(err ? err : result));
```

```javascript
// if you have 2 versions of python installed 2.7 and 3.6, you can use 3.6 using python3
// to run python3 using codehelp-compiler you can do something like
python.runFile('/home/projects/scripts/example.py',{
    executionPath: 'python3'
},(err,result)=>console.log(err ? err : result));
```

```javascript
// if you want to provide custom path of gcc in cpp
cpp.runFile('C:\\example.cpp',{
    compilationPath: '<path to gcc>' // something like C:\\Program Files\\gcc\\bin
},(err,result)=>console.log(err ? err : result));
```

## Result

The **Result** object has the following keys:

1. `exe` \<string> - The path to the executable file generated after successful compilation.
1. `stdout` \<string> - The standard output of the program execution. If the output is empty, an empty string is returned.
1. `stderr` \<string> - The standard error of the program execution, compilation, or if the public class name is not found in the provided source string (in Java). If there is no error, an empty string is returned.
1. `exitCode` \<number> - The exit code of the program.
1. `errorType` \<string|undefined> - If there is an error or a non-zero exit code, this is set to one of the following values:
    1. `'pre-compile-time'` - Only in the case of Java. This can occur if the public class name is invalid when using `runSource` for Java.
    1. `'compile-time'` - If an error occurs during compilation.
    1. `'run-time'` - If an error occurs during runtime.
1. `cpuUsage` \<number> - The CPU time, in microseconds.
1. `memoryUsage` \<number> - The amount of memory consumed, in bytes.
1. `signal` \<string|null> - The signal, if any, resulting from the code execution.


#### Disclaimer: The accuracy of `cpuUsage` and `memoryUsage` is not guaranteed.

## Options

The following is an optional options object that can be provided to the API:

1. `stdin` \<string> - The input or standard input that you want to pass to the program.
2. `timeout` \<number> - The time limit for program execution in milliseconds. The default value is 3000 milliseconds.
3. `compileTimeout` \<number> - The time limit for compilation for C, C++, and Java in milliseconds. The default value is 3000 milliseconds. This will be ignored if passed for Node or Python.
4. `compilationPath` \<string> - The path to the compiler for C, C++, and Java (i.e., GCC and javac, respectively). If provided, the paths defined by you will be used. If not, the default paths will be used.
5. `executionPath` \<string> - The path to the command used to execute the program for Java, Python, and Node.js (i.e., `java`, `python`, and `node`, respectively). If provided, the paths defined by you will be used. If not, the default paths will be used.
