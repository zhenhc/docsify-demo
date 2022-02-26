做 Java 的肯定都接触过 Linux 系统，那么很多时候我们在开发的过程中都是把我们项目打成一个jar包，或者是war包的形式，然后通过 XFTP 上传到我们服务器的指定目录，然后运行一端启动脚本，让我们的项目变得可以访问 就像 `./sh service.sh start` 然后启动我们写好的 `sh` 的shell脚本。接下来我们就来学习一下关于 Shell 脚本是如何写出来的。

#### Shell 脚本

Shell 脚本是什么？Shell是一个命令解释器，它的作用是解释执行用户输入的命令及程序等，也就是说，我们用户每输入一条命令，Shell 就会相对应的执行一条命令。当命令或程序语句不在命令行下执行，而是通过一个程序文件来执行时，该程序文件就被称为Shell脚本。

在我们的 Shell 脚本中，会有各种各样的内容，赋值，计算，循环等一系列的操作，接下来我们就来看看这个 Shell 脚本怎么写吧

1.查看自己当前系统默认的 Shell

```
echo $SHELL
```

输出：/bin/bash

2.查看系统支持的Shell

```
cat /etc/shells
```

输出：

/bin/sh /bin/bash /usr/bin/sh /usr/bin/bash

也就是说，我们的云服务器是支持我们在这里给他安排 Shell 脚本的

##### Shell 脚本怎么写出来的

我们这时候先来安排一下 `sh` 的文件,创建一个文件夹，然后在其中创建一个 `sh` 的文件。

```
mkdir /usr/local/shelltest
touch test.sh
```

创建完成我们编辑一下内容

```
vim test.sh
#!/bin/bash
echo "Hello World Shell"
```

然后我们出来运行一下我们的 Shell 的第一个脚本

```
bash test.sh
```

出来的结果是 `Hello World Shell`

一个及其简单的脚本出现了，接下我们就分析一波我们写了点啥？

```
#!/bin/bash
```

\#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell

我们在之前也使用了 `echo $SHELL` 来查看了自己系统默认的是哪一种 `sh` 解析器，之前看到的是/bin/bash，所以我们在写 Shell 脚本的时候，我们在开头默认的约定中，我们写了这个是用 /bin/bash 来进行解释的，

那么我们如何像之前调用我们的当前目录中的 Shell 脚本一样去调用他呢？就像这个样子的 `./sh service.sh start`

1.授权，

我们先不授权试一下看看能通过 ./test.sh 进行调用么

bash: ./test.sh: Permission denied 会提示这个，也就是没有授权定义，

授权命令：chmod +x test.sh

2.执行 ./test.sh

然后调用就能正常输出了,就是说，在当前的目录下执行这个脚本命令。

##### Shell 脚本的变量

1. 定义变量和使用

变量命名实际上很简单，我们先来试一下

```
name=zhiyikeji
```

这时候我们怎么使用变量呢？实际上只要在前面加上一个符号就可以 $

```
echo $name
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo $name
zhiyikeji
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo ${name}
zhiyikeji
```

上面的两种写法都是可以的，外面的大括号加和不加区别不大，可以省略，直接就`$name` 就可以使用你定义的变量

使用括号的意义一般在于区别某些变量，比如你写了一串的内容，可能写的是 `echo $nameismyfriend`,如果连在一起，是不是有点尴尬，这时候就可以使用括号区别一下，`echo ${name}ismyfriend` 不使用括号的时候，他就去找nameismyfriend这个变量了，就无法出来我们要的效果。

1. 删除自己定义的变量

```
unset name
```

这时候我们就把我们刚才定义的 `name=zhiyikeji` 这个变量给去掉了，我们可以调用一下我们的变量看是什么？

```
echo $name
[root@iZbp10j01t7sgfqekyefpoZ ~]# unset name
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo $name
```

这是不是就证明我们自己定义的变量已经删除了

1. 只读变量

那么我们需要一个关键字，大家肯定能想到是什么关键字 `readonly`

我们先给name赋值，然后使用 readonly 设置只读，然后再改变一下试试，

```
[root@iZbp10j01t7sgfqekyefpoZ ~]# name=zhiyikeji
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo $name
zhiyikeji
[root@iZbp10j01t7sgfqekyefpoZ ~]# readonly name
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo $name
zhiyikeji
[root@iZbp10j01t7sgfqekyefpoZ ~]# name=ceshi
-bash: name: readonly variable
[root@iZbp10j01t7sgfqekyefpoZ ~]# 
```

竟然是真的，如果不设置只读，是不是会重新可以进行赋值，我们测试个年龄，

```
[root@iZbp10j01t7sgfqekyefpoZ ~]# age=10
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo $age
10
[root@iZbp10j01t7sgfqekyefpoZ ~]# age=20
[root@iZbp10j01t7sgfqekyefpoZ ~]# echo $age
20
```

所以我们就可以肯定，readonly就是设置只读的关键词，记住了么？

那么设置只读的变量可以删除么？毕竟总有杠精的面试官会提问这个棘手的问题，但是，阿粉试过的所有方式好像都是不行的，阿粉就直接重启了自己的服务器，这样临时的变量就不存在了！

##### Shell 脚本的流程控制

说真的，Shell脚本的流程控制数一般才是yyds，为什么这么说，因为你在写大部分的脚本的时候，流程控制的地方永远是最多的，判断，选择，等等一系列的函数，当时熟练使用的时候，就发现这东西确实很有意思。

##### IF

我们先说最简单的 `if else` 这也是我们最经常使用的判断，在写 `Shell` 脚本的时候,就不像我们的 Java 中直接写

```
if(...){
    
}else{
    ....
}
```

`Xshell` 中的语法就不是这个样子的，`Xshell`语法：

```
if ...
then
    
    ...
else
    ...
fi
```

末尾的 fi 就是 if 倒过来拼写,我们可以写一个 `if` 的脚本试一下这个流程能否理解。

```
#! /bin/bash
  
if [ $1 -gt 2 ];


then
        echo "值大于2"

else
        echo "值小于2"

        exit
fi
```

这里申明一下，

- `-ge`标识的是大于等于符号;
- `-le`表示的是小于等于符号;
- `-gt` 表示大于符号;
- `-lt` 表示小于符号;
- `-eq` 表示等于符号;
- `-ne` 表示不等于符号;

我们在上面这段脚本中写就是内容就是，我们给脚本传入一个值，然后比对这个值和2的大小关系，然后输出我们指定的内容。

运行后就能看到

```
[root@iZbp10j01t7sgfqekyefpoZ shelltest]# sh test2.sh 1
值小于2
[root@iZbp10j01t7sgfqekyefpoZ shelltest]# sh test2.sh 3
值大于2
```

`$1` 表示我们给 Shell 脚本输入的第一个参数, `$0`就是你写的shell脚本本身的名字,$2 是我们给 Shell 脚本传的第二个参数

大家在部署某些项目的时候，是不是启动命令就很简洁，就是 `sh service.sh start` 类似这种的，那我们来看看一般这种是怎么写的，这就用到了另外一块的内容，和 `if` 类似，在 Java 中也有，那就是 `Case`.

##### Case

我们先来看看 `Case` 的语法，

`case ... esac` 实际上就和 Java 中的 Case 是非常相似的，case 语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令.`esac`是一个结束的标志。

```
case 值 in
匹配值1)
    command1
    command2
    
    ;;
匹配值2）
    command1
    command2
    ;;
esac
```

光说不练，假把式，我们来搞一下试试写一个脚本来搞一下。就用我们刚才说的 `sh servic.sh start` 来进行测试。

```sh
case $1 in
  
start)
        #输出启动命令
        echo "start已经开始"
        ;;
stop)
        #输出停止命令
        echo "stop命令执行"
        ;;
esac
exit
```

我们来看看运行结果

```
[root@iZbp10j01t7sgfqekyefpoZ shelltest]# sh service.sh start
start已经开始
[root@iZbp10j01t7sgfqekyefpoZ shelltest]# sh service.sh stop
stop命令执行
```

那么这段 Shell 脚本是什么意思呢？其实很简单，匹配我们传入的第一个字符，和 `start` 还有 `stop` 进行比较，如果匹配上之后，输出命令，最后退出即可。

是不是感觉没有那么复杂了呢？

##### For

说到流程控制,那么肯定不能不说 for , 毕竟 for 循环在 Java 中那可是重头戏。

我们先看他的格式

```
for i in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

那么我们有没有说像是 Java 中那种 for 循环一样的方式呢？比如说这个`for ((i=1; i<=j; i++))`

实际上也是支持这种的，我们来写一个试试。

```
j=$1
for ((i=1;i<=j;i++))
do
        echo $i
done
```

执行一下看看

```
[root@iZbp10j01t7sgfqekyefpoZ shelltest]# sh fortest.sh 6
1
2
3
4
5
6
```

既然有 for 那是不是就有 while 呢？是的，没错，确实是有 while ，也是循环的意思，但是写法有略微不一样的地方

##### While

```
while condition
do
    command
done
```

我们来举个尝试打印九九乘法表来看一下

```
a=1
b=1
while ((a <=9))
do
        while ((b<=a))
                do
                        let "c=a*b"   #声明变量c
                        echo -n  "$a*$b=$c "
                        let b++
                done
                        let a++

     let b=1  #因为每个乘法表都是1开始乘，所以b要重置

             echo "" #显示到屏幕换行

     done
[root@iZbp10j01t7sgfqekyefpoZ shelltest]# sh whileTest.sh 
1*1=1 
2*1=2 2*2=4 
3*1=3 3*2=6 3*3=9 
4*1=4 4*2=8 4*3=12 4*4=16 
5*1=5 5*2=10 5*3=15 5*4=20 5*5=25 
6*1=6 6*2=12 6*3=18 6*4=24 6*5=30 6*6=36 
7*1=7 7*2=14 7*3=21 7*4=28 7*5=35 7*6=42 7*7=49 
8*1=8 8*2=16 8*3=24 8*4=32 8*5=40 8*6=48 8*7=56 8*8=64 
9*1=9 9*2=18 9*3=27 9*4=36 9*5=45 9*6=54 9*7=63 9*8=72 9*9=81 
```

是不是也挺简单的？

其实 Shell 脚本的编写一般都是在实际应用中提升，单纯的写测试脚本，也是可以让自己对知识的掌握比较充分，而我们一般都是写一些比较简单的脚本，复杂的不是还有运维么？