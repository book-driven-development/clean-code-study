# Chapter 3 - 함수

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Chapter 3 - 함수](#chapter-3-함수)
  - [작게 만들어라!](#작게-만들어라)
    - [블록과 들여쓰기](#블록과-들여쓰기)
  - [한 가지만 해라!](#한-가지만-해라)
  - [함수 당 추상화 수준은 하나로!](#함수-당-추상화-수준은-하나로)
    - [위에서 아래로 코드 읽기: 내려가기 규칙](#위에서-아래로-코드-읽기-내려가기-규칙)
  - [Switch case](#switch-case)
  - [서술적인 이름을 사용하라!](#서술적인-이름을-사용하라)
  - [함수 인수](#함수-인수)
    - [많이 쓰는 단항 형식](#많이-쓰는-단항-형식)
    - [플래그 인수](#플래그-인수)
    - [인수 목록](#인수-목록)
    - [동사와 키워드](#동사와-키워드)
  - [부수 효과를 일으키지 마라!](#부수-효과를-일으키지-마라)
    - [출력 인수](#출력-인수)
  - [명령과 조회를 분리하라!](#명령과-조회를-분리하라)
    - [오류 코드보다 예외를 사용하라!](#오류-코드보다-예외를-사용하라)
    - [오류처리도 한 가지 작업이다](#오류처리도-한-가지-작업이다)
    - [의존성 자석](#의존성-자석)
  - [반복하지 마라!](#반복하지-마라)
  - [구조적 프로그래밍](#구조적-프로그래밍)
  - [함수를 어떻게 짜죠?](#함수를-어떻게-짜죠)
- [요약](#요약)
- [토론](#토론)

<!-- /code_chunk_output -->


## 작게 만들어라!
- 함수는 첫번째로 작게, 두번째로 더 작게 만들어라.
- 함수는 2~5줄 내외로 작성하는것이 좋다  

### 블록과 들여쓰기
- if문 else문 while문 들에 들어가는 블록은 한 줄이여야한다.
- 중첩구조를 가질만큼 함수가 크면 안된다.
- 들여쓰기는 1단으로 한다
```
// 이런식으로 ?
public void main() {
  if (FLAG) {
      somethingA();
  } 
  else if(FLAG) {
      somethingB();
  }
  else {
      somethingC();
  }
}
```  


## 한 가지만 해라!
- 함수는 한 가지를 해야 한다. 그 한 가지를 잘 해야 한다. 그 한 가지만을 해야 한다.
- 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 하는 것이다
- 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈이다.


## 함수 당 추상화 수준은 하나로!
- 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.
- 특정 표현이 개념인지 세부 구현체인지 구분하기 어려워 진다.
```
getHTML();
String pagePathName = PathParser.render(pagePath);
Object.append("\n");
```
위의 세 함수는 모두 추상화의 레벨이 다르다(내려갈수록 저수준이다)

### 위에서 아래로 코드 읽기: 내려가기 규칙
- 코드는 위에서 아래로 이야기처럼 읽혀야 좋다.
- 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수를 써야한다.


## Switch case
Switch 문은 본질적으로 N 가지를 처리하게 되므로 '한 가지' 작업만 하는 함수의 규칙을 명백히 위반한다.
다형성을 이용하여 switch 문을 저차원 클래스에 숨기고 들어내지 않는다.

```
public Money calculatePay(Employee e) throws InvalidEmployeeType {
	switch (e.type) { 
		case COMMISSIONED:
			return calculateCommissionedPay(e); 
		case HOURLY:
			return calculateHourlyPay(e); 
		case SALARIED:
			return calculateSalariedPay(e); 
		default:
			throw new InvalidEmployeeType(e.type); 
	}
}
```
위 함수의 문제점

1. 함수가 길다
2. 한 가지 작업만 수행하지 않는다
3. SRP 를 위반한다 (코드를 변경할 이유가 여럿이기 때문이다)
4. OCP 를 위반한다 (새 직원 유형을 추가할 때마다 코드를 변경해야 한다)
5. 위 함수와 구조가 동일한 함수가 무한정 존재 할 수 있다
    - `isPayday(Employee e, Date date);`  `deliverPay(Employee e, Money pay);`등등
    - 새로운 함수가 생길 때 마다 Switch문을 계속 써야한다

[SOLID(객체지향 프로그래밍 설계를 위한 5가지 원칙)](https://github.com/beomjo/SOLID)

```
public abstract class Employee {
	public abstract boolean isPayday();
	public abstract Money calculatePay();
	public abstract void deliverPay(Money pay);
}
-----------------
public interface EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType; 
}
-----------------
public class EmployeeFactoryImpl implements EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
		switch (r.type) {
			case COMMISSIONED:
				return new CommissionedEmployee(r) ;
			case HOURLY:
				return new HourlyEmployee(r);
			case SALARIED:
				return new SalariedEmploye(r);
			default:
				throw new InvalidEmployeeType(r.type);
		} 
	}
}
```
Switch문을 팩토리 내부로 숨겨서 최소한으로 사용한다. 로직이 퍼지지 않게 하며 일관성을 유지시키는것이 좋다
Switch문을 사용하는 곳에서 구현하는 것이 아닌 구현된 팩토리 메서드를 사용하는것이 좋다


## 서술적인 이름을 사용하라!
- 이름이 길어도 괜찮다. 길고 서술적인 이름이 짧고 어려운 이름보다 좋다. 
    - 길고 서술적인 이름이 길고 서술적인 주석보다 좋다.
- 이름을 붙일 때는 일관성이 있어야 한다.


## 함수 인수
- 함수에서 이상적인 인수의 개수는 0개다. 
- 인자가 여러개인 함수는 좋지않다.
- 갖가지 인수 조합으로 함수를 검증한다고 하면 엄청나게 많은 조합의 가지가 생긴다
    - 테스트 관점에서 볼떄 인수가 많은 것은 좋지 않다.

### 많이 쓰는 단항 형식
- 인수에 질문을 던지는 경우
    - `boolean fileExists(“MyFile”);`
- 인수를 뭔가로 변환해 결과를 변환하는 경우
    - `InputStream fileOpen(“MyFile”);`
- 이벤트 함수일 경우 
    - 이 경우에는 이벤트라는 사실이 코드에 명확하게 드러나야 한다.
    - `passwordAttemptFailedNtimes(int attempts);`
위의 세 가지가 아니라면 단항 함수는 가급적 피하는것이 좋다.

### 플래그 인수
플래그 인수는 쓰지마라. 
bool 값을 넘기는 것 자체가 함수가 한꺼번에 여러가지 일을 처리한다고 가정하는 것 이다.

### 인수 목록
```
String.format("%s worked %.2f hours.", name, hours);

public String format(String format, Object... args)
```
가변 인수를 모두 동등하게 취급하면 List형 인수 하나로 취급할 수 있다.(이로인해 사실상 이항 함수가 된다)
다항 함수보다는 차라리 가변 인수를 쓰는것이 낫다

### 동사와 키워드
- 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야한다.
    - `writeField(name);`
- 함수이름에 키워드(인수 이름)을 추가하면 인수 순서를 기억할 필요가 없어진다.
    - `assertExpectedEqualsActual(expected, actual);`


## 부수 효과를 일으키지 마라!
함수내에서 클래스 전역변수를 바꾸거나 
함수 외에 코드를 변경하는 일은 부수효과를 일으킨다(Side Effect)
```
public class UserValidator {
	private Cryptographer cryptographer;
	public boolean checkPassword(String userName, String password) { 
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) {
			String codedPhrase = user.getPhraseEncodedByPassword(); 
			String phrase = cryptographer.decrypt(codedPhrase, password); 
			if ("Valid Password".equals(phrase)) {
				Session.initialize();
				return true; 
			}
		}
		return false; 
	}
}
```
무해해 보이는 코드지만 `Session.initialize()` 는 함수명과 상이한 비즈니스 로직이다
`checkPasswordAndInitializeSession` 이라는 이름이 훨씬 좋다. 

### 출력 인수
`public void appendFooter(StringBuffer report)`
일반적으로 출력 인수는 피해야 한다. 함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식을 취한다.
`report.appendFooter()`


## 명령과 조회를 분리하라!
함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야 한다.
```
public boolean set(String attribute, String value);

if(set("username", "unclebob") {

}
```
set 이라는 함수가 굉장히 모호하다. `setAndCheckIfExists` 라고 하는게 훨씬 좋지만, 명령과 조회를 분리해 애초에 혼란이 일어나지 않도록 한다.

```
if (attributeExists("username")) {
  setAttribute("username", "unclebob");
}
```

### 오류 코드보다 예외를 사용하라!
명령 함수에서 오류 코드를 반환하는 방식은 명령/조회 분리 규칙을 미묘하게 위반한다.
오류 코드를 반환하는 것은 유지보수에 치명적이고 비즈니스 로직을 한 눈에 알기 어렵다.
또한 오류 코드를 만났을 경우 바로 해결해야만 하는 문제가 있다.
```
// Bad
if (deletePage(page) == E_OK) {
	if (registry.deleteReference(page.name) == E_OK) {
		if (configKeys.deleteKey(page.name.makeKey()) == E_OK) {
			logger.log("page deleted");
		} else {
			logger.log("configKey not deleted");
		}
	} else {
		logger.log("deleteReference from registry failed"); 
	} 
} else {
	logger.log("delete failed"); return E_ERROR;
}

// Good
public void delete(Page page) {
	try {
		deletePageAndAllReferences(page);
  	} catch (Exception e) {
  		logError(e);
  	}
}

private void deletePageAndAllReferences(Page page) throws Exception { 
	deletePage(page);
	registry.deleteReference(page.name); 
	configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) { 
	logger.log(e.getMessage());
}
```
try/catch 블록을 별도 함수로 뽑아내 오류처리와 비즈니스 로직을 분리시킨다.

### 오류처리도 한 가지 작업이다
오류 처리도 한 가지 작업이다.
함수가 '한 가지' 작업만 하듯 오류 처리도 '한 가지' 작업을 해야한다.

### 의존성 자석
```
public enum Error { 
	OK,
	INVALID,
	NO_SUCH,
	LOCKED,
	OUT_OF_RESOURCES, 	
	WAITING_FOR_EVENT;
}
```
오류를 처리하는 곳곳에서 오류코드를 사용하게 되면 enum class를 쓰게 되는데 이런 클래스는 의존성 자석이 된다.
새 오류코드를 추가하거나 변경할 때 코스트가 많이 필요하다.(재컴파일 및 재배치 등) 그러므로 예외를 사용하는 것이 더 안전하다.


## 반복하지 마라!
중복은 악의 근원이다.


## 구조적 프로그래밍
- 모든 함수와 함수 내 모든 블록에 입구와 출구가 하나여야 된다 - 다익스트라
- 함수는 `return` 문이 하나여야 된다
- 루프 안에서 `break` 나 `continue` 를 사용해선 안된다
- `goto` 는 절대로 사용하면 안된다


## 함수를 어떻게 짜죠?
1. 초안은 서투르고 어수선한 코드를 작성한다
2. 서투루고 어수선한 코드르 빠짐없이 테스트하는 단위 테스트 케이스를 만든다
3. 코드를 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거한다.
4. 위 과정중 항상 단위테스트를 통과해야한다




# 요약



# 토론
1. 한 가지만 해라 라는 것이 보는 범위에 따라서 다른것이 아닐까? 넓게보면 한 가지만 하는것이지만 그 내부에서는 여러개의 함수를 호출 할 수도 있지않은가?
    ```
    public void doProcess(){
        doRead();
        doPreProcess();
        doWrite();
    }
    ```
2. 한 가지만 하는 함수를 작성할때 플래그 인수를 넘기는 경우가 생기는데 어떻게 보는것이 좋을까?
    ```
    public void doSomethingBySearchType(int searchType){
        if(searchType == SEARCH_TYPE_MONTHLY)
            ...
        else if(searchType == SEARCH_TYPE_WEEKLY)
            ...
        else if(searchType == SEARCH_TYPE_DAILLY)
            ...
    }
    ```
3. 반복문에서 `break`나 `continue`를 쓰는것이 성능적을 좋은 경우가 있는데 반드시 사용을 지양해야할까?