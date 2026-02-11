# 🦀 Chapter 03_01. 변수 / 상수 / 섀도잉

> **작성자:** taewonki

> **학습 날짜:** 2026.02.11

> **주요 키워드:** #함수 #if #loop #for #while

---

## 1. 🥘 핵심 재료 (Key Concepts)
Rust에서 함수 시그니처(프로토타입) 을 어떻게 적는지 알아보자

### (1) 함수 작성
함수 시그니쳐 작성시 매개변수를 넣을 수 있는데 이 매개변수에는 반드시 타입이 지정되어 있어야 한다.

C 처럼 선언 순서가 따로 중요하지는 않다. 그냥 해당 스코프안에 앞이든 뒤든 선언 되어 있기만 하면 됨

또한 리턴 타입은 `->` 로 표현된다! 좀 생소하다!

```rust
fn main()
{
	print_labeled_measurement(5, 'h');
    let x = five();
	println!("value of x is {x}");
}

fn print_labeled_measurement(value: i32, unit_label:char)
{
	println!("the measurement is {value}{unit_label}");
}

fn five() -> i32
{
	5
}
```

### (2) 제어 흐름문
- **특징:** 자세히 보면 제어 흐름문이 표현식과 구문으로 나뉘어 있는 것을 확인할 수 있다. 이 부분은 따로 다루겠다.
- **종류**
    1. `if` 표현식
    - 코드가 조건에 따라 분기하도록 함
    - `if` 코드의 조건식은 반드시 bool 값이어야만 한다.

        ```rust
        fn main() {
            let number = 3;

            if number {
                println!("number was three");
            }
        }

        // if 안에 들어오는 number 값이 bool 값이 아닌 3이기 때문에 컴파일 오류난다
        ```
    - else if 에서 첫 조건을 만족하면 그 뒤의 조건은 검사하지 않는다.
    - if 는 표현식이기 때문에 let 구문의 우변에 위치 가능하다.
    2. `loop` 표현식

    그만두라고 명시적으로 표현할 때 까지 영원히 반복

    `break` 키워드를 통해 값을 반환할 수 있는 특별 기능 가지고 있음

    loop의 사용 방법은 어떤 스레드가 실행 완료 되었는지 등의 실패 가능한 연산을 재시도 할때 사용. 그리고 해당 연산의 값을 `break` 를 사용해서 전달 가능

    루프 라벨을 이용해서 break하기 : 루프 라벨은 반드시 `‘` 로 시작해야한다.

    루프 라벨을 이용하면 `break` , `continue` 할 때, 이 키워드들이 바로 바깥루프 대신 특정 루프에 적용되도록 할 수 있다.
    3. `for` 구문

    핵심 : while 과 같은 방식에서는 배열의 크기가 바뀌었을때 while 종료 조건을 결정하는 변수의 값을 수정하지 않으면 오류가 발생 할 수 있는데 for는 배열의 각 요소에 접근하기 때문에 안전해서 선호되는 구문이다.

    - 기본 문법 : 범위 사용
    ```rust
        fn main() {
        // 1부터 4까지 반복 (5는 포함되지 않음)
        for i in 1..5 {
            println!("숫자: {}", i);
        }

        println!("---");

        // 1부터 5까지 반복 (5를 포함하고 싶을 때 '=')
        for i in 1..=5 {
            println!("숫자(포함): {}", i);
        }
    }
    ```

    4. `while` 구문
    우리가 그 전에 알던거랑 비슷하다


---

## 2. 🍳 조리 과정 (Code & Analysis)
1. if가 우변에 올 수 있다는게 무슨 말일까??
    ```rust
    fn testif1()
    {
        let condition = true;
    let number = if condition { 5 } else { 6 };

        println!("The value of number is: {number}");
    }
    // 이건 가능

    fn testif2()
    {
            let condition = true;

        let number = if condition { 5 } else { "six" };

        println!("The value of number is: {number}");
    }
    // 이건 컴파일 에러난다.
    // 왜냐면 if 표현식의 결과가 5 또는 문자열 "six" 이기 때문이다.
    // 러스트는 컴파일 시점에 변수의 타입을 정확히 알기를 원하기 때문에
    // 변수의 타입이 런타임에 정의되도록 할 수 없다.
    ```
2. 예제 코드로 알아보자 loop label

    ```rust
    fn main() {
        let mut count = 0;
        'counting_up: loop {
            println!("count = {count}");
            let mut remaining = 10;

            loop {
                println!("remaining = {remaining}");
                if remaining == 9 {
                    break;
                }
                if count == 2 {
                    break 'counting_up;
                }
                remaining -= 1;
            }

            count += 1;
        }
        println!("End count = {count}");
    }
    ```

---

## 3. 🧂 깊은 맛 (Deep Dive)

---

## 4. 🔥 실패와 해결 (Troubleshooting)

---

## 5. 😋 시식평 (Tasting Note)

- **난이도:** ⭐☆☆☆☆
- **한줄평:** 비슷한 것도 있지만, 확실히 다르다.
