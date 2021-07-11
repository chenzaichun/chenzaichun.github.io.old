+++
categories = ["programming"]
comments = true
date = "2010-05-31T13:12:32+08:00"
published = true
status = "publish"
tags = ["boost", "Linux"]
title = "boost::timer"
type = "post"
description = ""
+++


在linux平台下使用boost::timer,得到的并不是所经过的时间,比如以下代码，因为sleep了1s，所以得到的结果应该大于1s，但是得出来的结果确是0

```cpp
#include <boost/timer.hpp>
#include <iostream>

using namespace std;
using namespace boost;

int main()
{
    cout << "test boost::timer" << endl;
    timer tmer;
    sleep(1);
    cout << tmer.elapsed() << endl;

    return 0;
}
```


```sh
$ ./boosttimer
test boost::timer
0
```

究其原因：察看boost::timer的源代码/usr/include/boost/timer.hpp

```cpp
class timer
{
 public:
         timer() { _start_time = std::clock(); } // postcondition: elapsed()==0
//         timer( const timer& src );      // post: elapsed()==src.elapsed()
//        ~timer(){}
//  timer& operator=( const timer& src );  // post: elapsed()==src.elapsed()
  void   restart() { _start_time = std::clock(); } // post: elapsed()==0
  double elapsed() const                  // return elapsed time in seconds
    { return  double(std::clock() - _start_time) / CLOCKS_PER_SEC; }

  double elapsed_max() const   // return estimated maximum value for elapsed()
  // Portability warning: elapsed_max() may return too high a value on systems
  // where std::clock_t overflows or resets at surprising values.
  {
    return (double((std::numeric_limits<std::clock_t>::max)())
       - double(_start_time)) / double(CLOCKS_PER_SEC);
  }

  double elapsed_min() const            // return minimum value for elapsed()
   { return double(1)/double(CLOCKS_PER_SEC); }

 private:
  std::clock_t _start_time;
}; // timer
```

可以看到boost::timer的实现在linux下是通过std::clock()实现的.经测试std::clock()每次返回的值都是一样的。原因就是我们在这里使用的是sleep，而man 3 clock得到的是function returns an approximation of processor time used by the program.即得到的是cpu执行的时间，sleep的时候cpu执行时间（cpu  time）为0，所以每次都得到的一样的值，最后结果为0，如果我们将sleep改为一个循环，结果就将是这是的值了！测试的时候不能被sleep所迷惑了，所以我们用来测实际经过的时间的时候不能用boost::timer!可以选择alarm
<!--more-->