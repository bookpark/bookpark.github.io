---
layout: post
title:  "Python 기본"
categories: python
date: 2017-09-12
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


# 정규표현식 (Regular Expressions)

특정한 패턴에 일치하는 복잡한 문자열을 처리할 때 사용하는 기법.

파이썬에서는 표준 모듈 `re`를 사용해서 정규표현식을 사용할 수 있다.

```python
import re
result = re.match('Lux', 'Lux, the Lady of Luminosity')
```
위에서 `match`의 첫 번째 인자에는 패턴이 들어가며, 두 번째 인자에는 문자열 소스가 들어간다. `match()`는 소스와 패턴의 일치 여부를 확인하고, 일치할 경우 `Match object`를 반환한다.

조금 복잡하거나 자주 사용되는 패턴은 미리 **컴파일**하여 속도를 향상시킬 수 있다.

```python
pattern1 = re.compile('Lux')
```

컴파일된 패턴객체를 문자열 대신 첫 번째 인자로 사용 가능하다.

## match : 시작부터 일치하는 패턴 찾기

```python
>>> import re
>>> source = 'Lux, the Lady of Luminosity'
>>> m = re.match('Lux', source)
>>> if m:
...   print(m.group())
...
Lux
```

`match()`는 시작부분부터 일치하는 패턴만 찾기 때문에, `Lady`라는 패턴으로는 찾을 수 없다.

```python
>>> m = re.match('.*Lady', source)
>>> if m:
...   print(m.group())
...
Lux, the Lady
```

위 결과는 다음을 의미한다.

* `.`은 문자 1개를 의미
* `*`는 해당 패턴이 0회 이상 올 수 있다는 의미이다
* 따라서 `.*Lady`는 앞에 아무 문자열(또는 빈) 이후 Lady로 끝나는 패턴을 의미한다

## search : 첫 번째 일치하는 패턴 찾기

* `*` 패턴 없이 `Lady`만 찾고 싶을 경우 `search()` 사용

```python
>>> m = re.search('Lady', source)
>>> if m:
...   print(m.group())
...
Lady
```

## findall : 일치하는 모든 패턴 찾기 ```이 부분 이해 안감```

```python
>>> m = re.findall('y', source)
>>> print(m)
['y', 'y']
>>> m = re.findall('y..', source)
>>> print(m)
['y o']
```

끝자리의 y는 뒤에 문자가 더 없어 포함되지 않으므로, `?`를 추가한다.
`.`은 문자 1개를 의미하며, `?`는 0 또는 1회 반복을 사용한다. `.?`은 문자가 0 또는 1회 올 수 있음을 의미한다. 


```python
>>> m = re.findall('y.?.?', source)
>>> print(m)
['y o', 'y']
```

## split : 패턴으로 나누기

문자열의 `split()`메서드와 비슷하지만 패턴을 사용할 수 있다.

```python
>>> m = re.split(' of ', source)
>>> print(m)
['Lux, the Lady', 'Luminosity']
```

## sub : 패턴 대체하기

문자열의 `replace()`메서드와 비슷하지만 패턴을 사용할 수 있다.

```python
>>> m = re.sub('L', 'B', source)
>>> print(m)
Bux, the Bady of Buminosity
```

## 정규표현식의 패턴 문자 (공부하자)

패턴|문자
---|---
\\d|숫자
\\D|비숫자
\\w|문자
\\W|비문자
\\s|공백 문자
\\S|비공백 문자
\\b|단어 경계 (\w와 \W의 경계)
\\B|비단어 경계

```python
import string
>>> printable = string.printable
>>> # re.findall('\w', printable)
>>> re.findall('\d', printable)
>>>print(printable)
0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ 	
```

## 정규표현식의 패턴 지정자 (공부하자)

**expr**은 정규표현식을 말한다

패턴|의미
---|---
abc|리터럴 `abc`
(expr)|expr
expr1 \| expr2 | expr1 또는 expr2
`.` | `\n`을 제외한 모든 문자
`^` | 소스문자열의 시작
`$` | 소스문자열의 끝
expr`?` | 0 또는 1회의 expr
expr`*` | 0회 이상의 최대 expr
expr`*?`| 0회 이상의 최소 expr
expr`+` | 1회 이상의 최대 expr
expr`+?`| 1회 이상의 최소 expr
expr`{m}`| m회의 expr
expr`{m,n}`| m에서 n회의 최대 expr
expr`{m,n}?` | m에서 n회의 최소 expr
[abc] | a or b or c
[^abc] | not (a or b or c)
expr1(?=expr2) | 뒤에 expr2가 오면 expr1에 해당하는 부분
expr1(?!expr2) | 뒤에 expr2가 오지 않으면 expr1에 해당하는 부분
(?<=expr1)expr2 | 앞에 expr1이 오면 expr2에 해당하는 부분
(?<!expr1)expr2 | 앞에 expr1이 오지 않으면 expr2에 해당하는 부분

[점프 투 파이썬 - 정규표현식 (https://wikidocs.net/4309)](https://wikidocs.net/4309)

**럭스 세계관**

Born to the prestigious Crownguards, the paragon family of Demacian service, Luxanna was destined for greatness. She grew up as the family's only daughter, and she immediately took to the advanced education and lavish parties required of families as high profile as the Crownguards. As Lux matured, it became clear that she was extraordinarily gifted. She could play tricks that made people believe they had seen things that did not actually exist. She could also hide in plain sight. Somehow, she was able to reverse engineer arcane magical spells after seeing them cast only once. She was hailed as a prodigy, drawing the affections of the Demacian government, military, and citizens alike.

As one of the youngest women to be tested by the College of Magic, she was discovered to possess a unique command over the powers of light. The young Lux viewed this as a great gift, something for her to embrace and use in the name of good. Realizing her unique skills, the Demacian military recruited and trained her in covert operations. She quickly became renowned for her daring achievements; the most dangerous of which found her deep in the chambers of the Noxian High Command. She extracted valuable inside information about the Noxus-Ionian conflict, earning her great favor with Demacians and Ionians alike. However, reconnaissance and surveillance was not for her. A light of her people, Lux's true calling was the League of Legends, where she could follow in her brother's footsteps and unleash her gifts as an inspiration for all of Demacia.

```python
story = '''Born to the prestigious Crownguards, the paragon family of Demacian service, Luxanna was destined for greatness. She grew up as the family's only daughter, and she immediately took to the advanced education and lavish parties required of families as high profile as the Crownguards. As Lux matured, it became clear that she was extraordinarily gifted. She could play tricks that made people believe they had seen things that did not actually exist. She could also hide in plain sight. Somehow, she was able to reverse engineer arcane magical spells after seeing them cast only once. She was hailed as a prodigy, drawing the affections of the Demacian government, military, and citizens alike.

As one of the youngest women to be tested by the College of Magic, she was discovered to possess a unique command over the powers of light. The young Lux viewed this as a great gift, something for her to embrace and use in the name of good. Realizing her unique skills, the Demacian military recruited and trained her in covert operations. She quickly became renowned for her daring achievements; the most dangerous of which found her deep in the chambers of the Noxian High Command. She extracted valuable inside information about the Noxus-Ionian conflict, earning her great favor with Demacians and Ionians alike. However, reconnaissance and surveillance was not for her. A light of her people, Lux's true calling was the League of Legends, where she could follow in her brother's footsteps and unleash her gifts as an inspiration for all of Demacia.'''
```

위와같이 `story`변수에 스토리 문자열을 할당한다. **다 써보고 이해해보자**

```python
re.findall('Lux', story)
re.findall('Lux|her|she', story)
re.findall('[Ll]ux|[Hh]er|[Ss]he', story)
re.findall('^Born', story)
re.findall('Demacia$', story)
re.findall('was', story)
re.findall('(?<=she) was', story)
re.findall('\w+(?<!she) was', story)
re.findall('\bwas\b', story)
re.findall(r'\bwas\b', story)
```

`\`로 시작하는 패턴 문자나, 정규표현식에서 `\`를 직접 사용해야 하는 경우 문자열의 이스케이프문을 사용하지 않고, 정규식 내에서 `\`로 해석됨을 나타내기 위해 앞에 `r`을 붙인다 (`raw string`으로 취급된다)

정규표현식의 패턴에는 항상 앞에 `r`을 붙인다고 생각하는 것이 좋다. (만약 정규표현식 내부에서 `\`를 쓰지 않을 경우, `r`을 붙임과 붙이지 않음은 같은 결과를 가져온다)

## 매칭 결과 그룹화 

정규표현식 패턴중 괄호로 둘러싸인 부분이 있을 경우, 결과는 해당 괄호만의 그룹으로 저장된다.

`Match`객체의 `group()`함수는 매치된 `전체 문자열`을 리턴하며, `groups()`함수는 `지정된 그룹 리스트`를 리턴해준다.
`group(0)`은 `group()`과 같은 동작을 하며, `group(숫자)`는 매치된 `숫자`번째의 그룹요소를 리턴해준다.

```python
s = re.search(r'\w+\w(was)', story)
s.groups()
s.group(0)
s.group(1)
```

`(?P<name>expr)` 패턴을 사용하면 매칭된 표현식 그룹에 이름을 붙여 사용할 수 있다.

```
m = re.search(r'(?P<before>\w+)\s+(?P<was>was)\s+(?P<after>\w+)', story)
m.groups()
m.group('before')
m.group('was')
m.group('after')
```

## 최소 일치와 최대 일치

```
<html><body><h1>HTML</h1></body></html>
```

위 항목을

```
m = re.match(r'<.*>', html)
```

로 검색하면, `.*`표현식이 첫 번째 `>`에서 멈추는것이 아니라 맨 마지막 `>`까지 검색을 진행한다.
`*`이나 `+`에 최소일치인 `?`를 붙여주면, 표현식 다음부분에 해당하는 문자열이 처음 나왔을 때 그 부분까지만 일치시키고 검색을 마친다.

