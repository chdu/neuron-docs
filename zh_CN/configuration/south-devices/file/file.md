# 概览

## 模块描述

File 插件用于读写文件。

## 参数配置

| 字段         | 说明                  |
| ----------- | --------------------- |
| file_length | 设置读写文件内容的字符长度 |

## 支持的数据类型

* STRING

## 地址格式用法

### 地址格式

> FILE PATH</span>

### 地址示例

| 地址                      | 数据类型 | 说明                     |
| ------------------------ | ------ | ------------------------ |
| /home/root/test/test.txt | string | 读写 test.txt 文件中的内容 |

:::tip
地址需填写文件的绝对路径。

写操作时，写入的内容将会覆盖之前的文件内容。
:::