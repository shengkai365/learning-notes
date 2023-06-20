
# Python语法

## 一、保留字

| and      | elif    | import | raise  | global   |
| -------- | ------- | ------ | ------ | -------- |
| as       | else    | in     | return | nonlocal |
| assert   | except  | is     | try    | True     |
| break    | finally | lambda | while  | False    |
| class    | for     | not    | with   | None     |
| continue | from    | or     | yield  |          |
| def      | if      | pass   | del    |          |

评估函数`eval()`:去掉参数最外侧引号并执行余下语句的函数



## 二、数据类型

### 0. 布尔（bool）

- 形式：True、False
- 操作符：<、<=、>=、>、==、!=
- 组合保留字：and、or、not



### 1. 数字（int、float）

#### （1）整数类型，int()
- 整数类型的数据范围没有限制

| 形式       |     描述 | examples |
| :-------- | :--------| :------ |
| 十进制     |          |  `66`，`99`  |
| 二进制		| `0b/0B`开头 | `0b010`，`-0B101` |
| 八进制		| `0o/0O`开头 | `0o123`，`-0O456` |
| 十六进制	| `0x/0X`开头 | `0x9a`，`-0X89` |
####  （2）浮点数类型，float()

```python
 print(0.1+0.2==0.3)#False
 print(round(0.1+0.2,1)==0.3)#True
```
- 浮点数取值范围数量级约为`-pow(10,307)`到`pow(10,308)`，精度数量级`pow(10, -16)`
- 浮点间运算存在不确定尾数，不确定尾数一般发生在10的16次方左右
- `round(x,d)`：对x四舍五入，d是小数截取位，常用来辅助浮点间比较、运算

|  科学计数  | 浮点数    |
| :-------- |  --------|
| 4.3e-3    | 4.3*pow(10,-3)|
| 9.6E5		| 9.6*pow(10,5) |
#### （3）复数类型，complex()

```python
z=1 + 2j	#其中j=pow(-1,1/2)
z.real 		#获取实部1.0
z.imag 		#获取虚部2.0
```
#### （4）数值运算
 +、-、\*、/、//、%、\*\*、二元增强赋值操作符(如+=)
 abs()、divmod()、pow()、round()、max()、min()、int()、float()、complex()

| divmod(x,y)| pow(x,y[,z])|round(x[,d]) |
| :--------: | :--------:| :------: |
|商余，返回`(x//y,x%y)`|幂余，`(x\*\*y)%z`, z默认为1|四舍五入，d为保留小数位数，默认为0|

#### （5）取整

| 向上取整   |   向下取整 |   向零取整| 四舍五入 |
| :-------: | :-------:| :------: |:------: |
|math.ceil()|math.floor()、整除| int() | round() |

### 2. 集合（set）
#### （1）定义及创建
- 集合元素之间无序，每个元素唯一，是不可变数据类型
- 建立集合类型用{}或set()，建立空集合时必须用set()


#### （2）集合操作符

| 操作符及应用      |     描述 |
| :-------- | :-------- |
| S \| T   |   并，返回一个新集合，包含在集合S和T中所有的元素 |
| S - T	|  差，返回一个新集合，包含在S中但不包含在T中的元素|
| S & T	| 交，返回一个新集合，包含同时在S和T中的元素		|
| S ^ T	| 补，返回一个新集合，包括集合S和T中的非相同元素 	|
|S <= T 或 S < T	| 返回True/False，判断S和T的子集关系			|
|S >= T 或 S > T	| 返回True/False，判断S和T的包含关系			|
|S \|= T, S -= T, S &= T, S ^= T| 增强操作符							|



#### （3）集合处理方法

| 操作函数和方法      |     描述 |
| :-------- | :-------- |
| len(S)   	|  返回集合S的元素个数 |
| x in S	|  判断，x在集合S中，返回True，否则返回False|
| x not in S|  判断，x不在集合S中，返回True，否则返回False|
| set(x)	| 将其它数据类型变量x转化为集合类型 	|
| S.pop()	| 随机返回S中一个元素，更新S，若S为空产生KeyError	|
| S.copy()	| 返回集合S的一个副本			|
| S.add(x)  | 如果x不在集合S中，将x增加到S					|
|S.discard(x)| 移除S中元素x，如果x不在集合S中，不报错 |
|S.remove(x)|  移除S中元素x，如果x不在集合S中，产生KeyError  |
|S.clear()	|  移除S中所有元素 |



### 3.字典（dict）
#### （1）基本概念
- 映射关系采用键值对表达
- 字典类型使用`{}`和`dict()`创建，键值对之间用`:`分隔
- `d[key]`方式及可以索引，也可以赋值
#### （2）操作函数及方法

| 函数或方法  |    描述   |
| :-------- | :--------|
| k in d    |判断键k是否在字典d中，如果在返回True，否则返False|
| len(d)	|返回子典中元素的个数							|
| d.keys() 	|返回字典d中的所有键信息的可迭代类型				|
| d.values()|返回字典d中的所有值信息的可迭代类型				|
| d.items()	|返回字典d中的所有键值对信息，以元组的可迭代类型表现|
| d.get(k,<.default>)|键k存在返回相应值，不在返回<.default>	|
| d.pop(k,<.default>)|键k存在取出相应值，不在返回<.default>	|
| d.popitem()|随机从字典中取出一个键值对，以元组形式返回		|
| d.clear()	|删除所有键值对信息							|
| del d[k]	|删除键k对应的数据值，若k不存在产生KeyError		|




### 4. 序列类型（str、list、tuple）
#### 1. 序列类型定义
- 序列是一块可存放多个值的连续内存空间，这些值按一定顺序排列，可通过索引访问它们
- 序列类型包含：字符串、元组、列表

#### 2. 序列类型通用方法

| 函数      |     描述 |
| :-------- | :--------|
| len(s)	|返回序列s的长度，即元素个数 |
| min(s)	|返回序列s的最小元素，s中的元素需要可比较 |
| max(s)	|返回序列s的最大元素 |
| s.index(x)、s.index(x,i,j) |返回序列s从i开始到j位置中第一次出现x的位置 |
| s.count(x)|返回序列中出现x的总次数 |
| x in s	|判断x是否是序列s的元素  |
| x not in s|判断x是否是s的元素		|
| s + t	|连接两个序列s和t			|
| `s*n`或`n*s`	|将序列s复制n次			|
| s[i]		|索引，返回s中的第i个元素，i是序列的序号|
|s[i:j]、s[i:j:k]|切片，返回序列s中第i到j以k为步长的元素子序列|
|s += s1		|更新序列s，将序列s1元素增加到s中		|
|s *= n		|更新序列s，将其元素重复n次				|



### 5. 字符串（str）
#### （1）字符串处理函数

| 函数及使用      |     描述 |
| :-------- | :--------|
| len(x)    |长度，返回字符串x的长度 |
| str(x)	|任意类型x所对应的字符串形式 |
| hex(x)、oct(x) |整数x的16进制或8进制小写形式字符串，`hex(425)=="0x1a9"`|
| bin(x)	| 整数x的二进制小写形式字符串	|
| chr(x)	|x为Unicode编码，返回其对应的字符，`chr(65)=='A'`|
| ord(x)	|x为字符，返回其对应的Unicode编码 |



#### （2）字符串处理方法

| 方法及使用      |     描述 |  example |
| :-------- | :--------| :--------- |
| s.lower()、s.upper() |返回字符串副本，全部小/大写 |`"AbCdE".lower()=="abcde"`|
| s.split(sep=None)		|返回一个列表，由s根据sep被分割的部分组成 |`"a,b,c".split(',')==['a','b','c']`|
| s.count(sub)			|返回字符字串sub在s中出现的次数|`"an apple a day".count('a')==4`|
| s.replace(old,new)	|返回s的副本，所有old字串被替换我为new|`'abc'.replace('a','z')=='zbc'`|
| s.center(width[,fillchar]) |字符串s根据宽度width居中，fillchar默认为空格 |`'she'.center(5,'-')=='-she-'`|
| s.strip(chars)		|从s中去掉其左侧和右侧chars中列出的字符|`"=aaa= ".strip("=")=="aaa"`|
| s.join(iter)			|在iter可迭代类型中除最后一个元素外每个元素后增加一个s|`",".join('liu')=='l,i,u'`|


#### （3）字符串的格式化: `fromat()`

格式：` {<参数序号>: <格式控制标记>}`

| :        | <填充>   | <对齐>                             | <宽度>   | <,>        | <.精度>                        | <类型>                                 |
| -------- | -------- | ---------------------------------- | -------- | ---------- | ------------------------------ | -------------------------------------- |
| 引导符号 | 单个字符 | < 左对齐<br />> 右对齐<br />^ 居中 | 输出宽度 | 千位分隔符 | 浮点数精度<br />字符串最大长度 | 整数：b,c,d,o,x,X<br />浮点数：e,E,f,% |

```python
"{0:=^20}".format("Python")     					# '=======Python======='
"{0:*>20}".format("BIT")							# '*****************BIT'
"{:10}".format("BIT")								# 'BIT       '
"{0:,.2f}".format("12345.6789") 					# '2,345.68'
"{0:b},{0:c},{0:d},{0:o},{0:x},{0:X}".format(425)	# '110101001,Ʃ,425,651,1a9,1A9'
"{0:e},{0:E},{0:f},{0:%}".format(3.14)				# '3.140000e+00,3.140000E+00,3.140000,314.000000%'
```




### 6. 元组（tuple）
#### （1）元组类型定义
- 元组是一种序列类型，一旦创建就不能被更改
- 使用小括号（）或tuple（）创建，元素间用逗号分隔
- 可以使用或不使用小括号
- 因为创建后不能更改，没有特殊操作



### 7. 列表（list）

#### （1）定义
- 列表是一种序列类型，创建后可以随意修改
- 使用方括号`[]`或`list()`创建，元素间用逗号分隔
- 列表中各元素类型可以不同，无长度限制



#### （2）操作函数和方法

| 函数或方法      |    描述		|
| :-------- | :--------|
| ls[i]=x	|替换列表ls中i位置元素为x				|
| del ls[i]	|删除列表中i位置元素					|
| del ls[i:j:k]	|删除列表中切得到片的所有元素		|
| ls.append(x)	|在列表ls最后增加一个元素			|
| ls.insert(i,x)|在列表ls中i位置增加元素x			|
| ls.pop(i)		|将列表ls中i位置元素取出并删除		|
| ls.reverse()	|将列表ls中元素反转				|
| ls.remove(x)	|将列表ls中出现的第一个元素x删除		|
| ls.clear()	|删除列表ls中所有元素				|
| ls.copy()		|复制列表返回副本					|



## 三、控制成分

### 1.分支结构s
#### （1）二分支结构

```python
if <条件>:
    <语句块1>
else:
	<语句块2>
#紧凑型
<语句块1> if <条件> else <语句块2>
```
#### （2）多分支结构

```python
if <case1>:
    <sentence1>
elif <case2>:
	<sentence2>
......
else:
	<ssentence3>
```
#### （3）异常处理

```python
try:
    <sentence1>  #sentence1出现异常执行sentence2，正常执行sentence3
except:
	<sentence2>  #出现异常时执行
    raise xxxError("please xxxx.")	# 产生指定异常信息
else:
 	<sentence3>  #作为正常执行sentence1的奖励   
finally:
	<sentence4>  #不管是否出现异常，都执行sentence4
```


### 2.循环结构

#### （1）for循环

```python
for <variable> in <iter>:
	if <case>: 
	    break
	<sentence1>
else:
	<sentence2> 	#sentence2作为正常完成循环的奖励，即没有执行break  
```
#### （2）while循环

```python
while <case1>:
    if <case2>:
	    continue 	#跳过下面语句进入下一轮循环
	<sentence1>
else:
	<sentence2> 	#只要循环没有被break，执行sentence2
```





## 四、文件和数据处理
### 1. 文件的打开和关闭

```python
f = open(<file_path>,<mode>) 	#打开文件
f.close()  						#关闭文件
```
由于文件读写时都有可能产生IOError，一旦出错，后面的f.close()就不会调用。所以，为了保证无论是否出错都能正确地关闭文件，我们可以使用try ... finally来实现：

```python
try：
    f = open(<file_path>,<mode>)
finally:
	if f:
		f.close()
```
每次都这么写实在太繁琐，所以，Python引入了with语句来自动帮我们调用close()方法：

```python
# with的作用是自动调用close()方法
with open(<file_path>,<mode>) as f: 
    print(f.read())
```

`open(file_name,mode)`中mode参数：

| 文件打开模式  |    描述   |
| :--------:  | :-------- |
|   'r'	|只读模式，默认值，如果文件不存在，返回FileNotFountError|
|  'w'	|覆盖写模式，文件不存在则创建，存在则完全覆盖|
|  'x'	|创建写模式，文件不存在则创建，存在则返回FileExistsError|
|  'a'	|追加写模式，文件不存在则创建，存在则在文件最后追加内容|
|  'b'	|同r/w/x/a一起使用，二进制文件模式|
|  't'	|同r/w/x/a一起使用，文本文件模式，默认值|
|  '+'	|同r/w/x/a一起使用，在原功能的基础上增加同时读写的功能|



### 2. 文件的读取和写入

#### （1）操作方法

| 操作方法    |     描述  |
| :-------- | :--------|
| f.read(size=-1) |读入全部内容，如果给出参数，读入前size长度，返回字符串|
|f.readline(size=-1)|读入一行内容或前size长度，字符串形式返回|
|f.readlines(hint=-1)|读入文件所有行或前hint行，列表形式返回，一行代表一个元素|
|f.write(s)			|向文件写入一个字符串或字节流|
|f.writelines(lines)|将一个元素全为字符串的列表写入文件，不换行写入（紧凑）|
|f.seek(offset)	|改变当前文件操作指针的位置，offset含义如下：0-文件开头；1-当前位置；2-文件结尾|
#### （2）逐行遍历

```python
#方法一: 一次读入，分行处理
fname = input("请输入要打开的文件名称：")
fo = open(fname,"r")
for line in fo.readlines():
    print(line)
fo.close()

#方法二: 分行读入逐行处理，文件对象fo是一个迭代器
fname = input("请输入要打开的文件名称：")
fo = open(fname,'r')
for line in fo:   
	print(line)
fo.close()
```

#### （3）一维数据的读写

```python
#读入
with open(fname) as fo:
    txt = fo.read()
    ls = txt.split(',') #逗号分隔的文本文件

#写入
with open(fname,'w') as fo:
    s = ','.join(ls)
    fo.write(s)
```

#### （4）CSV文件的读写

```python
#读入
ls = []	#保存二维字符串列表
with open(fname) as fo:
	for line in fo:
		line = line.replace('\n','')
		ls.append(line.split(','))
#写入
with open(fname,'w') as fo:
	for item in ls:
		fo.write(','.join(item) + '\n')
```
