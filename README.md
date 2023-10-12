# C-Stacks-Queues-LIFO-FIFO

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

