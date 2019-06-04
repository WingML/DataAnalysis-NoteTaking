> 相信绝大多数学习此课程的朋友和我一样都是有一定 Python 使用基础的，所以这一部分完全是提纲挈领式地带过了，推荐有需要的小伙伴通过其他途径学习基础语法

# python 基础语法：
1. 版本选择：2.7.x 目前在商业项目中仍是主流，不可忽略；如果可能的话尽量选择 3.x，因为前者到 2020 年就不再有人维护了
2. IDE推荐：个人首推 Jupyter NoteBook；PyCharm 适合项目开发，Sublime Text 轻便快速（还好看），Vim 是大佬专属的武器，Eclipse+PyDev 适合使用过 Java 的朋友
3. 输入与输出：raw_input (2.7）或 input (3.x）；print()，有传统的 % 式，3.x 新出的 format 式以及最新的 f-string 式，自个了解下即可，个人首推第二种
4. 判断语句：if, elif, else
5. 循环语句：for, while；break, continue；注意 range 的用法
6. 数据类型：tuple, list, dict, set；进阶的有 namedTuple, defaultdict 等，可在 collections 中导入
7. 注释语句：# 用于短语句，较长的注释一般会用 '''''' 或 """""" （其实也就是长字符串）；代码文件首行有时会出现 # --coding: utf-8 - 之类的语句，各有作用，比如这句是指定了注释内容编码方式为 utf-8，适用于有中文注释的情况
8. 引用模块：import; from …(package) import …/*(module，* 表示引入 package 中所有 module)，有多个的话用逗号隔开；一般 from 后边跟着的是 package，而 import 后边跟着的是 module——实际上是直接引用一个 .py 文件或通过其他方法间接引用 .py 文件。因此这种从 package 中 import module 的方法实际上是从一个目录中引用某个 module，也要求目录结构中添加有 __init__.py 文件，可以参考下 sklearn 的源码结构
9. 函数：长函数用 def, 短函数用 lambda；函数中有嵌套、闭包、修饰等用法；如果在函数中用到全局变量，需用 global 关键词来声明；如果在嵌套函数中用到上一层的函数变量，则用的是 nonlocal 而非 global
10. 练习平台建议：leetcode 比较初级，Kaggle/天池 很有实战性，各种 ACM Online Judge 是比较高阶的玩法

# Numpy
1. 推荐理由：
	a. Numpy 的主要数据结构为 array，与 list 类似，不同点在于前者为连续存储，因而只需要记录一个地址指针，而后者为离散存储，每个元素都需要有一个指针，故使用起来需要消耗更多的计算资源。
	b. Numpy 在访问内存时使用的是 CPU 的矢量化指令，简单理解就是直接用了最接近计算机底层的语言，因而效率更高。
	c. Numpy 充分采用了多线程的开发方式，CPU 越多核其效率优势越明显。
	d. 一个 Tips：尽量对原变量进行操作，而不要隐式地拷贝生成新的变量，如尽量用 x *= 2 而非 y = x * 2。
2. 数据结构—— ndarray object：
	a. Nd 是多维/秩/轴的意思，创建方式有 np.array, np.ndarray 等等，参数可以是 list/tuple/另一个 ndarray。
	b. 结构数组：类似于 C++ 中的 struc，通过 np.dtype 来为 ndarray 每一项中的各项定义【名称】和【变量类型】，如 test = np.array([('myname', 100, 200), ('yourname', 300,400)], dtype={'names': ['name', 'id1', 'id2'], 'formats': ['U2', 'i', 'i']})中其实只有 2 个 tuple 元素，每个 tuple 中有 3 个元素，元素名与 dtype 中的 names 对应，元素的变量类型与 dtype 中的 formats 对应。这样一来就可以用诸如 test['id1'] 的方式来调用同个名称下的元素了。**强调一下：必须是 outer-list + inner-tuple 的组合**！
3. 重要操作—— ufunc：
	a. 创建数组：np.arange(start, excluded_end, gap), np.linspace(start, included_end, num_of_space), np.logspace(start, included_end, num_of_space) 等等。
	b. 算数运算：np.add(a, b), np.subtract(a, b), np.multiply(a, b)（约等于 * ）,np.dot(a, b) （建议用 @ ）, np.divide(a, b), np.power(a, power), np.remainder(a, b)/np.mod(a, b) 等等。
	c. 统计函数：np.amin(a, axis), np.amax(a, axis), np.min(), np.max(), np.ptp(a, axis)（统计最值之差）, np.percentile(a, per, axis)（统计百分位数）, np.mean(), np.median(), np.mode(), np.average(), np.std(), np.var() 等等
	d. 排序：np.sort(a, axis, kind, order)（支持快速排序，归并排序与堆排序）
4. 思考题：用 Numpy 统计下表 5 名学员在 3 门学科中的 平均成绩，最小成绩，最大成绩，方差，标准差，将按总分排序
![avatar](https://raw.githubusercontent.com/WingML/DataAnalysis-NoteTaking/master/pictures/04.jpg)
	
```python
import numpy as np

mytype = np.dtype({'names': ('name', 'Chinese', 'English', 'Math'), 
                  'formats': ('U2', 'i', 'i', 'i')})
# note that outer-listed(rather than tupled) if necessary for np to recognize here
data = [('张飞', 66, 65, 30), 
       ('关羽', 95, 85, 98), 
       ('赵云', 93, 92, 96), 
       ('黄忠', 90, 88, 77), 
       ('典韦', 80, 90, 90)]
# the following would cause ValueError
# data = [['张飞', 66, 65, 30], 
#        ['关羽', 95, 85, 98], 
#        ['赵云', 93, 92, 96], 
#        ['黄忠', 90, 88, 77], 
#        ['典韦', 80, 90, 90]]

grades = np.array(data, dtype=mytype)

def stata(subjects, func):
    global grades
    result = np.array([.0] * len(subjects))
    for i, sub in enumerate(subjects):
        result[i] = func(grades[sub])
    
    return result

subjects = mytype.names[1:]
avg_grade = stata(subjects, np.mean)
min_grade = stata(subjects, np.amin)
max_grade = stata(subjects, np.amax)
var_grade = stata(subjects, np.var)
std_grade = stata(subjects, np.std)

# tot_grade = np.sum(grades[:][1:], axis=1)
ranking_grade = sorted(grades, key=lambda x: x[1] + x[2] + x[3], reverse=True)

print('{: <10s}{: <10s}{: <10s}{: <10s}'.format('Stata', 'Chinese', 'English', 'Math'))
print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Average', avg_grade))
print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Min', min_grade))
print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Max', max_grade))
print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Variance', var_grade))
print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Std', std_grade))

print('\nRanking')
for i, person in enumerate(ranking_grade):
    print(i + 1, person)
```
	
# Pandas
1. 推荐理由：
	a. 其基础的数据结构与 json 契合度很高，方便转换。
	b. 其整合了 Numpy 的诸多用法，数据操作方便。
	c. 方便与 Excel 和 SQL 交互。
2. 数据结构——Series & DataFrame
	a. Series 为定长的字典序列，在存储时相当于两个 ndarray，一个存 index，一个存 values。
	b. Series 生成方法最常见的为 pandas.Series(data, index)，其中 index 可选；data 可以是 list/tuple/ndarray/dict。
	c. DataFrame 是 Series 的升级版，包含了两种 index，一种是如上的行索引 index，另一种是列索引 columns；可以看作是多个 Series 拼接成的新字典。
	d. DataFrame 生成方式为 pandas.DataFrame(data, index, columns)，其中 index 与 columns 可选；data 可以是 list/tuple/ndarray/dict。
3. 数据处理（先生成一个变量 df = pandas.DataFrame(data) 方便说明，注意下面有些方法可以用 pandas.xxx 的方法使用，而有些只能生成变量后用 df.xxx 的方法调用）：
	a. 导入/出 df.read_excel/.read_csv；df.to_excel/to_csv 可以与 xlsx/xls/csv 等文件交互，不过用法略有不同。
	b. 数据清洗：df.drop(columns/index, how) 用于删除指定的列/行；df.rename(columns, inplace) 用于修改列名；df.drop_duplicates() 用于去除重复的行。
	c. 格式问题：df[column].astype() 用于修改指定列的数据类型；df[column].map() 用于对指定列执行特定操作，如 map(str.strip) 是去除指定列中 str 元素首尾的空格；也可以直接用 df[column].str.strip('$') 的方法调用，这种方法的好处在于方便传入默认值以外的参数，如这里传入 ￥ 指的是去除元素首尾的 $ 符号；df[column].str.upper/lower/title 用于处理大小写转换；注意在 columns 没有空格或其他特殊字符的情况下， df[column] 等价于 df.column；df.isnull() 用于查找缺失值，df.isnull().any() 用于查找哪一列有缺失值。
	d. Apply 函数：上述几乎所有的方法，都可以用 df.apply 来代替，如 df.column.str.upper() 等价于 df.column.apply(str.upper)；不过 apply 更常用的是传入自定义的 def 或 lambda 匿名函数。
	e. 数据合并：merge，有 inner/outer/left/right/on 等合并方式，与 SQL 原理相同；类似的还有 concat 与 join。
4. 数据统计：
	a. 神级操作：describe()！
	b. 单独的操作：count, min, max, sum, mean, median, var, std, argmin, argmax, idxmin, idxmax。其中大部分含义自明，需要强调的是：argmin 统计最小值所在的索引位置（i.e. 第几个），idxmin 统计最小值的索引值（i.e. 索引名称）。
5. 用 SQL 的方式打开：
	a. 工具：pandasql，主要模块有 sqldf, load_meat, load_births 等等。
	b. 举例：
	pysql = lambda statement: sqldf(statement, globals())
	sql = "select * from df where name = 'myname'"
	pysql(sql)
	这里定义了一个匿名函数，而 sqldf 中的 globals() 是指传入全局参数，因为笔者此处用到的 df 为全局变量。
6. 思考题：对下表使用 Pandas 进行创建与数据清洗，并新增一列“总和”计算每个学员的总成绩
![avatar](https://raw.githubusercontent.com/WingML/DataAnalysis-NoteTaking/master/pictures/05.png)

```python
import pandas as pd


# data building
subjects = ('Chinese', 'English', 'Math')
names = ('张飞', '关羽', '赵云', '黄忠', '典韦', '典韦')
grades = [(66, 65, ), 
         (95, 85, 98), 
         (95, 92, 96), 
         (90, 88, 77), 
         (80, 90, 90), 
         (80, 90, 90)]

df = pd.DataFrame(grades, columns=subjects, index=names)

# data cleaning
df.drop_duplicates(inplace=True)
df['Math'].fillna(df['Math'].mean(), inplace=True) # 感觉这个操作有点高估了张飞 emmmm

# sum up and sort by total scores
df['Total'] = df.sum(axis=1)
df.sort_values(by='Total', ascending=False)
```
