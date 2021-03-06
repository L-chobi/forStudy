## DTO를 따로 두는 이유?
### DTO란?
- DTO란 Data Transfer Object의 약자로 계층 간 데이터 교환 역할을 한다. 
- DTO는 데이터 조작을 위한 것이 아닌 순수하게 계층간 데이터 교환이 이루어질 수 있도록 하는 객체이기 때문에 특별한 로직을 가지고 있으면 안된다. 

### DTO를 이해해 보자
- DTO를 이해하기 전, DTO가 필요한 이유를 먼저 알면 이해가 더 쉽지 않을까 싶다.
- DTO란 개념은 왜 나왔을까? 우리는 프로그램의 불안전성에 크게 한 몫하는게 네트워크 통신임을 알고 있다. 코드의 오작동을 떠나서 네트워크 통신의 오류로 인해 주고받는 데이터에 결함이 생긴다거나, 아예 신호가 안갔다거나 하는 등의 오류는 나처럼 네트워크는 잘 모르는 초보 개발자에겐 큰 절망이 될 수 있다. 서론이 길었지만 어쨌거나 네트워크는 예상치 못한 에러가 될 수 있다. 오죽하면 체크섬 같은 패킷 헤더 등으로 데이터의 무결성을 체크할까.
- 그렇기에 외부와 통신하는 프로그램에 있어 호출은 큰 비용이자 잠재적 오류가 될 수 있다. 그렇다면 이를 줄이고 더욱 효과적으로 데이터를 주고받을 필요가 있다. 
- 이를 위해 데이터를 모아 한 번에 전달하는 방법이 고안되었다. 이때 이 클래스를 DTO라고 한다.

### DTO가 필요한 이유
- 앞서 우리는 DTO가 필요한 이유를 알아보았다. 정리해보자면 DTO가 필요한 이유는 다음과 같다.
1. 필요한 데이터를 묶어 하나의 요청으로 보내어 호출 비용을 최소화하고, 검증, 로직 처리를 한 번에 할 수 있다.
2. 엔티티 내부 구현을 캡슐화 할 수 있다(불필요한 속성이나 메소드를 감춤).
3. validation 코드와 모델링 코드를 분리할 수 있다(Nest의 경우 어노테이션으로 타입 검증 가능).
4. 순환참조를 예방할 수 있다(이 부분은 겪어보지 않아 잘 모르겠다).

### DAO와의 차이
- DAO란 Data Access Object의 약자로 실제 DB에 접근하는 객체다. 이름부터가 Access가 들어가니 무슨 역할을 하는지는 직관적이다. DTO가 계층간 데이터 이동을 위한 Wrapper라면, DAO는 오직 Persistent를 위해서만 사용한다.
- 이를 이해하기 위해선 커넥션 풀(connection pool)이란 것도 알아두면 좋다.
- 커넥션 풀이란 웹서버는 DB와 연결하기 위해 매번 커넥션 객체를 생성하는데, 이런 커넥션 객체를 미리 만들어 놓고 그것을 사용하는 것을 말한다.
- 유저가 CRUD 게시판을 사용한다고 하면 게시물을 읽을 때나 게시판을 조회할 때나 적지 않은 DB와의 커넥션이 일어난다. 그러나 이를 위해 계속 커넥션을 생성하는 것은 매우 비효율적인 일이다. 이런 오버헤드를 효과적으로 관리하기 위해 DB에 접속하는 객체를 전용으로 하나만 만들고 모든 페이지에서 그 객체를 호출하여 사용하는데, 이렇게 커넥션을 하나만 가져오고 그 커넥션을 가져온 객체가 모든 DB와의 연결을 하는 것이 DAO객체이다.
- 즉, <b>쉽게 말하여 DAO는 DB를 사용해 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 오브젝트를 의미</b>한다.

## NoSQL에서 스키마 필드를 추가, 제거 하는 등의 변경이 일어날 경우 기존 데이터에 어떻게 적용할 것인지
### NoSQL은 Schemaless?
- NoSQL의 특성 중 하나는 유연한 스키마 구조이다. 그러나 이는 스키마가 없다는 것(Schemaless)이 아니라, RDB의 스키마 보다 유연하고 탄력적인 스키마를 의미한다. 
- RDB가 ALTER 쿼리를 사용해서 스키마를 변경한다면, NoSQL은 그저 필드를 추가하는 것으로 스키마가 반영되어 저장된다.

### mongoDB의 필드 추가, 제거
- 일단 내가 알고 있는 NoSQL은 mongoDB 밖에 없으므로 이를 예시로 들자.
- 몽고db는 $unset, $set 연산자가 존재한다. mongoose ODM으로만 몽고db를 다뤄봐서 ODM에선 이를 어떻게 적용할지 아직 모르지만 $set은 특정 도큐먼트에 추가할 필드와 그 값을 지정하여 업데이트할 수 있다. 이는 모든 도큐먼트에도 가능하고, 특정한 하나의 도큐먼트에만 적용하는 것도 가능하다.
- $unset의 경우 $set과는 반대로 특정 도큐먼트 혹은 모든 도큐먼트의 특정 필드를 제거할 수 있다. 제거할 필드를 지정하고 그 값을 1로 지정하면 해당 필드를 제거할 수 있다.

### 데이터의 버전관리?
- 그러나 위와 같은 방법은 스키마의 필드를 추가, 제거할 수는 있으나 사용자가 필요할 때마다 변경하는 것이므로 전체적인 관점에서 봤을 때 올바른 데이터관리라고는 보기 어려울 것이다. 
- 그렇다면 버전 값을 적용해보는 것은 어떨까? 초기 스키마의 데이터 버전은 0에서 시작하여 1 혹은 0.1 씩 증가시키는 것 말이다.
- 데이터의 스키마를 변경하기로 할 때마다 특정 문서에서 스키마의 버전 값을 읽어오고, 데이터들의 버전 값을 확인하여 버전이 다를 경우 변경된 버전에 맞게 변경하는 함수를 호출하는 것이다.
- 예를 들어 유저 정보 스키마에 적립금이란 스키마가 추가된다면 해당 유저 정보가 호출됐을 때 스키마 버전이 이전 버전일 경우 적립금 필드를 추가하는 것이다(적립금은 이제 추가되는 것이므로 당연히 0원에서 시작할 것이다).
- 그러나 누군가 해당 유저 정보를 호출할 때마다 버전 체크를 하는 것은 굉장히 불필요한 일일 것이다. 그러므로 최선의 방법은 DB관리자가 DB에 변경이 있을 경우 이를 일괄변경하도록 하고, 버전 필드는 혹시 모를 에러를 위한 조치 혹은 일정 주기마다 데이터를 업데이트할 경우 참조하기 위한 수단으로 두면 좋을 것 같다.

## MSA(Microservices Architecture)
### MSA란?
- 마이크로 서비스 아키텍쳐의 약자로, 단일 프로그램을 각 컴포넌트 별로 나누어 작은 서비스의 조합으로 구축하는 방법. 
- 각 컴포넌트는 서비스 형태로 구현되고(작은 서비스들로 나눠짐) API를 이용하여 타 서비스와 통신함.
- 전체 어플리케이션을 특정 목적을 가진 어플리케이션 단위로 나누는 것. 어플리케이션 간 약한 결합도와 강한 응집도를 목표로 함.
- 나눠진 어플리케이션들이 각각 독립적으로 나누어지기 때문에 배포 또한 독립적으로 가능해짐.
- 각 어플리케이션은 독립적이고 API로 소통하므로 환경(프로그래밍 언어, 프레임워크 등) 또한 제각각으로 구성 가능.
- 일부 기능에 변경이 있다면 해당 부분 어플리케이션만 재배포, 재시작하면 됨. 
- 에러가 발생한다면 해당 에러가 전체 시스템에 영향을 끼치지 않음.
- 코드 복잡성 및 의존성을 제거하여 영향도 파악을 용이하게 함.

### 모놀리식(Monolithic) 구조
- 전체 어플리케이션은 단일 단위로 설계되고 개발 및 배포됨(하나로 뭉쳐짐).
- MSA가 API를 호출하여 서로 통신한다면 모놀리식은 네트워크를 사용하지 않고 단순히 내부 함수 호출로 통신 가능.
- 일반적으로 전체 어플리케이션이 단일 프로그래밍 언어로 작성됨.
- 일부 기능에 변경이 있다면 전체를 다시 재배포, 재시작 해야함.
- 에러가 발생한다면 해당 에러가 전체 시스템에 영향을 끼칠 수 있음.

## 모놀리식 구조 문제점
- 부분 장애가 전체 서비스로 전파된다.
- 부분적인 서비스의 scale-out이 어렵다.
- 서비스의 개선이 어렵고, 수정 시 장애의 영향도 파악이 어렵다. 
- 서비스의 전체 코드가 하나의 프로젝트로 구성되어 배포가 오래걸린다.
- 하나의 framework와 개발언어에 종속되어 서비스에 적절한 기술을 사용하기 어렵다. 

### MSA의 단점 
- 한 트랜잭션의 처리 및 각각의 어플리케이션 에러에 대한 처리가 필요. 한 트랜잭션에 여러 어플리케이션이 상호작용 해야 한다면 그에 대한 처리나 에러 처리가 복잡해짐.
- 어플리케이션의 숫자가 많아질수록, 복잡해질수록 테스트가 어려워짐.
- 모놀리식 구조에 비해 네트워크 레이턴시와 트래픽이 증가함. 모놀리식 구조가 각 서비스가 하나의 어플리케이션에 뭉쳐있기에 내부 함수를 호출하는 식으로 통신이 가능했다면, MSA 구조는 각각 독립적으로 나뉘어 API로 통신하기에 네트워크 호출이 증가하며 그에 따른 트래픽이 증가함.
- 각각의 어플리케이션의 데이터 무결성을 책임지지 못함.

## MVP(Minimum Viable Product)
- 최소기능제품을 의미. 고객의 피드백을 바탕으로 가치를 제공할 수 있는 최소한의 기능을 구현한 제품이라고 한다.
- 애자일 방법론에서 주로 등장하는 단어라고 하는데, 고객이 실제 실행하려는 비즈니스가 올바르게 동작하는 최소한의 기능을 갖추도록 개발하는 방법을 말하는 것 같다.

## SDLC(소프트웨어 생명주기)
- 각 단계에서 소프트웨어 개발과 관련된 단계를 정의하는 프레임워크. 소프트웨어 구축, 배포 및 유지 관리에 대한 세부 계획을 다룬다.
- 전체 개발주기, 즉 소프트웨어 제품의 계획, 생성, 테스트 및 배포와 관련된 모든 작업을 정의한다.
- 소프트웨어 개발부터 폐기까지 전 과정을 하나의 생명주기로 정의한 모델로, 당연히 목적은 고객의 요구사항에 맞는 고품질 제품을 제공하는 것이다.
- 요구사항 분석 단계, 요구사항 확정 단계, 아키텍처 설계 단계, 소프트웨어 개발, 검증 단계, 시장 출시/유지 보수 단계로 나뉜다.
- 폭포수 모델, 나선형 모델, 애자일 모델, 빅뱅 모델 등 다양한 모델이 있다.

## gRPC
- gRPC란 구글에서 개발한 어느 환경에서 실행할 수 있는 최신 오픈소스 고성능 RPC 프레임워크라고 한다.
- RPC란 Remote Procedure Call의 약자로, 별도의 원격 제어를 위한 코딩 없이 다른 주소 공간에서 함수나 프로시저를 실행할 수 있게하는 프로세스 간 통신 기술이다.
- MSA 구조로 서비스를 만들게 되면 다양한 언어와 프레임워크로 개발되는 경우가 있는데, 이러한 구조에 RPC를 이용하여 언어에 구애받지 않고 원격에 있는 프로시저를 호출하여 고유 프로그램의 개발에 집중할 수 있게 해주는 기술이다.
- gRPC를 이용하면 원격에 있는 어플리케이션의 메소드를 로컬 메소드인 것처럼 직접 호출할 수 있기 때문에 분산 어플리케이션과 서비스를 보다 쉽게 만들 수 있다고 한다.
- 프로토콜 버퍼 형태의 메시지를 HTTP/2 프로토콜로 전달하여 기존 REST에 비해 성능이 빠르고, 로컬 함수 호출의 형식이라 직관적이라고 한다. REST에 비해 성능이 많이 뛰어나 MSA 구조에서 내부 통신으로 많이 사용한다고 한다.

### Stub?
- RPC의 핵심 개념으로 파라미터 객체를 메시지로 Marshalling/Unmarshalling하는 레이어라고 한다. 마샬링(marshalling)이란 한 객체의 메모리에서 표현방식을 저장 또는 전송에 적합한 다른 데이터 형식으로 변환하는 과정을 의미한다. 자바에서는 자바 객체를 바이트 스트림으로 변환하는 과정을 의미한다. 언마샬링(unmarshalling)은 이를 복원(바이트 스트림을 자바 객체로)하는 것이다.
- 서버와 클라이언트는 서로 다른 주소 공간을 사용하므로 함수 호출에 사용된 매개 변수를 꼭 변환해줘야 한다. 그렇지 않으면 메모리 매개 변수에 대한 포인터가 다른 데이터를 가리키게 되기 때문이라고 하는데, 잘 모르겠다.
- 클라이언트의 stub은 함수 호출에 사용된 파라미터의 변환(마샬링) 및 함수 실행 후 서버에서 전달된 결과의 변환을 담당한다.
- 서버의 stub은 클라이언트가 전달한 매개 변수의 역변환(언마샬링) 및 함수 실행 결과에 대한 변환을 담당한다.
 
## gRPC Gateway
- 프로토콜 버퍼 컴파일러(protoc)의 플러그인이라고 한다.
- gRPC 서비스 정의(proto file)을 읽고 리버스 프록시 서버를 생성하는데, 이 서버는 RESTful HTTP API를 gRPC로 변환해주는 역할을 한다.

## gateway layer?
-rails를 없애고 gateway layer를 추가하는 이유?

## API Gateway
- MSA 구조와 뗄 수 없는 용어라고 한다.
- API 게이트웨이는 API 서버 앞단에서 모든 API 서버들의 엔드포인트를 단일화하여 묶어주고 API에 대한 인증과 인가 기능에서부터 메시지에 따라서 여러 서버로 라우팅하는 고급 기능까지 등의 많은 기능을 담당한다.

### 인증/인가에 관련된 기능
- 가장 기본적인 기능이라고 한다.
- 인증은 api를 호출하는 클라이언트에 대한 identity(신분)를 확인해주는 기능이고, 인가는 클라이언트가 api를 호출할 수 있는 권한이 있는지를 확인해주는 기능이다.

### API 토큰 발급
- 인증 인가를 거칠 때 마다 매번 사용자의 인증/인가 절차를 거치기는 불편하므로, 토큰 방식을 이용한다.
- 사용자 인가가 끝나면 사용자가 API를 호출할 수 있는 토큰을 발급해준다. API 서버는 이 토큰으로 사용자의 identity와 권한은 확인한 후, API 호출을 허가해준다.
- 즉, API 게이트웨이는 클라이언트를 인증한 후, 이러한 API 토큰을 생성 및 발급해주는 역할을 한다.

## CDN?
- 지리/물리적으로 떨어져 있는 사용자에게 컨텐츠를 더 빠르게 제공할 수 있는 기술이라고 한다.
- 예를 들어 넷플릭스의 경우 해외 서비스인데도 우리나라에 문제없이 제공이 되고있다. 이처럼 CDN은 지리적으로 먼 거리에 떨어져 있는 사용자에게 지연없이 컨텐츠를 분산해 전달할 수 있는 컨텐츠 전송 기술을 의미한다.

## GSLB(Global Server Load Balancing)?
- 이름에 로드 밸런싱이 들어가있지만 이름과 다르게 DNS 서비스의 발전된 형태라고 한다(근데 로드 밸런싱 역할도 한다고 한다)
- 재난 복구(실패에 대해 대체할 수 있는 서버 제공), 부하 분산(트래픽을 여러 서버로 분산), 성능(클라이언트의 위치나 네트워크를 기반으로 최적의 성능을 낼 서버를 선택) 을 목표로 한다. 
- 그냥 DNS가 서버의 상태를 알 수 없다면, GSLB는 서버의 상태를 모니터링(헬스 체크)하고, 응답 실패한 서버의 IP는 응답에서 제외한다.
- 그냥 DNS가 Round Robin 방식(별다른 처리 없이 순서대로 할당하는 방식)을 사용하여 정교한 로드 밸런싱을 수행하지 못한다면, GSLB는 서버의 부하(로드)를 모니터링하며 부하가 적은 서버의 IP를 반환하는 방식이 가능하다.
- DNS가 Round Robin 방식으로 레이턴시가 높은 서버로 연결할 수도 있다면, GSLB는 지역별 레이턴시 정보를 가져 유저에게 더 적은 레이턴시를 가지는 서버로 연결한다.

## WAS?

## Logging Platform

## 배포없이 특정 기능에 대한 폴백(fallback)

## ruby on rails로 개발된 모놀리틱 구조

## ruby on rails 모놀리틱 구조 뒤에 kotlin, spring을 사용한 msa구조

## 개발 서버와 프로덕션 서버

## 개발 패턴
-FACADE 패턴

## 클라우드 오토 스케일링?
- 클라우드 환경에서의 오토 스케일링을 의미하는 것 같다. 물리적 서버를 직접 구축하여 활용하는 형태인 온프레미스 방식과는 반대된다.
- 온프레미스의 경우 실제 서버를 구축하는 것이기에 장비 유지보수비가 비싸거나 서버 증설 작업이 까다롭다고 한다면 클라우드 오토 스케일링은 클라우드 환경에서 자동으로 서버 대수를 늘려준다. 

### 오토 스케일링?
- 서버의 과부하나 장애 등 서비스 불능상태가 되면 자동으로 서버를 복제/추가하여 서버를 늘려주는 작업이라고 한다.
- 즉, 갑작스런 상황(서버 과부하, 장애 등)에 대처하기 위해 CPU 사용률과 같은 리소스를 모니터링 하다가 자동으로 서버를 증가시켜줌으로써 적절한 트래픽 분산을 해준다.

## Scale up, Scale out
### Scale up
- 하나의 서버 자체를 증강하는 것에 의해서 처리 능력을 향상시키는 것을 의미한다. 수직 스케일로도 불린다.
- 보통 갱신이 많이 발생하는 데이터베이스 서버의 경우 정합성(데이터가 모순없이 일관되게 일치되어야 함) 유지를 위해 서버의 수를 늘리는 스케일 아웃 방식보다는 서버 하나의 성능을 증가시키는 스케일 업 방식을 선호한다.
- 더 빠른 cpu로 변경하거나, 더 많은 RAM을 추가하는 등 하드웨어 장비의 성능을 높인다(수직 확장). 다만 성능 확장에 한계가 있다고 한다.
- 레거시 어플리케이션(오래된 기술로 구성된 어플리케이션)의 고성능화에 주로 쓰이며, 구축이 쉽고 관리가 용이한 대신, 단계적 증가가 어렵고 근본적인 해결이 안될 수 있다.

### Scale out
- 하나의 장비에서 처리하던 일을 여러 장비에 나눠서 처리할 수 있도록 설계를 변경한다(수평 확장). 한계가 있는 스케일 업과 달리 지속적으로 확장이 가능하다.
- 저렴한 서버를 사용하여 서버를 추가할 수 있으므로 고성능의 하드웨어를 위해 큰 비용을 쓰는 스케일 업과는 달리 비용부담이 적다.
- 다만 스케일 업은 하나의 서버의 성능을 끌어올리는 것이므로 관리 편의성이나 운영 비용이 큰 변화가 없지만 스케일 아웃의 경우 서버의 수를 늘리는 것이므로 수가 늘어날수록 관리 편의성이 떨어지며, 운영 비용이 증가한다.
- 스케일 업의 경우 하나의 서버에 부하가 집중되므로 장애시 그 영향이 크지만 스케일 아웃은 여러 대의 서버에 분산되어 처리되므로 그 영향이 상대적으로 적게 끼친다.
- 분산처리 시스템, 글로벌 웹 어플리케이션 등에 사용되며, 스케일 업보다는 저렴하지만 설계/구축/관리 비용이 증가한다.
