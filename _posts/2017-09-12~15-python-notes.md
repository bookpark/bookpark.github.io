---
layout: post
title:  "Python 정리"
categories: note
---

# Python 환경설정

## pyenv

* 프로젝트별로 파이썬 버전을 따로 관리할 수 있도록 도와주는 라이브러리

## virtualenv

* 파이썬 **개발환경**을 프로젝트별로 분리해서 관리할 수 있게 해주는 라이브러리
* pyenv는 **파이썬의 버전**을 관리해주는 것이며, virtualenv는 **파이썬 패키지 설치 환경**을 따로 관리한다.

## pyenv-virtualenv

* pyenv를 사용할 경우 쉽게 virtualenv를 사용할 수 있도록 만든 라이브러리 (pyenv 제작자가 제작)

## pyenv 설치

* 맥

```
brew install pyenv
brew install pyenv-virtualenv
```

---

# 변수 (variable)

* 파이썬은 모든 것이 객체 (Object)로 이루어져 있음
* 변수는 객체를 가리킴

> 일반적으로, 프로그래밍 언어에서 같다는 =이 아닌 ==이 담당한다.

* 변수는 언제든지 다른 객체를 가리킬 수 있음

## 변수의 타입 확인

* 내장함수 type 사용
* class와 type은 거의 같은 의미로 사용

## 변수의 이름 제한

### 사용가능한 문자

* 소문자
* 대문자
* 숫자
* 언더스코어(_)

> 이름은 숫자로 시작할 수 없으며, 언더스코어로 시작하는 변수명은 특별한 처리방법을 따르므로 일반적으로 사용하지 않는다.

---

# 내장 데이터 타입 (숫자)

## 수학 연산자

연산자|설명|예|결과|
---|---|---|---|
+|더하기|32 + 7|39|
-|빼기|82 - 2| 80|
*|곱하기|3 * 7|21|
/|나누기|7 / 2|3.5|
//|정수나누기|7 // 2|3|
%|나머지|7 % 3|1|
**|지수|2**10|1024|

## 진수

* 2진수(binary): 0b또는 0B로 시작
* 8진수(octal): 0o또는 0O로 시작
* 16진수(hex): 0x또는 0X로 시작

## 형변환

* 내장함수 int, float 사용

# 문자열

## 이스케이프 문자

* 특수문자, 또는 특별한 역할을 하는 의미를 나타내는 문자를 뜻한다.

이스케이프문자|설명|
---|---|
\a |비프음 발생
\t |탭
\n |줄바꿈
\ |역슬래시 입력
\' |작은따옴표 입력
\\\\\" |큰따옴표 입력

## 슬라이스 연산

	[START:END]

## 길이

	len

## 문자열 나누기

	.split()

## 문자열 결합

	.join()

	>>> girlsday_list = girlsday.split(',')
	>>> girlsday_str = ', '.join(girlsday_list)
	>>> print(girlsday_str)
	민아, 유라, 소진, 혜리

## 대소문자 다루기

	.capitalize()
	.title()
	.upper()
	.lower()
	.swapcase()

## 문자열 포맷

### 옛 스타일 (%)

	string % data

	>>> '%s' % 42
	'42'
	>>> '%d x %d : %d' % (3, 4, 12)
	'3 x 4 : 12'

### 정렬

* %[정렬기준(-,없음)][전체글자수].[문자길이 또는 소수점 이후 문자길이][변환타입]

```python
	>>> d = 37
	>>> f = 3.14
	>>> s = 'Fastcampus'
	>>> '%d %f %s' % (d, f, s)
	'37 3.140000 Fastcampus'
	>>> '%10d %10f %10s' % (d, f, s)
	'        37   3.140000 Fastcampus'
	>>> '%14d %14f %14s' % (d, f, s)
	'            37       3.140000     Fastcampus'
	>>> '%-14d %-14f %-14s' % (d, f, s)
	'37             3.140000       Fastcampus    '
	>>> '%-14.3d %-14.3f %-14.3s' % (d, f, s)
	'037            3.140          Fas           '
```

### 새 스타일 ({}, format)

* {}.format(변수)

```python
# 기본형태
>>> '{} {} {}'.format(d, f, s)
'37 3.14 Fastcampus'

# 각 인자의 순서를 지정
>>> '{1} {2} {0}'.format(d, f, s)
'3.14 Fastcampus 37'

# 각 인자에 이름을 지정
>>> '{d} {f} {s}'.format(d=50, f=1.432, s='WPS')
'50 1.432 WPS'

# 딕셔너리로부터 변수 할당
>>> dict = {'d': d, 'f': f, 's': s}
>>> '{0[d]} {0[f]} {0[s]} {1}'.format(dict, 'WPS')
'37 3.14 Fastcampus WPS'

# 타입 지정자 입력
>>> '{:d} {:f} {:s}'.format(d, f, s)
'37 3.140000 Fastcampus'

# 이름과 타입지정자를 모두 사용
>>> '{digit:d} {float:f} {string:s}'.format(digit=700, float=1.4323, string='Welcome')
'700 1.432300 Welcome'

# 필드길이 10, 우측정렬
>>> '{:10d}'.format(d)
'        37'
>>> '{:>10d}'.format(d)
'        37'

# 필드길이 10, 좌측정렬
>>> '{:<10d}'.format(d)
'37        '

# 필드길이 10, 가운데 정렬
>>> '{:^10d}'.format(d)
'    37    '

# 필드길이 10, 가운데 정렬, 빈 공간은 ~로 채움
>>> '{:~^10d}'.format(d)
'~~~~37~~~~'
```

---

# 시퀀스 타입

* 파이썬에 내장된 시퀀스 타입에는 문자열, 리스트, 튜플이 있다.
* 문자열은 인용부호(작은따옴표, 큰따옴표)를 사용하며, 리스트는 대괄호[], 튜플은 괄호()를 사용하여 나타낸다.
* 시퀀스 타입의 객체는 인덱스 연산을 통해 내부 항목에 접근 할 수 있다.

## 리스트

* 리스트는 순차적인 데이터를 나타내는 데 유용하며, 문자열과는 달리 내부 항목을 변경할 수 있다.

### 리스트 생성

```python
>>> empty_list1 = []
>>> empty_list2 = list()
>>> sample_list = ['a', 'b', 'c', 'd']
>>> sample_list2 = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
```

### 리스트 항목 추가

* append

* extend

* insert

```
fruits.insert(1, 'mango')
```

### 특정 위치 리스트 항목 삭제

* del

```
del fruits[0]
```

### 값으로 리스트 항목 삭제

* remove

```
fruits.remove('mango')
```

### 리스트 항목 추출 후 삭제

* pop

```
fruits.pop()
fruits.pop(3)
```

### 값으로 리스트 항목 오프셋 찾기

* index

```
fruits.index('red')
```

### 존재 여부 확인

* in

```
'red' in fruits

>>> True
```

### 값세기

* count

```
fruits.append('red')
fruits.append('red')
fruits.count('red')

>>> 3
```

### 정렬하기

* sort는 리스트 자체를 정렬
* sorted는 리스트의 정렬 복사본을 반환


### 리스트 복사

* copy 함수
* list 함수
* 슬라이스 연산 [:]

## 튜플

* 튜플은 리스트와 비슷하나, 정의 후 내부 항목의 삭제나 수정이 불가능하다.

### 튜플 생성

```
>>> empty_tuple = ()
```
```
>>> colors = 'red',
>>> fruits = 'apple', 'banana'
```

* 튜플을 정의할 때는 괄호가 없어도 무관하나, 괄호로 묶는것이 좀 더 튜플임을 구분하기 좋다.
* 또한, 튜플의 요소가 1개일 때는 요소의 뒤에 쉼표(,)를 붙여야 한다.

### 튜플 언패킹

```
>>> f1, f2 = fruits
```

* 값의 교환

### 형 변환

* tuple 함수를 사용
* 리스트를 튜플로 변환

### 튜플을 사용하는 이유

* 리스트보다 적은 메모리 사용
* 정의후에는 변하지 않는 내부 값

# 딕셔너리, 셋

## 딕셔너리

### 딕셔너리 생성

```
>>> empty_dict1 = {}
>>> empty_dict2 = dict()
>>> champion_dict = {
... 'Lux': 'the Lady of Luminosity',
... 'Ahri': 'the Nine-Tailed Fox',
... 'Ezreal': 'the Prodigal Explorer',
... 'Teemo': 'the Swift Scout',
... }
```

### 형변환

* dict 함수를 사용, 두 값의 시퀀스(리스트 또는 튜플)을 딕셔너리로 변환 한다.

```
>>> sample = [[1,2], [3,4], [5,6]]
>>> dict(sample)
{1: 2, 3: 4, 5: 6}
```

### 항목 찾기/추가/변경 [key]

```
>>> champion_dict['Lux']
'the Lady of Luminosity'
>>> champion_dict['Sona'] = 'Maven of the Strings'
>>> champion_dict['Lux'] = 'Demacia'
```

### 결합 (update)

```
>>> item_dict = {
... 'Doran\'s Ring': 400,
... 'Doran\'s Blade': 450,
... 'Doran\'s Shield': 450,
... }
>>> com_dict = {}
>>> com_dict.update(champion_dict)
>>> com_dict.update(item_dict)
>>> com_dict
```

* 서로 같은 키가 있을 경우, update에 주어진 딕셔너리의 값이 할당된다.

### 삭제 (del)

```
>>> del com_dict['Doran\'s Blade']
>>> del com_dict['Doran\'s Ring']
>>> del com_dict['Doran\'s Shield']
```

## 셋(Set)

* 셋은 키만 있는 딕셔너리와 같으며, 중복된 값이 존재할 수 없다.

### 셋 생성

```
>>> empty_set = set()
>>> champions = {'lux', 'ahri', 'ezreal'}
```

### 형 변환

* 문자열, 리스트, 튜플, 딕셔너리를 셋으로 변환할 수 있으며, 중복된 값이 사라진다.

```
>>> set('ezreal')
{'e', 'z', 'a', 'l', 'r'}
```
```
>>> set(champion_dict)
{'Ahri', 'Lux', 'Ezreal', 'Sona', 'Teemo'}
```

* 딕셔너리는 키만 남는다.

### 집합 연산

연산자|설명
---|---
l|합집합 (Union)
&|교집합 (Intersection)
-|차집합 (Difference)
^|대칭차집합 (Exclusive)
<=|부분집합 (Subset)
<|진부분집합 (Proper Subset)
>=|상위집합 (Superset)
>| 진상위집합(Proper Superset)

```
>>> A = {1,2,3,4,5}
>>> B = {4,5,6,7,8,9}
>>> C = {4,5,6}
>>> A|B
>>> A&B
>>> A-B
>>> B-A
>>> A^B
>>> A <= B
>>> C <= B
>>> C < B
>>> B <= B
>>> B < B
```

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
[표현식 for 항목 in iterable객체]
```
```python
>>> [item for item in range(1, 6)]
[1, 2, 3, 4, 5]
```

### 셋 컴프리헨션

```python
{표현식 for 표현식 in iterable객체}
```
```python
gugu_list = [{'title': '%d단' % x, 'items': ['%d x %d = %d' % (x, y, x * y) for y 
	in range(1, 10)]} for x in range(2, 10)]
```

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


# 모듈과 패키지

## 모듈

### 모듈 불러오기

`import`

### __name__변수

파이썬 인터프리터를 이용해 실행한 코드인지를 확인하여 단순히 `import`한 모듈의 경우 실행을 막는 방식을 사용할 수 있다.

각 모듈은 자신의 이름을 가지며, 모듈 이름은 모듈의 전역변수 `__name__`에서 확인 할 수 있다.

### from을 사용해 모듈의 함수를 직접 import

`import` 모듈명의 경우, 모듈의 이름이 전역 네임스페이스에 등록되어 `모듈명.함수`로 사용가능하다.

모듈명을 생략하고 모듈 내부의 함수를 쓰고 싶다면, `from 모듈명 import 함수명`으로 불러들일 수 있다.

### from 모듈명 * 을 사용해 모듈 내 모든 식별자 (변수, 함수) import

### as로 별칭 할당

`from 모듈명 import` 또는 `import 모듈명`에서, 같은 모듈명이 존재하거나 혼동 될 수 있을 경우, 뒤에 `as`를 붙여 사용할 모듈명을 변경할 수 있다.

## 패키지

패키지는 모듈들을 모아 둔 특별한 폴더를 뜻한다.

### *, _\_all\_\_

패키지에 포함된 하위 패키지 및 모듈을 불러올 때, *을 사용하면 해당 모듈의 모든 식별자들을 불러온다.

이 때, 각 모듈에서 자신이 import될 때 불러와질 목록을 지정하고자 한다면 `__all__` 을 정의하면 된다.

# 클래스

## 객체지향 프로그래밍

파이썬의 모든것은 객체이며, 객체를 사용할 때는 변수에 해당 객체를 참조(Reference)시켜 사용한다.
객체는 변수와 함수를 가지며, 특별히 객체가 가진 **변수**와 **함수**는 각각 **속성(attribute)**과 **메서드(method)**라고 부른다.

객체는 어떠한 타입, 즉 특정한 **클래스**의 형태를 가진 **인스턴스**를 나타낸다.

### 클래스

```python
class Shop:
    def __init__(self, name):
        self.name = name
```

위 코드에서, `__init__`은 클래스를 사용한 객체의 초기화 메서드이다. 객체를 생성할 때 인자를 어떻게 전달받고, 받은 인자를 이용해 어떤 객체를 생성할 지 정의할 수 있다.

### 메서드

#### 인스턴스 메서드

인스턴스 메서드는 첫 번째 인수로 `self`를 가진다. 인스턴스를 이용해 메서드를 호출할 때 호출한 인스턴스가 자동으로 전달되며, 전달받은 인스턴스는 상태를 확인하거나 조작하는데에 사용된다.

### 속성 접근 지정자 (attribute access modifier)

#### 캡슐화

객체를 구현할 때, 사용자가 반드시 알아야 할 데이터나 메서드를 제외한 부분을 은닉시켜 정해진 방법을 통해서만 객체를 조작할 수 있도록 하는 방식.

```
_ protected
__ private
```

```
shop_type
>>> _shop_type or __shop_type
```

실제 이름은 \_<클래스명>__<속성명>으로 되어있다.

### get/set 속성값과 프로퍼티

파이썬에서는 지원하지 않지만, 어떤 언어들은 외부에서 접근할 수 없는 `private`객체 속성을 지원한다. 이 경우, 객체에서는 해당 속성을 읽고 쓰기 위해 `getter`, `setter`메서드를 사용해야 한다.

```python
@property
def name(self):
    return self.__name

@name.setter
def name(self, new_name):
    self.__name = new_name
    print('Set new name ({})'.format(self.__name))
```

`setter`프로퍼티를 명시하지 않으면 읽기전용이 되어 외부에서 조작할 수 없게 된다.

### 상속(Inheritance)

거의 비슷한 기능을 수행하나, 약간의 추가적인 기능이 필요한 다른 클래스가 필요할 경우 기존의 클래스를 상속받은 새 클래스를 사용하는 형태로 문제를 해결할 수 있다.

이 때, 상속 되는 클래스를 부모(상위)클래스라고 하며, 상속을 받는 클래스는 자식(하위)클래스라고 한다.

상속을 받을때는 클래스의 정의 다음 괄호에 부모 클래스를 적어주면 된다.

```
class Restaurant(Shop):
    pass
```

상속받은 클래스는 부모 클래스의 모든 속성과 메서드를 사용할 수 있다.

#### 메서드 오버라이드

상속받은 클래스에서, 부모 클래스의 메서드와는 다른 동작을 하도록 할 수 있다. 이 경우 부모 클래스의 메서드를 덮어씌워서 사용하도록 하며, 이 방법을 메서드 오버라이드(method override)라고 한다.

#### 부모 클래스의 메서드를 호출 (super)

자식클래스의 메서드에서 부모 클래스에서 사용하는 메서드의 전체를 새로 쓰는것이 아닌, 부모 클래스의 메서드를 호출 후 해당 내용으로 새로운 작업을 해야 할 경우 super()메서드를 사용해서 부모 클래스의 메서드를 직접 호출할 수 있다.

```
class Restaurant(Shop):
    def __init__(self, name, shop_type, address, rating):
        super().__init__(name, shop_type, address)
        self.rating = rating
```

위 코드의 경우, super()메서드를 사용해서 부모의 \_\_init__ 메서드를 호출한다.

#### 정적(스태틱) 메서드

스태틱 메서드는 클래스 내부에 정의된 일반 함수이며, 단지 클래스나 인스턴스를 통해서 접근할 수 있을 뿐 해당 클래스나 인스턴스에 영향을 주는 것은 불가능하다.

스태틱메서드는 `@staticmethod`데코레이터를 붙여 선언한다.

스태틱메서드는 다양한 방식으로 인스턴스를 생성하는 클래스를 작성할 때 자주 사용된다. `__init__()`초기화 함수는 하나만 존재할 수 있기 때문에, 다른 생성함수를 정적 메서드로 만들어 사용할 수 있다.

#### 클래스 메서드

클래스 메서드는 클래스 속성에 대해 동작하는 메서드이다. 위의 인스턴스 메서드와 달리 호출 주체가 클래스이며, 첫 번째 인자도 클래스이다.
만약 인스턴스가 첫 번째 인자로 주어지더라도 해당 인자의 클래스로 자동으로 바뀌어 전달된다.

클래스메서드는 `@classmethod`데코레이터를 붙여 선언하며, 첫 번째 인자의 이름은 관용적으로 `cls`를 사용한다.

클래스 메서드를 사용하면 부모 클래스를 상속받은 자식클래스에서 해당 부모클래스의 클래스 메서드를 사용할 경우, 자기 자신(자식)의 클래스를 사용할 수 있다.

```
@classmethod
def make_dummy(cls):
	return cls('untitled', 'undefined', 'unknown')
```

### 다형성 동적 바인딩

동적 바인딩은 타입을 신경 쓰지 않고 인스턴스를 사용할 수 있게 해주는 방식이다.

동적 바인딩은 속성 검색과정을 통해서 이루어진다. `obj.attr`을 통해 속성에 접근하면, 인스턴스에서 `attr`을, 다음엔 클래스에서, 그 다음에는 상속받은 클래스에서 해당 속성을 검색하며 가장 먼저 검색한 속성을 반환한다.

동적으로 속성을 바인딩하는 과정은 `obj`객체의 타입에 영향받지 않으며, 오로지 해당 객체가 attr에 해당하는 속성을 가졌는지만 검사한다.

이러한 작동방식은 '오리처럼 울고걸으면 그것은 오리이다'라는 말에서 유래한 `덕 타이핑(duck typing)`이라고 한다.

```
User, Sword, Shield클래스를 정의하고 속성을 동적으로 접근해본다.

User:
	- attrs
	name
	atk = 0
	def = 0

	- methods
	equip(item)
		item의 'use()'메서드를 사용
		이때 자기자신(인스턴스)를 전달
	
Sword, Shield:
	- methods
	use(user)
		user로 전달된 인스턴스의 특정 능력치를 높임
```