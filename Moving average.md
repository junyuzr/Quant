[Moving average example in C](https://gist.github.com/bmccormack/d12f4bf0c96423d03f82)
``` c
#include <stdio.h>

int movingAvg(int *ptrArrNumbers, long *ptrSum, int pos, int len, int nextNum)
{
  //Subtract the oldest number from the prev sum, add the new number
  *ptrSum = *ptrSum - ptrArrNumbers[pos] + nextNum;
  //Assign the nextNum to the position in the array
  ptrArrNumbers[pos] = nextNum;
  //return the average
  return *ptrSum / len;
}

int main(int argc, char *argv[])
{
  // a sample array of numbers. The represent "readings" from a sensor over time
  int sample[] = {50, 10, 20, 18, 20, 100, 18, 10, 13, 500, 50, 40, 10};
  // the size of this array represents how many numbers will be used
  // to calculate the average
  int arrNumbers[5] = {0};

  int pos = 0;
  int newAvg = 0;
  long sum = 0;
  int len = sizeof(arrNumbers) / sizeof(int);
  int count = sizeof(sample) / sizeof(int);

  for(int i = 0; i < count; i++){
    newAvg = movingAvg(arrNumbers, &sum, pos, len, sample[i]);
    printf("The new average is %d\n", newAvg);
    pos++;
    if (pos >= len){
      pos = 0;
    }
  }

  return 0;
}
```
为了同时计算5日、10日和20日的移动平均值，可以定义三个滑动窗口数组，并分别存储它们的和。这样在遍历样本数组时，可以分别计算和打印这三个滑动窗口的移动平均值。下面是修改后的代码：

```c
#include <stdio.h>

// 计算移动平均值的函数
int movingAvg(int *ptrArrNumbers, long *ptrSum, int pos, int len, int nextNum)
{
  // 从之前的和中减去最旧的数字，添加新的数字
  *ptrSum = *ptrSum - ptrArrNumbers[pos] + nextNum;
  // 将新的数字分配到数组中的当前位置
  ptrArrNumbers[pos] = nextNum;
  // 返回平均值
  return *ptrSum / len;
}

int main(int argc, char *argv[])
{
  // 一个样本数组，表示传感器随时间读取的数据
  int sample[] = {50, 10, 20, 18, 20, 100, 18, 10, 13, 500, 50, 40, 10};
  // 定义三个数组用于存储5日，10日和20日的数字
  int arrNumbers5[5] = {0};
  int arrNumbers10[10] = {0};
  int arrNumbers20[20] = {0};

  int pos5 = 0, pos10 = 0, pos20 = 0;  // 初始化位置
  int newAvg5 = 0, newAvg10 = 0, newAvg20 = 0;  // 新的平均值
  long sum5 = 0, sum10 = 0, sum20 = 0;  // 初始化累积和
  int len5 = sizeof(arrNumbers5) / sizeof(int);  // 计算5日滑动窗口的大小
  int len10 = sizeof(arrNumbers10) / sizeof(int);  // 计算10日滑动窗口的大小
  int len20 = sizeof(arrNumbers20) / sizeof(int);  // 计算20日滑动窗口的大小
  int count = sizeof(sample) / sizeof(int);  // 计算样本数组的大小

  // 遍历样本数组中的每个数字
  for(int i = 0; i < count; i++){
    // 计算新的5日移动平均值
    newAvg5 = movingAvg(arrNumbers5, &sum5, pos5, len5, sample[i]);
    // 计算新的10日移动平均值
    newAvg10 = movingAvg(arrNumbers10, &sum10, pos10, len10, sample[i]);
    // 计算新的20日移动平均值
    newAvg20 = movingAvg(arrNumbers20, &sum20, pos20, len20, sample[i]);

    // 打印新的平均值
    printf("The new 5-day average is %d\n", newAvg5);
    printf("The new 10-day average is %d\n", newAvg10);
    printf("The new 20-day average is %d\n", newAvg20);

    // 更新5日滑动窗口的位置
    pos5++;
    if (pos5 >= len5){
      pos5 = 0;
    }

    // 更新10日滑动窗口的位置
    pos10++;
    if (pos10 >= len10){
      pos10 = 0;
    }

    // 更新20日滑动窗口的位置
    pos20++;
    if (pos20 >= len20){
      pos20 = 0;
    }
  }

  return 0;
}
```

在这个版本的代码中，添加了三个滑动窗口数组 `arrNumbers5`、`arrNumbers10` 和 `arrNumbers20`，分别用于计算5日、10日和20日的移动平均值。每个滑动窗口数组都有各自的位置指针和累积和。通过循环遍历样本数组中的每个数字，分别调用 `movingAvg` 函数计算这三个滑动窗口的移动平均值，并打印出来。最后，根据各自的窗口大小更新位置指针。
