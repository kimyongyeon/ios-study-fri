# 프로퍼티

# 종류

- 저장 프로퍼티 (stored property)
- 연산 프로퍼티 (computed property)
- 인스턴스 프로퍼티  (instance property)
- 타입 프로퍼티 (type property)

# 특징

프로퍼티는 구조체, 클래스, 열거형 내부에 구현할 수 있다.

다만 열거형 내부에는 연산 프로퍼티만 구현할 수 있다.

연산 프로퍼티는 var로만 선언할 수 있다. 

## 타입 프로퍼티 특징

- 선언할 당시 원하는 값으로 항상 초기화 되어 있어야 한다.
- static을 이용해 선언하며, 자동으로 lazy로 작동한다.(lazy를 직접 붙일 필요 또한 없다)
    - [Human.name](http://Human.name) 이란 프로퍼티를 호출하기 전까진 초기화 하지 않음!
    
    ```swift
    class Human {
        static let name: String = "sodeul"     // 저장 타입 프로퍼티
        static var alias: String {             // 연산 타입 프로퍼티
            return name + "은 바보"
        }
    }
    ```
    
- 인스턴스가 생성될 때마다 "매번 생성"되는 "기존 프로퍼티"와 다름 
인스턴스가 생성 된다고 매번 해당 인스턴스의 멤버로 매번 생성되는 것이 아니라,
언제 한번 누군가 한번 불러서 메모리에 올라가면, 그 뒤로는 생성되지 않으며, 언제 어디서든 이 타입 프로퍼티에 접근 할 수 있는 것임
- 인스턴스 생성 안함, 프로퍼티를 공유하는 형태

```swift
struct Student {
	// 인스턴스 저장 프로퍼티 
	var name: String = ""
	var `class`: String = "Swift"
	var koreanAge: Int = 0
	
	// 인스턴스 연산 프로퍼티 
	var westernAge: Int {
		get {
			return koreanAge - 1
		}
		set (inputvalue) {
			koreanAge = inputValue + 1
		}
	}

	// 타입 저장 프로퍼티 
	static var typeDescription: String = "학생"

	// 인스턴스 메서드 
	func selfIntroduce() -> String {
		print("저는 \(self.class)반 \(name) 입니다.")
	}

	// 읽기전용 인스턴스 연산 프로퍼티 
	var selfIntroduction: String {
		get {
			return "저는 \(self.class)반 \(name) 입니다."
		}
	}
	
	// 타입 메서드 
	static func selfStaticIntroduce() {
		print ("학생타입입니다.")
	}
	
	// 읽기전용 타입 연산 프로퍼티 
	// 읽기전용에서는 get을 생략할 수 있다.
	static var selfStaticIntroduction: String {
		return "학생타입입니다."
	}
}

// 인스턴스 생성
var yagom: Student = Student()
yagom.koreanAge = 10

// 인스턴스 저장 프로퍼티 사용
yagom.name = "yagom"
print(yagom.name)

// 인스턴스 연산 프로퍼티 사용
print(yagom.selfIntroduction)

// MARK: - 응용
class Money {
	var currencyRate: Double = 1100
	var dollar: Double = 0
	var won: Double {
		get {
			return dollar * currencyRate
		}
		set {
			// 매개변수 입력 안하면 newValue로 들어온다. 
			dollar = newValue / currencyRate
		}
	}
}

var moneyInMyPocket = Money()
moneyInMyPocket.won = 11000
print(moneyInMyPocket.won)
moneyInMyPocket.dollar = 10
print(moneyInMyPocket.won)
```

만약 선언과 동시에 저장 타입 프로퍼티를 초기화 안 해주면?

![Untitled](%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%91%E1%85%A5%E1%84%90%E1%85%B5%205a1e43de2b194cecaf952bbcdd6563cc/Untitled.png)

# 연산 타입 프로퍼티의 오버라이딩

## class: 오버라이딩이 가능한 “연산 타입 프로퍼티“

```swift
class Human {
    class var alias: String {
        return "Human Type Property"
    }
}
 
class Sodeul: Human {
    override class var alias: String {
        return "Sodeul Type Property"
    }
}
 
Human.alias             // "Human Type Property"
Sodeul.alias            // "Sodeul Type Property"
```

## static: 오버라이딩이 “불” 가능한 “연산 타입 프로퍼티”

```swift
class Human {
    static var alias: String {
        return "Human Type Property"
    }
}
 
class Sodeul: Human {
    override static var alias: String {     // error! Cannot override static property
        return "Sodeul Type Property"
    }
}
```

# 타입 프로퍼티는 왜 쓸까?

보통 모든 타입이 공통적인 값을 정의하는데 유용하게 쓰임