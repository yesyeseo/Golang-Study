# Chapter 06. 연산자

---

## 06-I. 산술 연산자

사칙 연산, 비트 연산, 시프트 연산이 속함

### 06-I-a. 연산의 결과 타입

모든 연산자의 각 항의 타입은 항상 같아야 함. 타임을 같도록 맞춰준 다음 연산해야 함.

### 06-I-b. 비트 연산자

- `^(XOR 연산자)`
  - XOR 연산자. A^B에서 A와 B가 다르면 1이 됨
  - ^은 ^A처럼 단독으로 이용 가능
    - 단독으로 사용하면 **비트 반전** 수행
  

- `&^(비트 클리어 연산자)`
  - 특정 비트를 0으로 바꾸는 연산자
  - 우변값에 해당하는 비트를 클리어하는 연산자
  
### 06-I-c. 시프트 연산자

- `<<`: 왼쪽으로 밀어냄
- `>>`: 오른쪽으로 밀어냄

---

## 06-III. 실수 오차

### 06-III-a. 작은 오차 무시하기

아주 작은 오차는 무시하는 방법으로 값 비교 가능

- ex6.7.go

도대체 얼마만큼의 오차가 무시할 만큼 작은 오차인지의 기준점을 정하기가 힘듦. 즉, **경우에 따라 epsilon이 무시할 만큼 작거나 그렇지 않기도 함**

### 06-III-b. 오차를 없애는 더 나은 방법

- `1비트` 차이만큼 비교함

- 지수부 표현에서 가장 작은 차이는 오른쪽 비트값 1 차이임!

- ex. 0.3은 float32 타입에서 두 값 중 하나로 표현해야 함
  - 둘 다 정확히 같지는 않음, 한 값은 0.3보다 아주 조금 작고 다른 값은 0.3보다 아주 조금 더 큼
  - 두 값은 **가장 마지막 비트 차이**밖에 나지 않음
  - 즉, 0.3을 표현할 수 있는 값의 실수 타입 범위에서는 **가장 작은 차이**임
  - 어떤 값이 `이 두 값 사이`이면 0.3과 같다고 간주


- `Nextafter()` 함수 사용

```go
func Nextafter(x, y float64) (r float64)
```

float64 타입 두 개를 받아서 float64 타입 하나를 반환

- x에서 y를 향해 1비트만 조정한 값 반환
  - x < y라면 x에서 1비트 증가 값 반환
  - x > y라면 x에서 1비트 감소 값 반환
  - 즉, **가장 작은 오차만큼** y를 향해 더하거나 빼 줌


---

## 연습 문제

1. -128 (Overflow)

2. 1010(two) = 10(ten)

> 1000 | 10 == 1010
>
> 1010 | 100 == 1110
>
> 1010 | 1000 == 1110
>
> a는 1110
>
> b는 100
>
> 1110 &^ 100 == 1010

3. 1000 0000(two) = -1(ten)