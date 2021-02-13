**一.统计英语文章词频并排序**
统计英语6级试题中所有单词的词频，并返回一个如下样式的字典

{'and':100,'abandon':5}

英语6级试题的文件路径./artical.txt

Tip: 读取文件的方法

```python
> def get_artical(artical_path):
    with open(artical_path) as fr:
        data = fr.read()
    return data
 
get_artical('./artical.txt')
```
处理要求

(a) '\n'是换行符 需要删除
(b) 标点符号需要处理
['.', ',', '!', '?', ';', '\'', '\"', '/', '-', '(', ')']
 

(c) 阿拉伯数字需要处理
['1','2','3','4','5','6','7','8','9','0'] 
(d) 注意大小写 一些单词由于在句首，首字母大写了。需要把所有的单词转成小写
'String'.lower()
(e) 高分项
通过自己查找资料学习正则表达式，并在代码中使用(re模块)

可参考资料：https://docs.python.org/3.7/library/re.html

代码实现如图

```python
def get_artical(artical_path):
    with open(artical_path) as fr:
        data = fr.read()
    return data

# get_artical()为自定义函数，可用于读取指定位置的试题内容。
txt=get_artical('./artical.txt')
for k in set([i for i in txt if i.isalpha() == False]): #只留下字符
    txt = txt.replace(k,' ') #其他字符均认为是单词分隔符
txt=txt.rstrip(' ').lower().split() #去掉空格，全部变小写，变为列表
#print(txt)

dic = {}
for word in txt:
    if word not in dic:
        dic[word] = 1
    else:
        dic[word] = dic[word] + 1
swd = sorted(dic.items(),key=lambda v:v[1],reverse=True)
print(swd)
```
部分输出结果：

>('the', 169), ('to', 95), ('a', 87), ('of', 82), ('and', 74), ('in', 61), ('is', 34), ('antarctic', 33), ('they', 30), ('their', 28), ('are', 25), ('on', 25), ('what', 25), ('that', 25), ('s', 23), ('for', 22), ('d', 21), ('b', 20), ('c', 20), ('fishing', 20), ('with', 19), ('it', 19), ('be', 18), ('as', 18), ('penguins', 18), ('you', 16), ('students', 15), ('king', 15), ('krill', 14), ('we', 13), ('about', 13), ('can', 13), ('one', 12), ('have', 12), ('will', 12), ('from', 12), ('passage', 11), ('do', 11), ('t', 11), ('them', 11), ('change', 11), ('breeding', 11), ('was', 10), ('new', 10), ('may', 10), ('not', 10), ('many', 10), ('grounds', 10)]

**二.继承与多态**
数据格式如下：

> stu5.txt 特长同学,2020-10-5,20,'男',180,87,98,77,76,92,58,-76,84,69,-47
stu6.txt 特长同学,2020-10-6,20,'女',230,76,48,82,88,92,58,-91,84,69,-68


以上两个txt文档在work路径下可以找到。

定义Spostudent、Artstudent为Student的子类，在子类的属性里面新增了spe为特长分数。Spostudent包括的top3方法返回的是最低的3个得分（可重复），Artstudent包括top3方法返回的是最高的3个得分（可重复），最后使用多态的方式输出2个特长同学的姓名、生日、年龄、性别、分数、特长分。

代码如下：

```python
class Spostudent(Student):
    def __init__(self,name,dob,age,gender,score,spe):
        Student.__init__(self,name,dob,age,gender,score)
        self.spe = spe
    
    def top3(self):
        return sorted(set([self.sanitize(t) for t in self.score]),reverse=False)[0:3]
    def sanitize(self,score_string):
        if '-' in score_string:
            score_string = score_string.replace('-', '')
        return int(score_string)

class Artstudent(Student):
    def __init__(self,name,dob,age,gender,score,spe):
        Student.__init__(self,name,dob,age,gender,score)
        self.spe = spe
    
    def top3(self):
        return sorted(set([self.sanitize(t) for t in self.score]),reverse=True)[0:3]
    def sanitize(self,score_string):
        if '-' in score_string:
            score_string = score_string.replace('-', '')
        return int(score_string)

def show(filename,student):
    data = get_coach_data(filename)
    name = data.pop(0)
    dob = data.pop(0)
    age = data.pop(0)
    gender = data.pop(0)
    spe = data.pop(0)

    if student == 'Spostudent':
        spostudent = Spostudent(name, dob, age, gender, data, spe)
        print('姓名：%s  生日：%s  年龄：%s  性别：%s  分数：%s  特长分：%s'%(spostudent.name,spostudent.dob,spostudent.age,spostudent.gender,spostudent.top3(),spostudent.spe))
    elif student == 'Artstudent':
        artstudent = Artstudent(name, dob, age, gender, data, spe)
        print('姓名：%s  生日：%s  年龄：%s  性别：%s  分数：%s  特长分：%s'%(artstudent.name,artstudent.dob,artstudent.age,artstudent.gender,artstudent.top3(),artstudent.spe))


show('work/stu5.txt','Spostudent')
show('work/stu6.txt','Artstudent')
```
输出结果：

> 姓名：特长同学  生日：2020-10-5  年龄：20  性别：'男'  分数：[56, 58, 69]  特长分：180
姓名：特长同学  生日：2020-10-6  年龄：20  性别：'女'  分数：[92, 91, 84]  特长分：230

继承与多态的使用可以大大简化代码量，提高工作效率。
