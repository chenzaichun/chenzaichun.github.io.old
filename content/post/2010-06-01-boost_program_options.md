+++
categories = ["programming"]
comments = true
published = true
status = "publish"
date = "2010-06-01T12:32:33+08:00"
tags = ["boost", "Linux"]
title = "boost::program_options"
type = "post"
description = ""
+++


由于windows上没有getopt，使用第三方库也比较麻烦，考虑boost提供了program_options可以用来替代getopt，见[Tutorial]

我在linux下测试了一下，挺方便的。

贴一下代码，唯一注意的是链接的时候需要`libboost_program_options`, `ubuntu`下安装

```sh
sudo apt-get install libboost-program-options-dev
```

链接的时候加入`-lboost_program_options`就行了。

```cpp
#include <iostream>
#include <boost/program_options.hpp>

using namespace std;
using namespace boost::program_options;

// compile: g++ boostprogramoption.cpp -o boostprogramoption -lboost_program_options

int main(int argc, char** argv)
{
    // need to pass the buffer size (e.g. 1024) to the constructor, otherwise:
    // undefined reference to 'boost::program_options::options_description::m_default_line_length'
    options_description desc("Allowed options", 1024);
    desc.add_options()
        ("help,h", "produce help message")
        ("compression,c", value<int>(), "set compression level")
        ;

    variables_map vm;
    store(parse_command_line(argc, argv, desc), vm);
    notify(vm);

    if (vm.count("help")) {
        cout << desc << endl;
        return 1;
    }

    if (vm.count("compression")) {
        cout << "Compression level was set to " << vm["compression"].as<int>() << endl;
    } else {
        cout << "Compression level was not set." << endl;
    }

    return 0;
}
```

[Tutorial](http://www.boost.org/doc/libs/1_43_0/doc/html/program_options/tutorial.html)

<!--more-->