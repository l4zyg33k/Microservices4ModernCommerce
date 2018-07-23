# 내부 아키텍처

  기술은 마이크로서비스의 이로운 기능이지만, 정의된 특성은 아닙니다. 
  마이크로서비스는 주로 소형 빌딩, 다방면의 기업 비즈니스 팀과 관련이 있습니다. 흥미 있는 각 팀들은, 자연스레 최선의 기술로 자체 마이크로 소프트를 구축할 것입니다. 단일 마이크로서비스의 범위는 내부 작업이기에, Charter 3은 내부 아키텍처를 언급합니다

  기술면에서는, 대부분의 마이크로소프트 기술 에코시스템 내 것들은 마이크로소프트 실행에 필수는 아니며, 기술 스택만 을 이용해 마이크로서비스를 실행시킬 수도 있습니다. 다시 말해, 마이크로서비스는 특정 기술 보다, 조직구조에 관련 된 것 이라는 겁니다. 기술은 마이크로서비스를 규모에 맞게 조작할 수 있게 합니다. 적절한 모니터링 시스템을 통해서는 마이크로서비스, 버전, 문제 발생 가능 인스턴스를 식별할 수 있습니다.

  내/외부 아키텍처 소프트웨어의 대부분은 오픈소스입니다. 마이크로서비스는 해커 커뮤니티에서 만들어졌습니다. 기존 소프트웨어 공급업체는 해당 분야 관련 제품의 생산은 너무 느렸습니다. 내/외부 스택은 다양한 오픈소스 제품으로 구성되지만, 클라우드 공급업체는 일부 지원합니다.

  기업 내 대부분의 기술 조달은 중앙 집중화 되어있습니다. CIO는 특정 데이터베이스, 프로그래밍 언어 등을 선택하여, 조직 구성원에게 사용을 강요할 것입니다. 최소 비용으로 갖춰진 조직에는 해당 전략이 타당합니다. 조달 부서는 공급업체로부터 최선의 가격을 짜내 제품 전문 지식을 갖춘 팀을 만들 수도 있지만, 이는 특정 계층에 구성원이 집중된 수평적 계층화를 강요하기에 마이크로서비스의 원리에는 완전히 반하고, 팀 간 결합으로 개발 속도는 현저하게 느려지게 됩니다.

  오늘날 대부분의 기술 조달은 중앙 집중화 되어있는 반면, 마이크로서비스의 기술 조달은 분명하게 분산되어 있습니다. 
  Chapter 2에서 논의된 바와 같이, 각 팀의 자체 스택 선택에는 충분한 자율성을 필요합니다. 이같은 자율성에는 운영 및 유지관리에 대한 책임이 수반되기에, 팀은 현명한 결정을 내려야만 합니다. 여유 있을 때, 새 버전 0.02프레임워크를 실행해 봐도 좋을 것 같습니다. 기존 기업의 기술 선택 자가 실제 기술자는 아니기떄문에, 조심스런 결정을 내리지않을 수도 있습니다.

- APIs
  각 마이크로 서비스 팀은 하나 이상의 API를 전세계에 공개하기에, API 설계가 잘 되어있어야 합니다. 그러나 해당 Chapter에서는 내부 아키텍처에 초점을 맞춰, 외부 아키텍처에 대해 논의할 Chapter 4를 위하여, API게이트 웨이 및 API로드 밸런서에 대한 언급은 줄일 것입니다.

  API들은 HTTP REST인 경우가 많지만, 필수는 아닙니다. 이는 우연히 최 하단 공통분모이며 가독성이 좋기에 사실상 표준이 되었습니다. JSON(JavaScript Object Notation)과 XML 중 어떤 형식을 선호하십니까?

  JSON은 더 함축적이지만 읽기에도 어렵습니다. 특히, 데이터가 복잡하고 계층구조로 이루어진 때가 그렇습니다. XML은 보다 구조적이고 장황한 데이터에 맞습니다. 모든 최신 도구는 JSON과 XML으로 작동합니다만, XML의 광범위 사용 때문에 그에 대한 에코시스템을 많이 보게 될 것입니다.
두 방법 다 괜찮습니다! 고민하지 마십시오. 어느 쪽이든 조직에 적합하고 편한 것을 사용하십시오.

- Richardson Maturity Model
  레놀드 리처드슨의 저서RESTful Web APIs (O’Reilly, 2013)은 REST API 설계를 위한 4단계 완성 모델을 설명하고있습니다. 재사용을 장려하려면, 가능한 이 계층을 기반으로 API를 설계해야 합니다.

  1) Level 0: RPC
  대부분의 REST 사용은 다음과 같습니다. HTTP는 단순히 전송 메커니즘으로 사용되어 두 가지 방법(하나는 로컬, 다른 하나는 원격) 간 데이터를 교환합니다. 이는 단일 애플리케이션에 가장 많이 사용됩니다.
  각 완성도 수준을 보여주는 예시로, 재고 예약을 살펴보겠습니다. 인벤토리 질의를 위해, 당신은 단일 응용 프로그램에 HTTP POST를 수행할 것입니다. 해당 예시에서는 /App 뒤에 활용 가능하다고 간주합니다. 응용시스템 자체가 요청자의 인벤토리 질의 시도 식별을 위해, XML 구분을 면밀히 분석합니다. 이미, 여기서 애플리케이션 내 요청 전달 위치를 파악하기 위한 비즈니스 로직을 많이 요구하기에 이슈가 됩니다. 
  해당 예시에서는, 쿼리 인벤토리는 인벤토리 레벨을 반환하는 기능으로 재 매핑 되어야 합니다 :

    HTTP POST to /App
    <queryInventory productId="product12345" />

  이는 당신에게 아래와 같이 응답할 것 입니다 :
    
    200 OK
    <inventory level="84" />
  
  인벤토리 점유를 위해, 당신은 아래와 같이 할 수 있습니다:

    HTTP POST to /App
    <reserveInventory>
    <inventory productId="product12345" />
    </reserveInventory>

   유일하게 사용되는 HTTP 동사는 POST이며, 응용프로그램과 상호작용만 한다는 것을 알아차려야 합니다. 개별 서비스에 대한 개념은 없거나 (/Inventory) 서비스에 기능이 더해집니다(/inventory6reserveInventory).

  2) Level 1: resources
  level1은 전체 애플리케이션과 상호작용하는 것 보다, 특정 리소스(/Inventory)와 해당 리소스 내 개체(product122345)와의 상호작용을 요구합니다. 자원들은 깔끔하게 마이크로소프트에 다시 연결됩니다.
  여기 level1을 통해 인벤토리에 질의하는 방법이 있습니다 :
    
    HTTP POST to /Inventory/product12345
    <queryInventory />

  /App 이 아닌 _/Inventory_와 상호작용하는 것을 알아 둡니다. 또한, URL 에서 제품(Product1234)을 직접 참조하고있습니다.
그리고 당신은 응답 받을 것입니다 :

    200 OK
    <inventory level="84" />

  실제 인벤토리를 점유하기위해서는, 아래와 같이 합니다 :

    HTTP POST to /Inventory/product12345
    <reserveInventory>
    <inventory qty="1" />
    </reserveInventory>

  인벤토리든 인벤토리 점유를 질의하든 관계없이, /inventory9/product12345 만 사용하는 것을 기억해 두십시오. 이는 _/App_으로 처리하는 것 보단 확실   히 개선되었지만, 이는 여전히 마이크로서비스를 입력으로 변환하고 올바른 함수로 전달하는 데에 많은 양의 비즈니스 로직을 요구합니다.

  3) Level 2: HTTP verbs
   Level 2는 Level1보다 약간 향상된 것으로, HTTP 동사를 사용합니다. 지금까지, 모든 HTTP 요청은 데이터 검색 또는 업데이트 여부에 상관없이 POST동사를 사용해왔습니다. HTTP동사는 정확하게 해당 목적을 위해서 만들어졌습니다. HTTP GET은 검색에 사용되고, HTTP PUT/POST는 생성 또는 업데이트에, HTTP DELETE는 삭제에 사용됩니다.
  예제로 돌아가자면, 우리는 이제 현재 인벤토리에 질의할 때 제품을 위해 POST 보다 GET을 사용할 것입니다 :

    HTTP GET to /Inventory/product12345
    <inventory level="84" />

  인벤토리 점유는 이전과 같습니다 :

    HTTP POST to /Inventory/product12345
    <reserveInventory>
    <inventory qty="1" />
    </reserveInventory>

  여기서, HTTP PUT 대 POST를 논의하는 것은 범위를 넘어서기에 생략하며, 새로운 인벤토리 기록을 생성하기위해, 아래와 같이 할 수 있습니다 :

    HTTP POST to /Inventory
    <createInventory>
    <inventory qty="1" productId="product12345" />
    </createInventory>

   표준화된 HTTP200 OK 응답이 아니라, 아래와 같이 돌려받을 수 있습니다 :

    201 Created
    Location: /Inventory/product12345
    <inventory level="84" />

  응답에서 반환된 신규 개체 위치로 호출자는 해당 위치의 알림 없이, 새 개체에 프로그래밍 방식으로 접근할 수 있습니다. 비록 이것이 HTTP동사를 도입한다는 점에서 레벨 1보다 향상되었지만, 여전히 그 객체 내 개별적 기능이 아닌 객체를 다루고 있습니다.

  4) Level 3: HATEOAS (Hypertext As The Engine Of Application State)
  Level3는 REST인터페이스 처리 레벨 중 가장 완성된 상태의 형식입니다. Level 3은 HTTP동사를 완전히 사용하고, URI로 객체를 식별하며, 프로그래밍 방식으로 객체들과 상호 작용할 지침을 제공합니다. API는 자동문서화되어 API와 API호출자의 상호 작용을 쉽게 하며, 그에 대한 정보 은닉도 제공 합니다. 이러한 형태의 자기문서화로 서버가 클라이언트 손상없이 URI들을 변경할 수 있습니다
  Level 3/HATEOAS 는 실제 실행 시, 사용하기가 매우 어렵습니다. API와 API소비자의 작성은 어렵지만, 그 원칙들은 따를 가치가 있습니다.
인벤토리 예시로 돌아갑시다. 인벤토리 객체를 조회하기위해서, 당신은 아래와 같이 호출해야 합니다 :

    GET /Inventory/product12345

  그리고 당신이 응답 받는 것은 이와 같습니다 :
    
    200 OK
    <inventory level="84">
    <link rel="Inventory.reserveInventory" href="/Inventory/reserveInventory" />
    <link rel="Inventory.queryInventory" href="/Inventory/queryInventory" />
    <link rel="Inventory.deleteInventory" href="/Inventory/deleteInventory" />
    <link rel="Inventory.newInventory" href="/Inventory/newInventory" />
    </inventory>

  응답에는 인벤토리 개체에 사용 가능한 모든 작업 수행 법을 알려 주는 링크 태그가 포함됩니다. API 호출자는 인벤토리에 대해 알아야할 필요가 있으며, [인벤토리점유] 키를 사용하여 URL을 조회할 수 있습니다 (/Inventory/reserveInventory). evel 3 준수를 위해 노력은 하지만, 높은 수준이니 부족해도 걱정하지 마십시오.
