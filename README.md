# Project1 implement the naïve birthday attack of reduced SM3
## 生日攻击原理：

哈希碰撞的概率取决于两个因素（假设哈希函数是可靠的，每个值的生成概率都相同）。
取值空间的大小（即哈希值的长度），
整个生命周期中，哈希值的计算次数。
这个问题在数学上早有原型，叫做"生日问题"（birthday problem）：一个班级需要有多少人，才能保证每个同学的生日都不一样？

答案很出人意料。如果至少两个同学生日相同的概率不超过5%，那么这个班只能有7个人。事实上，一个23人的班级有50%的概率，至少两个同学生日相同；50人班级有97%的概率，70人的班级则是99.9%的概率

这意味着，如果哈希值的取值空间是365，只要计算23个哈希值，就有50%的可能产生碰撞。也就是说，哈希碰撞的可能性，远比想象的高，这种利用哈希空间不足够大，而制造碰撞的攻击方法，就被称为生日攻击（birthday attack）。


## cpu型号
11th Gen Intel(R)Core(TM)i7-11800H@2.30GHz

## 代码原理:

利用穷举攻击方法，随机生成一个字符串，并将其填充、调整成正确的输入格式后代入SM3加密函数中，得到对应的密文，存放在cipher数组中。

## 关键代码展示

```python
def SM3_birthday_attack(n):
    cipher = set()
    while True:
        list_r = random.randint(0, pow(2, 64))#生成64比特随机数
        m = padding(str(list_r))
        V = SM3(Input(m))
        text = ""
        #穷举
        for x in V:
            text += hex(x)[2:]
        # 这里的n表示4*n比特，取前4*n比特字符作为哈希值
        j = text[:n]
        if j in cipher:
            print("攻击成功！碰撞值：", j)
            break
        else:
            cipher.add(j)
```
## 未加入多线程结果展示：

16bit攻击时间：

<img src="https://github.com/1-14/Project1/blob/main/1.png" width="500px">

24bit攻击时间：

<img src="https://github.com/1-14/Project1/blob/main/2.png" width="500px">

32bit攻击时间：

<img src="https://github.com/1-14/Project1/blob/main/3.png" width="500px">

## 加入多线程结果展示

32bit攻击时间：

<img src="https://github.com/1-14/Project1/blob/main/4.png" width="500px">

## 结果分析

采用了比较单纯的穷举，可以看到16和24比特时运行速度还是可以的，但是32比特运行时间就骤增了。

当加入多线程后，32比特攻击时间降为10.5s左右，比单纯的穷举方法提速40%。






