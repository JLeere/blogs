---
title: Vector
date: 2019-12-19 12:03:48
categories:
- 编程学习
- C++
tags:
- C++
---

[学习教材](https://www.runoob.com/cplusplus/cpp-tutorial.html)
学习要及时总结
## [vector](https://www.runoob.com/w3cnote/cpp-vector-container-analysis.html)

```C++
# include <vector>
# include <algorithm> //包含sort函数
using namespace std;
int main(){
    int N=5,M=6;
    //一维向量
    vector<int> obj(N);//int 类型，obj变量名，N数据尺寸
    //二位向量
    vector<vector<int> > obj2(N,vector<int>(M));//注意中间有个空格
    obj2[i][j];

    //数据增减
    obj.size();
    for(int i=0;i<10;i++)
    {
        obj.push_back(i);  //在末尾添加数据i
        obj.pop_back(); //在末尾移除数据 
    } 
    obj.clear();//清除所有数据
    
    //排序
    sort(obj.begain(),obj.end());//从小到大排序
    reverse(obj.begain(),obj.end());//从大到小排序

    //访问
    vector<int>::iterator it;//创建迭代器对象访问向量地址
    for(it=obj.begain();it!=obj.end();it++)
    {
        cout<<*it<<" ";
    }
}
```
