逛知乎时无意间看到的，经验主义的优雅常识，适用于 1582 年 10 月 15 日（包括）之后的日期，原文链接：https://www.zhihu.com/question/30262900/answer/2863211053

```cpp
int dow(int y, int m, int d) { 
    static int t[] = {0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4}; 
    y -= m < 3; 
    return (y + y/4 - y/100 + y/400 + t[m-1] + d) % 7; 
} 
```

输入年月日的整型，返回整数，代表是星期几。