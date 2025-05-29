# 栈
## 基本操作
- 初始化
- 销毁栈
- 清空栈
- 栈空判定
- 栈长
- 入栈
- 出栈
- 取栈顶
- 从栈底到栈顶遍历
## 实现
### 顺序栈
- 定义
~~~~
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 100

typedef struct {
    int data[MAX_SIZE];
    int top;
} Stack;
~~~~
- 初始化栈
~~~~
void initStack(Stack *s) {
    s->top = -1;
}
~~~~
- 判断栈是否为空
~~~~
int isEmpty(Stack *s) {
    return s->top == -1;
}
~~~~
- 判断栈是否已满
~~~~
int isFull(Stack *s) {
    return s->top == MAX_SIZE - 1;
}
~~~~
- 入栈操作
~~~~
void push(Stack *s, int value) {
    if (isFull(s)) {
        printf("Stack is full, cannot push %d\n", value);
        return;
    }
    s->data[++(s->top)] = value;
}
~~~~
- 出栈操作
~~~~
int pop(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty, cannot pop\n");
        return -1;
    }
    return s->data[(s->top)--];
}
~~~~
- 获取栈顶元素
~~~~
int peek(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty, cannot peek\n");
        return -1;
    }
    return s->data[s->top];
}
~~~~
### 链栈
- 新建Stack变量要分配内存
- 结束使用Stack之后要释放内存
- 定义
~~~~
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next;
} Node;

typedef struct {
    Node *top;
} Stack;
~~~~
- 初始化
~~~~
void initStack(Stack *s) {
    s->top = NULL;
}
~~~~
- 判断栈是否为空
~~~~
int isEmpty(Stack *s) {
    return s->top == NULL;
}
~~~~
- 入栈
~~~~
void push(Stack *s, int value) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("Memory allocation failed\n");
        return;
    }
    newNode->data = value;
    newNode->next = s->top;
    s->top = newNode;
}
~~~~
//-~~~~
int pop(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty, cannot pop\n");
        return -1;
    }
    Node *temp = s->top;
    int value = temp->data;
    s->top = temp->next;
    free(temp);
    return value;
}
~~~~
- 获取栈顶元素
~~~~
int peek(Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty, cannot peek\n");
        return -1;
    }
    return s->top->data;
}
~~~~
## 例题
### 括号匹配
#### C
```
#include<stdio.h>
#include<stdlib.h>

typedef struct {
    char data[100];
    int top;
} Stack;

void pop(Stack* s)
{
    if (s->top > -1)
    {
        s->top--;
    }
}

void push(Stack* s, char c)
{
    if (s->top < 99)
    {
        s->data[++(s->top)] = c;
    }
}
int main()
{
    char expr[101] = { 0 };
    fgets(expr, 101, stdin);

    Stack s;
    s.top = -1;

    for (int i = 0; expr[i] != '\0' && expr[i] != '\n'; i++)
    {
        if (expr[i] == '(')
        {
            push(&s, '(');
        }
        else if (expr[i] == ')')
        {
            if (s.top == -1)
            {
                printf("No\n");
                return 0;
            }
            else
            {
                pop(&s);
            }
        }
    }

    printf(s.top == -1 ? "Yes\n" : "No\n");
    return 0;
}
```
