# Project1 implement the naïve birthday attack of reduced SM3
生日攻击原理：

哈希碰撞的概率取决于两个因素（假设哈希函数是可靠的，每个值的生成概率都相同）。
取值空间的大小（即哈希值的长度），
整个生命周期中，哈希值的计算次数。
这个问题在数学上早有原型，叫做"生日问题"（birthday problem）：一个班级需要有多少人，才能保证每个同学的生日都不一样？

答案很出人意料。如果至少两个同学生日相同的概率不超过5%，那么这个班只能有7个人。事实上，一个23人的班级有50%的概率，至少两个同学生日相同；50人班级有97%的概率，70人的班级则是99.9%的概率

这意味着，如果哈希值的取值空间是365，只要计算23个哈希值，就有50%的可能产生碰撞。也就是说，哈希碰撞的可能性，远比想象的高，这种利用哈希空间不足够大，而制造碰撞的攻击方法，就被称为生日攻击（birthday attack）。


代码原理：
利用穷举攻击方法，随机生成一个字符串，并将其填充、调整成正确的输入格式后代入SM3加密函数中，得到对应的密文，存放在cipher数组中。

结果展示：
