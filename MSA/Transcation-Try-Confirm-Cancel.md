# Transaction - Try-Confirm/Cancel 

Assuming that we are going to design a payment behavior among microservices we already know a transactional boundary. Then, how could we implement a *transaction*? One of the practical way is employ *Try-Confirm/Cancel (TCC) pattern*.

Cancel and Timeout

The reservation table might be very big. Clearing (deleting the reservation table and adjusting the quantity of each stock) is necessary.

We need to join the *stock* table and the *reservation* table to get an accurate number of the stock.

What if the transaction fails while confirmation phase? It is necessary to deal with these *Heuristic Exceptions* out of the main process. At least, *Eventual Consistency* should be met. It could be fulfilled by adding a reliable queueing system such as Kafka between confirm request/response and confirm process where provides capability of retrying the confirm process to the service. We might employ complicated *Compensating Transaction*. This part could be very tough.

## more
여러 단계로 이루어진 복합 task의 각 단계의 성공 여부를 파악하며 실행 순서를 보장해야 할 일들은 적잖이 있었다. 삼성과 네이버의 DRM 프로젝트를 할 때나, Fetcher Cluster를 설계할 때는 여러 컴포넌트가 하나의 데이터베이스를 공유하면서 task의 처리 단계를 제어했다. 물론 task가 중간 단계에서 실패할 경우에는 roll-back도 해야 했다. 데이터베이스 공유가 나쁜 방법은 아니지만, microservices의 관점에서는 polyglot persistence을 위배한다. 이벤트 기반이 아니니까 느리기도 하고, 요청 이벤트를 추가한다면 굳이 데이터베이스를 공유하는 것이 낭비로 여겨질 수도 있다. MSA에 TCC 패턴을 도입한다면 성능과 간결함을 함께 획득할 수 있다.


## See more:
Request-Reply Pattern
Two-Phase Commit

## Reference
https://www.popit.kr/rest-%EA%B8%B0%EB%B0%98%EC%9D%98-%EA%B0%84%EB%8B%A8%ED%95%9C-%EB%B6%84%EC%82%B0-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B5%AC%ED%98%84-1%ED%8E%B8/

https://www.popit.kr/rest-%EA%B8%B0%EB%B0%98%EC%9D%98-%EA%B0%84%EB%8B%A8%ED%95%9C-%EB%B6%84%EC%82%B0-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B5%AC%ED%98%84-2%ED%8E%B8-tcc-cancel-timeout/