# Chapter 14. Pointer

## 14.1 포인터란?

포인터란? 메모리 주소를 값으로 갖는 타입   

예를 들어, int 타입 변수 a의 값이 3이고, 메모리 주소는 0x0100 번지라면,

```go
p = &a
```

p는 0x0100 이 된다.   

이렇게 메모리 주솟값을 변숫값으로 가질 수 있는 변수를 **포인터 변수**라고 한다.   

→ '포인터 변수 p가 변수 a를 가리킨다'고 한다.

### 📌 포인터 변수 선언

포인터 변수는 가리키는 데이터 타입 앞에 * 을 붙여서 선언한다. → `var p *int`

```go
var a int
var p *int

p = &a  // a의 주솟값(&a)을 p에 대입
```

그렇다면, p를 이용해서 변수 a값을 변경하는 방법은?   

→ `*p = 20`     

// a값이 20으로 변경된다.       

이렇게 * 을 붙이면 그 포인터 변수가 가리키는 메모리 공간에 접근할 수 있다. 

### 📌 포인터 변숫값 비교하기

`==` 연산을 사용해 포인터가 같은 메모리 공간을 가리키는지 확인할 수 있다.

```go
	var a int = 10
	var b int = 20

	var p1 *int = &a // a의 메모리 공간
	var p2 *int = &a // a의 메모리 공간
	var p3 *int = &b // b의 메모리 공간

	fmt.Printf("p1 == p2 : %v\n", p1 == p2)
	fmt.Printf("p2 == p3 : %v\n", p2 == p3)
```

### 💡 포인터의 기본값은 nil

```go
var p *int
if p != nil { ... }  
```

## 14.2 포인터는 왜 쓰나?

변수 대입이나 함수 인수 전달은, 항상 값을 **복사**하기 때문에, 변경사항이 적용되지 않는다.     

또한 커다란 메모리 공간을 복사하면 성능 문제도 있다.    

이렇게 복사하는 문제를 해결하기 위해, 포인터로 직접 주솟값에 접근해주는 것이다. 

```go
type Data struct {
	value int
	data [200]int
}

// 단순 값 복사 -> 실제 main에서 변경되지 않는 문제
func ChangeData(d Data){
	d.value = 200
	d.data[100] = 999
}

// 포인터 사용 -> 값 변경 가능
func ChangeData_withPointer(d *Data){
	d.value = 200
	d.data[100] = 1000
}
```

## 14.3 인스턴스

인스턴스란? 메모리에 할당된 데이터의 **실체**이다.    

```go
var data Data
```

이렇게 할당된 메모리 공간의 실체를 **인스턴스**라고 부른다.   

### 💡 포인터 변수 VS 인스턴스

```go
var data Data
var p *Data = &data   //변수 data의 주솟값
```

이때 포인터 변수 p는 data를 가리킨다고 말한다.   

p가 생성될 때,  새로운 Data 인스턴스가 만들어진 게 아니다. 기존에 있던 data 인스턴스를 가리킨 것이므로, 만들어진 인스턴스 개수는 **1개**이다. 

### → 포인터 변수가 아무리 많아도 인스턴스가 추가로 생성되는 것은 아니다.

```go
var p1 *Data = &Data{}
var p2 *Data = p1
var p3 *Data = p1
```

이 경우에도 인스턴스는 **1개**이다. 

그럼 다음 코드에서는 몇 개일까? → **3개** 

```go
var data1 Data
var data2 Data = data1
var data3 Data = data1
```

### 📌 new() 내장 함수

포인터 변수를 초기화하는 방법은 3가지가 있다. 

- 방법 1

```go
var data Data
var p *Data = &data
```

- 방법 2: 포인터값을 별도의 변수를 선언하지 않고 초기화하는 방법

```go
p := &Data{}  
```

- 방법 3: `new()` 함수 사용

```go
var p = new(Data)
```

→ new() 함수는 인수로 타입을 받아, 타입을 메모리에 할당하고 기본값으로 채워 그 주소를 반환한다.

### 📌 인스턴스는 언제 사라지나

가비지 컬렉터 (Garbage Collector) 가 메모리에서 쓸데없는 데이터를 해제한다.    

그렇다면 사용되는 데이터인지 아닌지는 어떻게 알까? 

```go
func TestFunc(){
	u := &User{}
	u.Age = 30
	fmt.Println(u)
}  // 내부 변수 u는 사라진다. 더불어 인스턴스도 사라진다.
```

함수 TestFunc() 이 종료되면 함수의 내부 변수 u는 사라져 User 인스턴스를 가리키는 포인터 변수가 없게 된다.    

그러므로 쓸모가 없어진 User 인스턴스는 다음번에 가비지 컬렉터가 청소한다. 

# Chapter 14 연습문제

1. 8
2.  

```go
func NewActor(name string, hp int, speed float64) *Actor {
	a := Actor{name, hp, speed}
	return &a
}

// 또는 return &Actor{name, hp, speed}
```

3. 1개
