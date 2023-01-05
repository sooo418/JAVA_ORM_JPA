# JPA

- JPA (Java Persistence API)

## 목표 - 객체와 테이블 설계 매핑

- 객체와 테이블을 제대로 설계하고 매핑하는 방법
- 기본 키와 외래 키 매핑
- 1:N, N:1, 1:1, N:M 매핑
- 실무 노하우 + 성능까지 고려
- 어떠한 복잡한 시스템도  JPA로 설계 가능

## 목표 - JPA 내부 동작 방식 이해

- JPA의 내부 동작 방식을 이해하지 못하고 사용
- JPA 내부 동작 방식을 그림과 코드로 자세히 설명
- JPA가 어떤 SQL을 만들어 내는지 이해
- JPA가 언제 SQL을 실행하는지 이해

# JPA와 모던 자바 데이터 저장 기술

- SQL 중심적인 개발의 문제점
- JPA 소개

## SQL 중심적인 개발의 문제점

- 무한 반복, 지루한 코드 (CRUD)
- 객체의 필드 추가시 객체에 관련된 쿼리문을 전부 수정해야함.

### **객체를 관계형 데이터베이스에 저장**

---

객체 → SQL 변환 → RDB에 접근

객체와 관계형 데이터베이스는 같은 구조가 아니므로 개발자가 객체를 관계형 데이터베이스에 저장 또는 조회를 하기 위해 SQL 매퍼를 작성해줘야함.

### **객체와 관계형 데이터베이스의 차이**

---

1. 상속
2. 연관관계
3. 데이터 타입
4. 데이터 식별 방법

**상속**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/81fa39e4-4cc3-4c88-8f55-8bb3588d0e13/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143215Z&X-Amz-Expires=86400&X-Amz-Signature=cccb9c9e4cbeedb992dc31ff3234b33b0068e302aa46dec1ed06f8343a80b8c0&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 기본적으로 관계형 데이터베이스에는 상속이란 개념이 없다.
- 관계형 데이터베이스에서는 상속 대신 슈퍼타입 서브타입 관계라는 개념이 존재함

**연관관계**

- 객체는 참조를 사용: member.getTeam()
- 테이블은 외래 키를 사용: JOIN ON M.TEAM_ID = T.TEAM_ID

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4a3f6807-9c99-4a8c-adad-33100037a458/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143234Z&X-Amz-Expires=86400&X-Amz-Signature=44cdc5830f897986713f2d431381e555883b86619ad9008ad65762f8022cf2a2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*객체를 테이블에 맞추어 모델링*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/da489530-e9c8-46b0-8f28-95f41ab619fa/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143248Z&X-Amz-Expires=86400&X-Amz-Signature=7fb626df9cef0b168fdf9fde15a8b9147c5eb1a90f0704507142b0b96f503389&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*테이블에 맞춘 객체 저장*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f650129d-dc27-4f23-b132-02b8c2147f1b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143259Z&X-Amz-Expires=86400&X-Amz-Signature=6fa9724d93325687c7ad4ddf3fdd389f4f2f3e2f21c0bf10a3e9a4076dd95eb9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 관계형 데이터베이스에 저장할때 바로 `Member` 객체가 가지고 있는 필드를 가져와서 인자로 넣어주면 된다.
- 하지만 `Member` 객체와 `Team` 객체의 연관관계가 객체다운 형태는 아니다.

*객체다운 모델링*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/40aeaac7-8b3f-46d9-b691-90d4e7db555f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143307Z&X-Amz-Expires=86400&X-Amz-Signature=d249e47f84ec725e0897a6276340aa9f058e2348dd25645583327dd0011ee999&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*객체 모델링 저장*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5174cf23-7500-4d79-867a-96a04df1f815/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143317Z&X-Amz-Expires=86400&X-Amz-Signature=0cd58036746b60b77fa34428103db48bb3cc467aa62023c0b9550888bcae56bd&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `Member` 객체에서 `Team` 객체를 꺼내고 또 `team_id`를 가져 와야 하는 번잡함이 있다.

*객체 모델링 조회*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7e2f11d2-be94-40a2-8682-3672571fdaaf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143327Z&X-Amz-Expires=86400&X-Amz-Signature=6ed2359a5a41f4db9187b101ce974421ba86c4c831c31aa8c3a72b84ba9224e3&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 데이터베이스의 성능을 위해 `Member`와 `Team` 객체를 한번에 조회를 하게된다. 조회쿼리 실행 후 `Member` 객체의 정보를 `set()` 해주고 또 `Team` 객체의 정보를 `set()` 해주고 그리고 `Member` 객체에 `Team` 객체를 `set()` 해줘야하는 번거로움이 발생한다.

*객체 모델링, 자바 컬렉션에 관리*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ae8d1adb-e063-4d08-8106-5c672f993bc8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143518Z&X-Amz-Expires=86400&X-Amz-Signature=4c3d55f429aea0dbd0859bbd83e7592455972d543cc7395f6d4af639b1ea3f6a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 만약 이렇게 객체를 관계형 데이터베이스가 아닌 자바 컬렌션에 저장을 할 수 있다 가정하면 코드 한줄로 객체를 저장하고 조회가 가능하다.

**※ 객체와 관계형 데이터베이스의 연관관계를 바라보는 방법이 다르기 때문에 복잡한 변환과정이 필요하게 된다.**

*객체 그래프 탐색*

---

객체는 자유롭게 객체 그래프를 탐색할 수 있어야 한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/88783334-d5a7-40f8-8a33-a2af79a53182/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143533Z&X-Amz-Expires=86400&X-Amz-Signature=a6b89f07e49411c3da43f161fa2d762f2bb659d0a3443ae01bfbc1b1d7d0a8f7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Member.getTeam(), Member.getOrder(), Order.getDelivery() 등등.. 객체에서 객체로 다 따라갈 수 있어야 한다.

*처음 실행하는 SQL에 따라 탐색범위 결정*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b9745e55-e8f3-43ef-ba94-f859c84fbb4a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143541Z&X-Amz-Expires=86400&X-Amz-Signature=93b90e23999b67a9da43bbf88c63d7cf5944008f7fec3c7aaf6743360eefdfd9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 조회 쿼리 작성시 `Member`와 `Team` 객체만 조회를 하게되면 `Member` 객체에 `Order` 객체는 존재하지 않게 된다.

*엔티티 신뢰 문제*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7e08dc6f-2641-4cfe-9c29-827a4610d0da/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143550Z&X-Amz-Expires=86400&X-Amz-Signature=24af1dd2db97874dd08b4dec3511aad2b91c44d5fa316dfaf637a0dc633bdbee&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 내가 아닌 누군가 `memberDAO.find(id)`메서드를 짰을때 어떻게 짰는지 확인 해 봐야만 `membe.getTeam()`, `member.getOrder().getDelivery()` 메서드를 호출할 수 있다.

*모든 객체를 미리 로딩할 수는 없다.*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/55381d56-3d8b-43bc-98d7-b5c3e5acd220/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143601Z&X-Amz-Expires=86400&X-Amz-Signature=8b95b73e3c5394f94ec66305105ad0bfec0eb24e2611661a89a3d9361c7748a9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**※ 진정한 의미의 계층 분할이 어렵다.**

**물리적으로는 분할이 되어있으나 논리적으로는 엮여있다.**

*비교하기*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a1e3862f-b19c-46c2-87d9-9a07600a5ace/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143608Z&X-Amz-Expires=86400&X-Amz-Signature=4d5772719300f54155c5c6cc49e4ea822d736ce06e20bd0b0275726e96f63ad8&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `memberDAO.getMember(id)`를 실행시 보통 새로운 객체를 만들어 반환하기 때문에 같은 id로 조회를 한다고 해도 데이터는 같아도 인스턴스는 다르게 된다.

*비교하기 - 자바 컬렉션으로 조회*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/38662800-d93e-4b0e-a473-ed90805bb79f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230103T143616Z&X-Amz-Expires=86400&X-Amz-Signature=0f06a7c99133c9f6f723550323ea6501f9bd0413a9601912be657911bf201663&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- **객체가 컬렉션에 저장되어있다고 가정하고 컬렉션에서 객체를 같은 id로 조회시 데이터도 같고 객체도 같게 된다.**
- **객체답게 모델링 할수록 매핑 작업만 늘어난다.**

> **객체를 자바 컬렉션으로 저장 하듯이 DB에 저장할 수는 없을까?
이런 고민이 계속 있었고 그 문제를 해결하는게 바로 JPA이다.**
>

# JPA 소개

## JPA?

---

- Java Persistence API
- 자바 진영의 ORM 기술 표준

## ORM?

---

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

## JPA는 애플리케이션과 JDBC 사이에서 동작

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a65d56f2-0732-4c14-9cf8-c10cd88dfbe7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T150908Z&X-Amz-Expires=86400&X-Amz-Signature=6efd4b2fc5f5e7864a91a7a19a6f8d04e045fbadf900d4491dbcd262365bedeb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JAVA 애플리케이션에서 DB에 접근하려면 JDBC API를 통해서 접근해야하는데 기존에는 개발자가 직접 JDBC API 썼다면 JPA가 대신 해준다고 생각하면 된다.

## JPA 동작 - 저장

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/115b5f88-18a6-4140-bd21-c0d4f232cd06/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T150924Z&X-Amz-Expires=86400&X-Amz-Signature=98cf9e8f136bab4be3281ad0d25fcd20d71e92cd1acbc9971b70d8361a05b81a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- MemberDAO에서 JPA에 Member 객체를 저장하는 명령어를 내리면 JPA가 해당 Member 객체를 분석해서 INSERT SQL을 생성하고 JDBC API를 사용하여 DB에 INSERT SQL을 실행한다.
- 객체와 관계형 데이터베이스의 패러다임을 해결

## JPA 동작 - 조회

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1642816f-4e29-418c-b2ca-69c0667eb21a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T150935Z&X-Amz-Expires=86400&X-Amz-Signature=4e8d814788c7166815a58ae37c5f32450d64c4b4031b929ad2f5a4490dea70a9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- MemberDAO에서 JPA에 Member 객체를 조회하는 명령어를 내리면 JPA가 SELECT SQL을 생성하고 JDBC API를 사용하여 DB에 SELECT SQL을 실행하여 결과를 받으면 Member 객체에 맞게 ResultSet 매핑까지 다 해서 Member 객체를 반환해준다.
- 객체와 관계형 데이터베이스의 패러다임을 해결

## JPA왜 사용해야 하는가?

---

- SQL 중심적인 개발에서 객체 중심으로 개발
- 생산성
- 유지보수
- 패러다임의 불일치 해결
- 성능
- 데이터 접근 추상화와 벤더 독립성
- 표준

### 생산성 - JPA와 CRUD

---

- 저장: jpa.persist(member)
- 조회: Member member = jpa.find(memberId)
- 수정: member.setName(”변경할 이름”)
- 삭제: jpa.remove(member)

### 유지보수

---

**기존: 필드 변경시 모든 SQL 수정**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/70829e11-7b4b-4e0c-b090-b4540348c691/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151013Z&X-Amz-Expires=86400&X-Amz-Signature=d26bbd71cf0e984cda114aca6217042f690d7710731fbf840602f55cccde49bc&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**JPA: 필드만 추가하면 됨, SQL은 JPA가 처리**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4b4ad9fb-cee3-4547-be6a-0eeb1e4753c8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151020Z&X-Amz-Expires=86400&X-Amz-Signature=0a8601824770dcce40ed67a2865c058454d5e29732c13930c6bae1cef0ee6b91&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### JPA와 패러다임의 불일치 해결

---

1. JPA와 상속
2. JPA와 연관관계
3. JPA와 객체 그래프 탐색
4. JPA와 비교하기

**JPA와 상속**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ce9199dc-42ee-4a44-a6a1-47c811bd6af4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151031Z&X-Amz-Expires=86400&X-Amz-Signature=6d1c70f85c1aee200591b5663604c1d8a5d449860471a254a6b54247846f41cb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*저장*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3d29688b-3f74-45a7-9e29-2a958b580d76/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151043Z&X-Amz-Expires=86400&X-Amz-Signature=1b514f3c26dc740e89af9d2e648989f5e4a337796e1280f8e4f2b89f8aecc9de&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 기존의 방식이라면 Album 객체를 저장하려면 개발자가 직접 Item 데이터를 Insert하고 Album 데이터를 Insert해야하는데 `jpa.persist(album);`이라는 명령어만 사용하면 JPA가 알아서 처리를 해준다.

*조회*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/492277a6-54eb-4aee-a80f-87f0c2174b77/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151052Z&X-Amz-Expires=86400&X-Amz-Signature=dc59fcc188086d470a75096168cc9f949095d560036cb89cf53d1f42e5997061&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Album 객체를 조회하려면 Item 테이블도 Join하여 Item 객체도 조회도 조회해야하는데 `jpa.find(Album.class, albumId);`명령어로 JPA가 Select 쿼리를 생성해서 알아서 조회까지 해준다.

**JPA와 연관관계, 객체 그래프 탐색**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/231edaf1-39a7-469c-bff1-acda8b99056a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151101Z&X-Amz-Expires=86400&X-Amz-Signature=6dc575438aafbc7be55e5aced7a23445b5241fdbf207fca2dc3a8320be34d61c&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Member 객체에 Team 객체를 set해주고 `jpa.persist(member);`명령어를 실행해주면 Member 객체와 Team 객체가 DB에 Insert된다.
- Member 객체를 저장하고 `jpa.find(Member.class, memberId);`명령어로 Member 객체를 조회하면 Member 객체에 Team 객체가 set() 되어있는 Member 객체가 반환이 된다.

**신뢰할 수 있는 엔티티, 계층**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0ebe3e14-d3ae-4f11-ae9c-6fdcd6bf1029/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151111Z&X-Amz-Expires=86400&X-Amz-Signature=ae5d2b46b47a3fc5ee445ecc53cdd8b2df01fa76025378474355373346a2f417&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JPA를 통해 Member 객체를 조회하면 DB에 데이터가 있다는 존재하에 Member 객체의 연관관계에 의해서 `member.getTeam()`, `member.getOrder().getDelivery()`코드를 사용해 객체를 다 꺼낼 수 있다.
- 지연로딩을 사용

> 참고: JPA가 관리하는 객체를 엔티티라고 함
>

**JPA와 비교하기**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9510445c-e5a9-4e77-af47-d63463a5e606/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151120Z&X-Amz-Expires=86400&X-Amz-Signature=8033be793cd9478321a040be92eabbbd4a926d00e56c5c14dab9644d698b193e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- jpa를 통해 데이터를 가져왔을때 같은 데이터를 조회하게 되면 두 객체는 동일한 객체이다.
    - 단 같은 트랜잭션에서 조회한 객체만 보장해줌.

**JPA의 성능 최적화 기능**

---

1. 1차 캐시와 동일성(identity)보장
2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
3. 지연 로딩(Lazy Loading)

*1차 캐시와 동일성 보장*

---

1. 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
2. DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a25a6e60-62c8-439d-9330-dbb4d4e3d18d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151129Z&X-Amz-Expires=86400&X-Amz-Signature=09b82e9be9d63dad2af5f3623ecb383199ac5d8b73439a9862e0fd965020ec1b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. `jpa.find(Member.class, memberId);`가 처음 실행 될 때는 SQL을 실행하여 데이터를 가져온 후 메모리에 해당 객체를 올려놓는다.
2. 같은 트랜잭션 상에서 이전에 실행한 memberId로 `jpa.find(Member.class, memberId);`메소드가 호출 시 JPA가 SQL을 실행하지 않고 메모리에 올려놓은 객체를 반환하게 되면서 SQL을 1번만 실행하게 된다.

*트랜잭션을 지원하는 쓰기 지연 - INSERT*

---

1. 트랜잭션을 커밋하기 전까지 버퍼 메모리에 INSERT SQL을 모음
2. JDBC BATCH SQL 기능을 사용하여 트랜잭션을 커밋하기 전에 INSERT SQL을 한번에 전송

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/feb2a513-a628-4d76-b482-97669e4d27ea/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151149Z&X-Amz-Expires=86400&X-Amz-Signature=6855e9ae2dc959dd0db8ef88c1daa41f466d13922892d56efd277774d1d0996a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*지연 로딩과 즉시 로딩*

---

- 지연 로딩: 객체가 실제 사용될 때 로딩
- 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2a4d2c86-ec16-4ede-9071-9aea0145093d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151201Z&X-Amz-Expires=86400&X-Amz-Signature=1b3fb07c2567529dc6b00415ee662311ba157af3bae58c3aa0d0e3bf0e46270f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Member 객체를 사용하면서 Team 객체를 자주 사용안하면 지연 로딩으로 설정을 해줄 수 있게 제공하여 최적화를 가능하게 해준다.
