---week 1---
• 프로젝트는 책 "코드로 배우는 스프링 웹 프로젝트 - 개정판" 을 토대로 수행하였으며, 책에 설명되지 않은 요소를 찾기위해 대부분의 변수나 attribute 이름을 
  책에 작성된 이름과 동일한 이름으로 설정하지 않고 유사한 의미를 가진 단어로 설정
• 스프링MVC프로젝트 생성후 persistence 계층 테스트 수행위해 mybatis 3.4.6 버전을 사용
• mybatis 세부설정을 하지않고 quick setup 을 사용
• quick setup 을 했을 경우 최소 sqlsessionfactory와 mapper interface 를 필요로 하기 때문에 root-context.xml에 sqlsessionfactory 생성 후 mapper interface 파일생성
• sqlsessionfactory 는 dataSource를 필요로 하기때문에 hikariDataSource를 사용하여 dataSource를 설정
• root-context.xml 에 mybatis가 스캔할 mapper interface 가 있는 패키지를 지정

• sql이 복잡하거나 길이가 과도하게 길어질 경우에 대비하기위해 쿼리를 mapper.xml 에 작성
• 이 때 mapper interface 파일명은 tblMapper, mapper.xml 의 이름은 pracMapper.xml, mapper namespace 의 value는 heo.yu.mapper.tblMapper로 설정했음
• 이 때 pracMapper.xml 에 작성된 namespace 의 value 가 pracMapper.xml에 작성된 쿼리의 토대가 되는 mapper interface 의 이름과 같다면, mapper.xml 파일의 이름은 쿼리 실행과정에
	아무런 영향을 끼치지 않는다고 가정했음
• 이 경우에 mapper test 를 실행했을 때 BindingException 이 발생
•	에러 발생 후, interface 파일과 mapper.xml 파일의 이름을 일치시키니 정상적으로 작동, 에러 해결됨

•	이 에러가 발생한 이유는 mapper interace 와 mapper.xml 이 같아야 mybatis가 두 파일을 자동으로 매핑하지 못해서발생

• 이 외에도 namespace를 바꿔서 테스트 했는데 illegalStateException 에러가 발생
•	이 에러가 발생한 이유는 mybatis 의 namespace에 관한 name resolution rules 때문인데, mybatis 는 statments, result maps, cashes 등 과 같은 모든 naming 된 요소들에 
	동일한 name resolution rules를 적용함
• 이 rules 에는 두가지 분류가 있는데
	1. fully qualified names 의 경우
		 대상의 이름을 패키지 이름을 포함한 직접적인 형태로 지정했을 경우, 모든 조건을 만족하는 요소가 스캔 대상으로 설정되고, 발견되면 사용됨  
	2. Short names 의 경우
		 대상의 이름을 모두 적지 않고 한 요소만 작성하여 대상을 지정할 수 있는데, 이 때 작성된 요소의 value와 일치하는 대상이 복수일 때 
		 (e.g. “com.foo.selectAllThings and com.bar.selectAllThings”) short name is ambiguous 라는 메세지가 출력되면서 에러가 발생하기 때문에, 이 경우 
		 위와 같은 fully qualified name 을 사용해야함

• namespace의 value를 mapper interace파일의 이름과 일치시키니 해결됨
• 결국 mybatis는 interface나 xml, namespace의 value 등 이름을 토대로 스캔한다는 것을 알 수 있었음
• 간편하게 사용할 경우, 편리하게 mapper interface, mapper.xml, mapper.xml 의 namespace를 모두 일치시키면 정상적으로 작동한다는 것을 알게됨


