---
-date: 2021-01-10 20:05:28
---

# C/C++ 中 int float double 最大值，最小值

https://blog.csdn.net/ACb0y/article/details/5336822

```cpp
#include <iostream>
#include <float.h>
using namespace std;
int main()
{
	cout << "int 类型能存储的最大值和最小值" << endl;
	cout << "INT_MAX = " << INT_MAX << endl;
	cout << "INT_MIN = " << INT_MIN << endl;
	cout << "long 类型能存储的最大值和最小值" << endl;
	cout << "LONG_MAX = " << LONG_MAX << endl;
	cout << "LONG_MIN = " << LONG_MIN << endl;
	cout << "long long 类型能存储的最大值和最小值" << endl;
	cout << "LONG_LONG_MAX = " << LONG_LONG_MAX << endl;
	cout << "LONG_LONG_MIN = " << LONG_LONG_MIN << endl;
	cout << "float 类型能存储的最大值和最小值" << endl;
	cout << "FLT_MAX = " << FLT_MAX << endl;
	cout << "FLT_MIN = " << FLT_MIN << endl;
	cout << "double 类型能存储的最大值和最小值" << endl;
	cout << "DBL_MAX = " << DBL_MAX << endl;
	cout << "DBL_MIN = " << DBL_MIN << endl;
	return 0;

}
```

int和long都是用32位来存储最大值和最小值分别为2147483647（2^31-1 ~ 10^9）， -2147483648(-2^31)；

long long 是用64位来存储最大值和最小值分别为9223372036854775807（10^18），-9223372036854775808；

float的最大值和最小值分别为3.40282e+038（10^38），1.17549e-038（10^-38）；

double的最大值和最小值分别为1.79769e+308（10^308），2.22507e-308（10^-308）