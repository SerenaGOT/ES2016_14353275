# Dol实例修改

## 1.修改后的.dot截图

1)  example1.dol

![](http://p1.bpimg.com/567571/b587cce336723a44.png)

2) example2.dol

![](http://p1.bpimg.com/567571/8379dc99b1176623.png)

## 2.修改步骤

1）example1

​	修改 dol/examples/example1/src原来的square.c文件，把square_fire函数中的

```c
i = i*i;
```

 	修改为

```c
i = i*i*i;
```

​	如果之前运行过example1例子，dol/build/bin/main中会生成example1文件夹，需要把文件夹删除再运行样例才会看到修改之后的结果。运行修改后的结果如下：

![](http://p1.bpimg.com/567571/735ecb4d74c9674d.png)

2）example2

​	修改 dol/examples/example2/example.xml, 把原来的value=“3”改为value="2",即可实现减少一个迭代的模块。

```xml
<processnetwork xsi:schemaLocation="http://www.tik.ee.ethz.ch/~shapes/schema/PROCESSNETWORK http://www.tik.ee.ethz.ch/~shapes/schema/processnetwork.xsd" name="example2"><variable value="2" name="N"/>
```

​	运行例子的结果如下（1-19的四次方）：

![](http://p1.bpimg.com/567571/982c941fe0855dfc.png)

3）扩展：修改example1中的文件名

​	因为把平方运算换成了立方运算，所以把原来所有涉及square的名字都换成cube。所以涉及以下文件的更改操作：

​	a. 把dol/examples/example1/src中的square.c 和square.h文件更名为cube.c和cube.h,同时修改文件中的函数名，涉及square的全部换成cube：

- cube.c

```c
#include <stdio.h>

#include "cube.h"

void cube_init(DOLProcess *p) {
    p->local->index = 0;
    p->local->len = LENGTH;
}

int cube_fire(DOLProcess *p) {
    float i;

    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        i = i*i*i;
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        p->local->index++;
    }

    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }

    return 0;
}
```

- cube.h

```c
#ifndef CUBE_H
#define CUBE_H

#include <dol.h>
#include "global.h"

#define PORT_IN  1
#define PORT_OUT 2

typedef struct _local_states {
    int index;
    int len;
} Cube_State;

void cube_init(DOLProcess *);
int cube_fire(DOLProcess *);

#endif
```



- 同时把dol/examples/example1/ 中example1.xml , map\_1.xml,  map\_3.xml三个文件中涉及square的都替换成cube，就可以完成更名操作。
- 完成之后重新在dol文件目录下执行编译指令，再运行一次example就完成全部操作了。

```sh
ant -f build_zip.xml all
cd build/bin/main
ant -f runexample.xml -Dnumber=1
```



## 3.实验感想

​	本次实验非常简单，在不太理解具体dol文件布局的情况下都可以很轻松地完成作业。通过改名操作了解了文件调用，模块进程布局的位置，稍微对dol的配置有所了解。