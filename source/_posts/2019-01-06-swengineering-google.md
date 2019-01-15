---
title: Software engineering in Google
tags: 
- google
---

[software engineering in google](https://arxiv.org/ftp/arxiv/papers/1702/1702.01715.pdf)논문을 보고 몇가지 인상적인 부분을 요약해 봤다. 

# sw development

- 하나의 소스 코드 repo. 
  - 하나의 거대한 repo에 모든 프로젝트를 관리하고 있고, 누구나 read 권한을 가지고 있다. 
  - Chrome이나 안드로이드는 오픈 소스이므로 별도의 repo에 있다. 
  - subtree는 owner만 write권한을 가지고 있고 이들의 리뷰 및 승인을 받아야 반영된다.
  - subtree는 최소한 두명이상의 오너가 등록이 필요하고, 하위폴더는 상위폴더의 오너를 상속.
  - 리뷰를 받아야지만 변경이 메인라인에 반영됨
- 빌드 시스템
  - Blaze라고 하는 분산 빌드환경을 제공
  - Blaze BUILD 파일을 작성하는데, 이름, 소스 파일들, 필요로 하는 라이브러리들(dependency) 이를 build rules이라 함.
  - 빌드는 몇가지 step으로 이루어짐. 예로 빌드 스텝과 링킹 스텝
  - 빌드는 구글 cloud 인프라위에서 돌기 때문에 매우 빨리 결과가 나옴
  - 빌드 결과는 클라우드에 cache됨.
  - presumit checks. 반영되기 전에 테스트를 돌린다거나 static analysis를 돌리는 등의 결과를 피드백 받음.
- 코드 리뷰
  - 웹 기반의 코드리뷰 시스템이 있고, 이메일과 연동되어 이메일로도 모든 리뷰 상황이 피드백 된다. 
  - 리뷰어를 자동으로 추천해주는 시스템도 있음.
  - 리뷰어가 리뷰는 늦게 하거나 승인을 꺼리는 경우 문제가 개발 속도가 저하되는 문제가 있는데, 리뷰어가 여러명이기 때문에 더 나이스한 사람에게 리뷰를 
  요청하는 것으로 이문제를 회피할 수 있음.
  - 코드 리뷰 과정은 메일링 리스트에 자동으로 기록으로 남음.
  - experimental repo는 코드 리뷰없이 반영할 수도 있음. 하지만 이를 권장하지는 않음.
  - 변경의 크기를 300라인이하로 작게 만들것을 권장함.
- 테스팅
  - unit testing은 기본. 
  - code coverage를 소스 코드 탐색기에 보여주기도 함. 
  - 프로덕션 환경에 디플로이 하기전에는 로드 테스팅을 기본으로 함.
- 버그 트래킹
  - Buganizer라고 하는 이슈 트래킹 시스템을 사용.
  - 코드 반영을 할때 관련된 이슈를 레퍼런스하게 되어 있음
- 프로그래밍 언어
  - 5가지 공인 언어(C++,java, python, go, javascript)를 사용할 것을 독려
  - 각 언어에 대한 style guide가 있음.
  - 다른 언어에 대한 연결은 protocol buffer로 함. 이는 Google RPC 라이브러리와 통함되어 있음
- 디버깅 & 프로파일링
  - 서버들은 디버깅 라이브러리들을 링킹하고 있는데, 크래쉬 상황이나 OOM 상황에서 signal handler가 스택 덤프를 로그 파일에 적게 되어 있고, core file도 남기게 되어 있음
  - incoming/outgoing RPC packet들에 대한 로그나 타이밍등도 남기게 되어 있음
- Release engineering
  - 대부분의 프로세스가 자동화 되어 있기에 자주 릴리즈 된다. 주별로, 일별로 진행된다. 
  - green 빌드로 확인된 변경을 싱크해서 릴리즈 브랜치로 보내고, 이번 릴리즈에 필요한 특정 변경사항들을 cherry-pick한다. 테스트가 실패하거나 하면 다시 cherry-pick한다. 모든 테스트가 성공하면 바이너리와 데이터들을 같이 패키지 한다.
  - 이렇게 릴리즈 후보가 만들어지면, staging 서버로 보내고 integration test를 한다. 
  - 이때 production의 트래픽을 staging서버로 보내서 테스트하는 테크닉을 쓴다. 물론 실제 production으로 보내고 카피하여 스테이징으로 보내는것. 이렇게 하면 미리 production환경을 테스트 해볼수 있어 유용하다.
  - 다음은 canary deploy인데, 트래픽을 몇%만 새 서버로 주고, 나머지는 기존 서버로 보내어 새 서버에 큰 문제가 없는지 본후, 서서히 새 서버로의 트래픽을 늘려주고 100%까지 채운후 기존 서버군을 셧다운 한다.
- launch approval
  - 사용자가 보게되는 서비스는 launch approval을 받아야 한다. 이는 reliability, legal, privacy, security등의 모든 것이 충족되어야 허가가 난다.
- 회고(post-mortems)
  - 큰 장애가 나거나 사소한 에러가 나도 관련된 사람은 회고 문서를 작성하게 된다. 문제에 포커스가 있고, 재발방지를 위해 어떻게 해야 하는지에 집중한다. 누구를 비난하기 위한 것이 아니다. 문제는 수치로 디테일하게 작성된다.
- 잦은 재개발(frequent rewrite)
  - refactoring은 비용이 높다. 하지만 구글의 민첩성과 미래에 도움이 된다. 
  - 리팩토링은 불필요한 복잡함을 없애고, 더이상 필요하지 않은 기능들을 없앨 수 있다. 또한 새 개발자들에게 ownership을 전파하는 효과도 있다. 그들의 것이라고 느끼면 사람들의 생산성은 높아진다. 

# project management 

- OKR(Objectives and Key Results)
  - OKR 년단위 또는 분기 단위로 모든 임직원은 OKR을 작성해야 한다. OKR은 측정 가능한 목표다.
  - 좋은 성적을 낸 팀은 다음에 더 높은 OKR 목표를 입력하게 되고, 못한 팀은 더 적은 목표를 입력하게 된다.
- 프로젝트 승인
  - 정형화된 프로세스는 없다. 대부분은 bottom-up 방식으로 진행된다.

# people management 

- engineering manager
  - 사람을 매니지만 하는 역할이다. 엔지니어도 매니징을 할 수 있지만, 엔지니어링 매니저는 사람 관리만! 한다.
  - tech lead는 중요한 테크니컬 결정을 하고, 매니저는 tech lead를 선택하고, 그들의 팀과 성과를 매니지 한다.
  - 코칭을 하고, 팀원의 커리어 발전을 도와주고, 평가를 하고, 보상을 한다.
  - 채용도 관장한다. 
  - 보통은 8-12명까지는 관리하고, 3-30명까지 관리할 수도 있다.
- Software engineer(SWE)
  - 엔지니어와 매니저는 다른 트랙을 가지고 있다
  - 사람을 매니징하는 것은 승진과 별 상관이 없다. 
  - 리더십을 보여주는게 중요한데, 리더쉽은 많은 개발자가 사용하는 임팩트 있는 SW를 개발하는 것이다.
  - 이렇게 함으로서, 테크니컬 스킬은 좋으나 매니징에 서툰 사람에게 좋은 커리어 패스를 제공한다.
- research scientist
  - 예외적은 리서치를 할 수 잇는 사람을 뽑는다
  - 대개 논문등으로 성과를 측정하지만 하는 일은 SWE와 크게 차이가 나지는 않는다.
  - 대개는 SWE와 같은 팀에서 같이 프로젝트를 하게 된다. 
- Site Reliability Engineer (SRE)
  - 운영에 관련된 엔지니어이다.
- product manager
  - product을 매니징하는 역할. 
  - 엔지니어와 조율하면서, 필요한 피처들을 구체화하고 우선순위를 매겨서 전달하는 역할을 한다. 
  - 코드를 쓰지는 않지만, 필요한 코드가 쓰여지게 조율하는 역할이다.
- program manager/technical program manager
  - product manager와 비슷하지만 product을 관리하지 않고 프로젝트와 운영등을 관리한다.
  - TPM은 비슷하지만 특정 기술이 더 필요한 분야에 필요한 사람이다. 
