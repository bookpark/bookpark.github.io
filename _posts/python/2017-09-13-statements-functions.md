---
layout: post
title:  "제어문, 함수"
categories: Python
---

# 제어문

## 조건표현식

```
참일경우 if 조건식 else 거짓일 경우
```
```
print('Good') if is_holiday else print('Bad')
```

## for문

```
for 항목 in 순회가능(iterable)객체:
   <항목을 사용한 코드>
```
```python
>>> champion_list = ['lux', 'ahri', 'leesin', 'ezreal']
>>> for champion in champion_list:
... 	print(champion)
lux
ahri
leesin
ezreal
```

## 컴프리헨션 (Comprehension)

* iterable한 객체로부터 파이썬의 자료구조를 만드는 방법. 가독성과 사용성에서 이득을 얻을 수 있을 경우 항상 사용해준다.

### 리스트 컴프리헨션

```python
[표현식 for 항목 in iterable 객체]
```
```python
>>> [item for item in range(1, 6)]
[1, 2, 3, 4, 5]
```

### 셋 컴프리헨션

```python
{표현식 for 표현식 in iterable 객체}
```
```python
gugu_list = [{'title': '%d단' % x, 'items': ['%d x %d = %d' % (x, y, x * y) for y 
	in range(1, 10)]} for x in range(2, 10)]
```

---

# 함수

반복적인 작업을 하는 코드를 재사용이 가능하게 정의해 놓은 것.

```python
def 함수명(매개변수[parameters]):
	동작
```

### 함수의 정의, 실행

```python
# 실행 시 'call func'를 print하는 함수 정의
>>> def func():
...   print('call func')
... 

# 함수 자체는 function객체를 참조하는 변수
>>> func
<function func at 0x10dabf378>

# 함수를 실행시키기 위해 () 사용
>>> func()
call func
```

### 리턴값이 있는 함수 정의

```python
>>> def return_true():
...   return True
... 
>>> return_true()
True
```

### 매개변수를 사용하는 함수 정의

```python
>>> def print_arguments(something):
...   print(something)
... 
>>> print_arguments('ABC')
ABC
```

### 매개변수(parameter)와 인자(argument)의 차이

함수 내부에서 함수에게 전달되어 온 변수는 매개변수라 부르며, 함수를 호출할 때 전달하는 변수는 인자로 부른다.


```
# 함수 정의때는 매개변수
def func(매개변수1, 매개변수2):
  ...
  
# 함수 호출시에는 인자
>>> func(인자1, 인자2)
```

### 리턴값이 없을 경우

함수에서 리턴해 주는 값이 없을 경우, 아무것도 없다는 뜻을 가진 None 객체를 얻는다.

### 위치 인자(Positional arguments)

매개변수의 순서대로 인자를 전달하여 사용하는 경우

```python
>>> def student(name, age, gender):
...   return {'name': name, 'age': age, 'gender': gender}
... 
>>> student('hanyeong.lee', 30, 'male')
{'name': 'hanyeong.lee', 'age': 30, 'gender': 'male'}
```

### 키워드 인자(Keyword arguments)

매개변수의 이름을 지정하여 인자로 전달하여 사용하는 경우

```
>>> student(age=30, name='hanyeong.lee', gender='male')
{'name': 'hanyeong.lee', 'age': 30, 'gender': 'male'}
```

**위치 인자와 키워드 인자를 동시에 쓴다면, 위치 인자가 먼저 와야 한다.**

### 위치 인자 묶음

```python
def print_args(*args):
  print(args)
```

### 키워드 인자 묶음

```python
def print_kwargs(**kwargs):
  print(kwargs)
```

### 스코프 (영역)

* 글로벌
  * 로컬 스코프에서 글로벌 스코프의 변수 사용
```
global
```
* 로컬
  * 내부 함수에서의 로컬 스코프
```
nonlocal
```

#### 글로벌 키워드와 인자 전달의 차이

인자로 전달한 경우

```
global_level = 100
def level_add(value):
    value += 30
    print(value)

level_add(global_level)
print(global_level)
```

global 키워드를 사용한 경우

```
global_level = 100
def level_add():
    global global_level
    global_level += 30
    print(global_level)

level_add()
print(global_level)
```

### 람다함수

한 줄 짜리 표현식으로 이루어지며, 반복문이나 조건문 등의 제어문은 포함될 수 없다.
또한, 함수이지만 정의/호출이 나누어져 있지 않으며 표현식 자체가 바로 호출된다.

```
lambda <매개변수> : <표현식>
```

### 클로져

함수가 정의된 환경을 말하며, 파이썬 파일이 여러개일 경우 각 파일은 하나의 모듈역할을 하고, 각 모듈은 독립적인 환경을 가진다.
독립된 환경은 각자의 영역을 전역 영역으로 사용한다.

### 내부함수의 클로져

```python
>>> level = 0
>>> def outter():
...   level = 50
...   def inner():
...     nonlocal level
...     level += 3
...     print(level)
...   return inner
...
>>> func = outter()
```

위의 경우, outter함수는 inner함수를 반환하여 결과를 func전역변수에 할당한다.
inner함수의 호출 결과가 아닌 함수 자체를 반환하기 때문에, func변수는 실행할 수 있는 함수객체이다.
이 때 inner함수가 사용하는 level변수는 nonlocal키워드를 사용했기 때문에 outter에 새로 정의된 지역변수 level을 사용한다.
반복적으로 func함수를 실행하면 inner의 외부(outter)에 존재하는 level변수의 값을 증가시키고 print시키기 때문에, 값은 계속해서 증가한다.

### 데코레이터

함수를 받아 다른 함수를 반환하는 함수. 예를 들면, 기존에 존재하던 함수를 바꾸지 않고 전달된 인자를 보기위한 디버깅 print함수를 추가한다던가 하는 기능을 할 수 있다.

1. 기능을 추가할 함수를 인자로 받음
2. 데코레이터 자체에 추가할 기능을 함수로 정의
3. 인자로 받은 함수를 데코레이터 내부에서 적절히 호출
4. 위 2가지를 행하는 내부 함수를 반환

여러 함수에 같은 기능을 추가할 경우, 데코레이터를 사용한다.

```python
def print_debug(func):
    def inner_func(*args, **kwargs):
        print('args :', args)
        print('kwargs :', kwargs)
        result = func(*args, **kwargs)
        return result
    return inner_func
```
데코레이터를 사용할 경우에는 함수 위에 데코레이터를 추가해서 사용 가능하다.

```python
@print_debug
def any_func():
    pass
```

또한 데코레이터는 여러개를 가질 수 있으며, 함수에서 가장 가까운 것 부터 실행한다.

### 제너레이터

제네레이터는 함수를 통해서 만들어지며, 함수 내부의 반복문에서 `yield`키워드를 사용하면 제네레이터가 된다.

```python
>>> def range_gen(num):
...   i = 0
...   while i < num:
...     yield i
...     i += 1
...
>>> gen = range_gen(10)
>>> gen
<generator object range_gen at 0x10b682168>
>>> type(gen)
<class 'generator'>
>>> gen.__next__()
0
>>> gen.__next__()
1
>>> gen.__next__()
2
>>> gen.__next__()
3
>>> gen.__next__()
4
>>> gen.__next__()
5
>>> gen.__next__()
6
>>> gen.__next__()
7
>>> gen.__next__()
8
>>> gen.__next__()
9
>>> gen.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

함수 내부에 `yield`키워드가 사용되어 제네레이터 함수가 되었으며, 함수를 실행하면 제네레이터 객체를 반환한다.  
`yield`부분에서 멈춘 제네레이터 객체를 순회하기 위해서는 `__next__()` 함수를 실행해준다.