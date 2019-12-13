---
layout: post
tags: Tech
---
This notenook is taken according to the video ***Python advanced programming*** on [www.bilibili.com](https://www.bilibili.com/video/av19983355/?from=search&seid=1992914606051276112#page=3).


- [Bilibili Python Advanced programming](#bilibili-python-advanced-programming)
    - [Filter list dict and set](#filter-list-dict-and-set)
        - [List](#list)
        - [Dict](#dict)
        - [Set](#set)
    - [Name tuple](#name-tuple)
    - [calculate frequency](#calculate-frequency)
    - [Sort dict](#sort-dict)
    - [Find the common key in multiple dicts](#find-the-common-key-in-multiple-dicts)
    - [Iteration and Iterator](#iteration-and-iterator)
    - [reserve a list using iterator](#reserve-a-list-using-iterator)
    - [To slice an Iterator](#to-slice-an-iterator)
    - [To slice string with multiple seperator](#to-slice-string-with-multiple-seperator)
    - [endswith, startswith](#endswith-startswith)
    - [Format : alignment](#format-alignment)
    - [string substitute](#string-substitute)
    - [generator comprehansion](#generator-comprehansion)
    - [strip unwanted charaters](#strip-unwanted-charaters)
    - [reading binary file - .wav](#reading-binary-file---wav)
    - [file buffering](#file-buffering)
    - [映射文件](#%E6%98%A0%E5%B0%84%E6%96%87%E4%BB%B6)
    - [access file info & stat](#access-file-info-stat)
    - [use tempfile](#use-tempfile)
    - [XML : Parse and Create](#xml-parse-and-create)
    - [read and write Excel](#read-and-write-excel)
    - [class initialization](#class-initialization)
    - [`__slot__`](#slot)
    - [Context Management: `__enter__`, `__exit__`](#context-management-enter-exit)
    - [class propertise](#class-propertise)
    - [Type check in Class attribute](#type-check-in-class-attribute)
    - [gc.collect and weakref](#gccollect-and-weakref)
    - [call a function given the name of the function](#call-a-function-given-the-name-of-the-function)
    - [multiThread and inter communication](#multithread-and-inter-communication)


## Filter list dict and set

### List

```python
from random import randint
data = [randint(-10,10) for i in range(10)]
print(data)
```

> **TASK** :
To filter the negative numbers and get the positive ones

A most general solution is to use interation : `for`. Buuuut...

Example 1

```python
filter(lamnda x: x>= 0,data)
```

Example 2

```python
[x for x in data if x >= 0]
```

we can use ipython `timeit` to evaluate the performance

### Dict

```python
from random import randint
d = {x:randint(60,100) for x in range(1,11)}
{k : v for k,v in d.items() if v > 90}
```

### Set

```python
s = set(data)
{v for v in s if v % 3 == 0}
```

## Name tuple

Example 1

```python
Jim = ('Jim',16, 'male','jim123@gmail.com')
NAME, AGE, SEX, EMAIL = RANGE(4)
Jim[EMAIL]
```

Example 2

```python
from collections import namedtuple
Student = namedtuple('Student',['name','age','sex','email'])
Jim = Student(*Jim) #Jim from example 1
Jim = Student('Jim',16, 'male','jim123@gmail.com')
Jim['age']

```

## calculate frequency

Example 1

```python
from random import randint
d = [randint(0,20) for _ in range(100)]
c = dict.fromkeys(d,0)
for x in data:
    c[x] += 1
```

Example 2

```python
from collections import Counter
c = Counter(d)
c.most_common(3)
```

> If we want to calculation frequency of words in a article we may use the `Counter(re.split('\W+',txt))` and `most_common(n)`

## Sort dict

> sorted()

Example 1

```python
from random import randint
d = {x : randint(50,100) for x in 'xyzabc'}
sorted(zip(d.values(),d.keys()))
```

Example 2

```python
sorted(d.items(),key = lambda x : x[1] )
```

## Find the common key in multiple dicts

Example 1

```python
from random import randint,sample
s1 = {x : randint(1,4) for x in sample('abcdefg',randint(3,6))}
s2 = {x : randint(1,4) for x in sample('abcdefg',randint(3,6))}
s3 = {x : randint(1,4) for x in sample('abcdefg',randint(3,6))}
res = []
for k in s1: # key interation
    if k in s2 and k in s3 :
        res.append(k)
```

Example 2
> dict.keys() will generate `set-like` object and use set `&` to get intersection

`s1.keys() & s2.keys() & s3.keys()`

Example 3
> `map` and `reduce`

```python
from functools import reduce
#generate 10 random dict using list comprehansion
ss = [{x : randint(1,4) for x in sample('abcdefg',randint(3,7))} for _ in range(10)]
keys = map(dict.keys,ss)
reduce(lambda a,b :a & b, keys)
```

## Iteration and Iterator

Example 1 :

```python
l = [1,2,3]
for i in l:
    print(i)

iter(l)  #which will use l.__iter__()

s = 'abc'
for x in s:
    print(x)

iter(s)  #which will use s.__getitem__(index)
```

`iter(iterable)` will return a iterator and the iterator has a `.next()` method to get each element.
`For` loop will stop upon the **Exception** `StopIteration`

Example 2 :

```python
import requests
import json
from collections import Iterable,Iterator

Iterator.__abstractmethods_
Iterable.__abstractmethods__

class WeatherIterator(Iterator):
    def __init__(self,cities):
        self.cities = cities
        self.index = 0

    def __next__(self):
        if self.index == len(self.cities):
            raise StopIteration
        city  = self.cities[self.index]
        self.index += 1
        return self.getWeather(city)

    def getWeather(self,city):
        response = requests.get('http://wthrcdn.etouch.cn/weather_mini?city={}'.format(city))
        data = json.loads(response.content.decode())
        high = data['data']['forecast'][0]['high']
        low  = data['data']['forecast'][0]['low']
        return '%s : %s , %s' %(city,low,high)


class WeatherIterable(Iterable):
    def __init__(self,cities):
        self.cities = cities

    def __iter__(self):
        return WeatherIterator(self.cities)


for x in WeatherIterable(['北京','杭州','无锡','湖州']):
    print(x)
```

Example 3 :

```python
import requests
import json

class Weather():
    def __init__(self,cities):
        self.cities = cities

    def getWeather(self):
        for city in self.cities:
            yield self._getWeather(city)

    def _getWeather(self,city):
        response = requests.get('http://wthrcdn.etouch.cn/weather_mini?city={}'.format(city))
        data = json.loads(response.content.decode())
        high = data['data']['forecast'][0]['high']
        low  = data['data']['forecast'][0]['low']
        return '%s : %s , %s' %(city,low,high)

w = Weather(['北京','杭州','无锡','湖州'])

for x in w.getWeather():
    print(x)
```

Example 4 :

```python
def foo():
    print('first 1')
    yield 1

    print('first 2')
    yield 2

    print('first 3')
    yield 3

f = foo()
f.__next__()  # == next(f)
f.__next__()
f.__next__()
f.__next__()
```

## reserve a list using iterator

Example 1 :

```python
l = [1,2,3,4,5,6]
l.reverse()  # it will change the real order of the element within
l
l = [1,2,3,4,5,6]
l[::-1] # return a full list, large consumption/cost if the list is large
reversed(l)  #return an iterator,  the oppsite of 'iter(l)'
# iter(l) uses the l.__iter__
# reversed(l) uses the l.__reversed__
```

Example 2 :

```python
class FloatRange():
    def __init__(self,start,end,step = 0.1):
        self.start = start
        self.end = end
        self.step = step

    def __iter__(self):
        t = self.start
        while t <= self.end:
            yield t
            t += self.step

    def __reversed__(self):
        t = self.end
        while t >= self.start:
            yield t
            t -= self.step

f = FloatRange(1,4,0.5)
for i in f:
    print(i)
for i in reversed(f):
    print(i)
```

## To slice an Iterator

Example 1 :

```python
#read line by line
with open('xxxx') as f:
    for i in f:
        print(i)

# read the whole file
f.readlines() ; f.read()
```

Example 2 :

```python
# islice provide slice of iterator
from itertools import islice
with open('xxxx') as f:
    for i in islice(f,20,30):
        print(i)

#islice(f,50)  # 0 - 50
#islice(f,30,None) # 30 - EOF

l = list(range(20))
t = iter(l)
for i in islice(t,5,10):
    print(i)
print('==========')
for i in t:
    print(i)
```

## To slice string with multiple seperator

Example 1 :

```python
s = 'ab;cd|def|hi,jkl|mn\topq;rst,uvw\txyz'
res = s.split(';')
t= []
map(lambda x: t.extend(x.split('|')),res)
res = t
t =[]
map(lambda x: t.extend(x.split(',')),res)

# and so on...
```

> In **python3** the `map` function is a genarator which won't excute without interation. So we will use a for loop to exhaust it.

Example 2 :

```python
s = 'ab;cd|def|hi,jkl|mn\topq;rst,uvw\txyz'
def my_split(s,seps):
    res = [s]
    for sep in seps:
        print(sep)
        t = []
        for i in map(lambda x: t.extend(x.split(sep)),res):pass
        res = t
    # we will need to filter the empty list
    return [i for i in res if i]

my_split(s,'|,;\t')
# and so on...
```

Example 3 :

```python
#recommend
import re
s = 'ab;cd|def|hi,jkl|mn\topq;rst,uvw\txyz'
re.split('[,;\t|]+',s)
```

## endswith, startswith

Example 1 :

```python
import os,stat
os.listdir()
s ='g.sh'
s.endswith('.sh')
s.endswith('.py')
s.endswith(('.sh','.py')) # tuple, can't be list
#change the mode of some files
oct(os.stat('filename').st_mode)
os.chmod('filename',os.stat('filename').st_mode |stat.S_IXUSE)
```

## Format : alignment

Example 1 :

```python
s = 'abc'
s.ljust(10)
s.ljust(10,'-')
s.rjust(10,'-')
s.center(10,'-')

```

Example 2 :

```python
s = 'abc'
format(s,'<20')
'{:<20}'.format(s)
```

Following is a table illustrating syntax of format

| syntax | explanation|
| :---   | :--------  |
| fill   |Any character except `}`
| align  | < left <br> > right <br> ^ center <br> = pad between sign and digits for numbers
|  sign  | + force sign <br> - sign if needed <br> " " space or - as appropriate 
|   #    | prefix ints with 0b,0o,0x
|   0    | 0-pad numbers
| width  |Minimum filed width
|  ,     |use commas for grouping 
| .precision| Maximum field width for strings;<br> number of decimal palces for floats
|  type  | ints : b, c, d, n, o, x, X; <br> floats : e, E, f, g, G, n, %

## string substitute

Example 1 :

```python
s = '''
2016-05-23 aaa
2016-05-24 bbb
2016-05-27 ccc
'''
import re
res_1 = re.sub('(\d{4})-(\d{2})-(\d{2})',r'\2/\3/\1',s) # \1 \2 \3 the 1st 2nd  3rd group
print(res_1)
res_2 = re.sub('(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})',r'\g<m>/\g<d>/\g<y>',s)
print(res_2)
```

## generator comprehansion

```python
x = ['I','am',20,'years','old']
#' '.join(x) #str only
#use `(str(i) for i in x)` to create a generator to reduce cost
' '.join((str(i) for i in x))
```

## strip unwanted charaters

Method 1:
`strip()`, `rstrip()`,`lstrip()`

Method 2:
    use slice and join

Method 3:
`replace()` ,`re.sub()`

Method 4:
`translate()`

Example :

```python
#re.sub
import re
s = 'abc\tdef\ng'
re.sub('[\t\n]','',s)


# str.translate
s = 'abc1230323xyz'
t =str.maketrans('abcxyz','xyzabc')
s.translate(t)

def delete(s,chrs):
    deleteChar = (ord(i) for i in chrs)
    table = dict.fromkeys(deleteChar,None)
    return s.translate(table)


s = 'abc\tdef\ng'
chrs = '\n\t'
delete(s,chrs)
```

## reading binary file - .wav

Example :

```python
import struct, array
file  = 'start.WAV'
f = open(file,'rb')
#for a .wav file, first 44 bytes are info of the file and the rest is the music data.
#the music data is stored in a 'short' data structure
#so each data is 2 bytes in size
info = f.read(44)
struct.unpack('h',info[22:24]) # number of channels
struct.unpack('i',info[24:28]) # Sample rate
struct.unpack('h',info[34:36]) # bits per sample
f.seek(0,2)
n = (f.tell() -44 )//2  # the number of data
# / operator will always return a float while // will return a int when both of the numerator and denominator are int , otherwise float
data_buf = array.array('h',(0 for _ in range(n)))
f.seek(44)
f.readinto(data_buf)
f.close()
for i in range(n):
    data_buf[i] //= 8

f  = open('start_2.WAV','wb')
f.write(info)
data_buf.tofile(f)
f.close()
```

## file buffering

> python 默认缓冲方式为全缓冲：当缓冲区大小大于一个块大小时，写入  
f.open('demo.txt','w',buffering  = 2048)`  
如上，若改变缓冲区大小为 2048，即缓冲区写入2049个字节时，才会向硬盘写入 2048个字节/一个块  
当buffering = 1 时，为行缓冲，即写入 '\n' 时，写入硬盘  
当buffering = 0 时，无缓冲，实时写入  

_btw, in markdown 'space' + 'space' + 'return' is soft linebreak_

## 映射文件

> 文件映射到内存，用修改内存的方式来修改文件

Example :

```python
import mmap
f = open('demo.bin','r+b')
n = mmap.mmap(f.fileno(),0,access = mmap.ACCESS_WRITE)
n[0] = '\x88'
```

## access file info & stat

Example 1:

```python
import os,stat,time
os.stat(filename)
os.lstat(filename)
os.fstat(fileno)

s = os.stat(filename)
stat.S_ISDIR(s.st_mode) # file type
stat.S_IRUSR & s.st_mode # get whether user readable
time.localtime(s.st_atime) # get last access time
s.st_size  #file size
```

Example 2:

```python
import os
# use `os.path`
os.path.isdir()
os.path.isfile()
os.path.getatime()
os.path.getctime()
os.path.getsize()
```

## use tempfile

```python
from tempfile import TemporaryFile,NamedTemporaryFile
# we can't access TemporaryFile from the file system/file explorer,only the `f` variable.
# we can access the NamedTemporaryFile through file explorer
f = TemporaryFile()
f.write('abd'*1000)
f.seek(0)
f.read(100)

f = NamedTemporaryFile()
f = NamedTemporaryFile(delete = False) # won't delete file when closing file
```

## XML : Parse and Create

Example 1:

```python
from xml.etree.ElementTree import Element,ElementTree,tostring, parse
#create
e = Element('student')
e.set('name','shawn')
tostring(e)
e1 = Element('math')
e1_1 = Element('high_score')
e1_1.text = '100'       # only string allowed
e1_2 = Element('low_score')
e1_2.text = '90'
e1.append(e1_1)
e1.append(e1_2)
e.append(e1)
e2 = Element('English')
e2_1 = Element('high_score')
e2_1.text = '100'
e2.append(e2_1)
e.append(e2)
tostring(e)
et = ElementTree(e)
et.write('demo.xml')
# parse
et = parse('demo.xml')
root = et.getroot()
tostring(root)
list(root.find('math'))
root.attrib
root.getchildren()
for _ in root:
    print(_.tag)
root.find('math')
root.findall('math')
root.iter()
root.iter('math')
root.findall('math/*')
root.findall('.//high_core')
```

> for more information, please visit [The Python Standard Library Documentain](https://docs.python.org/3.6/library/xml.etree.elementtree.html).

**Supported XPath syntax**

Syntax | Meaning
----|----
`tag` | Selects all child elements with the given tag. For example, `spam` selects all child elements named `spam`, and `spam/egg` selects all grandchildren named `egg` in all children named `spam` .
`*` | Selects all child elements. For example, `*/egg` selects all grandchildren named`egg`.
`.` | Selects the current node. This is mostly useful at the beginning of the path, to indicate that it’s a relative path.
`//` | Selects all subelements, on all levels beneath the current element. For example, `.//egg` selects all `egg` elements in the entire tree.
`..` | Selects the parent element. Returns `None` if the path attempts to reach the ancestors of the start element (the element `find` was called on).
`[@attrib]` | Selects all elements that have the given attribute.
`[@attrib='value']` | Selects all elements for which the given attribute has the given value. The value cannot contain quotes.
`[tag]` | Selects all elements that have a child named `tag`. Only immediate children are supported.
`[tag='text']` | Selects all elements that have a child named `tag` whose complete text content, including descendants, equals the given`text`.
`[position]` | Selects all elements that are located at the given position. The position can be either an integer (1 is the first position), the expression`last()`(for the last position), or a position relative to the last position (e.g.`last()-1`).


Example 2:

```python
import csv
from xml.etree.ElementTree import Element,ElementTree,tostring, parse
def csvToXml(fname):
    with open(fname,'r') as f:
        reader = csv.reader(f)
        header = reader.next()

        root = Element('Data')
        for row in reader:
            eRow = Element('row')
            root.append(eRow)
            for tag,text in zip(header,row):
                e = Element(tag)
                e.text = text
                eRow.append(e)
    prettyxml(root) # pretty xml
    return ElementTree(root)

#In order for pretty print
def prettyxml(e,level = 0):
    if len(3) > 0:
        e.text = '\n' + '\t' * (level + 1)
        for child in e:
            prettyxml(child, level + 1)
        child.tail = child.tail[:-1]
    e.tail = '\n' + '\t' * level
```

## read and write Excel

```python
import xlrd

rbook = xlrd.open_workbook(fanme)
rsheet = rbook.sheet_by_index(0)
cell = rsheet.cell(0,0)
print(cell.value)
row = rsheet.row(1)
rsheet.put_cell()

import xlwt
style = xlwt.easyxf('align: vertical center,horizontal center')
wbook = xlwt.Workbook()
wsheet = wbook.add_sheet('sheet_1')
wsheet.write(row,col,value,style)
wbook.save('name.xlsx')
```

## class initialization

> Task : inherit from immutable type and change their initialization

```python
class IntTuple(tuple):
    def __new__(cls,iterable):
        g = (i for i in iterable if isinstance(i,int) and i > 0)
        print(cls.mro())
        self = super().__new__(cls,g)
        print(self)
        return self

    # here only __new__ is needed. The following function is redundant
    def __init__(self,iterable):
        #before not possible
        super().__init__() # still don't undertand why __init__ take no args
        #after not possible

a = IntTuple([1,3,4.4,-4,8,9.0,9])
```

I will know try to explain this :
> In order to initialize an instance, there are two steps
> 1. create a object of that class, what `__new__` will do
> 2. fill the object with details, what `__init__` will do  

+ the `__new__()` will **take** the `cls` and other args for initializatin and will **return** a object that will be passed to `self` in the `__init__()`.  
+ when I try to create a instance of IntTuple, `__new__` will be called with the parameter `iterable` and a class type `cls`, in this case , the `cls` is `IntTuple`  
+ `super()` or `super(IntTuple,self)`(for init instance) or `super(IntTuple,cls)`(for new an object) will get the father class `tuple`'s scope and appliy the scope to `IntTuple`, so the scope will be changed temporially. 
+ So, father will `__new__` and `__init__`  the child. AND the father will recursively call `super()` to get their father.

**Important Note**

+ If the father class is immuntable, the `__new__` method should be used to initialize the child object. `__init__` will take no effect.
+ In the example above, `__new__` will return the `self` containing (1, 3, 8, 9). The `__init__` should simply call `super().__init__()` with no args, which is why the `__init__` override is unnecessary. Also, the args `iterable` in `__init__` is not the `g` taken from `super().__new__(cls,g)`. Instead, it is still [1,3,4.4,-4,8,9.0,9]
+ So, for `__init__` and `__new__`, the parameters passed in are the **same**
+ for mutable object, still waiting to be explored.

## `__slot__` 

> use `__slot__` will reduce the cost

```python
class Player(object):
    def __init__(self, uid, name, status=0, levels=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level
    
class Player2():
    __slots__ = ['uid', 'name', 'status', 'level']
    def __init__(self, uid, name, status=0, levels=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level


p1 = Player('001', 'Jim')
p2 = Player2('002', 'Tom')
set(dir(p1)) - set(dir(p2)) # there will be a __dict__
# the __dict__ allow for dynamically add and delete attributes, which is not wanted in most cases, e.g :
print(p1.__dict__)
p1.x
p1.x = 123
print(p1.__dict__)
p1.__dict__['y']  =1
p1.y
def p1.__dict__['x']
p1.x
print(p1.__dict__)

import sys
sys.getsizeof(p1.__dict__)

p2.x = 1    # throw an error
```

## Context Management: `__enter__`, `__exit__`


```python
from telnetlib import Telnet
from sys import stdin, stdout
from collections import deque


class TelnetClient():
    def __init__(self, addr, port=23):
        self.addr = addr
        self.port = port
        self.tn = None

    def start(self):
        # user
        t = self.tn.read_until('login: ')
        stdout.write(t)
        user = stdin.readline()
        self.tn.write(user)

        # password
        t = self.tn.read_until('Password: ')
        if t.startswith(user[:-1]):
            t = t[len(user) + 1:]
        stdout.write(t)
        self.tn.write(stdin.readline())

        t = self.tn.read_until('$ ')
        stdout.writ(t)
        while 1:
            uinput = stdin.readline()
            if not uinput:
                break
            self.history.append(uinput)
            self.tn.write(uinput)
            t = self.tn.read_until('$ ')
            stdout.write(t[len(uinput) + 1:])

    def __enter__(self):
        self.tn = Telnet(self.addr, self.port)
        self.history = deque()
        # here the `self` will be passed to the variable after `as` -> 'clinet'
        return self 

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.tn.close()
        self.tn = None
        with open(self.addr + 'history.txt', 'a') as f:
            f.writelines(self.history)


with TelnetClient('127.0.0.1') as clinet:  
# the client is the `self` __enter__ returned
    client.start()


```

## class propertise

Example 1
```python
from math import pi

class Circle():
    def __init__(self, radius):
        self.radius = radius

    def getRadius(self):
        return self.radius

    def setRadius(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self.radius = val
    
    def getArea(self):
        return self.radiud ** 2 * pi
```

Example 2
```python
from math import pi

class Circle():
    def __init__(self, radius):
        self._radius = radius

    def getRadius(self):
        return self._radius

    def setRadius(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._radius = val
    
    def getArea(self):
        return self._radius ** 2 * pi

    R = property(getRadius, setRadius)
    #property(fget=None,fset=None,fdel=None,doc=None)
```

Example 3
```python
from math import pi

class Circle():
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._radius = val
    
    def getArea(self):
        return self._radius ** 2 * pi

    def __repr__(self):
        return "<Circle ({})>".format(self._radius)

    def __eq__(self, obj):
        return self.getArea() == obj.getArea()

    def __gt__(self, obj):
        return self.getArea() > obj.getArea()


class Rectangle():
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def width(self):
        return self._width
    
    @width.setter
    def radius(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._width = val
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._height = val

    def getArea(self):
        return self._width * self._height

    def __repr__(self):
        return "<Rectangle ({0},{1})>".format(self._width,self._height)

#    def __eq__(self, obj):
#        return self.getArea() == obj.getArea()
#
#    def __gt__(self, obj):
#        return self.getArea() > obj.getArea()

c = Circle(5)
r = Rectangle(4,5)
r > c

```

```python
from math import pi
from abc import ABCMeta, abstractmethod

class Shape():
    @abstractmethod
    def area(self):
        pass

    def __eq__(self, obj):
        if not isinstance(obj,Shape):
            raise TypeError('obj not a shape')
        return self.getArea() == obj.getArea()

    def __gt__(self, obj):
        if not isinstance(obj,Shape):
            raise TypeError('obj not a shape')
        return self.area() > obj.area()


class Circle(Shape):
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._radius = val
    
    def area(self):
        return self._radius ** 2 * pi

    def __repr__(self):
        return "<Circle ({})>".format(self._radius)


class Rectangle(Shape):
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def width(self):
        return self._width
    
    @width.setter
    def radius(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._width = val
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, val):
        if not isinstance(val,(int, float)):
            raise ValueError('wrong radius type')
        self._height = val

    def area(self):
        return self._width * self._height

    def __repr__(self):
        return "<Rectangle ({0},{1})>".format(self._width,self._height)


c = Circle(5)
r = Rectangle(4,5)
r > c
```


## Type check in Class attribute

```python
class Attr():
    def __init__(self,name,type_):
        self.name = name
        self.type_ = type_
    
    def __get__(self,instance,cls):
        print(self)
        print(instance)
        print(cls)
        print(instance.__dict__)
        print(self.__dict__)
        return instance.__dict__[self.name]
    
    def __set__(self,instance,val):
        if not isinstance(val, self.type_):
            raise TypeError('expected an %s' %self.type_)
        instance.__dict__[self.name] = val

    def __delete__(self, instance):
        del instance.__dict__[self.name]


class person():
    name = Attr('name', str)
    age  = Attr('age',  int)
    height = Attr('height',float)


p = person()
p.name = 'bob'
print(p.name)
p.age = '1'
print(p.age)
```


## gc.collect and weakref

```python
import weakref
obj = Circle(1)
o_wref = weakref.ref(obj)
obj_ = o_wref()
obj is obj_
#import gc
#gc.collect()
```


## call a function given the name of the function

```python
class Circle():
    def __init__(self, radius):
        self._radius = radius

    def getArea(self):
        return self._radius ** 2 * 3.14


class Rectangle():
    def __init__(self, width, height):
        self._width = width
        self._height = height

    def get_area(self):
        return self._width * self._height

c = Circle(3)
r = Rectangle(3,5)

def area1(shape):
    for name in ['get_area','getArea']:
        f = getattr(shape, name, None)
        if f:
            return f()


from operator import methodcaller
s = 'abc123abc456'
print(s.find('abc',4))
f = methodcaller('find','abc',4)
print(f(s))

def area2(shape):
    for name in ['get_area','getArea']:
        if hasattr(shape, name):
            f = methodcaller(name)
            return f(shape)


area1(c)
area1(r)
area2(r)
area2(c)
```


## multiThread and inter communication


Thread

```python
from threading import Thread

def something(x):
    print(str(x))

#Method 1
#t = Thread(target = something,args = (1,))
#t.start()

#Method 2
class MyThread(Thread):
    def __init__(self,x):
        self.x = x
        super().__init__()

    def run(self):
        something(self.x)

threads =  []

for i in range(10):
    t = MyThread(i)
    threads.append(t)
    t.start()

for t in threads:
    t.join()

print('Done')
```

Writing, Reading and signaling between Threads

```python
from threading import Thread, Event
import os
import tarfile
import time
import random, string
from queue import Queue

class dThread(Thread):
    def __init__(self, uid, queue):
        self.uid = uid
        self.queue = queue
        super().__init__()

    def download(self):
        time.sleep(5)
        random.seed(self.uid)
        txt = ''.join(random.sample(string.ascii_letters,10))
        self.queue.put((self.uid, txt))
        return

    def run(self):
        self.download()


class wtThread(Thread):
    def __init__(self, queue, wtEvent, tarEvent):
        self.queue = queue
        super().__init__()
        self.wtEvent = wtEvent
        self.tarEvent = tarEvent

    def write(self):
        count = 0
        while 1:
            uid, txt = self.queue.get()
            if uid == -1:
                self.tarEvent.set()
                self.wtEvent.wait()
                break
            with open(str(uid) + '.txt', 'w') as f:
                f.write(txt)
                count += 1
            if count == 5:
                self.tarEvent.set()
                self.wtEvent.wait()
                self.wtEvent.clear()
                count = 0

    def run(self):
        self.write()


class tarThread(Thread):
    def __init__(self, wtEvent, tarEvent):
        super().__init__()
        self.count = 0
        self.wtEvent = wtEvent
        self.tarEvent = tarEvent
        self.setDaemon(True)

    def tar(self):
        self.count += 1
        tfname = '{}.tgz'.format(self.count)
        tf = tarfile.open(tfname, 'w:gz')
        for fname in os.listdir('.'):
            if fname.endswith('.txt'):
                tf.add(fname)
                os.remove(fname)
        tf.close()

        if not tf.members:
            os.remove(tfname)

    def run(self):
        while 1:
            self.tarEvent.wait()
            self.tar()
            self.tarEvent.clear()
            self.wtEvent.set()


q = Queue()
wtEvent = Event()
tarEvent = Event()
tarthread = tarThread(wtEvent,tarEvent)
wtthread = wtThread(q, wtEvent,tarEvent)
downthreads = [dThread(i, q)  for i in range(1,18)]

wtthread.start()
tarthread.start()
for t in downthreads:
    t.start()
for t in downthreads:
    t.join()

q.put((-1, None))
print('Main thread, Exiting')
```


```python
from concurrent.futures import ThreadPoolExecutor
import time

executor = ThreadPoolExecutor(3)

def f(a,b):
    print('f', a, b)
    time.sleep(5)
    return a ** b

executor.submit(f, 2, 3)
res = executor.submit(f, 2, 3).result()
executor.map(f, [2, 3, 5, 6 ,7], [4, 5, 6, 7, 8])
```



```python
import os, cv2, time, struct, theading
from BaseHTTPServer import HTTPServer, BaseHTTPRequestHandler
from socketServer import Thread, RLock
from select import select

class JpegStreamer(Thread):
    def __init__(self, camera):
        super().__init__()
        self.cap = cv2.VideoCapture(camera)
        self.lock = RLock()
        self.pipes = {}
    
    def register(self):
        pr, pw = os.pipe()
        self.lock.acquire()
        self.pipes[pr] = pw
        self.lock.release()
        return pr
        
    def unregister(self, pr):
        self.lock.acquire()
        self.pipes.pop(pr)
        self.lock.release()
        pr.close()
        pw.close()

    def capture(self):
        cap = self.cap
        while cap.isOpened():
            ret, frame = cap.read()
            if ret:
                ret.data = cv2.imencode('.jpg',
                frame, (cv2.IMWRITE_JPEG_QUALITY, 40))
                yield data.tostring()

    def send(self,frame):
        n = struct.pack('l', len(frame))
        self.lock.acquire()
        if len(self.pipes):
            _, pipes, _ = select([].self.pipes.itervalues(),[],1)
            for pipe in pipes:
                os.write(pipe,n)
                os.write(pipe,frame)

            self.lock.release()

    def run(self):
        for frame in self.capture():
            self.send(frame)

    
class JpegRetriver():
    def __init__(self, streamer):
        self.streamer = streamer
        self.pipe = streamer.register()

    def retrieve(self):
        while 1:
            ns = os.read(self.pipe, 8)
            n = struct.unpack('l', ns)[0]
            data = os.read(self.pipe,n)
            yield data

    def cleanup(self):
        self.streamer.unregister(self.pipe)

class Handler(BaseHTTPRequestHandler):
    retriever = None
    @staticmethod
    def setJpegRetriever:
        Handler.retriever = retriever

    def do_GET(self):
        if self.retriver is None:
            raise RuntimeError('No retriever')
        
        if self.path != '/':
            return
        
        self.send_response(200)
        self.send_header('Content-type', 'multipart/x-mixed-replace;boundary=abcde')
        self.send_headers()

        for frame in self.retriever.retrieve():
            self.send_frame(frame)

    def send_frame(self, frame):
        self.wfile.write('--abcde\r\n')
        self.wfile.write('Content-Type: image/jpeg\r\n')
        self.wfile.write('Content-Length: %d' %len(frame))
        self.wfile.write(frame)

if __name__ == '__main__':
    streamer = JpegStreamer(0)
    streamer.start()
    retriever = JpegRetriever(streamer)
    Handler.setJpegRetriever(retriever)
    print('starting...')
    httpd = TCPServer(('',9000), Handler)
    httpd.server_forever()






```
