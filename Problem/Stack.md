## 括号匹配（最长有效括号）
- 如何区分出有效但是并不连续嵌套的括号子串
- 鉴于括号匹配很适合使用栈结构，所以关键点在于如何计算有效子串的起始点与终止点
- 这里采用了记录字符索引，内含了括号的顺序关系，无效的右括号会不断更新基准，使得新的有效子串从新基准开始算起
~~~~
#include <stdio.h>
#include <string.h>

#define MAX_LEN 10001

int main() {
    char s[MAX_LEN];
    scanf("%s", s);
    int max_len = 0;
    int stack[MAX_LEN];
    int top = -1;
    stack[++top] = -1; // 初始基准点

    for (int i = 0; s[i] != '\0'; i++) {
        if (s[i] == '(') {
            stack[++top] = i; // 压入左括号的索引
        } else {
            top--; // 弹出栈顶元素（匹配左括号的索引或基准点）
            if (top == -1) {
                stack[++top] = i; // 更新基准点
            } else {
                int current_len = i - stack[top];
                if (current_len > max_len) {
                    max_len = current_len;
                }
            }
        }
    }

    printf("%d\n", max_len);
    return 0;
}
~~~~
