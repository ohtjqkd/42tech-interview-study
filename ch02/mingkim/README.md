# 함수형 프로그래밍

참고 포스팅

https://jongminfire.dev/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80  
https://velog.io/@kyusung/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9A%94%EC%95%BD  
https://engineer-mole.tistory.com/214  
https://opentogether.tistory.com/76  
https://evan-moon.github.io/2019/12/15/about-functional-thinking/

## 정의

선언형 프로그래밍으로, 애플리케이션의 상태는 순수 함수를 통해 전달된다.

상태와 가변 데이터를 멀리하는 프로그래밍 패러다임을 의미한다.

애플리케이션의 상태가 일반적으로 공유되고 객체의 메서드와 함께 배치되는 OOP와는 대조되는 프로그래밍 방식이다.

함수형 코드는 OOP나 명령형 프로그래밍보다 더 간결하고 예측가능하여 테스트 하기가 쉽다.

#### 명령형 프로그래밍

    상태와 상태를 변경시키는 관점에서 연산을 설명하는 방식. 알고리즘을 명시하고, 목표는 명시하지 않음.

#### 선언형 프로그래밍

    How보다는 What을 설명하는 방식. 알고리즘을 명시하지 않고 목표만 명시함.

<br />

함수형 프로그래밍은 다음의 특징들을 가진다.

---

## 특징

### 순수함수

순수함수란, 동일한 input에 항상 같은 return 값을 보장하는 함수를 말한다.

또한 함수의 실행이 프로그램의 실행에 영향을 미치지 않으며,

함수 내부에서 외부의 인자 값을 변경하거나 프로그램 상태를 변경하는 Side Effect가 없는 함수이다.

```python
num = 10

def add_num(a):
	return a + num

def	add(a, b):
	return a + b
```

위의 예시에서 add_num은 전역변수 num을 참조하기 때문에 순수함수가 아니다.

밑의 add함수는 프로그램 실행에 영향을 미치지 않고 입력 값에만 의존하므로 순수함수이다.

---

### 비상태, 불변성

함수형 프로그래밍에서 데이터는 불변성을 유지해야한다.

변경이 필요한 경우, 원본은 그대로 두고 복사본을 만들어 변경 후 진행한다.

```python
class Person():
	def __init__(self, name, age) -> None:
		self.name = name
		self.age = age

mingkim = Person("mingkim", 27)

def increase(p):
	p.age += 1
	return p

print(mingkim.age) # 27
increase(mingkim)
print(mingkim.age) # 28
```

위의 예제에서 increase 함수는 전역변수 mingkim의 age 속성을 변경하므로 불변성 유지를 만족하지 않는다.

```python
class Person():
	def __init__(self, name, age):
		self.name = name
		self.age = age

mingkim = Person("mingkim", 27)

def increase(person: Person):
	return Person(person.name, person.age + 1)

print(mingkim.age) # 27
newPerson = increase(mingkim)
print(newPerson.age) # 28
```

---

### 선언형 함수

명령형 프로그래밍은 무엇을 _어떻게_ 할 것인가에 주목하고, 선언형 프로그래밍은 _무엇을_ 할 것인가에 주목한다.

```python
arr = [1, 2, 3, 4, 5, 6, 7, 8]


def multiply(numbers, multiplier):
    for i, j in enumerate(numbers):
        numbers[i] = j * multiplier


print(arr) #  [1, 2, 3, 4, 5, 6, 7, 8]
multiply(arr, 10)
print(arr) #  [10, 20, 30, 40, 50, 60, 70, 80]

```

위 예시에서는 for문을 사용해 배열의 각 요소에 multiplier를 곱해준다.

함수형 프로그래밍에서는 if, switch, for등 명령문을 사용하지 않고 함수형 코드로 사용한다.

```python
arr = [1, 2, 3, 4, 5, 6, 7, 8]


def multiply(numbers, multiplier):
    return list(map(lambda x: x * multiplier, numbers))


print(arr)
#[1, 2, 3, 4, 5, 6, 7, 8]
print(multiply(arr, 10))
#[10, 20, 30, 40, 50, 60, 70, 80]
```

for문을 map으로 대치한다. 곱해주는 부분을 lambda 를 이용해서 대치했다.

---

### 이해를 돕기위해 참조 블로그에서 가져온 javascript 예시.

```javascript
// 명령형 프로그래밍
let numbers = [1, 2, 3];

function multiply(numbers, multiplier) {
	for (let i = 0; i < numbers.length; i++) {
		numbers[i] = numbers[i] * multiplier;
	}
}

// 선언형 프로그래밍
function multiply(number, multiplier) {
	return number.map((num) => num * multiplier);
}
```

---

### 1급 객체와 고차함수

함수형 프로그래밍에서는 함수가 1급 객체가 된다. 1급 객체의 특징은 다음과 같다.

-   변수나 데이터 구조에 담을 수 있다.
-   파라미터로 전달 가능하다
-   리턴값으로 전달 가능하다
-   할당에 사용된 이름과 관계없이 고유한 구별이 가능하다
-   동적으로 프로퍼티 할당이 가능하다

고차함수는 함수를 인자로서 전달하며, 함수의 반환값으로 또 다른 함수를 사용할 수 있다는 것이다.

```python
arr = [1, 2, 3, 4, 5, 6, 7, 8]

def add_two(num):
    return num + 2

def multiply_two(num):
    return num * 2

def transform(numbers, func1, func2):
    return list(map(func1, map(func2, numbers)))

print(transform(arr, multiply_two, add_two))
```

예시처럼 함수를 변수에 할당하거나 반환하는 것이 1급 객체의 특징이다.

또한 함수를 parameter로 받고 반환도 가능할 것이다.

```python
from functools import partial


def doubleNumber(num):
    return 2 * num


arr = [1, 2, 3, 4]
double = partial(map, doubleNumber)
number = double(arr)
print(*number)
# 2 4 6 8
```

간단한 함수형 프로그래밍 예시

```python
def exp(base: int, power: int):
	return base ** power

# 2의 n승을 계산해주는 함수
def two_to_the(n):
	return exp(2, power)
```

```python
def exp(base, power):
    return base ** power


def two_to_the(power):
    return exp(2, power)


two_to = partial(exp, 2)
two_to_three = partial(exp, 2, 3)
print(two_to_the(3))	# 8
print(two_to(3))		# 8
print(two_to_three())	# 8
```

---

## 함수형 프로그래밍의 장점

1. 높은 수준의 추상화 제공
2. 함수 단위의 코드 재사용 수월
3. 불변성을 지향해서 프로그램 동작 예측 및 디버깅이 쉽다

## 함수형 프로그래밍의 단점

1. 순수함수를 구현하기 위해서는 코드의 가독성이 좋지 않을 수 있다.
2. 함수형 프로그래밍에서는 반복이 for문이 아닌 재귀를 통해 이루어지는데(deep copy), 재귀적 코드 스타일은 무한루프에 빠지기가 쉽다.
3. 순수함수를 사용하기는 쉽지만, 조합하기는 쉽지 않은 상황이 많다.
