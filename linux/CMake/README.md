### CMake练习

书写⼀个由 cmake 组织的 C++ ⼯程，要求如下：
1. include/hello.h 和 src/hello.c 构成了 libhello.so 库。hello.c 中提供⼀个函数 sayHello()，调⽤此函数时往屏幕输出⼀⾏“Hello SLAM”。我们已经为你准备了 hello.h 和 hello.c 这两个⽂件，见“code/”⽬录下。
2. ⽂件 useHello.c 中含有⼀个 main 函数，它可以编译成⼀个可执⾏⽂件，名为“sayhello”。
3. 默认⽤ Release 模式编译这个⼯程。
4. 如果⽤户使⽤ sudo make install，那么将 hello.h 放⾄/usr/local/include/下，将 libhello.so ⾄/usr/local/lib/下。

请按照上述要求组织源代码⽂件，并书写 CMakeLists.txt。