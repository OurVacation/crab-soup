# 🦀 Chapter 05_01. 구조체 (Struct)

> **작성자:** taewonki
> **학습 날짜:** 2026.02.13
> **주요 키워드:** #구조체 #Struct #인스턴스 #소유권이동

---

## 1. 🥘 핵심 재료 (Key Concepts)

### (1) 구조체 정의와 인스턴스화
- **정의:** 구조체의 구성 요소는 각각 다른 타입을 가질 수 있으며, 각 구성 요소별로 이름을 붙일 수 있다. 이로 인해 튜플처럼 순서에 의존할 필요 없이 각 요소에 의미를 부여할 수 있다.
- **인스턴스화:** 구조체 이름을 명시하고 중괄호 내부에 `필드명: 저장할 값` 형태로 값을 부여해 생성한다. 가변 인스턴스로 선언하면 내부 값을 변경할 수 있으나, 일부분만 가변으로 만들 수는 없다.
- **필드 초기화 축약법:** 함수 매개변수와 구조체 내부 필드명이 동일하면 할당 과정을 생략할 수 있다.

### (2) 구조체 업데이트 문법 (Struct Update Syntax)
- **정의:** 인스턴스 생성 시, 기존 인스턴스의 일부분만 갱신하고 나머지는 동일한 값으로 채우고 싶을 때 `..인스턴스명` 문법을 사용한다.
- **주의점:** 이 문법은 대입(`=`)과 같이 동작한다. `Copy` 트레이트가 없는 필드(예: `String`)는 소유권이 새 인스턴스로 이동(Move)되어 원본 인스턴스에서 사용할 수 없게 된다.

### (3) 튜플 구조체와 유사 유닛 구조체
- **튜플 구조체:** 구조체 자체에는 이름이 있으나 필드에는 이름 없이 타입만 있는 형태이다. `Color(i32, i32, i32)` 처럼 전체에 이름을 붙이고 싶을 때 사용하며, 내부 필드 구성이 같아도 이름이 다르면 서로 다른 타입이다.
- **유사 유닛 구조체:** 필드가 없는 구조체로 `()` 유닛 타입과 유사하게 동작한다. 데이터 저장 없이 트레이트만 구현하고 싶을 때 사용된다.

---

## 2. 🍳 조리 과정 (Code & Analysis)

```rust
struct User {
    active: bool,
    email: String,
    username: String,
    sign_in_count: u64,
}

fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username, // 필드 초기화 축약법 사용
        email,
        sign_in_count: 1,
    }
}

fn main() {
    let user1 = build_user(String::from("taewon990816@naver.com"), String::from("taewonkim"));

    // 구조체 업데이트 문법 사용
    let user2 = User {
        email: String::from("taewonki@42gs.com"),
        ..user1
    };

    if user2.active && user2.sign_in_count == 1 {
        println!("username : {0}", user2.username);
        println!("user email : {0}", user2.email);
    }

    // 아래 코드는 컴파일 에러를 발생시킨다.
    // println!("username : {0}", user1.username);
}
```

### 🔍 코드 분석
- **Line 11:** `username`과 `email`은 매개변수와 필드명이 같아 축약법이 사용되었다.
- **Line 22:** `..user1`을 통해 `user1`의 나머지 필드 값을 `user2`로 가져온다.
- **Line 31:** `user1.username`의 소유권이 `user2`로 이동했기 때문에 `user1`에서는 더 이상 `username`을 사용할 수 없다.

---

## 3. 🧂 깊은 맛 (Deep Dive)

### 구조체 데이터의 소유권
- **String vs &str:** 예제에서는 구조체가 데이터를 온전히 소유하기 위해 `String` 타입을 사용했다. 이는 구조체 인스턴스가 유효한 동안 데이터도 유효함을 보장하기 위함이다.
- **참조자 사용 시:** 구조체에 `&str` 같은 참조자를 저장할 수도 있지만, 이 경우 원본 데이터의 유효성을 보장받기 위해 **라이프타임(Lifetime)**을 명시해야 한다.

---

## 4. 🔥 실패와 해결 (Troubleshooting)

### ❌ Error Scenario
- `user2`를 생성할 때 구조체 업데이트 문법을 사용하여 `user1`의 내용을 가져온 후, `user1.username`을 출력하려 시도함.

### ✅ 해결 방법 (Solution)
- **원인:** 구조체 업데이트 문법은 단순 복사가 아니라 대입(`=`)과 같다. `username`은 `String` 타입이므로 소유권이 `user2`로 이동(Move)되었다. 반면 `active`나 `sign_in_count` 같은 `Copy` 타입 필드는 `user1`에서도 여전히 사용 가능하다.
- **해결:** `user1`의 데이터를 계속 쓰고 싶다면 `clone()`을 사용하거나 소유권 이동 규칙을 인지하고 코드를 작성해야 한다.

---

## 5. 😋 시식평 (Tasting Note)

- **난이도:** ⭐⭐⭐☆☆
- **한줄평:** "구조체 업데이트 문법이 편해 보였는데, 소유권 이동이 일어난다는 점은 주의해야겠다."
- **다음 목표:** "유사 유닛 구조체가 트레이트와 어떻게 쓰이는지 나중에 확인해봐야겠다."
