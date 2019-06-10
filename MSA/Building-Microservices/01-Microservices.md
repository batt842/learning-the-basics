# 1. Microservices

So far…

DDD(2011) - 실세계를 코드로 표현(represent)하는 일의 중요성, 그 중에서도 시스템 모델링의 더 나은 방법 제안.

CD - 체크인한 모든 것을 릴리스 후보로 대해야 함. 효율적으로 소프트웨어를 양산(get the software into production).

Web - 머신들이 통신하는 더 나은 방법 개발할 수 있게.

Hexagonal architecture concept - 비즈니스 로직이 숨을 수 있는 계층형 아키텍처를 지양. (Ports and adapters pattern)

가상화 플랫폼 - 자유로운 프로비저닝/규모조절.

Infrastructure automation - 머신 스케일링.

on-demand virtualization

아마존, 구글 등 - 작은 팀이 담당 서비스의 수명주기를 관리하는 것이 옳다. (작고 자율적인 팀)

넷플릭스 - 확장가능한 antifragile system을 구축하는 방법을 공유.
System at scale

등등

언제나 그렇지만, MSA도 (사라져버린 것 포함) 과거 위에 세워짐.

# 1.1 Microservices
작고 자율적으로 협업하는 서비스.

단일책임원칙 Single Responsibility Principle - 같은 이유로 변경되는 것은 한데 모으고, 서로 다른 이유로 변경되는 것들은 분리하라. Gather together the things that change for the. same reasons. Separate those things that change for the different reasons.

Jon Eaves from RealEstate.com.au - The service can be rewritten and. redeployed in 2 weeks.

얼마나 작아야?의 진부한 답변 - The service small enough that is not possible to get smaller.
더 중요한 건…. 서비스가 팀 구조와 서로 얼마나 적절하게 배치되는지부터 알아야.

자율성 - 서비스는 독립적으로 변경, 배포 가능해야.

## 1.2. 주요 혜택(?)
기술 이기종성, Resilience, Scalability, 배포 용이성,  조직 부합성, composability(seam을 잘 조합해서 재사용), 대체 가능성을 위한 최적화(against legacy system)

## 1.3. SOA
서비스의 최종 능력 집합 end set of capabilities을 제공하는 여러 서비스가 서로 협업하도록 하는 설계 접근 방식. 각 서비스는 완전히 분리된 운영시스템의 프로세스. 서비스 간 통신은 네트워크 호출로 이루어진다.

SOA는 좋은 개념이긴 하지만… 실제적이지 않아서(how to do practically) 실패. 반면 마이크로서비스는 실사용을 기반으로 등장. SOA가 개념이라면 MSA는 구체적인 접근법.

## 1.4 기타..
공유 라이브러리, 모듈, 등
어쨌든 there’s no silver bullet or don’t treat it as a golden hammer.
