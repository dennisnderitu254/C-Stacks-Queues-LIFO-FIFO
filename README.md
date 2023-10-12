# C-Stacks-Queues-LIFO-FIFO Docs

## Resources

Read or watch:
- [Google](https://intranet.alxswe.com/rltoken/tn1X658KGumYYq_szFJI5w)
- [How do I use extern to share variables between source files in C?](https://intranet.alxswe.com/rltoken/0KVWTdE8xXy__jUfBfakCw)
- [Stacks and Queues in C](https://intranet.alxswe.com/rltoken/udmomL4F4mF630D2Z-ltqg)
- [Stack operations](https://intranet.alxswe.com/rltoken/fj_-SJXW-pWxgAnstsARoQ)
- [Queue operations](https://intranet.alxswe.com/rltoken/6Y_GVoIH_rV45xd7w0a9FA)

## Data structures

Please use the following data structures for this project. Don’t forget to include them in your header file.

```
/**
 * struct stack_s - doubly linked list representation of a stack (or queue)
 * @n: integer
 * @prev: points to the previous element of the stack (or queue)
 * @next: points to the next element of the stack (or queue)
 *
 * Description: doubly linked list node structure
 * for stack, queues, LIFO, FIFO
 */
typedef struct stack_s
{
        int n;
        struct stack_s *prev;
        struct stack_s *next;
} stack_t;
```

```
/**
 * struct instruction_s - opcode and its function
 * @opcode: the opcode
 * @f: function to handle the opcode
 *
 * Description: opcode and its function
 * for stack, queues, LIFO, FIFO
 */
typedef struct instruction_s
{
        char *opcode;
        void (*f)(stack_t **stack, unsigned int line_number);
} instruction_t;
```

## Compilation & Output

Your code will be compiled this way:

```
$ gcc -Wall -Werror -Wextra -pedantic -std=c89 *.c -o monty
```

- Any output must be printed on stdout
- Any error message must be printed on stderr
    - Here is a [link to a GitHub repository](https://intranet.alxswe.com/rltoken/NUGvCZqs609VjEHeutkICw) that could help you making sure your errors are printed on stderr

## The Monty language

Monty 0.98 is a scripting language that is first compiled into Monty byte codes (Just like Python). It relies on a unique stack, with specific instructions to manipulate it. The goal of this project is to create an interpreter for Monty ByteCodes files.

## Monty byte code files

Files containing Monty byte codes usually have the `.m extension`. Most of the industry uses this standard but it is not required by the specification of the language. There is not more than one instruction per line. There can be any number of spaces before or after the opcode and its argument:

```
julien@ubuntu:~/monty$ cat -e bytecodes/000.m
push 0$
push 1$
push 2$
  push 3$
                   pall    $
push 4$
    push 5    $
      push    6        $
pall$
julien@ubuntu:~/monty$
```

Monty byte code files can contain blank lines empty or made of spaces only, and any additional text after the opcode or its required argument is not taken into account:

```
julien@ubuntu:~/monty$ cat -e bytecodes/001.m
push 0 Push 0 onto the stack$
push 1 Push 1 onto the stack$
$
push 2$
  push 3$
                   pall    $
$
$
                           $
push 4$
$
    push 5    $
      push    6        $
$
pall This is the end of our program. Monty is awesome!$
julien@ubuntu:~/monty$
```

The monty program

- Usage: `monty file`
    - where `file` is the path to the file containing Monty byte code
- If the user does not give any file or more than one argument to your program, print the error message USAGE: monty file, followed by a new line, and exit with the status `EXIT_FAILURE`
- If, for any reason, it’s not possible to open the file, print the error message Error: Can't open file `<file>`, followed by a new line, and exit with the status `EXIT_FAILURE`
    - where `<file>` is the name of the file
- If the file contains an invalid instruction, print the error message L`<line_number>`: unknown instruction `<opcode>`, followed by a new line, and exit with the status `EXIT_FAILURE`
    - where is the line number where the instruction appears.
    - Line numbers always start at 1
- The monty program runs the bytecodes line by line and stop if either:
    - it executed properly every line of the file
    - it finds an error in the file
    - an error occured
- If you can’t malloc anymore, print the error message `Error: malloc` failed, followed by a new line, and exit with status EXIT_FAILURE.
- You have to use `malloc` and `free` and are not allowed to use any other function from man malloc (realloc, calloc, …)

## Tasks

#### 0. push, pall

Implement the `push` and `pall` opcodes.

**The push opcode**

The opcode `push` pushes an element to the stack.

- Usage: push <int>
    - where <int> is an integer
- if <int> is not an integer or if there is no argument given to push, print the error message L<line_number>: usage: push integer, followed by a new line, and exit with the status EXIT_FAILURE
    - where is the line number in the file
- You won’t have to deal with overflows. Use the atoi function

**The pall opcode**

The opcode `pall` prints all the values on the stack, starting from the top of the stack.

- Usage `pall`
- Format: see example
- If the stack is empty, don’t print anything

```
julien@ubuntu:~/monty$ cat -e bytecodes/00.m
push 1$
push 2$
push 3$
pall$
julien@ubuntu:~/monty$ ./monty bytecodes/00.m
3
2
1
julien@ubuntu:~/monty$
```

**Repo:**

- GitHub repository: `monty`

#### 1. pint

Implement the `pint` opcode.

**The pint opcode**

The opcode pint prints the value at the top of the stack, followed by a new line.

- Usage: pint
- If the stack is empty, print the error message `L<line_number>: can't pint, stack empty`, followed by a new line, and exit with the status `EXIT_FAILURE`

```
julien@ubuntu:~/monty$ cat bytecodes/06.m
push 1
pint
push 2
pint
push 3
pint
julien@ubuntu:~/monty$ ./monty bytecodes/06.m
1
2
3
julien@ubuntu:~/monty$
```

**Repo:**

- GitHub repository: `monty`

#### 2.pop

Implement the `pop` opcode.

**The pop opcode**

The opcode pop removes the top element of the stack.

- Usage: pop
- If the stack is empty, print the error message `L<line_number>: can't pop an empty stack`, followed by a new line, and exit with the status `EXIT_FAILURE`

```
julien@ubuntu:~/monty$ cat bytecodes/07.m
push 1
push 2
push 3
pall
pop
pall
pop
pall
pop
pall
julien@ubuntu:~/monty$ ./monty bytecodes/07.m
3
2
1
2
1
1
julien@ubuntu:~/monty$
```
**Repo:**

- GitHub repository: `monty`

# 3.swap

Implement the `swap` opcode.

**The swap opcode**

The opcode swap swaps the top two elements of the stack.

- Usage: `swap`
- If the stack contains less than two elements, print the error message `L<line_number>: can't swap, stack too short`, followed by a new line, and exit with the status `EXIT_FAILURE`

```
julien@ubuntu:~/monty$ cat bytecodes/09.m
push 1
push 2
push 3
pall
swap
pall
julien@ubuntu:~/monty$ ./monty bytecodes/09.m
3
2
1
2
3
1
julien@ubuntu:~/monty$
```
**Repo:**

- GitHub repository: `monty`

#### 4.add

Implement the add opcode.

The add opcode

The opcode add adds the top two elements of the stack.

- Usage: add
- If the stack contains less than two elements, print the error message L<line_number>: can't add, stack too short, followed by a new line, and exit with the status EXIT_FAILURE
- The result is stored in the second top element of the stack, and the top element is removed, so that at the end:
    - The top element of the stack contains the result
    - The stack is one element shorter

```
julien@ubuntu:~/monty$ cat bytecodes/12.m
push 1
push 2
push 3
pall
add
pall

julien@ubuntu:~/monty$ ./monty bytecodes/12.m
3
2
1
5
1
julien@ubuntu:~/monty$
```
**Repo:**

- GitHub repository: `monty`


#### 5.nop

Implement the nop opcode.

**The nop opcode**

The opcode nop doesn’t do anything.

- Usage: nop

**Repo:**

- GitHub repository: `monty`

#### 6.sub

Implement the `sub` opcode.

**The sub opcode**

The opcode sub subtracts the top element of the stack from the second top element of the stack.

- Usage: sub
- If the stack contains less than two elements, print the error message L<line_number>: can't sub, stack too short, followed by a new line, and exit with the status EXIT_FAILURE
- The result is stored in the second top element of the stack, and the top element is removed, so that at the end:
    - The top element of the stack contains the result
    - The stack is one element shorter

```
julien@ubuntu:~/monty$ cat bytecodes/19.m
push 1
push 2
push 10
push 3
sub
pall
julien@ubuntu:~/monty$ ./monty bytecodes/19.m
7
2
1
julien@ubuntu:~/monty$
```

**Repo:**

- GitHub repository: `monty`

#### 7.div

Implement the div opcode.

`The div opcode`

The opcode div divides the second top element of the stack by the top element of the stack.

- Usage: div
- If the stack contains less than two elements, print the error message L<line_number>: can't div, stack too short, followed by a new line, and exit with the status EXIT_FAILURE
- The result is stored in the second top element of the stack, and the top element is removed, so that at the end:
    - The top element of the stack contains the result
    - The stack is one element shorter
- If the top element of the stack is 0, print the error message L<line_number>: division by zero, followed by a new line, and exit with the status EXIT_FAILURE

**Repo:**

- GitHub repository: `monty`

#### 8.mul

Implement the mul opcode.

**The mul opcode**

The opcode mul multiplies the second top element of the stack with the top element of the stack.

- Usage: mul
- If the stack contains less than two elements, print the error message L<line_number>: can't mul, stack too short, followed by a new line, and exit with the status EXIT_FAILURE
- The result is stored in the second top element of the stack, and the top element is removed, so that at the end:
    - The top element of the stack contains the result
    - The stack is one element shorter

**Repo:**

- GitHub repository: `monty`

#### 9.mod

Implement the mod opcode.

**The mod opcode**

The opcode mod computes the rest of the division of the second top element of the stack by the top element of the stack.

- Usage: mod
- If the stack contains less than two elements, print the error message L<line_number>: can't mod, stack too short, followed by a new line, and exit with the status EXIT_FAILURE
- The result is stored in the second top element of the stack, and the top element is removed, so that at the end:
    - The top element of the stack contains the result
    - The stack is one element shorter
- If the top element of the stack is 0, print the error message L<line_number>: division by zero, followed by a new line, and exit with the status EXIT_FAILURE

**Repo:**

- GitHub repository: `monty`

#### 10.comments

Every good language comes with the capability of commenting. When the first non-space character of a line is #, treat this line as a comment (don’t do anything).

**Repo:**

- GitHub repository: `monty`

#### 11.pchar

Implement the pchar opcode.

**The pchar opcode**

The opcode pchar prints the char at the top of the stack, followed by a new line.

- Usage: pchar
- The integer stored at the top of the stack is treated as the ascii value of the character to be printed
- If the value is not in the ascii table (man ascii) print the error message L<line_number>: can't pchar, value out of range, followed by a new line, and exit with the status EXIT_FAILURE
- If the stack is empty, print the error message L<line_number>: can't pchar, stack empty, followed by a new line, and exit with the status EXIT_FAILURE

```
julien@ubuntu:~/monty$ cat bytecodes/28.m
push 72
pchar
julien@ubuntu:~/monty$ ./monty bytecodes/28.m
H
julien@ubuntu:~/monty$
```

**Repo:**

- GitHub repository: `monty`

