### shell语法中grep命令的匹配

文本:

001 | 面包 | 10
002 | 牛奶 | 12
004 | 蛋糕	 | 101

```shell
grep "面包" test.txt 
```

输出:

001 | 面包 | 10

```shell
grep $'\t' test.txt 
```

输出:

004 | 蛋糕       | 101

```shell
grep $'\u2002' test.txt 
```

输出:

001 | 面包 | 10
002 | 牛奶 | 12
004 | 蛋糕       | 101