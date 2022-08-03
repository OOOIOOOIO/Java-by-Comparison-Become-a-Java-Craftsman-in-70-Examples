# [책] 똑똑하게 코딩하는 법, 자바 코딩의 기술(현장에서 뽑은 70가지 예제로 배우는 코드 잘 짜는 법, 사이먼 하러, 요르그 레너드, 리누스 디에츠)
##(Java by Comparison Become a Java Craftsman in 70 Examples, Simon Harrer, Jorg Lenhard, Linus Dietz)

## 1장 : 우선 정리부터
- 불 반환값과 불 원시 타입(true, false)를 병시적으로 비교하는 건 초보자 코드에서 종종 발견되는 안티 패턴이다.

```java
if(true == false){...} // 안티 패턴!
```

<br>

- 코드에서 긍정 표현이 부정 표현식보다 더 낫다. 누구나 부정이 없는 표현을 좋아한다는 사실을 기억해두자

```java
if(micro.isNotHuman()){...} // 안티!

if(!micro.isHunam()){...} // 권장!
```

<br>

- 훌륭한 그룹핑이란 조건의 의미에 따라 좌우되니 주제나 추상화 정도에 따라 그룹핑해라! 메소드로 뺄 수 있는 건 빼고 최대한 가독성 좋게! 이해도 있게 작성해라!

<br>

- 인수를 검증할 때 순서가 중요한데, 반드시 null부터 먼저 확인한 후 도메인에 따라 "유효하지 않은 값"을 검사해라! 파라미터 순서에 따라 검증해라!

<br>

- 코드는 대칭성 있게 작성하라! 서로 다른 관심사는 분리하고 같은 관심사끼리 묶어라!

<hr>

## 2장 : 코드 스타일 레벨 업

- 매직 넘버(그냥 숫자)를 상수((static) final)로 대체하라! 각 숫자마다 유의미하고 이해하기 쉬운 이름을 달아라!! 더 나아가 정수, 상수 대신 열거형(enum)을 선택하라!

<br>

- 변수명 명명 규칙 for(타입 단수명 : 복수명)!

<br>

- 순회할 때 컬렉션을 수정하지 마라! 어차피 ConcurrentModificationException이 발생한다! 단일 쓰레드 애플리케이션에서 동시(concurrency)실행이라니 큰일난다!
```java
// 큰일난다!!

class Inventory{

  private List<Supply> supplies = new ArrayList<>();
  
  void disposeContaminatedSupplies(){
    for(Supply supply : supplies){
      if(supply.isContaminated()){
        supplies.remove(supply);
      }
    }
  }
}

```

<br>

```java
// 좋다!

class Inventory{

  private List<Supply> supplies = new ArrayList<>();
  
  void disposeContaminatedSupplies(){
    Iterator<Supply> iterator = supplies.iterator();
    
    while(iterator.hasNext()){
      if(iterator.next().isContaminated()){
        iterator.remove();
      }
    }
  }
}

```

<br>

- 순회하면서 계산 집약적 연산하지 마라! 최대한 할 수 있는 계산은 미리 끝내고 순회하라!

- 문자열이 많다면 이어붙이기 대신 서식화를 하라! String.format을 사용하라!! String 레이아웃(String을 어떻게 출력할지)와 데이터(무엇을 출력할지)를 분리하는 것이다!
```java
String entry = author.toUpperCase() + "[" + .... + "]" + "(TODOTODO " + .... --> 요딴식 X

String entry = String.format("%s : ["%d : %d] %s%n", author, me, my, mo, ..) --> 레이아웃과 데이터를 분리하라!

```

<br>

- JAVA API를 사용하라! Object, Collections의 유틸리티 클래스인 Objects, Collections 처럼 유용한 클래스가 많다!







