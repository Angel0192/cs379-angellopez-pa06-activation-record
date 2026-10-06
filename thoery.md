# Part A:

## Question 1

When the program calls: 

```
call factorial(4)
```

It creates frame 4 (n = 4)

Then we go into the first recursive call where we call factorial(3) and n = 3 (frame 3). Where the dynamic link points to Frame 4

After this call we move onto factoria(2) called from inside factorial(3)

This creates frame 2 (n = 2) whose dynamic link points to Frame 3

The deepest point call factorial(1) whose dynamic link points to Frame 2 creates frame 1 whose dynamic link points to frame 2

The stack looks like:

**Top Frame (Current):** factorial, n = 1, Return Address, Dynamic Link -> Frame(n = 2), Static Link -> Frame(n = 2)

**Middle Frame 2:** factorial, n = 2, Return Address, Dynamic Link -> Frame(n = 3), Static Link -> Frame(n = 3)

**Middle Frame 3:** factorial, n = 3, Return Address, Dynamic Link -> Frame (n = 4), Static Link -> Frame (n = 4)

**Bottom Frame 4:** factorial, n = 4, Return Address, Dynamic Link -> Global, Static Link, Global

## Unwinding Stack

**Step 1 (Base Case Reached):** factorial(1) returns 1 and pops off the stack. 

**Step 2:** factorial(2) computes 2 * 1 = 2, returns 2, and pops off the stack.   

**Step 3:** factorial(3) computes 3 * 2 = 6, returns 6, and pops off the stack.   

**Step 4:** factorial(4) computes 4 * 6 = 24, returns 24, and pops off the stack.   

**Step 5 (Empty Stack):** The initial call returns 24 to the global scope, leaving the call stack completely empty

## Question 2:

```
 func makeAdder(x){
    func add(y){
        return x + y; // References x from the outer scope
    }
    return add
 }

 let my_adder = makeAdder(5);

 // Called later from global scope
 call my_adder(10); 
```

- When add(10) runs, its dynamic link points to the global scope because that's who actively called my_adder

- It's static link points to hte activation record of makeAdder(5) because that is where 'add' was textually defined in the source code, allowing it to rember that 'x=5'

- Dynamic links only track *who* called them at runtime
- Static links track where they were written lexically in the code
- Without static links, nested functions and closures wouldn't be able to access variables from their enclosing parent scopes if they were called from a completely different part of the program

### Question 3

**The Inequility:**


Maximum Depth (D) < $\frac{s}{r}$


**Tree-Walking vs. Machine Code:** In a production compiler or optimizing engine, tail-call optimization (TCO) reuses the current frames instead of pushing new ones because the recursive call is the final operation

**The Interpreter Problem:** A naive tree-walking interpreter runs in Python (or another host language), meaning every USILang function call executes an actual Python function call and builds a real Python stack frame

- Because the interpreter itself is just walking the AST node recursively or pushing records onto a simulated list without native machine-code TCO, it will still exhaust the call stack unless explicit iterative flattening or frame recycling is built in