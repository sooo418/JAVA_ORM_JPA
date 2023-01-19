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

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4a3f6807-9c99-4a8c-adad-33100037a458/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143401Z&X-Amz-Expires=86400&X-Amz-Signature=2d6438d422d9125ae92b8bebb955ff45e1cb038359ac0213088684538dd08022&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*객체를 테이블에 맞추어 모델링*

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/da489530-e9c8-46b0-8f28-95f41ab619fa/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143431Z&X-Amz-Expires=86400&X-Amz-Signature=205458643dabf22c0a0f0410d303da4acf9ab05d1fbda659440db02617d81864&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

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

- 내가 아닌 누군가 `memberDAO.find(id)`메서드를 짰을때 어떻게 짰는지 확인 해 봐야만 `member.getTeam()`, `member.getOrder().getDelivery()` 메서드를 호출할 수 있다.

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

- Java Persistence API
- 자바 진영의 ORM 기술 표준

## ORM?

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

## JPA는 애플리케이션과 JDBC 사이에서 동작

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a65d56f2-0732-4c14-9cf8-c10cd88dfbe7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T150908Z&X-Amz-Expires=86400&X-Amz-Signature=6efd4b2fc5f5e7864a91a7a19a6f8d04e045fbadf900d4491dbcd262365bedeb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JAVA 애플리케이션에서 DB에 접근하려면 JDBC API를 통해서 접근해야하는데 기존에는 개발자가 직접 JDBC API를 썼다면 JPA가 대신 해준다고 생각하면 된다.

## JPA 동작 - 저장

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/115b5f88-18a6-4140-bd21-c0d4f232cd06/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T150924Z&X-Amz-Expires=86400&X-Amz-Signature=98cf9e8f136bab4be3281ad0d25fcd20d71e92cd1acbc9971b70d8361a05b81a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- MemberDAO에서 JPA에 Member 객체를 저장하는 명령어를 내리면 JPA가 해당 Member 객체를 분석해서 INSERT SQL을 생성하고 JDBC API를 사용하여 DB에 INSERT SQL을 실행한다.
- 객체와 관계형 데이터베이스의 패러다임을 해결

## JPA 동작 - 조회

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1642816f-4e29-418c-b2ca-69c0667eb21a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T150935Z&X-Amz-Expires=86400&X-Amz-Signature=4e8d814788c7166815a58ae37c5f32450d64c4b4031b929ad2f5a4490dea70a9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- MemberDAO에서 JPA에 Member 객체를 조회하는 명령어를 내리면 JPA가 SELECT SQL을 생성하고 JDBC API를 사용하여 DB에 SELECT SQL을 실행하여 결과를 받으면 Member 객체에 맞게 ResultSet 매핑까지 다 해서 Member 객체를 반환해준다.
- 객체와 관계형 데이터베이스의 패러다임을 해결

## JPA왜 사용해야 하는가?

- SQL 중심적인 개발에서 객체 중심으로 개발
- 생산성
- 유지보수
- 패러다임의 불일치 해결
- 성능
- 데이터 접근 추상화와 벤더 독립성
- 표준

### 생산성 - JPA와 CRUD

- 저장: jpa.persist(member)
- 조회: Member member = jpa.find(memberId)
- 수정: member.setName(”변경할 이름”)
- 삭제: jpa.remove(member)

### 유지보수

**기존: 필드 변경시 모든 SQL 수정**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/70829e11-7b4b-4e0c-b090-b4540348c691/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151013Z&X-Amz-Expires=86400&X-Amz-Signature=d26bbd71cf0e984cda114aca6217042f690d7710731fbf840602f55cccde49bc&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**JPA: 필드만 추가하면 됨, SQL은 JPA가 처리**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4b4ad9fb-cee3-4547-be6a-0eeb1e4753c8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230104T151020Z&X-Amz-Expires=86400&X-Amz-Signature=0a8601824770dcce40ed67a2865c058454d5e29732c13930c6bae1cef0ee6b91&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### JPA와 패러다임의 불일치 해결

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

**DB의 트랜잭션 격리 수준 (Isolation Level)**

- **READ UNCOMMITED**
- **READ COMMITED**
- **REPEATABLE READ**
- **SERIALIZABLE**

*READ UNCOMMITED*

---

READ UNCOMMITED 격리 수준에서 트랜잭션 간의 데이터 변경 내용이 COMMIT이나 ROLLBACK에 상관없이 공유가 된다.

1. 트랜잭션 A가 물품의 가격을 10,000원 → 5,000원 으로 변경
2. 트랜잭션 A가 아직 커밋되지 않음
3. 트랜잭션 B에서 해당 물품을 조회시 가격이 5,000원으로 조회됨
   1. 이를 더티 리드 (dirty read)라고 한다.
4. 트랜잭션 A에서 문제가 생겨 ROLLBACK이 됨
5. 트랜잭션 B에서는 그대로 5,000원으로 프로세스가 진행됨

**※ READ UNCOMMITED는 이렇게 데이터의 정합성에 문제가 많아 RDBS 표준에서는 트랜잭션 격리수준으로 인정되지 않는다.**

*READ COMMITED*

---

READ COMMITED 격리 수준에서는 트랜잭션 간의 데이터 변경 내용이 COMMIT이 되었을 때만 공유가 된다. → 오라클 DB에서 사용하는 격리 수준이다.

1. 트랜잭션 B가 물품을 조회함 → 물품 가격 10,000원
2. 트랜잭션 A가 물품의 가격을 10,000원 → 5,000원으로 변경
3. 트랜잭션 A가 커밋함
4. 트랜잭션 B에서 물품을 다시 조회하면 10,000원이 아닌 5,000원으로 조회

**※ READ COMMITED는 한 트랜잭션 내에세 같은 데이터를 조회했을 때 데이터는 항상 같아야 한다는 REPEATABLE READ의 정합성에 부합한다.**

*REPEATABLE READ*

---

REPEATABLE READ 격리 수준에서는 트랜잭션이 시작되기 전에 커밋된 내용만 조회할 수 있다.
→ MySQL에서 사용하는 격리 수준이다.
(모든 InnoDB의 트랜잭션은 고유한 트랜잭션 번호(순차적으로 증가)를 가지고 있으며, UNDO 영역에 백업된 모든 레코드는 변경을 발생시킨 트랜잭션의 번호를 가지고 있다.)

1. 트랜잭션 B가 물품을 조회함 → 물품 가격 10,000원
2. 트랜잭션 A가 물품의 가격을 10,000원 → 5,000원으로 변경
3. 트랜잭션 A가 물품의 가격을 변경하면서 실제 DB에 존재하는 데이터를 UPDATE해주고 기존의 데이터는 UNDO 영역에 백업함
4. 트랜잭션 B에서 다시 물품을 조회해도 데이터는 UNDO 영역에 있는 데이터를 조회하기 때문에 REPEATABLE READ 정합성의 어긋나지 않게 된다.
   1. 하지만 UPDATE 부정합이 발생하게 된다.
5. 트랜잭션 B에서 물품의 가격을 10,000원 → 12,000원으로 변경 시도
6. **트랜잭션 B에서 물품의 가격을 변경하기 위해 물품 데이터에 쓰기 잠금(LOCK)을 하려고 실제 DB 테이블에서 데이터를 찾는다. 하지만 해당 물품 데이터는 UNDO 영역에 존재하는 데이터라 찾을 수가 없어 변경된 건 없이 트랜잭션이 끝나고 만다.**
   1. **→ 해당 방식으로 인한 Phontom READ가 발생할 수 있다.**

**※ REPEATABLE READ는 REPEATABLE READ의 정합성은 만족하지만, UPDATE 정합성은 만족하지 못한다.**

**JPA에선 DB의 UNDO 영역을 영속성 컨텍스트의 1차 캐시로 사용해 어플리케이션 자체에서 REPEATABLE READ의 트랜잭션 격리 수준을 보장해주게 된다.**

*SERIALIZABLE*

---

SERIALIZABLE은 가장 단순하고 가장 엄격한 격리 수준이다.
InnoDB에서는 기본적인 SELECT 쿼리에 잠금을 걸지 않고 실행하는데, 격리수준이 SERIALIZABLE일 경우엔 읽기 작업에도 `공유 잠금`을 설정하고 다른 트랜잭션에서 `공유 잠금`이 설정되어 있는 데이터에 대해 변경이 불가능하게 된다.

**※ SERIALIZABLE의 특성 때문에 다른 격리 수준에 비해 동시 처리 능력이 떨어져 성능 저하가 발생한다.**

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

# JPA 시작

- Hello JPA - 프로젝트 생성
- Hello JPA - 애플리케이션 개발

## Hello JPA - 프로젝트 생성

- java 8 이상 (8 권장)
  - 11 사용
- 메이블 설정
  - groupId : jpa-basic
  - artifactId : ex1-hello-jpa
  - version : 1.0.0

*Maven Project 생성*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5de5875a-a12e-459c-9953-d514437457dc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143104Z&X-Amz-Expires=86400&X-Amz-Signature=ecb29790bfdc5e516ee1baf19c4c67a2a56de57265a64b32e0ae6c8a98f5627e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**라이브러리 추가 - pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>jpa-basic</groupId>
    <artifactId>ex1-hello-jpa</artifactId>
    <version>1.0.0</version>
    <dependencies>
        <!-- JPA 하이버네이트 -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>5.6.14.Final</version>
        </dependency>
        <!-- H2 데이터베이스 -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <version>2.1.214</version>
        </dependency>
    </dependencies>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>
</project>
```

- Hibernate를 사용하기 위해 `hibernate-entitymanager` dependency를 추가해준다.
  - 다른거 다 필요없이 해당 dependency만 추가하면 사용가능
- DBMS는 h2를 사용 설치한 h2버전을 맞춰서 사용해야함.

**JPA 설정하기 - persistence.xml**

---

- JPA 설정 파일
- /META-INF/persistence.xml 위치
- persistence-unit name으로 이름 지정
- javax.persistence로 시작: JPA 표준 속성
- hibernate로 시작: 하이버네이트 전용 속성

*persistence.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <persistence-unit name="hello">
        <properties>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>

            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.use_sql_comments" value="true"/>
            <!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
        </properties>
    </persistence-unit>
</persistence>
```

- `persistence version="2.2"`는 JPA의 2.2 버전을 사용한다는 의미이다.
- `hibernate.dialect`속성은 value값에 특정 DBMS의 Dialect으로 값을 세팅해주면 해당 DB의 SQL을 생성해준다.
  - H2 : org.hibernate.dialect.H2Dialect
  - Oracle 10g : org.hibernate.dialect.Oracle10gDialect
  - MySQL : org.hibernate.dialect.MySQL5InnoDBDialect
- 하이버네이트는 40가지 이상의 데이터베이스 방언을 지원해준다.

**데이터베이스 방언**

---

- JPA는 특정 데이터베이스에 종속되지 않는다.
- 각각의 데이터베이스가 제공하는  SQL 문법과 함수는 조금씩 다름
  - 가변 문자 : MySQL은 VARCHAR, Oracle은 VARCHAR2
  - 문자열을 자르는 함수 : SQL 표준은 SUBSTRING(), Oracle은 SUBSTR()
- 페이징 : MySQL은 LIMIT, Oracle은 ROWNUM
- 방언 : SQL 표준을 지키지 않는 특정 데이터베이스만 고유한 기능

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/937ed801-8c67-46a8-b36b-d295807f5b08/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143128Z&X-Amz-Expires=86400&X-Amz-Signature=22b9a98c7d2afb075c1d1bb31fd5522dd789b5311d74a51eb78e28b30794025a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

## Hello JPA - 애플리케이션 개발

**JPA 구동 방식**

---

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c690d6a7-f92f-4420-a07c-e7b0f11d6660/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143138Z&X-Amz-Expires=86400&X-Amz-Signature=982accaea577da654833d37bbd80948518a56b56055f5f276f7a9ee16ac62b10&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. 먼저 Persistence가 persistence.xml에 설정 정보를 읽는다.
2. Persistence.xml을 읽어서 EntityManagerFactory를 만든다.
3. EntityManagerFactory라는 공장을 통해서 필요한 EntityManager를 생성해준다.

### **실습 - JPA 동작 확인**

- JpaMain 클래스 생성
- JPA 동작 확인

**객체와 테이블을 생성하고 매핑하기**

---

- `@Entity`: JPA가 관리할 객체
- `@Id`: 데이터베이스 PK와 매핑

```java
package hellojpa;

import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Member {

    @Id
    private Long id;
    private String name;

    //Getter, Setter...
}
```

```sql
create table Member (
	 id bigint not null,
	 name varchar(255),
	 primary key (id)
);
```

### **실습 - 회원 저장**

**회원 등록**

*JpaMain 클래스 생성*

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();
        
        try {
            Member member = new Member();
            member.setId(1L);
            member.setName("HelloA");

            em.persist(member);
            
            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }
        
        emf.close();
    }
}
```

- `EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");`
  - JPA는 항상 `EntityManagerFactory`라는 걸 만들어줘야 한다.
  - `Persistence.createEntityManagerFactory("hello");`는 `persistence.xml`파일의 `persistence-unit name="hello">` 설정 정보를 불러와서 `EntityManagerFactory`를 만들어준다.
- JPA는 트랜잭션이 꼭 실행되어야만 작동하기 때문에 `tx.begin();`이렇게 꼭 트랜잭션을 시작해주고 JPA를 호출해야한다.

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/dffbe2ca-5226-4b2d-9ce9-38e5c1469aaa/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143202Z&X-Amz-Expires=86400&X-Amz-Signature=8c4859e797c52258e47c0698ef94deb06a68e7eb3136535f38391c310911f0c4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 실행을 해보면 이렇게 실행된 쿼리가 로그에 찍힌다.
- 실행된 쿼리가 로그에 표시된 이유는 `persistence.xml`에서 hibernate속성들을 설정해줬기 때문이다.
  - `<property name="hibernate.show_sql" value="true"/>`
    - 실행 쿼리를 로그로 출력해줌
  - `<property name="hibernate.format_sql" value="true"/>`
    - 쿼리를 보기 편하게 개행이나 들여쓰기를 해줌
  - `<property name="hibernate.use_sql_comments" value="true"/>`
    - 쿼리 앞에 주석으로 쿼리가 왜 출력됐는지 코멘트를 남겨줌

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8ac3172f-f159-4e72-b727-de62a3e4b31f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143214Z&X-Amz-Expires=86400&X-Amz-Signature=44e309a58d3d155322c807ea49a4a19d87827265fbd1cbdd8f5ed0b9e7f965d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- h2 콘솔에서 조회 쿼리를 실행해보면 데이터가 조회되는게 확인됨.

> Member 객체를 보면 테이블 명을 지정해주지 않아도 알아서 MEMBER 테이블에 데이터가 적재되는데, 테이블을 지정해주지 않아도 관례를 따라 MEMBER 테이블에 적재가 된다.
엔티티 명과 테이블 명이 다를 경우 `@Table()`을 애노테이션을 사용하여 지정해줄 수 있고, 컬럼 명도  `@Column()`을 사용해 컬럼명도 지정해줄 수 있다.
>

**회원 조회**

*JpaMain*

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {

            Member findMember = em.find(Member.class, 1L);
            System.out.println("findMember.id = " + findMember.getId());
            System.out.println("findMember.name = " + findMember.getName());

            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }

        emf.close();
    }
}
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e329c33d-f368-409d-be78-59ec79d2d044/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143228Z&X-Amz-Expires=86400&X-Amz-Signature=d4d4485a16d2ebace4c1873e09ce4054c35ebcf82c939043e5d50cd0890ffd7e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**회원 삭제**

*JpaMain*

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {

            Member findMember = em.find(Member.class, 1L);
            
            em.remove(findMember);
            
            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }

        emf.close();
    }
}
```

**회원 수정**

*JpaMain*

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {

            Member findMember = em.find(Member.class, 1L);
            findMember.setName("HelloJPA");

            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }

        emf.close();
    }
}
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a0bfac9f-9676-4929-b5aa-3d4f973802d2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143243Z&X-Amz-Expires=86400&X-Amz-Signature=982c95a950f18f7bff80ff8e4c7b50b4c54b7fe6ced055a6d239b9b6c10e3bb7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

find()한 Member객체의 name값을 변경만 해주었는데 Update 쿼리가 실행된게 확인된다.

이유는

→ JPA를 통해서 Entity를 가져오게 되면 가져온 객체는 JPA가 관리를 하게 된다.

→ JPA가 관리중인 객체의 데이터가 바뀌었는지 체크를 하고 바뀐 데이터가 있을 경우, Update 쿼리를 만들어서 날리고 트랙잭션이 커밋이 된다.

**주의**

---

- `EntityManagerFactory`는 하나만 생성해서 애플리케이션 전체에서 공유해야 한다.
- `EntityManager`는 쓰레드간에 공유하면 안된다. (사용하고 버려야한다.)
- **JPA의 모든 데이터 변경은 트랜잭션 안에서만 실행되어야 한다.**
  - DB는 내부적으로 트랜잭션 개념을 가지고 있다.

### JPQL 소개

- 가장 단순한 조회
  - `EntityManager.find()`
  - 객체 그래프 탐색(a.getB().getC())
- 나이가 18살 이상인 회원을 모두 검색하고 싶다면?
  - JPQL이 해당 방법을 지원해줌

**실습 - JPQL 소개**

---

*JPQL로 전체 회원 검색*

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;
import java.util.List;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {

            List<Member> result = em.createQuery("select m from Member as m", Member.class)
                    .getResultList();

            for (Member member : result) {
                System.out.println("member.name = " + member.getName());
            }

            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }

        emf.close();
    }
}
```

- `em.createQuery("select m from Member as m", Member.class)`를 보면 `“select m from Member as m"`을 JPQL이라 한다.
- **JPA 입장에서 코드를 짤 때 테이블을 대상으로 절대 코드를 짜지 않고 객체를 대상으로 짠다.**

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0e43ca90-fefe-4e9b-921c-4b80a44bef10/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143258Z&X-Amz-Expires=86400&X-Amz-Signature=6c7250776e3efe844dc95ddce7f40529dca11cf660b9db175e8aea59debb66ce&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

*페이징 조회*

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;
import java.util.List;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {

            List<Member> result = em.createQuery("select m from Member as m", Member.class)
                    .setFirstResult(0)
                    .setMaxResults(10)
                    .getResultList();

            for (Member member : result) {
                System.out.println("member.name = " + member.getName());
            }

            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }

        emf.close();
    }
}
```

- `setFirstReulst(0).setMaxResult(10)`메소드를 사용 하면 페이징 처리가 가능해진다.
  - 0번째부터 시작하여 10개를 조회한다.


*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c858b91e-384a-46fa-aa50-691945e1efda/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230105T143314Z&X-Amz-Expires=86400&X-Amz-Signature=12ddaa9ce2f8711620fb3089a8a60cb672090567dee1bdcbd097abf349ec1b98&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**JPQL**

---

- JPA를 사용하면 엔티티 객체를 중심으로 개발
- 문제는 검색 쿼리
- 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색
- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능
- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요
- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공
- SQL과 문법 유사, SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 지원
- **JPQL은 엔티티 객체를 대상으로 쿼리**
- SQL은 데이터베이스 테이블을 대상으로 쿼리
- 테이블이 아닌 **객체를 대상으로 검색하는 객체 지향 쿼리**
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다.
- JPQL을 한마디로 정의하면 객체 지향 SQL
# 영속성 관리

## 영속성 컨텍스트

**JPA에서 가장 중요한 2가지**

- 객체와 관계형 데이터베이스 매핑하기(Object Relational Mapping)
- 영속성 컨텍스트

**엔티티 매니저 팩토리와 엔티티 매니저**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/96c871c0-f7de-41f3-a46a-077706ec7e1a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131012Z&X-Amz-Expires=86400&X-Amz-Signature=ba8fca401ed1246ce0ab26abfb43b77ec7253d94587d1899086f9653c7101a55&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 웹 어플리케이션의 서버에 고객의 요청이 들어오면 `EntityManagerFactory`에서 `EntityManager` 객체를 생성해준다. 또 새로운 고객의 요청이 들어오면 `EntityManagerFactory`에서 또 다른 `EntityManager`객체를 생성해준다.

### **영속성 컨텍스트**

- JPA를 이해하는데 가장 중요한 용어
- “엔티티를 영구 저장하는 환경”이라는 뜻
- `EntityManager.persist(entity);`
  - `Entity`를 DB에 저장한다는 의미가 아닌 영속성 컨텍스트를 통해서 `Entity`를 영속화 한다는 의미를 가진다.

**엔티티 매니저?**

**영속성 컨텍스트?**

- 영속성 컨텍스트는 논리적인 개념
- 눈에 보이지 않는다.
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근

**J2SE 환경**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cd3efce6-f21a-4aab-8fd1-6bf3417849c3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131035Z&X-Amz-Expires=86400&X-Amz-Signature=188ba8fb25f08cf37fc07c5b2000f85f1a2ae8f2499e6d9e62de02b9e18539ee&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 엔티티 매니저 안에 영속성 컨텍스트라는 눈에 보이지 않는 공간이 생긴다고 생각하면 된다.

### **엔티티의 생명주기**

- **비영속 (new/transient)**

  영속성 컨텍스트와 전혀 관계가 없는 **새로운** 상태

- **영속 (managed)**
  - `**EntityManager.persist(entity);`명령어가 실행되면 entity 객체가 영속상태가 된다.**

  영속성 컨텍스트에 **관리**되는 상태

- **준영속 (detached)**

  영속성 컨텍스트에 저장되었다가 **분리**된 상태

- **삭제 (removed)**

  **삭제**된 상태


![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4240bfea-2c3c-4bb3-9d1e-4260f7e882a7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131101Z&X-Amz-Expires=86400&X-Amz-Signature=f382478d346e51f30ba05d1e5c02891ec01bbb5da06834a39952ad830e43afd8&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

---

**비영속**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1562ea2c-a367-4f9e-bb29-4aabf04e6ac5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131112Z&X-Amz-Expires=86400&X-Amz-Signature=9037a1457f2d3c6975829e4e1c0c19ef2c86ea1e9cc6f53b831c0f8f5ab845f0&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

```java
//객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
```

**영속**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d94557ed-ccff-4e4c-96f5-4eb99a033f40/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131122Z&X-Amz-Expires=86400&X-Amz-Signature=5da27ce360a2974bf5fd97ccb8681ff189b9e80b40cc02118f4b9042e79cca8c&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

```java
//객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");
member.setUsername(“회원1”);

EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

//객체를 저장한 상태(영속)
em.persist(member);
```

- `persist(entity);`가 실행된다고 바로 DB에 entity 데이터가 저장되는건 아니다.
  - `pertsist(entity);`는 entity 객체를 영속 상태로 만들어 주지만, DB에 저장하는 명령어는 아니다.

*확인해보기*

```java
//비영속
Member member = new Member();
member.setId(1L);
member.setName("HelloJPA");

//영속
System.out.println("=== BEFORE ===");
em.persist(member);
System.out.println("=== AFTER ===");

tx.commit();
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6356a498-3a68-4f0d-97c1-7b205dbbb0d8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131138Z&X-Amz-Expires=86400&X-Amz-Signature=0870ae0ff7261ffeede3f16afeee4e904a2028198d49f5b9ecf8ef154ad043c9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Insert 쿼리가 ‘=== AFTER ===’ 로그가 찍히고 실행되는게 확인이 된다.
  - **Insert 쿼리는 `tx.commit();`이 명령어가 실행되면서 트랜잭션이 커밋되기 직전에 Insert 쿼리가 실행이 된다.**

**준영속, 삭제**

```java
//회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);
```

- 영속 상태에서 `em.detach(member);`를 실행하면 비영속 상태가 된다.

```java
//객체를 삭제한 상태(삭제)
em.remove(member);
```

- 실제 DB에서 데이터를 지운다는 명령어다.

### **영속성 컨텍스트의 이점**

- 1차 캐시
- 동일성(identity) 보장
- 트랜잭셕을 지원하는 쓰기 지원
  (transactional write-behind)
- 변경 감지
  (Dirty Checking)
- 지연 로딩
  (Lazy Loading)

**엔티티 조회, 1차 캐시**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2b4efecc-a62d-4083-8bb3-2b087ac8702f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131151Z&X-Amz-Expires=86400&X-Amz-Signature=7d1a1a47a88ef8df36232e760f9a9194575c83eab583d44721e7eee498048b18&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

```java
//엔티티를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");

//엔티티를 영속
em.persist(member);
```

- 1차 캐시를 영속성 컨텍스트라고 이해해도 된다.
- 1차 캐시에 Entity를 저장할 때는 Map형식으로 key가 Id값인 `member1` value는 `member` 객체 자신이 세팅되어 저장이 된다.

**1차 캐시에서 조회**

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");

//1차 캐시에 저장됨
em.persist(member);

//1차 캐시에서 조회
Member findMember = em.find(Member.class, "member1");
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/26c9bd60-0c44-4832-b6d9-2aa1d40019d4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131207Z&X-Amz-Expires=86400&X-Amz-Signature=c9c6c5e72bfea22c5d9e3ba90ba9a6a0802781f5ae521a38140cc7ae21c80b76&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `member`객체를 영속성 컨텍스트 즉 1차 캐시에 저장이 되면 `em.find(Member.class, "member1");`명령어 실행시 먼저 1차 캐시에 저장된 객체를 조회한다.

**데이터베이스에서 조회**

`Member findMember2 = em.find(Member.class, "member2");`

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f50d68c3-5be3-4789-8a5e-2c5d139612e0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131218Z&X-Amz-Expires=86400&X-Amz-Signature=b8061f15a980d5cf201836b2cad1c5f5d34af4b362bfdaa157a7c86ecc59a1f9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. `em.find(Member.class, "member2");`명령어가 실행이 되면 member2 객체를 먼저 1차 캐시에서 조회를 하게 된다.
2. 1차 캐시에서 조회가 안되기 때문에 결국에는 SELECT SQL을 생성해 DB에 접근해 member2 객체를 조회한다.
3. 조회한 member2 객체를 1차 캐시에 저장한다.
4. 1차 캐시에 저장된 member2 객체를 반환한다.

> 참고: 영속성 컨텍스트의 1차 캐시의 성능의 이점은 굉장히 미미하다.
→ 트랜잭션이 끝나면 1차 캐시도 날라가기 때문에 사실상 같은 트랜잭션상에서 같은 데이터를 조회하는 상황 자주 일어나지 않는다.
>

*1차 캐시 확인해보기*

```java
//비영속
Member member = new Member();
member.setId(101L);
member.setName("HelloJPA");

//영속
System.out.println("=== BEFORE ===");
em.persist(member);
System.out.println("=== AFTER ===");

Member findMember = em.find(Member.class, 101L);

System.out.println("findMember.id = " + findMember.getId());
System.out.println("findMember.name = " + findMember.getName());

tx.commit();
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/60a1d574-c0eb-44f1-8c58-25cdb17667e3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131231Z&X-Amz-Expires=86400&X-Amz-Signature=96eb4850538829c4da5d50930bb0bcd69f7d191f75532710f7dbe93316cbc83a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 분명히 `em.find(Member.class, 101L);`명령어를 실행했는데 SELECT SQL 로그가 찍히지 않았다.
  → `em.find(Member.class, 101L);`를 하기 앞서 `em.persist(member);`가 먼저 실행이 되서 member 객체가 1차 캐시에 올라가서 `em.find(Member.class, 101L);`가 실행될 때 SQL문으로 DB에 접근하지 않고 1차 캐시에서 key값이 ‘101L’인 member 객체를 조회해서 값을 받게 된다.

**영속 엔티티의 동일성 보장**

```java
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");

System.out.println(a == b); //동일성 비교 true
```

- 1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭션 격리 수준을 데이터베이스가 아닌 애플리케이션 차원에서 제공 → 사용하는 DB의 Isolation Level이 `COMMITED READ` 여도 `REPEATABLE READ`트랜잭션 격리 수준을 갖는다.

**엔티티 등록**

**트랜잭션을 지원하는 쓰기 지연**

```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
//엔티티 매니저는 데이터 변경시 트랜잭션을 시작해야 한다.
transaction.begin(); // [트랜잭션] 시작

em.persist(memberA);
em.persist(memberB);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

//커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```

*동작 그림*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/424a43c7-18b1-4640-9e7a-2d8071bd25ee/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131247Z&X-Amz-Expires=86400&X-Amz-Signature=ad6f30c84aa22bfbbf931b0b98129041a1182faaf0ed29efc576a13bcba1f5b0&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `em.persist(memberA);`명령어가 실행이 되면 영속성 컨텍스트의 1차 캐시에 memberA 객체가 저장되고 동시에 memberA 객체를 분석해 Insert SQL을 생성해 쓰기 지연 SQL 저장소에 저장을 한다.
- `em.persist(memberB);`명령어도 마찬가지로 memberB 객체가 1차 캐시에 저장되고 Insert SQL을 생성해서 쓰기 지연 SQL 저장소에 차곡차곡 쌓게 된다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0f6527f0-1a2a-4562-a759-d4ab65f0005c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131258Z&X-Amz-Expires=86400&X-Amz-Signature=5a81f8e052e383570b58356329cf593392425c3544b5a5b15ff6c5cd7fe9cf8b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `transaction.commit();`명령어가 실행되면 데이터베이스를 커밋하기 직전에
  `**flush`명령어를 날려서 쓰기 지연 SQL 저장소에 저장되있던 SQL 문들을 실행해주고 데이터베이스가 커밋이 된다.**
  - `flush`는 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화하는 작업인데 이때 등록, 수정, 삭제한 엔티티를 데이터베이스에 반영한다.

*확인해보기*

```java
Member member1 = new Member(150L, "A");
Member member2 = new Member(160L, "B");

em.persist(member1);
em.persist(member2);

System.out.println("==================================");

tx.commit();
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b58726ce-662b-48fd-b5ce-734409244192/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131311Z&X-Amz-Expires=86400&X-Amz-Signature=3edb9e185d476f2c2424ad6979d8860e9872db47faa668713bc26576f9711e33&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- “===========================” 로그가 찍히고 Insert 쿼리 2개가 로그에 찍힌게 확인이 된다.

*버퍼링 기능*

- `persistence.xml`파일에서 하이버네이트의 옵션 `<property name="hibernate.jdbc.batch_size" value="10"/>`을 사용하면 10의 사이즈만큼 쿼리를 모았다가 한 번에 전달할 수 있다.

> `em.persist(member1);`나 `em.persist(member2);`가 실행되자마자 즉시 등록 쿼리를 데이터베이스에 보내나 등록쿼리를 메모리에 모아놓고 트랜잭션이 커밋되기 직전에 데이터베이스에 보내나 트랜잭션 범위 안에서 실행되므로 둘의 결과는 같다.
member1, member2 모두 트랜잭션을 커밋하면 함께 저장되고 롤백하면 함께 저장되지 않는다. 등록 쿼리를 즉각적으로 데이터베이스에 전달해도 트랜잭션을 커밋하지 않으면 아무 소용이 없다. 어떻게든 커밋 직전에만 데이터베이스에 SQL을 전달하면 된다. 이것이 트랙잭션을 지원하는 쓰기 지연이 가능한 이유다.
**※ 이 기능을 잘 활용하면 모아둔 등록 쿼리를 데이터베이스에 한 번에 전달해서 성능을 최적화할 수 있다.**
>

**변경 감지**

```java
Member member = em.find(Member.class, 150L);
/* member id:150
	 member name:'A' */

member.setName("ZZZZZ");
/* member id:150
	 member name:'ZZZZZ' */

System.out.println("==================================");

tx.commit();
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d5a4f0c9-a5c6-4dda-937c-7465e9169c04/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131326Z&X-Amz-Expires=86400&X-Amz-Signature=b43c1c004d7e00e296272c5d8576c5b535dc7b6bacdb5ce65ebd9a09edc346e1&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- member 객체를 조회하는 SELECT 쿼리가 실행이되고 member 객체의 UPDATE 쿼리가 실행이 된다.
- 코드를 보면 `em.update();`라는 명령어가 실행되어야 할 것 같지만 이런 메소드는 없다.
- **변경 감지(dirty checking)**라는 기능 덕에 이렇게 DB에 UPDATE해주는 명령어가 없어도 변경사항을 데이터베이스에 자동으로 반영해준다.

*변경 감지 - 동작 방식 그림*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f54fe2a5-e390-4dbc-97ad-507dcbde5eba/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230106T131337Z&X-Amz-Expires=86400&X-Amz-Signature=3e9f06344e6c5b31e97a8c3dfb463f2c56eeeb25b705973c69f9a0159fe52160&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JPA는 엔티티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 저장해두는데 이것을 스냅샷이라 한다.
- 플러시 시전에 스냅샷과 엔티티를 비교해서 변경된 엔티티를 찾는다.

1. 트랜잭션을 커밋하면 `EntityManager` 내부에서 먼저 플러시(`flush()`)가 호출된다.
2. 엔티티와 스냅샷을 비교해서 변경된 엔티티를 찾는다.
3. 변경된 엔티티가 있으면 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 보낸다.
4. 쓰기 지연 저장소의 SQL을 데이터베이스에 보낸다.
5. 데이터베이스 트랜잭션을 커밋한다.

**엔티티 삭제**

```java
//삭제 대상 엔티티 조회
Member memberA = em.find(Member.class, “memberA");
em.remove(memberA); //엔티티 삭제
```

- DELETE SQL을 쓰기 지연 저장소의 저장하고 트랜잭션을 커밋하기 직전에 DB에 전달한다.
## 플러시

- 영속성 컨텍스트의 변경내용을 데이터베이스에 반영

**플러시 발생**

- 변경감지
- 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
- 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송(등록, 수정, 삭제 쿼리)

**영속성 컨텍스트를 플러시하는 방법**

- em.flush() - 직접 호출
- 트랜잭션 커밋 - 플러시 자동 호출
- JPQL 쿼리 실행 - 플러시 자동 호출

**JPQL 쿼리 실행시 플러시가 자동으로 호출되는 이유**

```java
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);

//중간에 JPQL 실행
query = em.createQuery("select m from Member m", Member.class);
List<Member> members= query.getResultList();
```

- JPQL을 실행되지 전에 memberA, memberB, memberC 객체들은 DB에 Insert 쿼리가 실행되지 않았기 때문에 DB에 저장된 데이터들이 아니다.
- **이때 JPQL일 실행되면 JPQL은 SQL로 변역이 되어 DB에 실행**되기 때문에 memberA, memberB, memberC 객체는 조회가 안되게 된다.
- 이런 현상을 방지하고자 JPQL 쿼리가 실행되면 SQL을 실행하기 전에 무조건 플러시를 호출하게 된다.

**플러시 모드 옵션**

`em.setFlushMode(FlushModeType.COMMIT)`

- FlushModeType.AUTO
  커밋이나 쿼리를 실행할 때 플러시(기본값)
- FlushModeType.COMMIT
  커밋할 때만 플러시

> 참고: 플러시 모드 옵션은 사실 알고 있을 필요는 없지만 FlushModeType.COMMIT이 가끔 사용되는 일이 있어서 이런게 있구나 라고만 알고 있으면 된다.
>

*FlushModeType.COMMIT 이 사용되는 사례*

```java
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);

//중간에 JPQL 실행
query = em.createQuery("select o from Order o", Order.class);
List<Order> orders= query.getResultList();
```

- 위의 코드처럼 JPQL이 실행될 때 Member 객체를 조회하는 JPQL이 아니라면 memberA, memberB, memberC 객체를 DB에 저장해놓아야 할 이유가 없기 때문에 가끔 사용이 된다.

**플러시는!**

- 영속성 컨텍스트를 비우지 않음
- 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화
- 트랜잭션이라는 작업 단위가 중요 → 커밋 직전에만 동기화하면 됨

## 준영속 상태

- 영속 → 준영속
- 영속 상태의 엔티티가 영속성 컨텍스트에서 분리(detached)
- 영속성 컨텍스트가 제공하는 기능을 사용 못함

**준영속 상태로 만드는 방법**

- em.detach(entity)
  특성 엔티티만 준영속 상태로 전환
- em.clear()
  영속성 컨텍스트를 완전히 초기화
- em.close()
  영속성 컨텍스트를 종료

# 엔티티 매핑

**목차**

---

- 객체와 테이블 매핑
- 데이터베이스 스키마 자동 생성
- 필드와 컬럼 매핑
- 기본 키 매핑
- 실전 예제 - 1. 요구사항 분석과 기본 매핑

**엔티티 매핑 소개**

---

- 객체와 테이블 매핑: `@Entity`, `@Table`
- 필드와 컬럼 매핑: `@Column`
- 기본 키 매핑: `@Id`
- 연관관계 매핑: `@ManyToOne`, `@JoinColumn`

## 객체와 테이블 매핑

**@Entity**

---

- `@Entity`가 붙은 클래스는 JPA가 관리, 엔티티라 한다.
- JPA를 사용해서 테이블과 매핑할 클래스는 `@Entity` 필수
- 주의
  - 기본 생성자 필수(파라미터가 없는 public 또는 protected 생성자)
  - final 클래스, enum, interface, inner 클래스 사용 X
  - 저장할 필드에 final 사용 X

**@Entity 속성 정리**

---

- 속성: name
  - JPA에서 사용할 엔티티 이름을 지정한다.
  - 기본값: 클래스 이름을 그대로 사용(예: Member)
  - 같은 클래스 이름이 없으면 가급적 기본값을 사용한다.


**@Table**

---

- `@Table`은 엔티티와 매핑할 테이블 지정

| 속성 | 기능 | 기본값 |
| --- | --- | --- |
| name | 매핑할 테이블 이름 | 엔티티 이름을 사용 |
| catalog | 데이터베이스 catalog 매핑 |  |
| schema | 데이터베이스 schema 매핑 |  |
| uniqueConstraints(DDL) | DDL 생성 시에 유니크 제약 조건 생성 |  |

**데이터베이스 스키마 자동 생성**

---

- DDL을 애플리케이션 실행 시점에 자동 생성
- 테이블 중심 → 객체 중심
- 데이터베이스 방언을 활용해서 데이터베이스에 맞는 적절한 DDL 생성
- 이렇게 **생성된 DDL은 개발 장비에서만 사용**
- 생성된 DDL은 운영서버에서는 사용하지 않거나, 적절히 다듬은 후 사용

**데이터베이스 스키마 자동 생성 - 속성**

---

`hibernate.hbm2ddl.auto`

| 옵션 | 설명 |
| --- | --- |
| create | 기존 테이블 삭제 후 다시 생성 (DROP + CREATE) |
| create-drop | create와 같으나 종료 시점에 테이블 DROP |
| update | 변경분만 반영(운영 DB에는 사용하면 안됨) |
| validate | 엔티티와 테이블이 정상 매핑되었는지만 확인 |
| none | 사용하지 않음 |

*persistence.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <persistence-unit name="hello">
        <properties>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>

            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.use_sql_comments" value="true"/>
            <property name="hibernate.jdbc.batch_size" value="10"/>
            <property name="hibernate.hbm2ddl.auto" value="create" />
        </properties>
    </persistence-unit>
</persistence>
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0423eb47-f02f-4890-a29c-55428aca54bd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230108T085427Z&X-Amz-Expires=86400&X-Amz-Signature=a0e53906d2144085750860caa4901170d8b0cc9498a5fb61ab721bb8e7601c5a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Member 테이블을 DROP하고 CREATE 한다.

*create-drop*

- `<property name="hibernate.hbm2ddl.auto" value="create-drop"/>`
  - create와 같이 DROP - CREATE를 실행한 후 애플리케이션이 종료되는 시점에 DROP을 실행

*update*

- `<property name="hibernate.hbm2ddl.auto" value="update"/>`
  - 기존에 있는 테이블 정보와 받은 객체의 정보를 비교해 ALTER 처리해줌
  - 추가하는건 되지만 지우는건 불가능

*validate*

- `<property name="hibernate.hbm2ddl.auto" value="validate"/>`
  - 기존에 있는 테이블 정보와 받은 객체의 정보를 비교하여 같을 경우에만 애플리케이션이 실행됨

*none*

- `<property name="hibernate.hbm2ddl.auto" value="none"/>`
  - `hibernate.hbm2ddl.auto` 옵션을 제거하거나 value에 none을 넣어서 아무것도 실행안하게 할 수 있다.
  - 관례상 none이라는 값을 사용하지 아무렇게나 입력해도 결과는 같다.

**데이터베이스 스키마 자동 생성 - 주의**

---

- **운영 장비에는 절대 create, create-drop, update 사용하면 안됨**
- 개발 초기 단계는 create 또는 update
- 테스트 서버는 update 또는 validate
- 스테이징과 운영 서버는 validate 또는 none

**DDL 생성 기능**

---

- 제약조건 추가: 회원 이름은 **필수**, 10자 초과 X
  - `@Column(nullable = false, length = 10)`
- 유니크 제약조건 추가
  - `@Table(uniqueConstraints = {@UniqueConstraint( name = "NAME_AGE_UNIQUE", columnNames = {"NAME", "AGE"} )})`
- DDL 생성 기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.

## 필드와 컬럼 매핑

### 매핑 어노테이션 정리

| 어노테이션 | 설명 |
| --- | --- |
| @Column | 컬럼 매핑 |
| @Temporal | 날짜 타입 매핑 |
| @Enumerated | enum 타입 매핑 |
| @Lob | BLOB, CLOB 매핑 |
| @Transient | 특정 필드를 컬럼에 매핑하지 않음(무시) |

**@Column**

---

| 속성 | 설명 | 기본값 |
| --- | --- | --- |
| name | 필드와 매핑할 테이블의 컬럼 이름 | 객체의 필드 이름 |
| insertable, updatable | 등록, 변경 가능 여부 | TRUE |
| nullable(DDL) | null 값의 허용 여부를 설정한다. false로 설정하면 DDL 생성 시에 not null 제약조건이 붙는다. |  |
| unique(DDL) | @Table의 uniqueConstraints와 같지만 한 컬럼에 간단히 유니크 제약조건을 걸 때 사용한다. |  |
| columnDefinition(DDL) | 데이터베이스 컬럼 정보를 직접 줄 수 있다.
ex) varchar(100) default ‘EMPTY’ | 필드와 자바 타입과 방언 정보를 사용 |
| length(DDL) | 문자 길이 제약조건, String 타입에만 사용한다. | 255 |
| precision,
scale(DDL) | BigDecimal 타입에서 사용한다(BigInteger도 사용할 수 있다).
precision은 소수점을 포함한 전체 자릿수를, scale은 소수의 자릿수다. 참고 double, float 타입에는 적용되지 않는다. 아주 큰 숫자나 정밀한 소수를 다루어야 할 때만 사용한다. | precision = 19,
scale = 2 |
- JPA를 통해서 특정 컬럼에 대하여 등록, 변경을 하고싶지 않다면 `insertable = false`, `updatable = false`를 사용하면 된다.
- `unique = true`로 사용할 수 있는데 유니크 제약조건명이 알아보기 힘든 이름으로 생성되어 운영에서는 사용하지 않는다.
  - @Table의 uniqueConstraints를 사용하면 제약조건명을 직접 입력하여 제약조건을 걸 수 있다.
- `columnDefinition = "varchar(100) default 'EMPTY'"`로 사용할 수 있는데 사용하는 DB에 맞춰 방언 정보를 사용해야 한다.
- `length = 10`으로 사용

**@Enumerated**

---

- 자바 enum 타입을 매핑할 때 사용

**주의! ORDINAL 사용 X**

| 속성 | 설명 | 기본값 |
| --- | --- | --- |
| value | - EnumType.ORDINAL: enum 순서 데이터베이스에 저장
- EnumType.STRING: enum 이름을 데이터베이스에 저장 | EnumType.ORDINAL |

*Member*

```java
@Entity
public class Member {
    @Id
    private Long id;

    @Column(name = "name")
    private String username;

    private Integer age;    //Integer로 타입을 정해주면 DB에 적절한 숫자 타입으로 적용시켜준다.

    @Enumerated
    private RoleType roleType;

    @Temporal(TemporalType.TIMESTAMP)
    private Date createdDate;

    @Temporal(TemporalType.TIMESTAMP)
    private Date lastModifiedDate;

    @Lob
    private String description;

    public Member() {
    }
    //Getter, Setter ...
}
```

- `@Enumerated`로 사용 (기본값으로 EnumType.ORDINAL 사용)

*RoleType*

```java
public enum RoleType {
    USER, ADMIN
}
```

*JpaMain*

```java

Member member = new Member();
member.setId(1L);
member.setRoleType(RoleType.USER);

em.persist(member);

tx.commit();
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fd1ff061-b9fd-4bac-ba2a-1f809eae64da/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140123Z&X-Amz-Expires=86400&X-Amz-Signature=5150041234223c3fa59a54adfed055bd78c8aedc0a2d9f072b3595a8e89ff95b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- ROLETYPE 컬럼에 0이 들어간게 확인된다. (순서로 적재 확인)

**ROLETYPE 컬럼값에 순서로 적재되게 되면 `RoleType`파일이 변경되면 어떻게 될까?**

*RoleType*

```java
public enum RoleType {
    GUEST, USER, ADMIN
}
```

*JpaMain*

```java
Member member = new Member();
member.setId(2L);
member.setRoleType(RoleType.GUEST);

em.persist(member);

tx.commit();
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/88a64bd8-5256-4d4a-9cb5-5004b564c49b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140141Z&X-Amz-Expires=86400&X-Amz-Signature=f462901eb4e1a3d975160a27f601d55004b93a565f3dde03061afde1945a89ee&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- ID값이 2인 행의 ROLETYPE 값도 0인걸 확인할 수 있다.
- 이런 문제점이 발생하기 때문에 `@Enumerated(EnumType.*STRING*)`이렇게 속성값을 사용해야 한다.

**@Temporal**

---

- 날짜 타입(java.util.Date, java.util.Calendar)을 매핑할 때 사용
- LocalDate, LocalDateTime을 사용할 때는 생략 가능(최신 하이버네이트 지원)
  - 거진 LocalDate, LocalDateTime을 사용

| 속성 | 설명 | 기본값 |
| --- | --- | --- |
| value | - TemporalType.DATE: 날짜, 데이터베이스 date 타입과 매핑(예: 2013-10-11)
- TemporalType.TIME: 시간, 데이터베이스 time 타입과 매핑(예: 11:11:11)
- TemporalType.TIMESTAMP: 날짜와 시간, 데이터베이스 timestamp 타입과 매핑(예: 2013-10-11 11:11:11) |  |

**@Lob**

---

데이터베이스 BLOB, CLOB 매핑

- `@Lob`에는 지정할 수 있는 속성이 없다.
- 매핑하는 필드 타입이 문자면 CLOB 매핑, 나머지는 BLOB 매핑
  - CLOB: String, char[], java.sql.CLOB
  - BLOB: byte[], java.sql.BLOB

**@Transient**

---

- 필드 매핑 X
- 데이터베이스에 저장 X, 조회 X
- 주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용

```java
@Transient
private Integer temp;
```

## 기본 키 매핑

**기본 키 매핑 어노테이션**

---

- `@Id`
- `@GeneratedValue`

```java
@Id @GeneratedValue(strategy = GenerationType.AUTO)
private Long id;
```

**기본 키 매핑 방법**

---

- 직접 할당: `@Id`만 사용
- 자동 생성(`@GeneratedValue`)
  - `IDENTITY`: 데이터베이스에 위임, MYSQL
  - `SEQUENCE`: 데이터베이스 시퀀스 오브젝트 사용, ORACLE
    - `@SequenceGenerator`필요
  - `TABLE`: 키 생성용 테이블 사용, 모든 DB에서 사용
    - `@TableGenerator`필요
  - `AUTO`: 방언에 따라 자동 지정, 기본값

**IDENTITY 전략 - 특징**

---

- 기본 키 생성을 데이터베이스에 위임
- 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용
  (예: MySQL의 AUTO_INCREMENT)
- JPA는 보통 트랜잭션 커밋 시점에 INSERT SQL 실행
- AUTO_INCREMENT는 데이터베이스에 INSERT SQL을 실행한 이후에 ID값을 알 수 있음
- IDENTITY 전략은 `em.persist()`시점에 즉시 INSERT SQL 실행하고 DB에서 식별자를 조회
  - 영속성 컨텍스트의 1차 캐시에 Entity를 등록하려면 키값인 ID값이 있어야 하기 때문에 예외적으로 em.persist()시점에 즉시 INSERT SQL을 실행한다.

*Member*

```java
@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "name", nullable = false)
    private String username;

    public Member() {
    }
    //Getter, Setter ...
}
```

*JpaMain*

```java
Member member = new Member();
member.setUsername("A");

em.persist(member);

tx.commit();
```

- `member.setId()`을 제거해서 null로 유지

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3a8873ff-959f-4219-8fd2-185cf0d72b4f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140215Z&X-Amz-Expires=86400&X-Amz-Signature=9a4ea1400d38efd5dab5c29a94ed3039fc3ac8cfc6b810f61805a2acc82aa3d9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- hibernate_sequence 에서 다음 시퀀스를 호출하는 쿼리문에 확인된다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4cd3621e-d8ed-4431-9206-bad00b59f499/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140225Z&X-Amz-Expires=86400&X-Amz-Signature=9aadc9308ce2ef4eb84e21a963ab230ae737ae6e75787932f316779326a1d8ea&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**SEQUENCE 전략 - 특징**

---

- 데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트(예: 오라클 시퀀스)
- 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용

**SEQUENCE - @SequenceGenerator**

---

- 주의: allocationSize 기본값 = 50

| 속성 | 설명 | 기본값 |
| --- | --- | --- |
| name | 식별자 생성기 이름 | 필수 |
| sequenceName | 데이터베이스에 등록되어 있는 시퀀스 이름 | hibernate_sequence |
| initalValue | DDL 생성 시에만 사용됨, 시퀀스 DDL을 생성할 때 처음 시작하는 수를 지정한다. | 1 |
| allocationSize | 시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용됨
데이터베이스 시퀀스 값이 하나씩 증가하도록 설정되어 있으면 반드시 이 값을 1로 설정해야 한다 | 50 |
| catalog, schema | 데이터베이스 catalog, schema 이름 |  |

*Member*

```java
@Entity
@SequenceGenerator(name = "member_seq_generator", sequenceName = "member_seq")
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "member_seq_generator")
    private Long id;

    @Column(name = "name", nullable = false)
    private String username;

    public Member() {
    }
    //Getter, Setter ...
}
```

*실행 (*`<property name="hibernate.hbm2ddl.auto" value="create"/>`)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ea7b4412-782b-4ed0-80c1-aa9630fc332b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140246Z&X-Amz-Expires=86400&X-Amz-Signature=900367d63e961c0d3a024700d20605401ab90313ea0d0ec0c19efbc533b693de&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- DDL 생성 기능에서 시퀀스를 생성하고 Member 테이블을 생성하는게 확인된다.
- 생성된 시퀀스를 호출하여 INSERT SQL을 실행함

**TABLE 전략**

---

- 키 생성 전용 테이블을 하나 만들어서 데이터베이스 시퀀스를 흉내내는 전략
- 장점: 모든 데이터베이스에 적용 가능
- 단점: 성능이 떨어짐

**TableGenerator - 속성**

---

| 속성 | 설명 | 기본값 |
| --- | --- | --- |
| name | 식별자 생성기 이름 | 필수 |
| table | 키 생성 테이블명 | hibernate_sequences |
| pkColumnName | 시퀀스 컬럼명 | sequence_name |
| valueColumnNa | 시퀀스 값 컬럼명 | next_val |
| pkColumnValue | 키로 사용할 값 이름 | 엔티티 이름 |
| initialValue | 초기 값, 마지막으로 생성된 값이 기준이다. | 0 |
| allocationSize | 시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용됨) | 50 |
| catalog, schema | 데이터베이스 catalog, schema 이름 |  |
| uniqueConstraints(DDL) | 유니크 제약 조건을 지정할 수 있다. |  |

*Member*

```java
@Entity
@TableGenerator(
        name = "MEMBER_SEQ_GENERATOR",
        table = "MY_SEQUENCES",
        pkColumnValue = "MEMBER_SEQ", allocationSize = 1)
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE, generator = "MEMBER_SEQ_GENERATOR")
    private Long id;

    @Column(name = "name", nullable = false)
    private String username;

    public Member() {
    }
    //Getter, Setter ...
}
```

*실행*

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/69232aea-b499-48d3-84a3-213d0b2d4e64/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140308Z&X-Amz-Expires=86400&X-Amz-Signature=0b3470c9fcb9f2a7182bbef16dde1b252a739d8ffd1e8eec42ae6799534674ec&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- MY_SEQUENCES라는 테이블을 생성하고 초기화값으로 0을 INSERT 해줌

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9640ba6a-3fb5-4e86-a0b5-d3dc614e4f82/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230109T140318Z&X-Amz-Expires=86400&X-Amz-Signature=3de8cfbdad0e9baa92bbd4334a89355115a3dad4c49399a2cc9aadd3fd4de59e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- MY_SEQUENCES 테이블에서 다음 시퀀스 값을 조회하고 UPDATE 하는 SQL이 실행됨

**권장하는 식별자 전략**

---

- 기본 키 제약 조건: null 아님, 유일, **변하면 안된다.**
- 미래까지 이 조건을 만족하는 자연키는 찾기 어렵다. 대리키(대체키)를 사용하자.
- 예를 들어 주민등록번호도 기본 키로 적절하지 않다.
- **권장: Long형 + 대체키 + 키 생성전략 사용**

> **주의:
SequenceGenerator.allocationSize의 기본값이 50인 것에 주의해야 한다. JPA가 기본으로 생성하는 데이터베이스 시퀀스는 create sequence [sequenceName] start with 1 increment by 50이므로 시퀀스를 호출할 때마다 값이 50씩 증가한다. 기본값이 50인 이유는 최적화 때문이다. 데이터베이스 시퀀스 값이 하나씩 증가하도록 설정되어 있으면 이 값을 반드시 1로 설정해야 한다.**
>

> **참고:
SEQUENCE 전략과 최적화
SEQUENCE 전략과 데이터베이스 시퀀스를 통해 식별자를 조회하는 추가 작업이 필요하다. 따라서 데이터베이스와 2번 통신한다.**

> 1. 식별자를 구하려고 데이터베이스 시퀀스를 조회환다.
   EX) SELECT BOARD_SEQ.NEXTVAL FROM DUAL
> 2. 조회한 시퀀스를 기본 키 값으로 사용해 데이터베이스에 저장한다.
  EX) INSERT INTO BOARD…
  JPA는 시퀀스에 접근하는 횟수를 줄이기 위해 `@SequenceGenerator.allocationSize`를 사용한다. 간단히 설명하자면 여기에 설정한 값만큼 한 번에 시퀀스 값을 증가시키고 나서 그만큼 메모리에 시퀀스 값을 할당한다. 예를 들어 allocationSize 값이 50이면 시퀀스를 한 번에 50 증가시킨 다음에 1-50까지는 메모리에서 식별자를 할당한다. 그리고 51이 되면 시퀀스 값을 100으로 증가시킨 다음 51-100까지 메모리에서 식별자를 할당한다.

> **이 최적화 방법은 시퀀스 값을 선점하므로 여러 JVM이 동시에 동작해도 기본 키 값이 충돌하지 않는 장점이 있다. 반면에 데이터베이스에 직접 접근해서 데이터를 등록할 때 시퀀스 값이 한 번에 많이 증가한다는 점을 염두해두어야 한다. 이런 상황이 부담스럽고 INSERT 성능이 중요하지 않으면 allocationSize의 값을 1로 설정하면 된다.**

> **참고로 앞서 설명한 `hibernate.id.new_generator_mappings` 속성을 true로 설정해야 지금까지 설명한 최적화 방법이 적용된다. 이 속성을 적용하지 않으면 하이버네이크가 과거에 사용하던 방법으로 키 생성을 최적화한다. 과거에는 시퀀스 값을 하나씩 할당받고 애플리케이션에서 allocationSize만큼 사용했다. 예를 들어 allocationSize를 50으로 설정했다고 가정하면, 반환된 시퀀스 값이 1이면 애플리케이션에서 1-50까지 사용하고 시퀀스 값이 2이면 애플리케이션에서 51-100까지 기본 키를 사용하는 방식이었다.**

- `실전 예제 1 - 요구사항 분석과 기본 매핑`은 jpabook 프로젝트에서 진행 