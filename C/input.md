## scanf读取失败后的数据处理
~~~~
while ((c = getchar()) != '\n' && c != EOF);
~~~~
## scanf循环读取
~~~~
while (scanf("%d", &num) == 1 && num != -1)
~~~~
## 处理标准输入中不定长字符串
- 动态分配内存
- 扩容
- 预留一元素空间放置'\0'
- 释放内存
~~~~
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char* read不定长字符串() {
    char* str = NULL;
    char ch;
    int size = 10;  // 初始分配大小
    int len = 0;    // 已存储字符数，用来锚定添加结束符的位置

    str = (char*)malloc(size * sizeof(char));
    if (str == NULL) {
        printf("内存分配失败!\n");
        return NULL;
    }

    while ((ch = getchar()) != '\n') {  // 输入以换行结束，可根据需求改EOF
        // 扩容，注意这里的扩容条件，留一个位置\0
        if (len >= size - 1) {
            size *= 2;  // 扩容策略，可自定义
            char* temp = (char*)realloc(str, size * sizeof(char));
            if (temp == NULL) {
                printf("内存重新分配失败!\n");
                free(str);
                return NULL;
            }
            str = temp;
        }
        str[len++] = ch;
    }
    str[len] = '\0';  // 添加结束符
    return str;
}
~~~~
## 分离操作标识与参数，根据参数对应到相应操作
- 使用strtok函数将输入的不定长字符串按分隔符（如空格、逗号等）拆分
- 分别提取操作标识与参数部分
- 使用strcspn函数来将操作标识与具体的操作名对应，将被解析的参数传递给对应的函数来执行
~~~~
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// 加法操作函数
void operate_add(char* params) {
    int a = atoi(strtok(params, " "));
    int b = atoi(strtok(NULL, " "));
    printf("加法结果: %d\n", a + b);
}

// 减法操作函数
void operate_sub(char* params) {
    int a = atoi(strtok(params, " "));
    int b = atoi(strtok(NULL, " "));
    printf("减法结果: %d\n", a - b);
}

int main() {
    char input[100];
    fgets(input, sizeof(input), stdin);
    input[strcspn(input, "\n")] = '\0'; // 去除换行符

    char* op = strtok(input, " "); // 提取操作标识
    char* params = strtok(NULL, ""); // 提取参数

    if (strcmp(op, "add") == 0) {
        operate_add(params);
    } else if (strcmp(op, "sub") == 0) {
        operate_sub(params);
    } else {
        printf("未知操作\n");
    }
    return 0;
}
~~~~
