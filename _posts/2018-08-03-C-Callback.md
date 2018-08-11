---
layout:     post
title:      "C语言回调函数"
subtitle:   "Callback function"
date:       2018-08-03
author:     "Jannon"
header-img: "images/about-bg.jpg"
tags:
    - C
---

## C语言回调函数
C语言常常需要设置一些回调函数，在合适的时候进行回调函数调用达到反馈结果的作用。

1.定义一个回调函数指针
> typedef void (*event_cb_t)(const struct event *evt, void *userdata);

2.定义一个函数，并将回调函数作为参数传入该函数
> int event_cb_register(event_cb_t cb, void *userdata);

3.定义回调函数的实现
```
static void my_event_cb(const struct event *evt, void *data)
{
 /* 回调函数应该要做的处理代码 */
}
```

4.调用将回调函数作为参数的函数
> event_cb_register(my_event_cb, &my_custom_data);

也可以将回调函数定义到一个结构体中
```struct event_cb {
    event_cb_t cb;
    void *data;
};
```
通过如下方式进行回调

``` c
struct event_cb *callback;

...

/* 获取到该结构体并执行回调函数 */

callback->cb(event, callback->data);
```
