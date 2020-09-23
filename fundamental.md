## 1. 기본타입

### 1) 변수 선언

- ts는 변수명 뒤에 타입을 명시하는 방법으로 타입을 선언한다.
- 선언한 타입에 맞지 않는 값을 할당하면 컴파일 시점에 에러가 발생한다.
```ts
let too: string = 'hello';
```
- boolean
```ts
let isDone: boolean = false;
```
- number
```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

- string

: ``을 사용하여 여러 줄의 문자열 작성가능하다.

: `${expr}`과 같은 표현식도 ``안에 포함할 수 있다.
```ts
// string
let color: string = "blue";
color = 'red';
let fullName: string = 'tomas';
let age: number = 30;
let setence: string = `hello, my name is ${fullName}.
  I'll be ${age+1} years old next month`;
let setence2: string = "hello, my name is" + fullName + "I'll be" + (age+1) + "years old next month";

```

- array
: 배열타입선언 2가지(`type[]`, `Array<type>`)이다. 

```ts
let list: number[] = [1,2,3];
let list2: Array<number> = [1,2,3];
```


- tuple

: tuple은 요소의 타입과 개수가 고정된 배열을 표현할 때 사용한다.

: 요소들이 모두 같은 타입일 필요는 없다.

```ts
//아래는 number, string이 쌍으로 있는 튜플
let x: [string, number];
x = ["hello", 10];
//정해진 인덱스에 위치한 요소에 접근하면 해당 타입이 나타남
console.log(x[0].substring(1));   //ello
```

- Enum(열거형)

: js의 집합과 사용하면 도움이 되는 데이터형이다.

: enum은 숫자값 집합에 이름을 지정한다.

: enum은 0부터 시작하여 멤버들의 번호를 매긴다.

:  멤버 중 하나의 값을 수동으로 설정할 수 있다.
```ts
enum Color {Red, Green, Blue}
let c: Color Color.Green;

//1부터 번호를 매긴 경우
enum Color {Red=1, Green, Blue}
let c: Color = Color.Green;

//수동설정
enum Color {Red=1, Green=2, Blue=4}
let c: Color = Color.Green;

// 매겨진 값을 사용해 enum멤버의 이름을 알아낼 수 있음
// 아래 예제는 2와 매치되는 멤버를 알아냄
enum Color {Red=1, Green, Blue}
let colorName: string = Color[2];
console.log(colorName);   //값이 2인 Green이 출력
```
- Any

: 타입 추론(type inference)할 수 없거나 타입 체크가 필요없는 변수에 사용한다.

: 애플리케이션을 만들 때 알지못하는 타입을 표현해야한다.

: Obejct와 비슷한 역할이라고 볼 수 있다.

```ts
let notSure: any = 4;
notSure = "maybr a string instead";
notSure = false;

let notSure: any = 4;
notSure.ifItExists();    //성공, ifItExists는 런타임엔 존재할 것...
notSure.toFixed();       //성공, toFixed는 존재함(하지만 컴파일러가 검사x)


//Object로 선언한 변수는 어떤 값이든 그 변수에 할당가능하지만 
//실제로 메서드가 존재해도 임의 호출이 불가
let pretty: Object = 4;
pretty.toFixed();        //오류, toFixed는 Object에 존재하지 않음(임의호출x)

//타입의 일부만 알고 전체는 알지 못핼 때 유용
// ex. 여러 다른 타입이 섞인 배열을 다룰 수 있음
let list: any[] = [1, true, "free"];
list[1] = 100;
```

- void

: 보통 함수에서 반환값이 없을 때 반환 타입을 표현하기 위해 쓰인다.

: void타입의 변수는 null, undefined만 할당가능하다.

```ts
function warnUser: void{
  console.log("this is warning message");
}

//void를 타입변수로 선언하는 건 좋지않음
let unusable: void = undefined;
unusable = null;
```

- null and undefined

: null, undefined는 다른 타입들의 하위타입이다.

: --strictNullChecks(엄격한 Null 검사)를 사용하면, null과 undefined는 오직 any와 각자 자신들 타입에만 할당가능하다

-> 이경우 string 또는 null or undefined를 허용하고 싶다면 
   유니언타입인 `string|null|undefined`를 사용하면 됨

```ts
let u: undefined = undefined;
let n: null = null;
```


- Never 

: never는 절대 발생할 수 없는 타입(값)이다.

: 함수표현식, 화살표함수표현식에서 항상 오류를 발생시키거나, 절대 반환하지 않는 반환타입으로 쓰인다. 

: never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.

```ts
function error(message: string): never{
  throw new Error(message);
}
//반환타입이 never로 추론된다.
function fail(){
  return error("something failed");
}
```

- Object

: num,str,bigint, symbol, null, undefined가 아닌 나머지이다.

```ts
declare function create(o: object | null): void;

create({prop: 0});  //성공
create(null);       //성공
create(42)          //fail
create("string")    //fail
create(false)       //fail
create(undefined)   //fail
```

- 타입단언(type assertions)

: 때론, 어떤 엔티티의 실제타입이 현재타입보다 더 구체적일 때 발생한다.

: 다른 언어의 형변환과 유사하지만, 다른 특별한 검사, 데이터 재구성은 하지 않는다.

: 런타임에 영향을 미치지 않으며, 온전히 컴파일러만 이를 사용한다.

```ts
//타입단언의 첫번째 형태 : angle-bracket
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```
```ts
//타입단언의 두번째 형태 : ~을 as ~type으로 형변환
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length; //jsx사용시 이 스타일만 허용
```

- 가능한 var보다는 let을 쓸 것

<br/>

### 2) 타입선언

- 함수 타입 선언

: 선언된 타입에 일치하지 않는 값이면 에러가 발생한다.

```ts
// 함수선언식[x,y인수를 가지는 number타입의 함수]
function multiply1(x: number, y: number): number {
  return x * y;
}

// 함수표현식
const multiply2 = (x: number, y: number): number => x * y;

console.log(multiply1(10, 2));
console.log(multiply2(10, 3));

console.log(multiply1(true, 1)); 
// error TS2345: Argument of type 'true' is not assignable to parameter of type 'number'.
```

- 타입은 소문자, 대문자 구분

: TypeScript가 기본 제공하는 타입은 모두 소문자이다. 

```ts
// string: 원시 타입 문자열 타입
let primiteveStr: string;
primiteveStr = 'hello'; // OK

// 원시 타입 문자열 타입에 객체를 할당하였다.
primiteveStr = new String('hello'); // Error

// String: String 생성자 함수로 생성된 String 래퍼 객체 타입
let objectStr: String;
objectStr = 'hello'; // OK
objectStr = new String('hello'); // OK
```

<br/>

### 3) 정적 타이핑

- 정적 타이핑이란?

: C나 Java언어는 변수 선언시 선언한 타입에 맞는 값을 할당해야 한다.(정적타이핑) 

: 자바스크립트는 변수의 타입 선언 없이 값이 할당되는 과정에서 동적으로 타입을 추론한다.(동적 타이핑)

: 동적 타이핑은 사용하기 간편하지만 코드를 예측하기 힘들어 예상치 못한 오류를 만들 가능성이 높다. 

: TypeScript은 타입을 명시적으로 선언하며, 타입이 결정된 후에는 타입을 변경할 수 없다.

: 정적 타이핑은 코드 가독성, 예측성, 안정성의 향상으로, 이는 대규모 프로젝트에 매우 적합하다.

```ts
let foo: string,   // 문자열 타입
    bar: number,   // 숫자 타입
    baz: boolean;  // 불리언 타입

foo = 'Hello';
bar = 123;
baz = 'true'; // error: Type '"true"' is not assignable to type 'boolean'.

function add(x: number, y: number): number {
  return x + y;
}

console.log(add(10, 10)); // 20
console.log(add('10', '10'));
// error TS2345: Argument of type '"10"' is not assignable to parameter of type 'number'.
```

<br/>

### 4) 타입 추론

만약 타입 선언을 생략하면 값이 할당되는 과정에서 동적으로 타입이 결정된다. 

```ts
let foo = 123; // foo는 number 타입

let foo = 123; // foo는 number 타입
foo = 'hi';    // error: Type '"hi"' is not assignable to type 'number'.

let foo; // let foo: any와 동치

foo = 'Hello';
console.log(typeof foo); // string

foo = true;
console.log(typeof foo); // boolean
```
