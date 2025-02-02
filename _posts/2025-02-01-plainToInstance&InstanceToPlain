---
layout: single
title: "[NodeJS] - class-transformer & Record타입 슬쩍 들여다보기"
categories: "library"
tag: ["library", "grammar", "nodejs", "nestjs", "typescript"]
toc: true
---

- plainToInstance, instanceToPlain에 대해
- Knex의 insert의 파라미터타입에 대해
- Record<string,any>에 대해

# 문제
```typescript
const insertDataList = plainToInstance(InsertDataDto, dataList);

    return repository.insert(insertDataList);
```
여기서 repository.insert시 정상작동은 하지만 아래 에러표시가 발생했다.

```
TS2559: Type InsertDataDto[]  has no properties in common with type  Partial<T>
```

이걸 어떻게 해결할 수 있을까?   
결론만 말하자면 insert의 파라미터타입을 Partial\<T>[]  로 변경해주면된다.


---

# 1.plainToInstancne, instanceToPlain?   
plainToInstancne, instanceToPlain 모두   
class-transformer라이브러리에 있는 메소드들이다.

class-transformer는
일반객체를 클래스객체로 전환(혹은 반대로)할 때 유용하게 쓰일 수 있는 라이브러리다.
- 일반객체 : 리터럴 객체
- 클래스객체 : 자체정의된 생성자, 속성, 메서드가있는 클래스의 인스턴스

뒤의 예시들은 User클래스가 있다는 가정하게 작성하겠다.
```typescript
class User {
  id: number;
  name: string;
  age: number;

  constructor(id: number, name: string, age: number) {
    this.id = id;
    this.name = name;
    this.age = age;
  }

  getIntroduceMyself() {
    return `my name is ${this.name} and i'm ${this.age}`;
  }
}
```

## 1.1.plainToClass
plainToClass란,   
일반자바스크립트 객체를 특정 클래스의 인스턴스로 변환해주는 메소드이다.

먼저 일반객체로 선언한 아래의 경우 출력은?
```typescript
const obj = {
  id: 1,
  name: 'jayden',
  age: 14,
};

console.log(obj);
console.log(obj instanceof User);
```
->   
![alt text](image-3.png)   
object타입의 일반객체로만 나오게된다.

그러나 일반객체를 클래스의 인스턴스로 변환해주는 아래의 코드를 수행한다면?
```typescript
const classObj = plainToClass(User, obj);

console.log(classObj);
console.log(classObj instanceof User);
console.log(classObj.getIntroduceMyself());
```
->    
![alt text](image-4.png)

## 1.2.classToPlain
classToPlain은,   
클래스의 인스턴스인 객체를 다시 자바스크립트 일반객체로 변환해주는 메서드이다.

위에 만들어준 classObj를 다시 아래코드 처리한다면?
```typescript
const classObjToPlain = classToPlain(classObj);
console.log(classObjToPlain);
console.log(classObjToPlain instanceof User);
```
->   
![alt text](image-5.png)   
결과는 최초 자바스크립트 일반객체였던 결과와 같다.

이 외에 아래의 메서드들이 존재한다.
- 디폴트값을 설정한 인스턴스를 참조하여 plainToClass를 할 수 있는 plainToClassFromExist(첫 번째 파라미터는 Class가 아닌 디폴트속성이 할당된 인스턴스)
- 인스턴스를 또 다른 클래스인스턴스로 만들어주는(깊은복사처리) classToClass
---
# 2.원인파악, 왜 Partial\<Entity>타입의 파라미터로 전달할 때 오류가 발생했나?
```typescript
async insert(data: Partial<T>, ...){...}
```
우선, 레포지토리의 메서드라 T는 엔티티타입이다.   

엔티티타입(T라 칭하겠다)을 PickType하는 InsertDataDto를 단일로 호출했을때,
```typescript
class insertData = plainToClass(InsertDataDto, data)
repository.insert(insertData)
```
문제가 되지않지만, 아래의 경우에는 문제가 발생한다.
```typescript
class insertDataList = plainToClass(InsertDataDto, dataList)
repository.insert(insertDataList)
```
```
TS2559: Type InsertDataDto[]  has no properties in common with type  Partial<T> 
```
InsnertDtoData[]는 T엔티티와 공통속성이 없다는 뜻   
InsnertDtoData만 전달했을때는 공통속성이 있다.   
그러나 InsnertDtoData[]를 전달했을때는 공통속성이 없다.

# 3.해결
knex는 일반 자바스크립트객체인 Record\<string,any>[] 또는 InsertDataDto[]를 기대하고있다.

따라서 아래의 두 방법이 있다.
## 3.1. insert의 파라미터타입을 Partial\<T> -> Partial\<T>[]로 변경
따라서 이 파라미터타입을 받는 batchInsert메서드를 ChunkSize로 지정하여 처리하는것이 좋다.

## 3.2. insertDataList를 instanceToPlain처리하여 insert로 전달
굳이굳이 Partial\<T>타입에 맞춰 넘기려고한다면 dataList를 instanceToPlain처리를 하면된다.   
왜냐하면 instanceToPlain은 클래스인스턴스를 자바스크립트 일반객체로 변환하기때문에   
Record<string,any>타입이 되므로 전달에 문제가없다.


# 4.결론
3.1의 해결책인 Partial\<T>[] 파라미터타입을 사용하는 batchInsert메서드로 처리를 해주었다.


# 추가적으로..
## Record<string,any>는 무슨용도일까?
Typescript를 사용하다보면 접하는 Record<string,any>.   
결론만 말하자면 자바스크립트의 객체를 좀 더 명확히 사용하기 위함이다.   
아래처럼, obj1는 value의 타입을 지정할수없지만   
obj2는 value타입을 지정할 수 있다.
```typescript
const obj1 = {
  key1:14,
  key2:"jayden"
}

const obj2:Record<string,number> = {
  key1:14,
  key2:2
}
```