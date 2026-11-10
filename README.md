# 🌱 Java_spring_Study

한국폴리텍대학 **AI소프트웨어과 하이테크과정** Java 수업 후반부에 **Maven + IntelliJ**로 진행한 Spring 웹 프로젝트입니다.
레이아웃(Tiles), 파일 업로드, REST API를 하나씩 익히고, 마지막에는 이것들을 모아 **답변형 게시판**을 만들었습니다.

| 항목 | 내용 |
|---|---|
| 기술 | Spring MVC 4.1 (pro27은 3.1) · MyBatis · Apache Tiles · Oracle DB · Maven · Java 8 |
| 도구 | IntelliJ IDEA · Apache Tomcat 9 |
| 이전 단계 | Java 기초 → Servlet·JSP → Spring 입문(Eclipse): [java-basics](https://github.com/JHKINI/java-basics) |

---

## 🗺 프로젝트 흐름

| 프로젝트 | 주제 | 실습 내용 |
|---|---|---|
| [`pro27`](pro27) | Maven 프로젝트 + Tiles + MyBatis | `pom.xml`로 라이브러리 관리 시작. **Tiles 레이아웃**(header / side / footer를 `layout.jsp` 하나에 조합)으로 회원 목록·가입·로그인 화면 구성, MyBatis로 회원 CRUD |
| [`pro28_1`](pro28_1) | 다중 파일 업로드 · 다운로드 | `MultipartHttpServletRequest`, `getFileNames()`로 여러 파일을 한 번에 업로드하고, 업로드한 파일을 다운로드하는 컨트롤러 |
| [`pro29_1`](pro29_1) | REST API | `@RestController`, `ResponseEntity`로 JSON 응답. `@PathVariable`, `@RequestBody`. 게시판 API를 `GET /boards/all`, `GET /boards/{articleNO}`, `POST`, `PUT`, `DELETE` 메서드로 나눠서 설계 |
| [`pro30_2`](pro30_2) | **게시판 (기본)** | 글 목록 → 글쓰기(이미지 1개 첨부) → 글 상세 → **글 수정** → 글 삭제. 글쓰기·수정은 로그인한 회원만 가능 |
| [`pro30_1`](pro30_1) | **답변형 게시판 (확장)** | 기본 게시판에 **답글**(`replyForm.do`, `addReply.do`)과 **이미지 여러 장 첨부**(별도 이미지 테이블 `t_imageFile`)를 추가 |

## 🧩 게시판 구조 (pro30)

```text
요청 (*.do)
  → ViewNameInterceptor   요청 URL에서 뷰 이름을 뽑아 둠
  → BoardController       파일 업로드 처리, 로그인 확인
  → BoardService
  → BoardDAO → MyBatis 매퍼(board.xml) → Oracle
  → Tiles 레이아웃 + JSP
```

| 기능 | 처리 내용 |
|---|---|
| 글쓰기 | 업로드한 이미지를 임시 폴더(`temp`)에 저장 → DB 저장이 끝나면 글 번호 폴더로 이동. 실패하면 임시 파일 삭제 |
| 글 수정 | 새 이미지를 올리면 기존 이미지를 지우고 교체 |
| 글 삭제 | 글과 함께 글 번호 폴더의 이미지도 삭제 |
| 이미지 보기 | `FileDownloadController`가 글 번호 폴더에서 이미지를 찾아 전송 |

- 테이블 생성 SQL: [`pro30_1/schema.sql`](pro30_1/schema.sql) (`t_member`, `t_board`, `t_imageFile`)

---

## ▶️ 실행

1. IntelliJ에서 각 프로젝트 폴더의 `pom.xml`을 열면 Maven이 라이브러리를 내려받습니다.
2. Oracle에 `schema.sql`로 테이블을 만들고 `src/main/webapp/WEB-INF/config/jdbc/jdbc.properties`의 접속 정보를 확인합니다. (로컬 `testdb`, 교재 기본 계정)
3. Tomcat 9에 배포해서 `/board/listArticles.do`로 접속합니다.

> 이 저장소에 있던 Java 기초 문법과 Eclipse 실습(Servlet·JSP, Spring 입문)은 [java-basics](https://github.com/JHKINI/java-basics)로 옮겼습니다.
