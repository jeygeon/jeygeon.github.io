---
title: "[ArrayList] ArrayList 정리"
toc: true
toc_sticky: true
date: 2025-01-05
categories: data-structure
comments: true
---

## ArrayList란?
`ArrayList`란 크기가 고정되어 있는 `Array`를 보안하기 위해서 설게된 컬렉션이다. 내부적으로는 `Object[]`을 사용하지만 클래스 내부적으로 유연하게 크기가 동적으로 증가하거나 감소할 수 있다.

내부적으로는 배열을 사용하기 때문에 인덱스를 통해 빠르게 내가 원하는 요소에 접근할 수 있다.

```java
public class ArrayListExample {
  public static void main(String[] args) {
    ArrayList<String> list = new ArrayList<>();

    // 요소 추가
    list.add("Apple");
    list.add("Banana");
    list.add("Cherry");

    // 요소 접근
    System.out.println(list.get(0)); // 출력: Apple

    // 요소 제거
    list.remove("Banana");
    System.out.println(list); // 출력: [Apple, Cherry]
  }
}
```
## ArrayList의 생성
ArrayList는 new 연산자로 생성했을 경우 기본적인 크기는 10이다.
```java
ArrayList<String> list = new ArrayList<>(); // 크기가 10인 리스트 생성
```

하지만 초기에 크기를 지정할 수도 있다.
```java
ArrayList<String> list = new ArrayList<>(20); // 크기가 20인 리스트 생성
```

여기서 말하는 크기가 10, 20은 해당 리스트 내부 배열이 메모리 상에서 차지하고 있는 크기이지 리스트의 실제 사이즈를 말하는 것은 아니다.
```java
ArrayList<String> list1 = new ArrayList<>(); // 크기가 10인 리스트 생성
System.out.println(list1.size()) // 출력: 0

ArrayList<String> list2 = new ArrayList<>(20); // 크기가 20인 리스트 생성
System.out.println(list2.size()) // 출력: 0
```

## ArrayList의 요소 추가, 제거
### 추가
위에서도 말했지만 `ArrayList`는 동적으로 `add()`, `remove()` 등의 메소드를 통해서 유연한 크기 조절이 가능하다.

하지만 한가지 알아두어야 할 내용은 ArrayList는 내부적으로 **배열을 사용**하고 있다는 점이다.

리스트를 생성하여 요소를 추가하는 한가지 상황을 가정해 보겠다.

```java
ArrayList<Integer> list = new ArrayList<>();

for (int i=0; i<11; i++>) {
  list.add(i);
}
```

위의 코드는 단순 리스트를 만들고 만든 리스트에 0부터 11까지의 숫자를 추가하는 코드이다.

위에서 계속 말했지만 리스트를 초기 생성했을 때의 크기는 10이다. 이 때 11을 추가할 떄 발생되는 과정은 아래와 같다.

1. **기존 사이즈의 1.5배인 새로운 배열을 메모리에서 할당 받음(초기 크기 `10` → 새로운 크기 `15`)**
2. **기존 배열에서 새로운 배열로 요소 copy**
3. **새롭게 추가된 남은 공간에 새로은 요소 추가**

이렇듯 정해진 크기를 넘어서 새로운 요소가 추가될 때는 새로운 배열을 생성하여 메모리에서 할당받고 기존 배열을 복사하는 이런 과정이 발생되게 된다.

만약 리스트에 요소 추가가 빈번하기 이루어지고 얼마나 추가가 될지 예상할 수 있는 상황이라면 아래 코드가 좀 더 성능상 좋을 것 이다.
```java
// 메모리 내부 배열 용량 : 30
ArrayList<Integer> list = new ArrayList<>(30);

// 0 ~ 25 까지 리스트에 추가 (남은 내부 배열 용량 : 5)
for (int i=0; i<25; i++) {
  list.add(i);
}

// 메모리 내부 배열 용량 최적화 (남은 내부 배열 용량 : 0)
list.trimToSize();
```

### 제거
`ArrayList`에서 요소를 제거할 때는, 내부적으로 배열 요소의 이동이 발생한다.

이 과정은 특정 요소를 삭제한 후 빈 공간을 채우기 위해 나머지 요소들이 앞으로 이동하는 현상이다.

```java
ArrayList<Integer> list = new ArrayList<>(List.of(1, 2, 3, 4, 5));

// 2번째 요소(3) 제거
list.remove(2);

// 결과: [1, 2, 4, 5]
```