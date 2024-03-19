---
title: Jsoncpp 的使用方法
description: ""
author: 清松
date: 2019-03-22T14:49:37+08:00
categories:
  - 工具
tags:
  - 开源库
  - jsoncpp
enableMath: true
url: 
draft: false
series:
---
下载地址 jsoncpp [Github](https://github.com/open-source-parsers/jsoncpp)
## 示例
以这个 json 串为例
``` json
{
    "item_0":"一个普通的 json 元素",
    "arrary":["数组元素1", "数组元素1"],
    "empty_arrary":[],
    "item_1":{
        "item_item_0":"",
        "item_item_1":""
    }
}
```

使用[在线工具](https://www.bejson.com/json/format/)检查 json 字符串是否合法，以及格式化、压缩转义等。

### 解析 json
示例代码
``` cpp
int ParseJson(std::string json_str)
{
    Json::Reader reader;
    Json::Value root;

    try {
        if (reader.parse(json_str, root)) {
            if (root.isMember("item_0"))
                std::cout << root["item_0"].asString() << std::endl;

            if (root.isMember("array")) {
                Json::Value val;
                val = root["array"];
                for (int i = 0; i < val.size(); ++i) {
                    std::cout << "array: " << val[i].asString() << std::endl;
                }
                std::cout << "array size: " << val.size() << std::endl;
            }

            if (root.isMember("empty_array")) {
                Json::Value val;
                val = root["empty_array"];
                for (int i = 0; i < val.size(); ++i) {
                    std::cout << "empty_array: " << val[i].asString() << std::endl;
                }
                std::cout << "empty_array size: " << val.size() << std::endl;
            }

            if (root.isMember("item_1")) {
                Json::Value val;
                val = root["item_1"];

                if (val.isMember("item_item_0")) {
                    std::cout << "item_1.item_item_0 " << val["item_item_0"].asString() << std::endl;
                }

                if (val.isMember("item_item_1")) {
                    std::cout << "item_1.item_item_1 " << val["item_item_1"].asString() << std::endl;
                }
            }
        }
    }
    catch (...)
    {
        std::cout << "error" << std::endl;
    }
    return 0;
}
```

### 转为 json
``` cpp
int GenerateJson(Json::Value& root) {
    std::string str = "一个普通的 json 元素";
    root["item_0"] = str.c_str();
    
    Json::Value val;
    for (int i = 0; i < 3; ++i) {
        val.append("数组元素1");
    }
    root["array"] = val;
    root["empty_array"] = Json::arrayValue;
    
    Json::Value item;
    item["item_item_0"] = "item_item_0";
    item["item_item_1"] = "item_item_1";
    root["item_1"] = item;

    Json::FastWriter json_fw;
    std::cout << "无格式的 Json: " << json_fw.write(root) << std::endl;

    Json::StyledWriter json_sw;
    std::cout << "有格式的 Json: " << json_sw.write(root) << std::endl;
    return 0;
}
```

### 其他接口
接口的详细介绍可以参考 jsoncpp
[官方文档](http://open-source-parsers.github.io/jsoncpp-docs/doxygen/index.html)

#### 类型判断
`isNull`: 是否为空  
`isBool`: 是否为布尔值  
`isInt`: 是否为 int  
`isArray`: 是否为数组  
`isMember`: 是否存在该项  
`isValidIndex`:

#### 类型转换
`asInt`  
`asString`  
...

#### 节点获取
`get`  
`[]`

#### 节点操作
`compare`  
`swap`  
`removeMember`  
`removeindex`  
`append`

### 完整示例代码
``` cpp
#include <iostream>
#include <string>

#include "json/json.h"

const char* kJsonStr = "{"
"\"item_0\":\"一个普通的 json 元素\","
"\"array\":[\"数组元素0\", \"数组元素1\"],"
"\"empty_array\":[],"
"\"item_1\":{"
"\"item_item_0\":\"\","
"\"item_item_1\":\"\""
"    }"
"}";

Json::Value ParseJson(std::string json_str)
{
    Json::Reader reader;
    Json::Value root;

    try {
        if (reader.parse(json_str, root)) {
            if (root.isMember("item_0"))
                std::cout << root["item_0"].asString() << std::endl;

            if (root.isMember("array")) {
                Json::Value val;
                val = root["array"];
                for (int i = 0; i < val.size(); ++i) {
                    std::cout << "array: " << val[i].asString() << std::endl;
                }
                std::cout << "array size: " << val.size() << std::endl;
            }

            if (root.isMember("empty_array")) {
                Json::Value val;
                val = root["empty_array"];
                for (int i = 0; i < val.size(); ++i) {
                    std::cout << "empty_array: " << val[i].asString() << std::endl;
                }
                std::cout << "empty_array size: " << val.size() << std::endl;
            }

            if (root.isMember("item_1")) {
                Json::Value val;
                val = root["item_1"];
                if (val.isMember("item_item_0")) {
                    std::cout << "item_1.item_item_0 " << val["item_item_0"].asString() << std::endl;
                }
                if (val.isMember("item_item_1")) {
                    std::cout << "item_1.item_item_1 " << val["item_item_1"].asString() << std::endl;
                }
            }
        }
    }
    catch (...)
    {
        std::cout << "error" << std::endl;
    }
    return root;
}

int GenerateJson(Json::Value& root, 
    std::string& not_format_json,
    std::string& format_json) {
    std::string str = "一个普通的 json 元素";
    root["item_0"] = str.c_str();
    Json::Value val;
    for (int i = 0; i < 3; ++i) {
        val.append("数组元素1");
    }
    root["array"] = val;
    root["empty_array"] = Json::arrayValue;
    
    Json::Value item;
    item["item_item_0"] = "item_item_0";
    item["item_item_1"] = "item_item_1";
    root["item_1"] = item;

    Json::FastWriter json_fw;
    not_format_json = json_fw.write(root);

    Json::StyledWriter json_sw;
    format_json = json_sw.write(root);
    return 0;
}

int main(int argc, char** argv)
{
    std::cout << "原始数据：" << std::endl << kJsonStr << std::endl;
    std::cout << "开始解析 json" << std::endl;
    Json::Value root = ParseJson(kJsonStr);
    std::cout << "json 解析完毕" << std::endl;

    std::string not_format_json, format_json;
    GenerateJson(root, not_format_json, format_json);

    std::cout << "无格式的 Json: " << not_format_json << std::endl;
    std::cout << "有格式的 Json: " << format_json << std::endl;

    return 0;
}
```

## 参考资料

[JsonCpp使用方法详解](https://zhangzc.blog.csdn.net/article/details/78901015?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-5.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-5.control)  
[使用 jsoncpp 创建空的 json数组](https://stackoverflow.com/questions/13293043/create-empty-json-array-with-jsoncpp)  
[C++通过jsoncpp类库读写JSON文件](https://blog.csdn.net/tennysonsky/article/details/48809835)
