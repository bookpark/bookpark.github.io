---
layout: post
title:  "Markdown 정리"
categories: markdown
---

## 헤더
	# 기호를 이용해 h1 부터 차례대로 늘려간다.

## 강조

	* 또는 _ 로 Italic 강조를 한다.  

	** 로 볼드를 표시한다.  

## 리스트

	* 또는 - 로 Unordered List를 표시한다.
	숫자로 Oredered List를 표시한다.

* 별표
- 대시

1. 숫자
2. 숫자
  1. 숫자
  2. 숫자

## 이미지
	느낌표로 시작, 대괄호 안에 텍스트를 넣고, 소괄호 안에 이미지가 위치한 주소를 넣는다.
	![텍스트](/images/logo.png)

![텍스트](https://goo.gl/KFrVXM)

## 링크
	링크는 이미지 표현법에서 느낌표만 빼준다.
	[GitHub](http://github.com)

[GitHub](http://github.com)


## 인용문
	인용문은 > 를 이용해 나타낸다.

> 인용문은 > 를 이용해 나타낸다.

## 인라인 코드
	인라인 코드를 작성할 때  
	<addr> 요소를 넣어준다.

인라인 코드를 작성할 때 <addr> 요소를 넣어준다.

## 구문 강조
	구문 강조를 하고 싶을 땐 느낌표 옆 기호를 쓴 후 언어명과 함께 작성한다.

```python
if (isAwesome){
  return true
}
```

## 할 일 목록
	 - [x] 과제 완성
	 - [x] 과제 제출
	 - [ ] 저녁 식사

- [x] 과제 완성
- [x] 과제 제출
- [ ] 저녁 식사

## 사용자 @멘션
	@mention

## 자동 링크 완성
	url은 보통 자동으로 링크가 만들어진다.

http://www.github.com/

## 취소선
	~~로 감싸준다.

~~과제 제출~~

## 테이블
	- 옆 기호로 row를 나타내고 | 이 기호로 column을 나타낸다.

| table    | head     | item     |
| -------- | -------- | -------- |
| content1 | content2 | content3 |
