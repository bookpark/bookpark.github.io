---
layout: post
title:  "파일 입출력"
categories: python
date: 2017-09-15
---
## 파일 열기

```
변수 = open(파일명, 모드)
```

내장함수 `open()`을 사용하며 파일명은 파일의 경로를 나타냄

```
f = open('sample.txt', 'rt')
```

#### 모드의 첫번째 글자

| 모드   | 설명                         |
| ---- | -------------------------- |
| r    | 읽기                         |
| w    | 쓰기 (파일이 이미 존재할 경우 덮어쓴다)    |
| x    | 쓰기 (단, 파일이 존재하지 않을 경우에만)   |
| a    | 추가 (파일이 존재할 경우 파일의 끝부터 쓴다) |

#### 모드의 두번째 글자

| 모드      | 설명       |
| ------- | -------- |
| t 또는 없음 | 텍스트타입    |
| b       | 이진데이터 타입 |

#### 이진데이터

이진형식(0과 1)로 이루어진 텍스트를 제외한 데이터를 말함.  
텍스트 역시 이진데이터의 일종이지만, 데이터의 구조가 인코딩과 개행문자 등 텍스트 형태로 바로 사용할 수 있게 되어있다는 차이가 있다.

파일을 열고 사용한 뒤에는 파일을 닫아야한다.

## 파일 쓰기: write()

```
>>> skills = '''Illumination
... Light Binding
... Prismatic Barrier
... Lucent Singularity
... Final Spark'''
>>>
>>> len(skills)
75
```

```
>>> f = open('skills.txt', 'wt')
>>> f.write(skills)
75
>>> f.close()
```

덮어쓰기를 방지하려면 `wt`대신 `xt`를 사용해서 이미 존재하는 파일은 쓸 수 없도록 처리한다.

```
>>> try:
...   f = open('skills.txt', 'xt')
...   f.write('Other contents')
... except FileExistsError:
...   print('skills.txt exists')
...
skills.txt exists
```

## 텍스트 파일 전체 읽기: read()

`read()`함수는 전체 파일을 한 번에 가져오므로, 메모리 사용에 유의해야한다.

```
>>> f = open('skills.txt', 'rt')
>>> skills = f.read()
>>> f.close()
>>> len(skills)
75
```

한 번에 읽을 크기 제한

```
>>> f = open('skills.txt', 'rt')
>>> chunk = 30
>>> while True:
...		part = f.read(chunk)
...		if not part:
...			break
...		skills += part
...
>>> f.close()
>>> len(skills)
75
```

파일을 전부 읽으면 빈 문자열이 리턴되고, `if`문에서 `False`로 판단하여 루프가 끝난다.

## 텍스트 파일 줄 단위 읽기: readline()

```
>>> f = open('skills.txt', 'rt')
>>> while True:
...		line = f.readline()
...		if not part:
...			break
...		skills += line
...
>>> f.close()
>>> len(skills)
75
```

파일을 라인단위로 읽어 문자열에 저장한다.

빈 라인(`\n`)은 길이가 1이며, 파일의 끝에서만 완전히 빈 문자열 (`''`)을 리턴한다.

## 이터레이터를 사용한 텍스트 파일 읽기

```
phones = ''
f = open('phones.txt', 'rt')
for line in f:
    phones += line
    
f.close()
print(len(phones))
```

## 텍스트 파일을 줄 단위 문자열 리스트로 리턴: readlines()

```
f = open('phones.txt', 'rt')
lines = f.readlines()
for line in lines:
	print(line)
	
iPhone

Galaxy

Nexus

Mi

for line in lines:
    print(line, end='')

iPhone
Galaxy
Nexus
Mi>>>
```

각 줄에 줄바꿈(`\n`)문자가 있으므로 print()함수에 `end`인자를 주어 줄바꿈을 없앨 수 있다.

마지막 라인에는 줄바꿈이 없으므로 인터프리터 프롬프트가 같은 줄에 표시된다.

## 자동으로 파일 닫기: with

연 파일을 닫지 않을 경우, 파이썬에서는 해당 파일이 더 이상 사용되지 않을 때 파일을 자동으로 닫아준다.

다만 메인프로그램이나 오랫동안 동작하는 함수에서 파일을 열 경우, 명시적으로 닫아주지 않을 경우 문제가 발생한다.

```
with open('phones.txt', 'rt') as f:
	lines = f.readlines()
	for line in lines:
		print(line, end='')
```

## 이진데이터 다루기

`t` 대신 `b`를 사용함



