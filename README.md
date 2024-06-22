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

## 5. 유틸리티 타입 훑어보기

### 1) Partial<T>

#### ☑️ Partial<T> 타입이란?

    - `Partial<T>`  타입은 타입 T의 모든 속성을 선택적으로 만들어요!
    - 이를 통해 기존 타입의 **일부 속성만 제공하는 객체**를 쉽게 생성할 수 있습니다.
    - 아직, 무슨 말인지 잘 모르시겠죠? 예제를 살펴볼게요!

#### ☑️ 사용 사례

```tsx
interface Person {
    name: string;
    age: number;
}

const updatePerson = (person: Person, fields: Partial<Person>): Person => {
    return { ...person, ...fields };
};

const person: Person = { name: "Spartan", age: 30 };
const changedPerson = updatePerson(person, { age: 31 });
```

    - 우선, Person이라는 인터페이스는 name, age라는 속성으로 구성이 되어있어요!
    - updatePerson 함수를 자세히 봐보세요. 2번째 인자로 `Partial<Person>` 타입의 fields를 받고 있어요.
    - 이 field라는 인자가 구성이 될 수 있는 경우의 수는 다음과 같아요.
        - name이라는 속성만 있어도 됩니다.
        - age라는 속성만 있어도 됩니다.
        - name, age라는 속성이 둘 다 있어도 됩니다.
        - 이 밖의 상황은 허용하지 않아요!
            - 예를 들어, `{ name, gender }`와 같이 기존에 없는 속성을 넣어서는 안됩니다!
    - 이렇게 Partial<T> 타입으로 유연하게 타입의 속성을 선택해서 객체를 만들 수 있어요!

### 2) Required<T>

#### ☑️ Required<T> 타입이란?

    -   Partial<T> 타입과는 반대로 `Required<T>` 타입은 타입 T의 모든 속성을 필수적으로 만듭니다!
    -   다시 말해서, T 타입 객체에 정의된 **모든 속성이 반드시 전부 제공**이 되는 객체를 생성해야 할 때 쓰이죠!

#### ☑️ 사용 사례

```tsx
interface Person {
    name: string;
    age: number;
    address?: string; // 속성 명 뒤에 붙는 ?가 뭘까요
}
```

    -   아까 봤던 Person이라는 인터페이스에 address라는 속성이 새로 생겼어요.
    -   그런데, address에 `?` 라는 문자가 붙었네요.
        -   이 친구는 **선택적 속성**입니다.
        -   있어도 되고 없어도 되는 친구라는 것이에요!
    -   하지만, address를 필수적으로 받아야 하는 제약사항이 있다고 하면 다음과 같이 할 수 있어요!

    ```tsx
        type RequiredPerson = Required<Person>;
    ```
    -   이렇게 Required<T> 타입을 통해 선언하면 address 입력도 필적으로 되는 것입니다!

### 3) Readonly<T>

#### ☑️ Readonly<T> 타입이란?

    -   이름부터 역할이 유추되는 `Readonly<T>` 타입은 타입 T의 모든 속성을 읽기 전용(read-only)으로 만들어요!
    -   이를 통해 readonly 타입의 속성들로 구성된 객체가 아니어도 **완전한 불변 객체로 취급**할 수 있어요!

#### ☑️ 사용 사례

```tsx
interface DatabaseConfig {
    host: string;
    readonly port: number; // 인터페이스에서도 readonly 타입 사용 가능해요!
}

const mutableConfig: DatabaseConfig = {
    host: "localhost",
    port: 3306,
};

const immutableConfig: Readonly<DatabaseConfig> = {
    host: "localhost",
    port: 3306,
};

mutableConfig.host = "somewhere";
immutableConfig.host = "somewhere"; // 오류!
```

    -   DatabaseConfig는 불변 객체라고 할 수 없어요. 왜냐하면 host가 readonly가 아니죠!
    -   하지만, ReadOnly<T> 타입으로 불변 객체로 만들 수 있으니 걱정마세요!

### 4) Pick<T, K>

#### ☑️ Pick<T, K> 타입이란?

    -   `Pick<T, K>` 유틸리티 타입은 타입 T에서 **K 속성들만 선택하여 새로운 타입**을 만듭니다!
    -   이를 통해 타입의 일부 속성만을 포함하는 객체를 쉽게 생성할 수 있어요.

#### ☑️ 사용 사례

```tsx
interface Person {
    name: string;
    age: number;
    address: string;
}

type SubsetPerson = Pick<Person, "name" | "age">;

const person: SubsetPerson = { name: "Spartan", age: 30 };
```

    -   SubsetPerson은 Person이라는 인터페이스에서 `name`, `age` 속성만 선택해서 구성된 새로운 타입이에요!

### 5) Omit<T, K>

#### ☑️ Omit<T, K> 타입이란?

    -   `Omit<T, K>` 유틸리티 타입은 타입 T에서 K 속성들만 제외한 새로운 타입을 만듭니다!
    -   Pick<T, K> 유틸리티 타입과는 반대의 동작이죠!
    -   이를 통해 기존 타입에서 특정 속성을 제거한 새로운 타입을 쉽게 생성할 수 있어요.

#### ☑️ 사용 사례

```tsx
interface Person {
    name: string;
    age: number;
    address: string;
}

type SubsetPerson = Omit<Person, "address">;

const person: SubsetPerson = { name: "Alice", age: 30 };
```

    -   여기서 SubsetPerson 타입은 Person 타입에서 `address` 속성만 제외한 새로운 타입입니다!
