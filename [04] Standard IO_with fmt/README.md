### 📌 fmt 패키지

표준 입출력 기능을 제공하는 Go 언어 기본 패키지

`import "fmt"`

## 4.1 표준 출력 `Print()`

### 📌 Print(), Println(), Printf() 함수 비교

- `Print()` → 출력
- `Println()` → 출력하고 개행
- `Printf()` → 서식(format)에 맞도록 출력 (%d, %f, %s 등 이용)

```go
	var a int = 10
	var b int = 20
	var f float64 = 123456789.9876

	fmt.Print("a:", a, "b:", b)
	fmt.Println("a:", a, "b:", b, "f:", f)
	fmt.Printf("a: %d b: %d f: %f\n", a, b, f)
```

→ 결과

```go
a:10b:20a: 10 b: 20 f: 1.234567899876e+08
a: 10 b: 20 f: 123456789.987600
```

여기서 주목할 점은, `Println()` 함수로 f값을 출력했을 때와 `Printf()` 함수로 출력했을 때 **형태가 다르다**는 점이다. 

`Println()` 함수로 실수 출력 시, %f가 아니라 **%g** 서식으로 출력되므로, 값이 **지수 형태**로 출력된다.

### 📌 Printf() 함수 사용하기

`Printf(서식 문자열, 인수1, 인수2, ...)`

서식 문자 종류: `%d`, `%f`, `%s`, `%v`

1. **최소 출력 너비** 지정이 가능하다.
    - **`%5d`**(최소 5칸을 이용해서 채움)
    - **`%05d`**(빈칸은 0으로 채움)
    - **`%-5d`**(왼쪽 정렬)
    
    ```go
    	var a = 123
    	var b = 456
    	var c = 123456789
    
    	fmt.Printf("%5d, %5d\n", a, b)    // %5d
    	fmt.Printf("%05d, %05d\n", a, b)  // %05d
    	fmt.Printf("%-5d, %-05d\n", a, b) // %-5d (왼쪽 정렬)
    
    	// 지정한 최소 너비보다 긴 값일 경우
    	fmt.Printf("%5d\n", c)
    	fmt.Printf("%05d\n", c)
    
    /* 결과 */
    123,   456
    00123, 00456
    123  , 456
    123456789
    123456789
    ```
    
2. 실수 소수점 이하 자릿수 지정이 가능하다.
    - **`%f`**
    - **`%g`** (실수를 정수부와 소수점 이하 숫자를 포함해 출력 숫자를 제한한다) → 나머지는 지수 표현으로 전환함.
    - **`%8.5f`** (최소 너비 8로 채우고, 소수점 이하는 5자리만 표현)
    - **`%8.5g`** (최소 너비 8로 채우고, 총 숫자를 5자리로 제한)
    
    아무 옵션 주지 않았을 시, 기본 숫자 길이는 6자리로 제한된다.
    
    ```go
    	var a = 123.12345
    	var b = 3.14
    
    	// %f와 %g 차이 비교
    	fmt.Printf("%08.2f\n", a) //최소 너비 8, 소수점 이하 2자리, 공백은 0으로 채움
    	fmt.Printf("%08.2g\n", a) //최소 너비 8, 총 숫자 2자리, 공백은 0으로 채움
    
    	fmt.Printf("%8.5g\n", a) //최소 너비 8, 총 숫자 5자리
    	fmt.Printf("%f\n", b) // 소수점 이하 6자리까지 출력
    
    /* 결과 */
    00123.12
    01.2e+02
      123.12
    3.140000
    ```
    
3. 특수 문자 사용 가능
- **`\n`**  → 줄바꿈
- **`\t`**  → 탭
- **`\\`**  → \ 자체를 출력한다.
- **`\"`**  → 따옴표 출력

## 4.2 표준 입력 `Scan()`

### 📌 Scan(), Scanln(), Scanf() 함수 비교

- `Scan()` → 입력
- `Scanln()` → 한 줄을 읽어서 입력 받음
- `Scanf()` → 서식(format) 형태로 값을 입력 받음

```go
	var a int
	var b int

	n, err := fmt.Scan(&a, &b) // n과 err에 입력 두 값 받기
	if err != nil {
		fmt.Println(n, err)
	} else {
		fmt.Println(n,a,b)
	}
```