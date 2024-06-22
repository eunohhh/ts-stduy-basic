# ts-study-basic

하하하하하하

## 1. readonly

### ☑️ readonly란?

    - 앞의 두 키워드는 JavaScript에서 많이 사용되는 키워드죠.
    - 하지만, readonly는 TypeScript에서 등장한 키워드에요!
    - `readonly`는 TypeScript에서 **객체의 속성을 불변**으로 만드는 데 사용되는 키워드에요!
    - 즉, 클래스의 속성이나 인터페이스의 속성을 변경할 수 없게 만들 수 있습니다.

### ☑️ 사용 사례

    ```tsx
    class Person {
        // 클래스는 다른 강의에서 자세히 설명해드릴게요!
        readonly name: string;
        readonly age: number;

        constructor(name: string, age: number) {
            this.name = name;
            this.age = age;
        }
    }

    const person = new Person("Spartan", 30);

    console.log(person.name); // 출력: 'Spartan'
    console.log(person.age); // 출력: 30

    person.name = "Jane"; // 에러: 'name'은 readonly 속성이므로 다시 할당할 수 없어요!
    person.age = 25; // 에러: 'age'은 readonly 속성이므로 다시 할당할 수 없어요!
    ```

### ⁉️ readonly를 const로 치환하면 어떻게 되나요? 이렇게 쓰면 안되나요?

    - 클래스의 속성에 const 키워드를 사용할 수 없다고 편집기가 에러를 뿜습니다!
    - const 키워드는 일반 변수를 상수화 할 때 사용하는 것이에요!

## 2. unknown 타입

### ☑️ any의 대체제 unknown이란?

    - `unknown` 타입은 `any` 타입과 비슷한 역할을 하지만 **더 안전한 방식으로 동작**합니다.
    - unknown 타입의 변수에도 모든 타입의 값을 저장할 수 있어요.
    - 하지만, 그 값을 다른 타입의 변수에 할당하려면 명시적으로 **타입을 확인해야 합니다!**

### ☑️ 사용 사례

    ```tsx
    let unknownValue: unknown = "나는 문자열이지롱!";
    console.log(unknownValue); // 나는 문자열이지롱!

    let stringValue: string;
    stringValue = unknownValue; // 에러 발생! unknownValue가 string임이 보장이 안되기 때문!
    stringValue = unknownValue as string;
    console.log(stringValue); // 나는 문자열이지롱!
    ```

    - `stringValue = unknownValue as string;` 코드를 **Type Assertion(타입 단언)**이라고 합니다.
    - unkwown 타입의 변수를 다른 곳에서 사용하려면 타입 단언을 통해 타입 보장을 하여 사용할 수 있어요!

    ```tsx
    let unknownValue: unknown = "나는 문자열이지롱!";
    let stringValue: string;

    if (typeof unknownValue === "string") {
        stringValue = unknownValue;
        console.log("unknownValue는 문자열이네요~");
    } else {
        console.log("unknownValue는 문자열이 아니었습니다~");
    }
    ```

    - 타입 단언만이 답은 아닙니다!
    - `typeof` 키워드를 이용하여 타입 체크를 미리한 후 unknown 타입의 변수를 string 타입의 변수에 할당할 수 있어요!

## 3. union

### ☑️ unknown의 한계

      unknown 타입이 그나마 재할당을 할 때 타입 체크가 되어서 안전함을 보장해요.
      하지만, unknown 타입도 결국 재할당이 일어나지 않으면 타입 안전성이 보장이 되지 않아요ㅠㅠ

### ☑️ union이란?

      이럴 때를 위해 union 타입이라는 것이 사용됩니다.
      union 은 여러 타입 중 하나를 가질 수 있는 변수를 선언할 때 사용됩니다!
      union은 | 연산자를 사용하여 여러 타입을 결합하여 표현합니다.

### ☑️ 사용 사례

```tsx
type StringOrNumber = string | number; // 원한다면 | boolean 이런식으로 타입 추가가 가능해요!

function processValue(value: StringOrNumber) {
    if (typeof value === "string") {
        // value는 여기서 string 타입으로 간주됩니다.
        console.log("String value:", value);
    } else if (typeof value === "number") {
        // value는 여기서 number 타입으로 간주되구요!
        console.log("Number value:", value);
    }
}

processValue("Hello");
processValue(42);
```

## 4. 언제 enum을 쓰고 어떨 때 object literal을 쓸까요?

### ☑️ enum을 쓰면 좋은 경우

    - enum은 간단한 상수 값을 그룹화해서 관리를 할 때 적합해요!
    - 또한, enum은 상수 값이기 때문에 **각 멤버의 값이 변하면 안된다는 조건**이 있어요.

### ☑️ 객체 리터럴을 쓰면 좋은 경우

    - 객체 리터럴은 **멤버의 값이나 데이터 타입을 맘대로 변경**할 수 있어요!
    - 복잡한 구조와 다양한 데이터 타입을 사용해야 할 때는 객체 리터럴을 사용하세요!
