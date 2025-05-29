## 内存分配
### 分配失败
~~~~
// 安全分配内存并检查是否分配成功
int* safe_malloc(size_t size) {
    int* ptr = (int*)malloc(size);
    if (ptr == NULL) {
        fprintf(stderr, "内存分配失败\n");
        exit(EXIT_FAILURE);
    }
    return ptr;
}
~~~~
