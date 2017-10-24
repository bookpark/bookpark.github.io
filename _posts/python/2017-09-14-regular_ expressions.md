---
layout: post
title:  "정규표현식"
categories: Python
---

복잡한 문자열을 처리할 때 사용하는 기법  
파이썬에서는 표준 모듈 `re`를 사용

```python
import re
result = re.match('Lux', 'Lux, the Lady of Luminosity')
```

`match`의 경우 첫번째 인자에 패턴이 들어가고 두번째 인자에 문자열 소스가 들어간다.  
`match()`는 소스의 패턴 여부를 확인하고 일치하면 `match object`로 반환한다.

복잡하거나 자주 사용되는 패턴은 `compile`을 사용하여 속도를 향상시킬 수 있다.

```python
p = re.compile('Lux')
```

컴파일 된 패턴은 첫번째 인자를 대신해 사용할 수 있다.


## match: 시작부터 일치하는 패턴 찾기

```python
>>> import re
>>> source = 'Lux, the Lady of Luminosity'
>>> result = re.match('Lux', source)
...		print(m.group())
...
Lux
```

`match()`는 시작부터 일치하는 패턴만 찾음.  
`Lady`라는 패턴으로는 찾을 수 없음

```python
>>> m = re.match('.*Lady', source)
>>> if m:
...   print(m.group())
...
Lux, the Lady
```

`.` : 문자 1개  
`*` : 해당 패턴이 0회 이상 올 수 있음  
`.*Lady`는 앞에 아무 문자열 이후 `Lady`로 끝나는 패턴을 의미

## search: 첫번째 일치하는 패턴 찾기

`*`패턴 없이 `Lady`만 찾을 경우 `search()`패턴을 사용

```python
>>> m = re.search('Lady', source)
>>> if m:
...   print(m.group())
...
Lady
```

## findall: 일치하는 모든 패턴 찾기

```python
>>> m = re.findall('y', source)
>>> m
>>> ['y', 'y']
>>> m = re.findall('y..', source)
>>> m
['y o']
```
끝자리의 `y`는 뒤에 문자가 더 없으므로 포함되지 않으므로, `?`를 추가한다.  
`.`은 문자 1개를 의미하며, `?`는 0 또는 1회 반복을 사용한다. `.?`은 문자가 0 또는 1회 올 수 있음을 의미한다.

```python
>>> m = re.findall('y.?.?', source)
>>> m
['y o', 'y']
```

## split: 패턴으로 나누기

문자열의 split()과 비슷하지만 패턴을 사용할 수 있음

```python
>>> m = re.split(' of ', source)
>>> m
['Lux, the Lady', 'Luminosity']
```

## sub: 패턴 대체하기

문자열의 replace()와 비슷하지만 패턴을 사용할 수 있음

```python
>>> m = re.sub('L', 'B', source)
>>> m
['Bux, the Baby of Buminosity]
```

## 정규표현식의 패턴 문자

| 패턴   | 문자                 |
| ---- | ------------------ |
| \\d  | 숫자                 |
| \\D  | 비숫자                |
| \\w  | 문자                 |
| \\W  | 비문자                |
| \\s  | 공백 문자              |
| \\S  | 비공백 문자             |
| \\b  | 단어 경계 (\w와 \W의 경계) |
| \\B  | 비단어 경계             |

```python
>>> import string
>>> printable = string.printable
>>> re.findall('\d', printable)
>>> re.findall('\w', printable)
```

## 정규표현식의 패턴 지정자 (Pattern specifier)

**expr**은 정규표현식을 말한다

| 패턴              | 의미                              |
| --------------- | ------------------------------- |
| abc             | 리터럴 `abc`                       |
| (expr)          | expr                            |
| expr1 \| expr2  | expr1 또는 expr2                  |
| `.`             | `\n`을 제외한 모든 문자                 |
| `^`             | 소스문자열의 시작                       |
| `$`             | 소스문자열의 끝                        |
| expr`?`         | 0 또는 1회의 expr                   |
| expr`*`         | 0회 이상의 최대 expr                  |
| expr`*?`        | 0회 이상의 최소 expr                  |
| expr`+`         | 1회 이상의 최대 expr                  |
| expr`+?`        | 1회 이상의 최소 expr                  |
| expr`{m}`       | m회의 expr                        |
| expr`{m,n}`     | m에서 n회의 최대 expr                 |
| expr`{m,n}?`    | m에서 n회의 최소 expr                 |
| [abc]           | a or b or c                     |
| [^abc]          | not (a or b or c)               |
| expr1(?=expr2)  | 뒤에 expr2가 오면 expr1에 해당하는 부분     |
| expr1(?!expr2)  | 뒤에 expr2가 오지 않으면 expr1에 해당하는 부분 |
| (?<=expr1)expr2 | 앞에 expr1이 오면 expr2에 해당하는 부분     |
| (?<!expr1)expr2 | 앞에 expr1이 오지 않으면 expr2에 해당하는 부분 |

`\`로 시작하는 패턴 문자나, 정규표현식에서 `\`를 직접 사용해야 하는 경우 문자열의 이스케이프문을 사용하지 않고, 정규식 내에서 `\`로 해석됨을 나타내기 위해 앞에 `r`을 붙인다 (raw string으로 취급된다)

정규표현식의 패턴에는 항상 앞에 `r`을 붙인다고 생각하는 것이 좋다. (만약 정규표현식 내부에서 `\`를 쓰지 않을 경우, `r`을 붙임과 붙이지 않음은 같은 결과를 가져온다)

## 매칭 결과 그룹화

정규표현식 패턴중 괄호로 둘러싸인 부분이 있을 경우, 결과는 해당 괄호만의 그룹으로 저장된다.

Match객체의 `group()`함수는 매치된 전체 문자열을 리턴하며, `groups()`함수는 지정된 그룹 리스트를 리턴해준다.
`group(0)`은 `group()`과 같은 동작을 하며, `group(숫자)`는 매치된 숫자번째의 그룹요소를 리턴해준다.

```python
s = re.search(r'\w+\w(was)', story)
s.groups()
s.group(0)
s.group(1)
```

(?P<name>expr) 패턴을 사용하면 매칭된 표현식 그룹에 이름을 붙여 사용할 수 있다.

```python
m = re.search(r'(P?<before>\w+)\s+(P?<was>\w)\s+(P?<after>\w+)', story)
m.groups()
m.group('before')
m.group('was')
m.group('after')
```

## 최소일치와 최대일치

```
html = <html><body><h1>HTML</h1></body></html>
```

```
m = re.match(r'<.*>', html
```

로 검색하면, `.*`표현식이 첫 번째 `>`에서 멈추는것이 아니라 맨 마지막 `>`까지 검색을 진행한다.
`*`이나 `+`에 최소일치인 `?`를 붙여주면, 표현식 다음부분에 해당하는 문자열이 처음 나왔을 때 그 부분까지만 일치시키고 검색을 마친다.