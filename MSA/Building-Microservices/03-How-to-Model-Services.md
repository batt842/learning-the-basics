# 3 How to Model Services

## 1
스킵

## 2
15년 가까이 듣는 듯-<br>
Loose coupling & High cohesion

하나가 변경될 때 다른 게 변경될 필요 없도록<br>
릴리즈가 줄어드니까 변경도 빠르고<br>
한꺼번에 여러 서비스를 변경해서 릴리즈하면 위험<br>

## 3 Bounded Context
Any 도메인 > multiple bounded contexts<br>
Context는 인터페이스가 있고<br>
Model을 주고받으며 통신하며, 모델은 shared/hidden<br>

모놀리식 시스템의 모듈은 마이크로 서비스가 될 수 있다<br>.
보통, 마이크로 서비스는 bounded context와 align되는데<br>
모놀리식 시스템의 모듈을 컨텍스트로 모델링하는 단계는 뛰어넘을 순 있어도<br>
웬만하면 조심조심…

Prematurely decomposing a system into microservices can be costly, especially if you are new to the domain.<br>
(어떻게 보면, 경험있는 전문가의 경력은 유니크하다)

## 4 Business Capabilities
Bounded context를 생각할 때는, 각 컨텍스트가 도메인의 나머지에게 제공하는 capabilities에 대해 생각해야. <br>
“What does this context do?”, and then “So what data does it need to do that?”<br>
(크롤러와는 다르다! 크롤러는 데이터와 capability 모두 중요한 특수한 케이스.)

## 5 Turtles all the way down
Coarse-grained bounded context -> nested contexts
Should be based on your organizational structure.

Testing simpler - with more coarse-grained APIs (유닛테스트 관점에서 반박할 점은 있을 듯… 하지만 트레이드 오프니까 뭐…)

## 6 Communication in Terms of Business Concept
Often, 시스템 변경은 비즈니스의 요구. 서비스 하나만 수정할 수도 있으니, 마이크로서비스가 손 갈 일이 적다.

비즈니스 용어는 인터페이스든 어디든 반영되어야 한다.

## 7 The Technical Boundary
성능을 생각하면 기술 접합부에 따라 서비스 경계를 모델링하는 게 나쁜건 아니지만, context boundary가 먼저다.



