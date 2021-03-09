### 이번 프로젝트에서 집중해서 공부한 내용

- Optional Chaining
- 변수와 함수의 naming
- 열거형의  사용
- 자연스러운 코드의 작성법 (모든 것을 장황하게 설명하지 않기)

<br/>

---

<br/>

## Step 1. 가위바위보 클래스 설계

> [PR #26 Step1 가위바위보 클래스 설계](https://github.com/yagom-academy/ios-rock-paper-scissors/pull/26)

- 가위바위보 클래스 설계

  - 사용자 입력에 따라 게임이 계속해서 진행되도록 하는 `startGame()` 구현
  - switch문을 이용해 GameResult를 반환해주는 `returnResultOfGame()` 구현
  - 사용자 입력을 받고 그 입력이 유효한지 확인해주는 `getUserInput()`구현
  - enum을 활용하여
    - 게임 결과를 저장하는 `GameResult`구현
    - 사용자와 컴퓨터의 손 모양을 저장하는 `Hand`구현
    - 에러를 처리하기 위해 필요한 `GameError` Error 프로토콜 구현

  

### 피드백을 통해 개선한 부분

- Optional Chaining을 사용하여 guard let구문을 여러번 사용하던 것을 압축함
- 변수명과 함수명을 더 자연스럽게 바꿈
- 제한된 선택지, 제한 선택지의 data만 들어있어야 하는 값의 모음에 enum을 활용함 
- enum의 rawValue를 활용하여 불필요한 switch문을 줄임
- nested enum을 사용하여 엉뚱하게 enum이 사용되는 일을 방지함

<br/>

---

<br/>

## STep 2. 묵찌빠 클래스 설계

> [PR #38 묵찌빠 클래스 설계](https://github.com/yagom-academy/ios-rock-paper-scissors/pull/38)

- 묵찌빠 클래스 설계
  - 가위바위보 게임이 종료되면 해당 결과를 인자로 받아 묵찌빠 게임 실행
  - 이전 턴에 이긴 사람이 공격권을 가지며 이번턴에 상대방과 같은 손모양을 내면 공격자의 승리, 그렇지 않은 경우는 가위바위보 룰에 따랐을 때의 승자에게 공격권이 주어짐



### 피드백을 통해 개선한 부분

- 상속을 통해 가위바위보 클래스의 일부 기능을 사용하려던 생각을 버림

  상속을 통해 받아온 메서드들을 오버라이드 하여 수정해야되는 사항들이 너무 많았고 이에 피드백을 수용하여 가위바위보 클래스와 묵찌빠 클래스를 별개의 클래스로 분리함

- Step 1과 마찬가지로 어색한 변수명들을 대거 수정함

  - `userWin`, `userLose` -> `win`, `lose`

    computerWin이나 computerLose라는 변수가 따로 존재했으면 고려해볼법한 네이밍이었지만 그렇지 않았기에 간결하게 수정

- 프로퍼티와 메서드 중 외부에서 접근해선 안되는 것들에 대해서는 private 접근 제어 지정자를 설정해줌

  - 이 과정에서 startGame() 메서드를 private으로 설정할 것인가 internal로 설정할 것인가에 대한 토론이 오갔고 각각의 장단점에 대해서 이야기 해봤음
  - startGame() 메서드를 private으로 설정하였을 때의 장점 및 게임 시작 방법
    - 우선 게임을 시작하기 위해선 init(생성자)에서 바로 startGame 메서드를 호출하는 방법이 있었다. 이렇게 해주면 startGame을 임의로 다시 호출하는 일을 방지할 수 있었다. instance를 생성하지 않고도 바로 게임을 시작할 수 있었기 때문에 코드가 더 간결해지기도 하였다.
    - 단점: init을 통해 자동으로 게임이 시작되므로 다른 사람이 내 코드를 유지보수 할 때 게임을 시작할 생각없이 미리 instance만 생성해두려는 의도로 코드를 짜게되면 생성자에 의해 startGame함수가 호출되어 문제가 발생할 우려가 있었다.

    - 결론 : RockPaperScissors 클래스는 instance를 만들어서 startGame을 private이 아닌 internal로 접근제어 지정자를 설정하여 startGame을 바로 호출하도록 했다. instance 생성만으로 게임이 시작되면 곤란한 부분이 많다는 판단하에서였다. 하지만 MukChiBa를 호출하는 부분에서는 instance 생성 시에 바로 MukChiBa의 메서드인 startGame()이 실행되도록 해주었다. 이 이유는 게임의 구조상 가위바위보 게임 실행 후 바로 묵찌빠 게임으로 넘어가게 되는데 이 과정에서 startGame을 다시 호출할 일이 없기 때문이다. instance의 이름도 필요하지 않았기에 와일드카드 ( "_" )를 사용하였다.

<br/>

> startGame의 접근제어 지정자를 private으로 설정한 묵찌빠 클래스의 인스턴스가 생성됨과 동시에 게임이 시작되는 코드
 

<img width="951" alt="privateStartGame" src="https://user-images.githubusercontent.com/67148595/110485717-4837e200-812f-11eb-9fbb-15f7c95734c9.png">

<br/>

> startGame의 접근제어 지정자를 internal로 설정한 가위바위보 클래스의 인스턴스를 만들고 startGame을 호출하는 코드


<img width="947" alt="internalStartGame" src="https://user-images.githubusercontent.com/67148595/110485729-4a9a3c00-812f-11eb-900d-ce6245d01726.png">

<br/>

---

<br/>

## 이번 프로젝트를 통해 발전한 부분

### 변수명 함수명을 자연스럽게 짓게 되었다.

Swift 공식문서에서 설명한 

> "명료함"이 "간결함"보다 우선이 되어야한다.

라는 말은 꼭 모든 변수명과 함수명을 장황하게 쓰라는 말이 아니었다.

```swift
class RockPaperScissors {
  enum HandOfUserAndComputer {
    case scissor
    case rock
    case paper
  }
// ...
  private func rockPaperScissorsGameResult() {
    //...
  }
}
```

기존에는 위의 코드처럼 최대한 구체적으로 naming을 하려는 시도를 했었는데 이것이 오히려 코드의 가독성을 해칠 수 있다는 점을 알게되었다. `RockPaperScissors`라는 class 내부에 있는 함수이기 때문에 굳이 `rockPaperScissorsGameResult()`와 같이 적지 않고 `gameResult()`정도로만 적어도 문맥상 이해하는데 전혀 지장이 없다는 점을 알게되었다. 

또한 함수명을 항상 동사로만 짓던 습관을 버리게 되었는데 `getUserInput()`와 같이 짓던 습관을 `userInput()`처럼 명사로 지으면서 더 간결한 코드를 작성할 수 있게 되었다.



### enum의 활용능력이 한층 발전했다

기존에 알고있던 enum이 채용할 수 있는 프로토콜은 Error 뿐이었다. 하지만 이번 프로젝트를 통해 CaseIterable, CustomStringConvertible, Comparable 등의 다양한 프로토콜들을 사용해보았고 enum의 활용능력이 발전했다.



### Optional을 해제하는 여러 방법들에 익숙해졌다. 

Optional을 해제하는 과정이 필요하단 것은 알고 있었지만 이를 실제로 많이 써본적은 없었다. 하지만 이번 프로젝트를 통해 단순히 Optional Binding을 통한 해결 외에도 map함수를 통한 해결, Optional Chaining을 통한 함수의 빠른 종료 등 다양한 상황에서 Optional을 다루는 능력이 성장했다. 

### Force Unwrapping(!)에 대해 조금 더 생각해보게 되었다.

기존에는 강제 언래핑을 무조건 지양하자는 주의였다. 지금도 이 생각에는 변함이 없지만 어쩔 수 없이 강제 언래핑을 진행해야되는 상황이나 명백하게 강제 언래핑이 가능한 상황에서 nil coalescing의 사용 대신 강제 언래핑을 해주는 것이 더 좋을 수 있다는 점에 대해 알게되었다. 만약 코드에 nil coalescing이 사용되었다면 다른 프로그래머는 이를 보고 

>  "정말 nil값이 나올 가능성이 있기 때문에 nil coalescing를 통해 default 값을 지정해주었구나"

와 같이 생각할 가능성에 대해 고려해봤을 때 강제 언래핑을 사용하여 확실하게 nil값이 나올 수 없다는 확신을 보여주는 편이 유지보수 측면에서 유리할 수 있겠다 싶었다.

이 사례로는 CaseIterable 프로토콜을 채용한 enum에서 쉽게 예시를 들 수 있었는데 enum의 값들을 allCases를 통해 배열로 만들어준 뒤 randomElement() 메서드를 통해 랜덤한 값을 구하는 과정에서 enum이 빈 enum일 가능성이 없으므로 (코드를 보았을 때) randomElement의 Optional 리턴값을 강제 언래핑 해줘도 괜찮다는 피드백을 받았고 이에 동의할 수 있었다. 앞으로도 정말 불가피한 상황이나 너무나도 명확한 상황에서는 강제언래핑에 대해서도 생각해볼 여지가 있을 것 같다. (사용을 지양해야 한다는 생각은 여전하지만.. )




