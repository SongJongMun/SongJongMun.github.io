---
layout: post
title:  "2017. 01. 20 BaseCamp에서의 회고.. 2주차"
categories: [BaseCamp]
tags: [Retrospection, BaseCamp]
description: 회고회고회공
---

2017. 01. 18 Retrospection Sprint #2

---

2주차 회고

NHN 엔터테인먼트의 2주는 아주 빨리 지나갔다.
월요일날 프로젝트의 기획을 시작으로 내가 속한 루키달님TF의 프로젝트 설계와 개발이 시작되었다.

# DB설계, 나누고 또 나누고

학부생 시절 이론으로만 DB를 배우고, 설계는 이번이 첫 경험인 만큼 팀의 생각을 쫒아가기가 힘들었다.
이번 DB 설계에서 가장 어려웠고, 또 중점적으로 생각한 것은 다음과 같다.

1. 도메인 별로 구분지어졌는가
2. 다른 Table간의 연관은 어떻게 지을 것인가
3. 정적인 속성을 가지는 도메인과 동적인 속성을 가지는 도메인을 어떻게 구별하여 나눌것인가
4. 속성별로 도메인을 나누어 테이블을 설계하였다면, 동적인 도메인(테이블)과 정적인 도메인과의 연결은 어떠한 방식으로 만들 것인가.

이 4개가 계속 머리속에 맴돌았다.

---

# 프로젝트 개발시작, 많이 복잡하다.
나 혼자만의 개발이 아닌 다른 사람들과의 협업이기 때문에 첫 발자국, 시작의 순간이 제일 중요했다.
팀원들 모두 사전과제를 어느정도 수행한 경험을 가지고 있어, 팀원들과의 토의로 기반이 되는 프로젝트를 정하여 시작하는 것은 어렵지 않았다.

하지만 프로그래밍을 할 때 각자의 스타일이 있기 때문에 다음과 같은 협의가 필요하였다(앞으로도 더 할 것 같지만)
1. Git Commit, Push규칙(Commit멘트나, Push Request 말, 리뷰규칙 등 혹은 따봉 날리기)
2. Git 충돌이나 각자의 인터페이스를 어떻게 규정하고 호출할 것인가...

더 많은 안건들이 있었지만 프로젝트 초기에 이러한 규칙들을 정했다.

그런 다음 우리 팀은 각 DB에 해당하는 DAO(Data Access Object), VO(Value Object), Service 즉 Model에 해당하는 영역을 먼저 각자 개발하고 개발한 부분에 가장 연관성이 깊은 View를 정해서 Controller와 같이 개발하기로 정하였다.
DB의 경우는 사타리 타기로 정하였는데 나는 모임의 정보를 저장하고 관리하는 Trip도메인을 맡게 되었고 서비스 부분에서 검색, 즉 여러가지 변수를 가지고 해당 속성을 가지는 모임을 찾아서 출력하거나 한개의 모임에 해당하는 상세 정보를 보여주는 서비스 기능을 개발하기로 하였다.

기본적인 DAO, VO, Service를 만들고 좀 더 복잡한 기능들을 추가하면서 골치아파지기 시작하였다.
가장 힘들었던 것은 검색 기능의 개발이였다. 단 한번의 검색을 수행하기 위해서 6개의 테이블을 전부 연결지어서(아이러니하게도 필요한 요소들이 각 테이블마다 1-2개씩 배치되어 있었다.) 출력할 수 밖에 없었다.
한 개의 쿼리문을 통해 검색한다면 총 5-7개의 중첩 조인을 할 수 밖에 없는 상황이었다.

하지만 해당 쿼리를 돌리면.. 특히나 엔트리가 많아지면 많아질 수록 쿼리문을 수행하는 시간동안 DB에 Lock이 걸릴 것이고, 차라리 모든 정보를 Trip에 넣는 방법이 더 나을듯 했다.

그래서 오늘, 어쩔 수 없이 몇개의 테이블에 반정규화를 통해 중복된 데이터를 넣을 수 밖에 없었다. 또한 한개의 쿼리문이 아닌 각 테이블에서 최소한의 검색과 조인을 수행하고 나온 결과를 서버에서 취합하는 방식으로 바꾸었다.

아직 코드가 완성되지 않았지만.. 코드가 많이 늘어나는 대신 속도가 좀 더 빨라질 것 같다. 또한 내가 맡은 서비스긴 하지만 다른 팀원들이 맡은 Model 구현 부분이 조금씩 수정될 것이기 미안한 감정도 들었다. 내가 구현일정에서 앞서게 된다면(그래야 하는데..) 많이 팀원들을 도와줘야겠다.

그리고 오늘 내가 제안한 변경안을 팀원들에게 설명해주면서 느낀건데.... 내가 생각한 것을 갈무리 하고 좀 더 알기 쉽게 설명할 수 있는 능력 좀 많이 키웠으면 좋겠다.

서버 설정에 스프링 스터디에 데이터베이스 모델링까지... 주말이 매우 바빠질 것 같다.

---

# 설계의 중요성을 몸으로 알게되었다.