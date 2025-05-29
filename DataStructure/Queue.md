# 队列
## 操作
- 初始化
- 销毁
- 清空
- 判断队列空
- 判断队列满
- 队列长度
- 出队列
- 入队列
- 取队头元素
- 遍历队头到队尾
### 循环队列（少用一元素空间）
下面的定义中，队头front指向队头元素，队尾rear指向队尾元素后；
- 定义
~~~~
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 100

typedef struct {
    int data[MAX_SIZE];
    int front;
    int rear;
} CircularQueue;
~~~~
- 初始化队列
~~~~
void initQueue(CircularQueue *queue) {
    queue->front = 0;
    queue->rear = 0;
}
~~~~
- 判断队列是否为空
~~~~
int isEmpty(CircularQueue *queue) {
    return queue->front == queue->rear;
}
~~~~
- 判断队列是否已满
~~~~
int isFull(CircularQueue *queue) {
    return (queue->rear + 1) % MAX_SIZE == queue->front;
}
~~~~
- 入队操作
~~~~
int enqueue(CircularQueue *queue, int item) {
    if (isFull(queue)) {
        printf("队列已满，无法入队！\n");
        return 0;
    }
    queue->data[queue->rear] = item;
    queue->rear = (queue->rear + 1) % MAX_SIZE;
    return 1;
}
~~~~
- 出队操作
~~~~
int dequeue(CircularQueue *queue, int *item) {
    if (isEmpty(queue)) {
        printf("队列为空，无法出队！\n");
        return 0;
    }
    *item = queue->data[queue->front];
    queue->front = (queue->front + 1) % MAX_SIZE;
    return 1;
}
~~~~
- 获取队头元素
~~~~
int getFront(CircularQueue *queue, int *item) {
    if (isEmpty(queue)) {
        printf("队列为空，无队头元素！\n");
        return 0;
    }
    *item = queue->data[queue->front];
    return 1;
}
~~~~
- 打印队列元素
~~~~
void printQueue(CircularQueue *queue) {
    if (isEmpty(queue)) {
        printf("队列为空！\n");
        return;
    }
    int i = queue->front;
    while (i != queue->rear) {
        printf("%d ", queue->data[i]);
        i = (i + 1) % MAX_SIZE;
    }
    printf("\n");
}
~~~~
- 销毁队列，这里因为是静态数组，简单重置即可
~~~~
void destroyQueue(CircularQueue *queue) {
    queue->front = 0;
    queue->rear = 0;
}
~~~~
- 清空队列元素
~~~~
void clearQueue(CircularQueue *queue) {
    queue->front = 0;
    queue->rear = 0;
}
~~~~
- 返回队列长度
~~~~
int getQueueLength(CircularQueue *queue) {
    return (queue->rear - queue->front + MAX_SIZE) % MAX_SIZE;
}
~~~~
### 循环队列（标志位）
下面的定义中，队头front指向队头元素，队尾rear指向队尾元素后；
- 定义
~~~~
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 100

typedef struct {
    int data[MAX_SIZE];
    int front;
    int rear;
    int size;
} CircularQueue;
~~~~
- 初始化队列
~~~~
void initQueue(CircularQueue *queue) {
    queue->front = 0;
    queue->rear = 0;
    queue->size = 0;
}
~~~~
- 判断队列是否为空
~~~~
int isEmpty(CircularQueue *queue) {
    return queue->size == 0;
}
~~~~
- 判断队列是否已满
~~~~
int isFull(CircularQueue *queue) {
    return queue->size == MAX_SIZE;
}
~~~~
- 入队操作
~~~~
int enqueue(CircularQueue *queue, int item) {
    if (isFull(queue)) {
        printf("队列已满，无法入队！\n");
        return 0;
    }
    queue->data[queue->rear] = item;
    queue->rear = (queue->rear + 1) % MAX_SIZE;
    queue->size++;
    return 1;
}
~~~~
- 出队操作
~~~~
int dequeue(CircularQueue *queue, int *item) {
    if (isEmpty(queue)) {
        printf("队列为空，无法出队！\n");
        return 0;
    }
    *item = queue->data[queue->front];
    queue->front = (queue->front + 1) % MAX_SIZE;
    queue->size--;
    return 1;
}
~~~~
- 获取队头元素
~~~~
int getFront(CircularQueue *queue, int *item) {
    if (isEmpty(queue)) {
        printf("队列为空，无队头元素！\n");
        return 0;
    }
    *item = queue->data[queue->front];
    return 1;
}
~~~~
- 打印队列元素
~~~~
void printQueue(CircularQueue *queue) {
    if (isEmpty(queue)) {
        printf("队列为空！\n");
        return;
    }
    int i = queue->front;
    for (int j = 0; j < queue->size; j++) {
        printf("%d ", queue->data[i]);
        i = (i + 1) % MAX_SIZE;
    }
    printf("\n");
}
~~~~
- 销毁队列
~~~~
void destroyQueue(CircularQueue *queue) {
    queue->front = 0;
    queue->rear = 0;
    queue->size = 0;
}
~~~~
- 清空队列元素
~~~~
void clearQueue(CircularQueue *queue) {
    queue->front = 0;
    queue->rear = 0;
    queue->size = 0;
}
~~~~
- 返回队列长度
~~~~
int getQueueLength(CircularQueue *queue) {
    return queue->size;
}
~~~~
