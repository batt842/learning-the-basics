# 4 Integration

## 1 Looking for the Ideal Integration Technology

### 1.1 Avoid Breaking Changes
컴포넌트의 변경이 인터페이스에 영향을 미치지 않게

### 1.2 Keep Your APIs Technology-Agnostic
시간이 지나도, 새로운 도구를 도입해도 영향을 받지 않게

### 1.3 Make Your Service Simple for Consumers
어렵다.

Client library : 소비자에겐 자유, 결합 비용은 증가

### 1.4 Hide Internal Implementation
결합 비용 감소, 소비자 변경의 자유, 기술 부채 감소

## 2 Interfacing with Consumers
No meaning

## 3 The Shared Database
Common, simple, fast, and popular.

1. 결합 증가, 깨지기 쉬움(brittle), 스키마를 바꿀 때마다 대규모의 회귀 테스트가 필요.
2. 기술 선택 범위가 제한. 자율도 없고, 구현 은폐도 없다.
3. No cohesion. Shared behavior. Spread logic.
Difficult to avoid making breaking changes.

## 4 Synchronous Versus Asynchronous
We know what sync or async is.

* Request/response : works with sync/async.
* Event-based : no request just say what happened, asynchronous, the business logic is not centralized but distributed, highly decoupled 
* What to choose? : how well suited (…하나마나한 이야기를)

## 5 Orchestration Versus Choreography
About managing business processes that scratch across the boundary of individual services.

Orchestration : We rely on a central brain to guide and drive the process, using req/res style, can use rule engine, too much authority to a single service, brittle, higher cost of change

Choreography : only emits an event, more decoupled, implicit business process, needs a monitoring system, flexible

## 6 Remote Procedure Calls
Executing a remote service by a local call, Easy to use

Some of RPC relies on an interface definitions : SOAP, Thrift, Protocol Buffer, … -> easy to generate client/server stubs.

Various message formats, various networking protocols, …

### 6.1 Technology Coupling
Interoperability 

Java RMI < Thrift or Protocol Buffer

### 6.2 Local Calls Are Not Like Remote Calls
Problem : No think or without knowing if the abstraction is overly opaque

Network is not reliable.

### 6.3 Brittleness
In case of Java RMI, many changes(it’s really usual!) could produces many methods which provide the same functionality but different prototypes.

Change of field -> deserialization problem -> need lock-step release…

Dictionary type : (I like it. It’s easy with JSON.)

Binary Serialization : Expend-Only…

### 6.4 Is RPC Terrible?
Better than the database integration.

Don’t abstract your remote calls too much. (Make developers aware of the network)

Ensure that you can evolve the server interface without having to insist on lock-step upgrades for clients.

Make sure your clients aren’t oblivious to the fact that a network call is going to be made. (By well structured libraries)

## 7 REST
Resource and Representation <- research more!

Richardson Maturity Model!

REST doesn’t have to use HTTP. (commonly HTTP)

### 7.1 REST and HTTP
REST goes really well with HTTP

### 7.2 Hypermedia As the Engine of Application State

### 7.3

### 7.4

## 7.5 Downsides to REST over HTTP
Too many thing to do (than RPC) 

Performance

## 8 Implementing Asynchronous Event-Based Collaboration
### 8.1 Technology Choices

Main parts of events : Emit(publish) & Find(subscribe)

Message broker : Handles both problems, and even handles the state of consumers. Usually scalable and resilient. It’s a new system -> increases complexity. Very good for a loosely coupled event-driven architecture.

“Keep your middleware dumb, keep the smarts in the endpoints.
HTTP feed such as ATOM : Easy. Scalable. Polling schedule is important -> not fast.

Consider ATOM first, than consider message brokers. But don’t spend too much resources on the previous one. Be aware of the sunk-cost fallacy.

### 8.2 Complexities of Asynchronous Architecture
It’s complex!

Long-running async request/response : what to do when the response comes back. To the same node? What if it’s down? Should its information store somewhere?

Short-lived async still requires a different way of thinking.
Consider correlation IDs. (it’ll be covered in Ch8)

## 9 Services as State Machines
A microservice OWNS all logics associated with behaviors in this context.

Decides to accept a request (maybe occurring state changes) or not. <- it’s cohesive.

Having the lifecycle of key domain concepts EXPLICITLY MODELED is powerful.

## 10 Reactive Extensions (Rx)
A mechanism which composes the results of multiple calls together and runs operations on them.

Regardless of whether the data is synchronous or asynchronous.

Rx implementations and Distributed systems go really well.

## 11 DRY and Perils of Code Reuse in a Microservice World

Not just for code, but for behavior and knowledge.

Shared (common) library, especially very core (not like logging libraries) -> more coupling -> complicated update

The evils of too much coupling between microservices are far worse than the problems caused by code duplication.

### 11.1 Client Libraries

more COUPLING!

The service logic could leak into the client.

AWS: provides various SDKs just abstract the underlying APIs. <- This works, because the client is in charge of when the upgrade happens.

Netflix: Client libraries for Reliability and Scalability. Not about service, but about service discovery, failure modes, logging, and so on.

The clients should have to be charge of when to upgrade their client libraries. <- independent release.

## 12 Access by Reference
Reference: smaller message, fresh state

Reference(fresh state) + Event-driven(the state when the event happened): good

Increased load on the service : freshness duration + cache control

Potential coupling: A service wants to know whole resource, actually doesn’t need to do.

## 13 Versioning
### 13.1 Defer it for as Long as Possible
Avoiding BEAKING CHANGES! We need less coupling.<br>
eg. Database makes it very hard to avoid breaking changes. REST makes it less likely to change.

Tolerant Reader (by Martin Fowler) : who is able to ignore not interesting things.

Postel’s Law (aka. Robustness Principle) : “Be conservative in what you do, be liberal in what you accept from others.”

### 13.2 Catch Breaking Changes Early
Sometimes it’s inevitable. Have a conversation with people who are in charge of consumer services. (웹검색 조직이 종종 하는 거다 ㅠ)

Consumer-Driven Contract (CDC)

### 13.3 Use Semantic Versioning
MAJOR.MINOR.PATCH
* MAJOR : backward incompatible
* MINOR : backward compatible, new functionality
* PATCH : bug fixes for existing functionality

### 13.4 Coexist Different Endpoints
The expand and contract pattern : v1 -> v1 & v2 -> v2

We can add version number in request header or URI. Which one is better? Don’t know.

### 13.5 Use Multiple Concurrent Service Versions
Not that good. But necessary when having a consumer which is hard to change like a legacy system.

Down sides : Double bug fixes. (Sometimes hidden) Consumer redirecting handlers. High complexity with persistent state.

Better to consider coexisting different endpoints.

## 14 User Interface
Client UI -> Thin UI Web -> Fatter Web…

### 14.1 Toward Digital
For various UIs, more granular APIs<br>
UI as compositional layer

### 14.2 Contraints
Type of browser, resolution, network throughput, battery, …

eg. Using SMS instead of network bandwidth.

### 14.3 API Composition
Down sides of being with APIs using XML or JSON via HTTP for a web page or a native mobile application are :
1. not tailorable for different sorts of devices,
2. if another team creates the UI, it would be hard to work, 
3. communication load (-> we need an API gateway)

### 14.4 UI Fragment Composition
Each service can provide a (coarser-grained) UI fragment. -> We need a assembly layer.

Pros. The team in charge of the service would change the UI fragment directly. -> Fast and Easy.

Cons.
1. Inconsistency of the user experience -> lack of seamless experience -> need living style guide of sharing of assets (HTML components, CSS, images, …)
2. In case of Native applications or Thick applications -> a Hybrid approach or falling back to API-call&UI-build approach -> Responsive component (single source) might be helpful…
3. What if the service and the UI fragment don’t fit to each other?

### 14.5 Backends for Frontends (BFF)
Aggregation Endpoint or API Gateway : can marshal backend calls, do something for various devices, …. -> getting thicker and powerful -> disaster (for management and changes)

Backend For Frontend : Single Backend for Single Frontend
- Multiple services -> single backend -> single frontend
- Should not contain the service’s logic (similar to the danger an aggregating layer has) -> BFF should only have behavior specific

### 14.6 A Hybrid Approach
No one-size-fits-all

## 15 Integrating with Third-Party Software
Such as commercial off-the-shelf softwares (COSTS) or Software as a Service (SaaS), …

Build if it is unique to what to do, and can be considered a strategic asset; buy if your use of the tool isn’t that special.

### 15.1 Lack of Control
The decisions have been made by the vendor : How to integrate, Which language, Using version control/rebuild/using CI for customization, …

### 15.2 Customization
Don’t trust vendors… their customization features are shit.
Sometimes causes coupling problems.

### 15.3 Integration Spaghetti
Shitty integration can also cause coupling problems.

### 15.4 On You Own Terms
Do any customization on a controllable platform, Limit the number of consumers of the tool.

eg1. CMS as a service : Hiding a CMS with the facade pattern
eg2. Multirole CRM system : More facades…

### 15.5 The Strangler Pattern
Capture and intercept calls to the old system.

### 16 Summary
Avoid database integration at all costs.

Understand the trade-offs between REST and RPC, but strongly consider REST as a good starting point for request/response integration.

Prefer choreography over orchestration.

Avoid breaking changes and the need to version by understanding Postel’s Law and using tolerant readers.
Think of user interfaces as compositional layer.

