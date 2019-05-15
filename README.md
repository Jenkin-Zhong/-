# -
各种排序方法
#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>
#include<time.h>

void Initial(int **InsertData, int **ShellData, int **SelectData, int **HeapData, int **BubbleData, int **QuickData, int **MergeData, int **RadixData, int n);
void InsertSort(int a[], int n);
void ShellSort(int a[], int n);
void SelectSort(int a[], int n);
void CreatHeap(int a[], int n, int h);
void InitCreatHeap(int a[], int n);
void HeapSort(int a[], int n);
void BubbleSort(int a[], int n);
void QuickSort(int a[], int low, int high);
void Merge(int a[], int n, int swap[], int k);
void MergeSort(int a[], int n);

int main()
{
	int *InsertData, *ShellData, *SelectData, *HeapData, *BubbleData, *QuickData, *MergeData, *RadixData;
	int i,n,begintime,endtime;
	/*初始化n个数据*/
	printf("请输入数据个数：");
	scanf("%d",&n);
	Initial(&InsertData, &ShellData, &SelectData, &HeapData, &BubbleData, &QuickData, &MergeData, &RadixData,n);
	printf("\n各种排序时间：\n");
	/*直接插入排序*/
	begintime = clock();
	//InsertSort(InsertData, n);
	endtime = clock();
	printf("直接插入排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime)%1000);
	/*希尔排序*/
	begintime = clock();
	//ShellSort(ShellData, n);
	endtime = clock();
	printf("希尔排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime) % 1000);
	/*直接选择排序*/
	begintime = clock();
	//SelectSort(SelectData, n);
	endtime = clock();
	printf("直接选择排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime) % 1000);
	/*堆排序*/
	begintime = clock();
	HeapSort(HeapData, n);
	endtime = clock();
	printf("堆排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime) % 1000);
	/*冒泡排序*/
	begintime = clock();
	//BubbleSort(BubbleData, n);
	endtime = clock();
	printf("冒泡排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime) % 1000);
	/*快速排序*/
	begintime = clock();
	QuickSort(QuickData, 0, n - 1);
	endtime = clock();
	printf("快速排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime) % 1000);
	/*归并排序*/
	begintime = clock();
	MergeSort(MergeData, n);
	endtime = clock();
	printf("归并排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime) % 1000);


	return 0;
}
/*
begintime = clock();

endtime = clock();
printf("直接插入排序耗时：%d.%03d秒\n", (endtime - begintime) / 1000, (endtime - begintime)%1000);
*/

/*初始化函数*/
void Initial(int **InsertData, int **ShellData, int **SelectData, int **HeapData, int **BubbleData, int **QuickData, int **MergeData, int **RadixData,int n)
{
	int i = 0,k;
	*InsertData = (int *)malloc(sizeof(int)*n);
	*ShellData = (int *)malloc(sizeof(int)*n);
	*SelectData = (int *)malloc(sizeof(int)*n);
	*HeapData = (int *)malloc(sizeof(int)*n);
	*BubbleData = (int *)malloc(sizeof(int)*n);
	*QuickData = (int *)malloc(sizeof(int)*n);
	*MergeData = (int *)malloc(sizeof(int)*n);
	*RadixData = (int *)malloc(sizeof(int)*n);
	for (i = 0; i < n; i++)
	{
		k = rand();
		(*InsertData)[i] = k;
		(*ShellData)[i] = k;
		(*SelectData)[i] = k;
		(*HeapData)[i] = k;
		(*BubbleData)[i] = k;
		(*QuickData)[i] = k;
		(*MergeData)[i] = k;
		(*RadixData)[i] = k;
	}
}

/*直接插入排序*/
void InsertSort(int a[], int n)
{
	int i, j, temp;
	for (i = 0; i < n - 1; i++)
	{
		temp = a[i + 1];
		j = i;
		while (j>-1 && temp < a[j])
		{
			a[j + 1] = a[j];
			j--;
		}
		a[j + 1] = temp;
	}
}

/*希尔排序*/
void ShellSort(int a[], int n)
{
	int *d,numOfD=0,zc=n,p;
	int i, j, k, m, span, temp;
	while (zc!=0)
	{
		numOfD++;
		zc /= 5;
	}
	d = (int *)malloc(sizeof(int)*numOfD);
	zc = n;
	for (p = 0; p < numOfD; p++)
	{
		zc /= 5;
		d[p] = zc;
	}
	d[p - 1] = 1;
	for (m = 0; m < numOfD; m++)
	{
		span = d[m];
		for (k = 0; k < span; k++)
		{
			for (i = k; i < n - span; i = i + span)
			{
				temp = a[i + span];
				j = i;
				while (j>-1 && temp < a[j])
				{
					a[j + span] = a[j];
					j = j - span;
				}
				a[j + span] = temp;
			}
		}
	}
}

/*直接选择排序*/
void SelectSort(int a[], int n)
{
	int i, j, small,temp;
	for (i = 0; i < n - 1; i++)
	{
		small = i;
		for (j = i + 1; j < n; j++)
			if (a[j] < a[small])
				small = j;
		if (small != i)
		{
			temp = a[i];
			a[i] = a[small];
			a[small] = temp;
		}
	}
}

/*堆排序*/
void CreatHeap(int a[], int n, int h)
{
	int i, j, flag, temp;
	i = h;
	j = 2*i + 1;
	temp = a[i];
	flag = 0;
	while (j < n&&flag != 1)
	{
		if (j < n - 1 && a[j] < a[j + 1])
			j++;
		if (temp > a[j])
			flag = 1;
		else
		{
			a[i] = a[j];
			i = j;
			j = 2 * i + 1;
		}
	}
	a[i] = temp;
}

void InitCreatHeap(int a[], int n)
{
	int i;
	for (i = (n - 2) / 2; i >= 0; i--)
		CreatHeap(a, n, i);
}

void HeapSort(int a[], int n)
{
	int i, temp;
	InitCreatHeap(a, n);
	for (i = n - 1; i > 0; i--)
	{
		temp = a[0];
		a[0] = a[i];
		a[i] = temp;
		CreatHeap(a, i, 0);
	}
}

/*冒泡排序*/
void BubbleSort(int a[], int n)
{
	int i, j, flag = 1, temp;
	for (i = 1; i < n&&flag == 1; i++)
	{
		flag = 0;
		for (j = 0; j < n - i; j++)
		{
			if (a[j]>a[j + 1])
			{
				flag = 1;
				temp = a[j];
				a[j] = a[j + 1];
				a[j + 1] = temp;
			}
		}
	}
}

/*快速排序*/
void QuickSort(int a[], int low, int high)
{
	int i = low, j = high, temp=a[low];
	while (i < j)
	{
		while (i < j&&temp < a[j])
			j--;
		if (i < j)
		{
			a[i] = a[j];
			i++;
		}
		while (i < j&&a[i] < temp)
			i++;
		if (i < j)
		{
			a[j] = a[i];
			j--;
		}
	}
	a[i] = temp;
	if (low < i)
		QuickSort(a, low, i - 1);
	if (i < high)
		QuickSort(a, j + 1, high);
}

/*归并排序*/
void Merge(int a[], int n, int swap[], int k)
{
	int m = 0, u1, l2, i, j, u2, l1 = 0;
	while (l1 + k <= n - 1)
	{
		l2 = l1 + k;
		u1 = l2 - 1;
		u2 = (l2 + k - 1 <= n - 1) ? l2 + k - 1 : n - 1;
		for (i = l1, j = l2; i <= u1&&j <= u2; m++)
		{
			if (a[i] <= a[j])
			{
				swap[m] = a[i];
				i++;
			}
			else
			{
				swap[m] = a[j];
				j++;
			}
		}
		while (i <= u1)
		{
			swap[m] = a[i];
			m++;
			i++;
		}
		while (j <= u2)
		{
			swap[m] = a[j];
			m++;
			j++;
		}
		l1 = u2 + 1;
	}
	for (i = l1; i < n; i++, m++)
		swap[m] = a[i];
}

void MergeSort(int a[], int n)
{
	int i, k = 1, *swap;
	swap = (int *)malloc(sizeof(int)*n);
	while (k < n)
	{
		Merge(a, n, swap, k);
		for (i = 0; i < n; i++)
			a[i] = swap[i];
		k *= 2;
	}
	free(swap);
}

/*基数排序（桶排序）*/
