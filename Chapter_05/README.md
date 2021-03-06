# Chapter 05. fmt 패키지를 이용한 텍스트 입출력

- fmt 패키지: **구조화된 입출력을 제공**하는 패키지

- fmt 패키지가 제공하는 Print(), Printf(), Println() 함수로 표준 출력 가능

- 표준 입력을 제공하는 Scan(), Scanf(), Scanln() 함수 또한 존재

---

## 05-I. 표준 입출력

- 프로그램 내부에서 입력과 출력을 간편하게 처리: `standard input/output stream` 표준 입출력 스트림
    - 운영체제가 제공함


- 표준 입출력 사용 → 목적지에 상관없이 간편하게 입출력 처리 가능
    - Go: `fmt package` 사용해서 간단하게 표준 입출력 처리 가능

### 05-I-a. fmt 패키지

```go
import "fmt"
```

- Go의 fmt 패키지는 3가지 표준 출력용 함수를 제공
    - `Print()`: 함수 입력값들을 출력
    - `Println()`: 함수 입력값들을 출력, 개행
    - `Printf()`: 서식(format)에 맞도록 입력값 출력

### 05-I-b. 서식 문자

Printf() 함수의 사용법

```go
Printf(서식 문자열, 인수1, 인수2, ...)
```

`%v`: 기본 서식에 맞추어 출력해 줌

### 05-I-c. 최소 출력 너비 지정

- `최소 출력 너비 지정`
- `공란 채우기`: 너비 앞에 `0`을 붙이면 **빈자리를 0으로 채움**
- `왼쪽 정렬하기`: 마이너스 `-` 붙이면 **왼쪽을 기준선 삼아 출력**
    - 일정 간격으로 숫자 출력 가능

### 05-I-d. 실수 소수점 이하 자릿수

실수에는 최소 출력 너비뿐 아니라, 소수점 이하 자릿수도 지정 가능

- `%f`: 실수 출력
    - ex. %5.2f: 최소 너비 5칸, 소수점 이하 값 2개 출력

- `%g`: 실수를 정수부 + 소수점 이하 숫자 포함하여 출력 숫자 제한
    - 정해진 길이로 정수부 숫자 모두 표현 X - 지수 표현으로 전환
    - 기본 숫자 길이는 **6개**
    - ex. %5.3.g: 최소 너비 5칸, 소수점 이하 포함 총 숫자 3개로 표현됨

[https://golang.org/pkg/fmt/](https://golang.org/pkg/fmt/) 참고

### 05-I-e. 특수 문자

|이름|기능|
|---|---|
|\n|줄바꿈|
|\t|탭 삽입|
|\\|\ 자체 출력|
|\"|" 출력. 큰따옴표로 묶인 문자열 내부에 따옴표 넣을 때 사용|

`%q`서식으로 출력: 모든 특수 문자가 기능을 잃고 **문자 자체로 동작**

---

## 05-II. 표준 입력

표준 입력은 표준 입력 장치에서 데이터를 얻어옴.

일반적으로 표준 입력을 변경하지 않았다면, `키보드`가 표준 입력 장치

fmt 패키지는 표준 입력으로부터 입력받는 Scan(), Scanf(), Scanln() 함수를 제공함

|이름|기능|
|---|---|
|Scan()|표준 입력에서 값을 입력받음|
|Scanf()|표준 입력에서 서식 형태로 값을 입력받음|
|Scanln()|표준 입력에서 한 줄을 읽어서 값을 입력받음|

### 05-II-b. `Scan()`

Scan() 함수: 값을 채워넣을 변수들의 **메모리 주소**를 인수로 받음. 한 번에 여러 값을 입력받을 때는 **변수 사이를 공란**으로 두어 구분

```go
func Scan(a ...interface{}) (n int, err error)
```

`함수 반환값`: **성공적으로 입력한 값 개수**, **입력 실패 시 에러** 반환

### 05-II-c. `Scanf()`

서식에 맞춘 입력을 받음

```go
func Scanf(format string, a ...interface{}) (n int, err error)
```

단, 서식을 맞추기가 힘들기 때문에 Scan()이나 Scanln() 함수 사용을 추천함

### 05-II-d. `ScanIn()`

`한 줄`을 입력받아, 인수로 들어온 변수 메모리 주소에 값을 채워 줌.

```go
func Scanln(a ...interface{}) (n int, err error)
```

`Scan()`과의 차이점: 마지막 입력값 이후 반드시 **enter 키로 입력을 종료**해야 함.

---

## 05-III. 키보드 입력과 Scan() 함수의 동작 원리

사용자가 표준 입력 장치로 **입력** → 입력 데이터는 컴퓨터 내부에 `펴준 입력 스트림`(Standard Input Stream)`이라는 메모리 공간에 임시 저장됨

**Scanf() 함수**: `표준 입력 스트림`에서 값을 읽음 → 입력값 처리

- 표준 입력 스트림
    - 스트림: 입력 데이터가 연속된 데이터 흐름 형태를 가지고 있음
    - 한 번 읽은 데이터를 **다시 읽을 수 없음**
    - `pipe`

```go
var a, b int
fmt.Scanln(&a, &b)
```

- 가장 **먼저** 입력한 데이터부터 읽어오기 때문에 데이터가 **거꾸로 저장**됨
    - **먼저 입력된 데이터가 먼저 읽히는** 데이터 구조: `FIFO`(First In First Out)

- Scan() 함수는 먼저 표준 입력 스트림에서 **한 글자** 읽어옴
    - 다시 호출됐을 때 새로운 입력을 받는 것이 아니라, 기존에 남아 있는 표준 입력 스트림에서 **다시 값을 가져옴**
    - `여러 번` Scan() 함수를 호출할 때 위와 같은 문제에서 벗어나려면, **입력에 실패한 경우 표준 입력 스트림을 지워야** 함
        - `stdin.ReadString('\n')`

```go
// ex5.8.go
package main

import (
	"bufio" // io을 담당하는 package
    // bufio는 입력 스트림부터 한 줄을 읽는 Reader 객체 제공
	"fmt"
	"os"
)

func main() {
	stdin := bufio.NewReader(os.Stdin) 
    // Reader() 함수는 인수로 입력되는 입력 스트림을 가지고 Reader 객체 생성함

	var a int
	var b int

	n, err := fmt.Scanln(&a, &b)

	if err != nil {
		fmt.Println(err)
		stdin.ReadString('\n')
        // 줄바꿈 문자가 나올 때까지 읽음 - 표준 입력 스트림이 비워짐
	} else {
		fmt.Println(n, a, b)
	}
	n, err = fmt.Scanln(&a, &b)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(n, a, b)
	}
}
```


---

**연습문제**

- fmt 패키지 이용해서 데이터를 표준 입출력 가능

- 표준 출력 함수: `Print()`, `Printf()`, `Println()`

- 서식 문자 이용 → 다양한 형식으로 출력 가능
    - 최소 출력 너비, 소숫점 이하 숫자 개수 지정 가능

- 서식 문자 `%v` 사용: 모든 타입의 **기본 서식**으로 출력

- 표준 입력 함수: `Scan()`, `Scanf()`, `Scanln()`

- 입력받을 때 에러 발생: 표준 입력 스트림 삭제

---

**연습문제**

1. 
> 00345
>
> __3.14 (_은 공란)

2. 
> fmt.Scanln(&a, &b)으로 적어야 함
>
> 메모리 주소에 써 주는 것이기 때문

3.
Chapter_05/prob3/prob3.go