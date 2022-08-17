# [책] 똑똑하게 코딩하는 법, 자바 코딩의 기술(현장에서 뽑은 70가지 예제로 배우는 코드 잘 짜는 법, 사이먼 하러, 요르그 레너드, 리누스 디에츠)


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

<hr>

## 3장 : 슬기롭게 주석 사용하기

<br>

- 지나치게 많은 주석은 자제하라. 자바 코드 규칙에서 정한 클래스 구조에 따를 경우 불필요한 이정표는 넣을 필요 없다. 필요한 주석은 코드만 보아서는 드러나지 않는 정보이다!

<br>

- 주석 처리된 코드는 과감하게 삭제! 어차피 다시 찾지 않는다.

<br>

- 주석을 상수로 대체하라! 2장의 매직넘버를 상수로 대체하는 것처럼 말이다! 주석을 상수나 변수, 필드, 메서드 이름으로 넣을 수 있다면 망설이지 말고 하세요!

<br>

- 주석을 유틸리티 메서드로 대체하라!

<br>

- 구현 결정을 설명하기. 사용 사례, 우려사항, 해법, 그리고 지불해야 할 트레이드 오프나 비용까지 명시하라!
   - In the context of [USE CASE] : 사용 사례의 맥락에서
   - facing [CONCERN] : 직면하는 우려사항과
   - we decided for [OPTION] : 우리가 선택한 해법으로
   - to achieve [QUALITY] : 얻게 되는 품질과
   - accepting [DOWNSIDE] : 받아들여야 하는 단점

<br>

- 예제 추가하기, 유효한 예제와 유효하지 않은 예제를 제공하라! 또는 단위 테스트를 추가해도 좋다!

<br>

- API를 를 개발하고 있다면 JavaDoc을 사용하라! JavaDoc은 패키지를 비롯해 코드에서 public인 요소를 설명하는 데 사용된다. 
- 모든 public 클래스나 인터페이스는 JavaDoc으로 설명해야 한다. 이것은 대부분의 자바 프로젝트에서 적용되는 규칙이다.
   - 먼저 짧고 간결한 요약으로 시작하라.
   - 요약을 클래스나 인터페이스가 보장하는 불변과 수직으로 분리하라.
   - 메서드 서명을 되풀이하지 마라.
   - 또한 예제는 항상 도움이 된다. 

<br>

- 생성자를 JavaDoc으로 구조화 하라! 생성자에는 메서드처럼 이름이 없다보니 JavaDoc이 그만큼 매우 중요하다!

<hr>

## 4
