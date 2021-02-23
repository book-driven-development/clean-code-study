# Chapter2 . 의미 있는 이름 

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Chapter2 . 의미 있는 이름](#chapter2-의미-있는-이름)
  - [의도를 분명히 밝혀라](#의도를-분명히-밝혀라)
  - [그릇된 정보를 피하라](#그릇된-정보를-피하라)
  - [의미 있게 구분하라](#의미-있게-구분하라)
  - [발음하기 쉬운 이름을 사용하라](#발음하기-쉬운-이름을-사용하라)
  - [검색하기 쉬운 이름을 사용하라](#검색하기-쉬운-이름을-사용하라)
  - [인코딩을 피하라](#인코딩을-피하라)
  - [자신의 기억력을 자랑하지 마라](#자신의-기억력을-자랑하지-마라)
  - [클래스 이름](#클래스-이름)
  - [메서드 이름](#메서드-이름)
  - [기발한 이름은 피하라](#기발한-이름은-피하라)
  - [한 개념에 한 단어를 사용하라](#한-개념에-한-단어를-사용하라)
  - [말장난을 하지 마라](#말장난을-하지-마라)
  - [해법 영역에서 가져온 이름을 사용하라](#해법-영역에서-가져온-이름을-사용하라)
  - [문제 영역에서 가져온 이름을 사용하라](#문제-영역에서-가져온-이름을-사용하라)
  - [의미 있는 맥락을 추가하라](#의미-있는-맥락을-추가하라)
  - [불필요한 맥락을 없애라](#불필요한-맥락을-없애라)
- [요약](#요약)
- [토론](#토론)

<!-- /code_chunk_output -->

## 의도를 분명히 밝혀라
- 좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 더 많다.
- 변수나 함수 그리고 클래스 이름은 다음과 같은 굵직한 질문에 모두 답해야 한다. 따로 주석이 필요하다면 의도를 분명히 드러내지 못했다는 말이다.
    - 변수(혹은 함수나 클래스)의 존재 이유는?
    - 수행 기능은?
    - 사용 방법은?
    
- Variable Example    
    ```
    // Variable Example - case 1
    int d; // 경과 시간(단위: 날짜)
    
    // Variable Example - case 2
    int elapsedTimeInDays;
    int daysSinceCreation;
    int daysSinceModification;
    int fileAgeInDays;
    ```
        
- Method Example        
    ```
    // Method Exaple - case 1
    public List<int[]> getThem(){
        List<int[]> list1 = new ArrayList<>();
        for (int [] x : theList) 
            if (x[0] == 4)
                list1.add(x);
        return list1;
    }
    ```     
- 위 코드 샘플엔 아래와 같은 정보가 드러나지 않는다.
    - theList에 무엇이 들어있는가?
    - theList에서 0번째 값이 어째서 중요한가?
    - 값 4는 무슨 의미인가?
    - 함수가 반환하는 리스트 list1을 어떻게 사용하는가?   

    ```
    // Method Exaple - case 2 (각 개념에 네이밍)
    public List<int[]> getFlaggedCells(){
        List<int[]> flaggedCells = new ArrayList<>();
        for (int [] cell : gameBoard) 
            if (cell[STATUS_VALUE] == FLAGGED)
                flaggedCells.add(cell);
        return flaggedCells;
    }
    
    // Method Exaple - case 3 (int 배열을 클래스화)
    public List<int[]> getFlaggedCells(){
        List<Cell> flaggedCells = new ArrayList<Cell>();
        for (Cell cell : gameBoard) 
            if (cell.isFlagged())
                flaggedCells.add(cell);
        return flaggedCells;
    }

    ```
- 단순히 이름만 고쳤는데도 함수가 하는 일을 이해가 쉬어졌다.

## 그릇된 정보를 피하라
- 여러 의미로 쓰일 수 있는 단어는 변수 이름으로 적합하지 않다.
    ```
    hp - 유닉스 플랫폼이나 유닉스 변종을 가리킴
    hp - 직각 삼각형의 빗변을 의미함
    ```
- 여러 객체를 묶을 때, 실제 List가 아니라면 xxxList라 명명하지 않는다. (나중에 살펴 보겠지만, 실제 컨테이너가 List인 경우라도 컨테이너 유형을 이름에 넣지 않는 편이 바람직하다.)
- 서로 흡사한 이름을 사용하지 않도록 주의한다.
    ```
    - XYZControllerForEfiicientHandlingOfStrings
    - XYZControllerForEfiicientStorageOfStrings
    ```
- 유사한 개념은 유사한 표기법을 사용한다. 일관성이 떨어지는 표기법은 그릇된 정보다.
    ```
    // 이름으로 그릇된 정보를 제공하는 끔찍한 예
    int a = l;
    if ( O == l)
        a = O1;
    else
        l = o1
    ```
    
## 의미 있게 구분하라
- 컴파일러나 인터프리터만 통과하려는 생각으로 코드를 구현하는 프로그래머는 스스로 문제를 일으킨다.
- 컴파일러를 통과할지라도 연속된 숫자를 덧붙이거나 불용어<sup>noise word</sup>를 추가하는 방식은 적절하지 못하다.
    ```
    // example 1
    public static void copyChars(char a1[], char a2[]){
        for (int i = 0; i < a1.length; i++){
            a2[i] = a1[i];
        }
    }
  
    public static void copyChars(char source[], char destination[]){
        for (int i = 0; i < source.length; i++){
            destination[i] = source[i];
        }
    }
    ```
- 불용어를 추가한 이름 역시 아무런 정보도 제공하지 못한다. (어떤 변수가 있다는 이유만으로 앞, 뒤에 불용어를 붙여서는 안된다.)
    - 표 변수 이름에 table
    - Name - NameString
    - Customer - CustomerObject
    - getActiveAccount() - getActiveAccounts() - getActiveAccountInfo()
    
## 발음하기 쉬운 이름을 사용하라
- 우리 두뇌에서 상당 부분은 단어라는 개념만 전적으로 처리한다. 말을 처리하려고 발달한 두뇌를 활용하지 않는다면 안타까운 손해다.
- 발음하기 어려운 이름은 토론하기도 어렵다.
    ```
    class DtaRcrd102{
        private Date genymdthms;
        private Date modymdhms;
        private final String pszqint = "102";
    }
    
    class Customer{
        private Date generationTimestamp;
        private Date modificationTimestamp;
        private final String recordId = "102";
    }
    ```
  
## 검색하기 쉬운 이름을 사용하라
- 문자 하나를 사용하는 이름과 상수는 텍스트 코드에서 쉽게 눈에 듸지 않는다.
    -  `int MAX_CLASSES_PER_STUDENT = 7` 와 같이 상수로 정의하면 grep으로 찾기가 쉽다.
    - 하지만 상수화 하지 않을 경우 상수 7을 검색하면 7이 들어가는 파일 이름이나 수식이 모두 검색된다. 검색은 되었지만, 7을 사용한 의도가 다른 경우도 있다.
    - 마찬가지로 e처럼 거의 모든 단어에 들어가는 짧은 이름은 검색하기가 어렵다. 

## 인코딩을 피하라
- 헝가리식 표기법
    - 이름 길이가 제한된 언어를 사용하던 옛날의 경우는 어쩔 수 없이 이규칙을 위반했다.
    - 당시는 컴파일러가 타입을 점검하지 않았으므로 프로그래머에게 타입을 기억할 단서가 필요 했기에 헝가리식 표기법을 굉장히 중요하게 여겼다.
    - 자바의 경우 객체는 강한타입<sup>strongly-typed</sup>이며, IDE는 코드를 컴파일하지 않고도 타입 오류를 감지할 정도로 발전했다.
    - 따라서 이제는 헝가리식 표기법이나 기타 인코딩 방식이 오히려 방해가 될 뿐이다.
- 맴버 변수 접두어
    - 이제는 맴버 변수에 m_이라는 접두어를 붙일 필요도 없다. 
    - 클래스와 함수는 접두어가 필요없을 정도로 작아야 마땅하다.
    - 또한 맴버 변수를 다른 색상으로 표시하거나 눈에 띄게 보여주는 IDE를 사용해야 마땅하다.
- 인터페이스 클래스와 구현 클래스
    - 인터페이스 클래스와 구현 클래스는 인코딩이 필요한 경우다.
    - 옛날 코드에서 많이 사용하는 접두어 I는 (잘해봤자) 주의를 흐트리고 (나쁘게는) 과도한 정보를 제공한다.
    - 인터페이스 클래스 이름과 구현 클래스 이름 중 하나를 인코딩 해야 한다면 구현 클래스 이름을 택하겠다.
        - ShapeFactory - (ShapeFactoryImp or CShapeFactory)  

## 자신의 기억력을 자랑하지 마라    
- 독자가 코드를 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면 그 변수 이름은 바람직하지 못하다.
    - 프로그래머가 자신만 알고 있는 변수이름을 사용 좋지 않다.
- 전문가 프로그래머는 __명료함이 최고__라는 사실을 이애한다.
- 전문가 프로그래머는 자신의 능력을 좋은 방향으로 사용해 남들이 이해하는 코드를 내놓는다.

## 클래스 이름
- 클래스 이름과 객체 이름은 명사나 명사구가 적합하다.
    - 좋은 예시 - Customer, WikiPage, Account, AddressParser
    - 나쁜 예시 - Manager, Processor, Data, Info 등, 동사 

## 메서드 이름
- 메서드 이름은 동사나 동사구가 적합하다.
    - postPayment, deletePage, save
    - 접근자, 변경자, 보건자는 javabean 표준<sup>4</sup>에 따라 값 앞에 get, set, is를 붙인다.
    
## 기발한 이름은 피하라
- 이름이 너무 기발하면 저자와 유머 감각이 비슷한 사람만, 농담을 기억하는 동안만, 이름을 기억한다.   
- 재미난 이름보다는 명료한 이름을 선택하라.   
- 특정 문화에서만 사용하는 농담은 피하는 편이 좋다.

## 한 개념에 한 단어를 사용하라
- 추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다.
    - 예를 들어 fetch, retrieve, get
- 메서드 이름은 독자적이고 일관적이어야 한다.

## 말장난을 하지 마라
- 한 단어를 두가지 목적으로 사용하지 마라.
    - add - 기존 값 두 개를 더함
    - add - 집합에 값 하나를 추가 (이미 add가 다른 의미로 사용 되었으므로 insert나 append를 사용)

## 해법 영역에서 가져온 이름을 사용하라 
- 기술 개념에는 기술 이름이 가장 적합한 선택이다.   

## 문제 영역에서 가져온 이름을 사용하라
- 적절한 '__프로그래머 용어__'가 없다면 문제 영역에서 이름을 가져온다.
   
## 의미 있는 맥락을 추가하라
- 스스로 의미가 분명하지 않을 경우 클래스, 함수, 이름 공간에 넣어 맥락을 부여한다.
- 모든 방법이 실패하면 마지막 수단으로 접두어를 붙인다.
    - firstName, lastName, street, houseNumber, city, state, zipCode
    - addrFirstName, addrLastName, addrStreet, addrHouseNumber, addrCity, addrState, addrZipCode
    ```
    private void printGuessStatistics(char candidate, int count) {
        String number;
        String verb;
        String pluralModifier;
        if (count == 0) {
            number = "no";
            verb = "are";
            pluralModifier = "s";
        } else if (count == 1) {
            number = "1";
            verb = "is";
            pluralModifier = "";
        } else {
            number = Integer.toString(count);
            verb = "are";
            pluralModifier = "s";
        }
        String guessMessage = String.format(
                "There %s %s %s%s", verb, number, candidate, pluralModifier
        );
        System.out.println(guessMessage);
    }
    ```
    ```
    public class GuessStatisticsMessage {
        private String number;
        private String verb;
        private String pluralModifier;
        public String make(char candidate, int count) {
            createPluralDependentMessageParts(count);
            return String.format(
                    "There %s %s %s%s",
                    verb, number, candidate, pluralModifier );
        }
        private void createPluralDependentMessageParts(int count) {
            if (count == 0) {
                thereAreNoLetters();
            } else if (count == 1) {
                thereIsOneLetter();
            } else {
                thereAreManyLetters(count);
            }
        }
        private void thereAreManyLetters(int count) {
            number = Integer.toString(count);
            verb = "are";
            pluralModifier = "s";
        }
        private void thereIsOneLetter() {
            number = "1";
            verb = "is";
            pluralModifier = "";
        }
        private void thereAreNoLetters() {
            number = "no";
            verb = "are";
            pluralModifier = "s";
        }
    }
    ```
## 불필요한 맥락을 없애라
- 의미가 분명한 경웨 한해서, __일반적으로는 짧은 이름이 긴 이름보다 좋다__.
- accountAddress와 customerAddress는 Address 클래스 인턴스로는 좋은 이름이나 클래스 이름으로는 적합하지 못하다.

# 요약



# 토론