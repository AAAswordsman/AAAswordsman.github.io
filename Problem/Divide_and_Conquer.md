## 统计正序对（统计逆序对）
- 分治算法
- 归并排序
- 归并排序可以很自然地统计当前子数组中某侧元素与另一侧元素的正序对（逆序对）数目，每一次合并都会扩大统计范围，之前的合并所统计的数目并不会被该次合并计算在内
~~~~
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

typedef struct {
	int value;
	int index;
	int count;
}Fish;

// 合并函数
void merge(Fish arr[], int left, int mid, int right) {
	int n1 = mid - left + 1;
	int n2 = right - mid;
	Fish* L = (Fish*)malloc(n1 * sizeof(Fish));
	Fish* R = (Fish*)malloc(n2 * sizeof(Fish));

    // 将原数组的元素映射到两个新数组（左数组与右数组）
	for (int i = 0; i < n1; i++) L[i] = arr[left + i];
	for (int i = 0; i < n2; i++) R[i] = arr[mid + 1 + i];

	int i = 0, j = 0, k = left;

    // 当左右数组未遍历完时，进行排序
	while (i < n1 && j < n2)
	{
		if (L[i].value < R[j].value)
		{
			arr[k++] = L[i++];
		}
		else
		{
			R[j].count += i;
			arr[k++] = R[j++];
		}
	}
    // 遍历左右数组中仍有剩余元素的数组，由于左右子数组已然有序（在之前的合并中排序完毕），所以可以直接添加到末尾
	while (i < n1)
	{
		arr[k++] = L[i++];
	}
	while (j < n2)
	{
		R[j].count += i;
		arr[k++] = R[j++];
	}

	free(L);
	free(R);
}

// 分解函数
void mergeSort(Fish arr[], int left, int right) {
	if (left < right)
	{
		int mid = left + (right - left) / 2;
		mergeSort(arr, left, mid);
		mergeSort(arr, mid + 1, right);
		merge(arr, left, mid, right);
	}
}

int main() {
	int n;
	scanf("%d", n);

	Fish* fish = (Fish*)malloc(n * sizeof(Fish));
	for (int i = 0; i < n; i++)
	{
		fish[i].index = i;
		fish[i].count = 0;
		scanf("%d", &fish[i].value);
	}

	mergeSort(fish, 0, n - 1);

    // index保留原数组元素排序前的顺序，据此创建新数组便于按原顺序输出
	int* result = (int*)malloc(n * sizeof(int));
	for (int i = 0; i < n; i++)
	{
		result[fish[i].index] = fish[i].count;
	}

	for (int i = 0; i < n; i++)
	{
		if (i > 0) printf(" ");
		printf("%d", result[i]);
	}
	printf("\n");

	free(fish);
	free(result);
	return 0;
}
~~~~
