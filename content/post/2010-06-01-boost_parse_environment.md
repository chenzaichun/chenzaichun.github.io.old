+++
categories = ["programming"]
comments = true
published = true
date = "2010-06-01T12:11:11+08:00"
status = "publish"
tags = ["boost", "Linux"]
title = "boost::parse_environment"
type = "post"
description = ""
+++


`boost::program_options::parse_environment`通过环境变量来设置`options`。

这里需要注意的是需要注意的是：一定要将所需要的环境变量加入到`options`中。


```cpp
#include <iostream>
#include <string>
#include <boost/program_options.hpp>

using namespace std;
using namespace boost::program_options;

// compile: g++ boostprogramoption.cpp -o boostprogramoption -lboost_program_options

// envs
const string ENVS[] = {
    "HOME",
    "PKG_CONFIG_PATH",
    "PATH",
    "http_proxy",
};

const int ENVCOUNT = 4;

static std::string name_mapper(const std::string &var_name){
    for (int i = 0; i < ENVCOUNT; ++i) {
        if (var_name == ENVS[i])
            return var_name;
    }

    return "";
}

int main(int argc, char** argv)
{
    // need to pass the buffer size to constructor (e.g. 1024), otherwise:
    // undefined reference to 'boost::program_options::options_description::m_default_line_length'
    options_description desc("Allowed options", 1024);
    desc.add_options()
        ("help,h", "produce help message")
        ("compression,c", value<int>(), "set compression level")

	// need to use the env as option, otherwise exception throwed:
        ("HOME,m", value<string>()->default_value(""), "home dir")
        ("PATH,p", value<string>()->default_value(""), "path")
        ("PKG_CONFIG_PATH,k", value<string>()->default_value(""), "pkg config")
        ("http_proxy", value<string>()->default_value(""), "http proxy");
        ;

    variables_map vm;
    store(parse_command_line(argc, argv, desc), vm);
    store(parse_environment(desc, name_mapper), vm);
    notify(vm);

    if (vm.count("help")) {
        cout << desc << endl;
        return 1;
    }

    if (vm.count("compression")) {
        cout << "Compression level was set to "
             << vm["compression"].as<int>() << endl;
    } else {
        cout << "Compression level was not set." << endl;
    }

    for (int i = 0; i < ENVCOUNT; ++i) {
        if (vm.count(ENVS[i])) {
            cout << ENVS[i] << " is "
                 << vm[ENVS[i]].as<string>() << endl;
        }
    }

    return 0;
}
```
<!--more-->