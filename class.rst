강좌
****

이 페이지에서는 강좌 정보를 처리하는 API를 다룬다.

.. _class-object:

강좌 객체
=========

강좌들을 처리하는 API는 강좌 정보가 포함된 객체 혹은 이것의 배열들을 사용한다. 강좌 객체에는 다음과 같은 속성이 있다.

   * `class_no` – 강좌 번호
   * `course_no` – 과목 번호
   * `name` – 강좌 이름
   * `major` – 강좌의 소속 전공 코드. :ref:`major-object` 의 `code` 에 해당한다.
   * `time` – 강좌의 강의 시간 정보. 다음과 같은 속성을 가진 객체들의 배열로 이루어져 있다.

      * `start_time` – 강의 시작 시간
      * `end_time` – 강의 종료 시간
      * `room` – 강의 장소

   * `score` – 강좌의 학점. 실수 형태로 주어진다.
   * `grade` – 강좌의 대상 학년. 정수 형태로 주어진다.
   * `instructor` – 강좌를 담당하는 교강사 성명
   * `category` – 강좌의 이수구분

강좌 API
===========

.. http:get:: /api/classes
    
   강좌 정보 목록
 
   **요청 예시**:
 
   .. sourcecode:: http
 
      GET /api/classes?major=MT303 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript
 
   :query name: 검색할 강좌 이름
   :query grade: 검색할 강좌의 대상 학년
   :query major: 검색할 강좌의 소속 전공
   :query class_no: 검색할 강좌의 강좌 코드
   :query course_no: 검색할 강좌의 과목 코드
   :query time: 검색할 강좌의 시간 범위. ``xyyzz`` 와 같은 형식으로 주어진다.
                x는 요일(1: 월요일, 2: 화요일, ...), yy는 시작 교시, zz는 끝 교시를 나타낸다.
                예를 들어, ``20308`` 로 주어지면 화요일 3교시부터 8교시까지에 속하는 강좌들의 목록을 반환한다.
   :request Accept: ``application/json`` 혹은 ``text/javascript``
 
   **응답 예시**:
 
   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      [
        {
          "class_no": "10462",
          "course_no": "COE301",
          "name": "공업수학",
          "major": "MT303",
          "time": [
            {"start_time": 106, "end_time": 108, "room": "H77-0207"},
            {"start_time": 415, "end_time": 417, "room": "H77-0207"}
          ],
          "score": 3.00,
          "grade": 2,
          "instructor": "최윤성",
          "category": "전공핵심"
        },
        {
          "class_no": "10464",
          "course_no": "CSE409",
          "name": "시스템프로그래밍",
          "major": "MT303",
          "time": [
            {"start_time": 411, "end_time": 414, "room": "H77-0607"},
            {"start_time": 515, "end_time": 518, "room": "H77-0607"}
          ],
          "score": 3.00,
          "grade": 2,
          "instructor": "조인휘",
          "category": "전공핵심"
        },
        {
          "class_no": "10465",
          "course_no": "ECE308",
          "name": "신호와시스템",
          "major": "MT303",
          "time": [
            {"start_time": 207, "end_time": 209, "room": "H77-0207"},
            {"start_time": 403, "end_time": 405, "room": "H77-0207"}
          ],
          "score": 3.00,
          "grade": 2,
          "instructor": "이상화",
          "category": "전공핵심"
        },
        {
          "class_no": "10474",
          "course_no": "MAT203",
          "name": "선형대수",
          "major": "MT303",
          "time": [
            {"start_time": 116, "end_time": 118, "room": "H77-0203"},
            {"start_time": 316, "end_time": 318, "room": "H77-0207"}
          ],
          "score": 3.00,
          "grade": 2,
          "instructor": "이병호",
          "category": "기초필수"
        }
      ]

.. http:post:: /api/classes

   새로운 강좌 생성
 
   **요청 예시**
 
   .. sourcecode:: http

      POST /api/classes HTTP/1.1
      Host: example.com
      Content-Type: application/json

      {
        "class_no": "10464",
        "course_no": "CSE409",
        "name": "시스템프로그래밍",
        "major": "MT303",
        "time": [
          {"start_time": 411, "end_time": 414, "room": "H77-0607"},
          {"start_time": 515, "end_time": 518, "room": "H77-0607"}
        ],
        "score": 3.00,
        "grade": 2,
        "instructor": "조인휘",
        "category": "전공핵심"
      }
 
   JSON 파라미터에 대한 정보는 :ref:`class-object` 참조.

   :reqheader Content-Type: ``application/json``
 
   **응답 예시**:
 
   .. sourcecode:: http
 
      HTTP/1.1 200 OK
 
   :statuscode 200: 강좌 추가 성공
   :statuscode 404: 강좌 추가 실패. 강좌 번호가 같은 강좌가 이미 존재하는 경우가 여기에 포함된다.

.. http:put:: /api/classes/(class_no)

   강좌 정보 수정
 
   **요청 예시**
 
   .. sourcecode:: http

      PUT /api/classes/10464 HTTP/1.1
      Host: example.com
      Content-Type: application/json

      {
        "class_no": "10464",
        "course_no": "CSE409",
        "name": "시스템프로그래밍",
        "major": "MT303",
        "time": [
          {"start_time": 411, "end_time": 414, "room": "H77-0607"},
          {"start_time": 515, "end_time": 518, "room": "H77-0607"}
        ],
        "score": 3.00,
        "grade": 2,
        "instructor": "조인휘",
        "category": "전공핵심"
      }
 
   JSON 파라미터에 대한 정보는 :ref:`class-object` 참조.

   :param class_no: 강좌 번호
   :reqheader Content-Type: ``application/json``
  
   **응답 예시**
 
   .. sourcecode:: http
 
      HTTP/1.1 200 OK
 
   :statuscode 200: 강좌 정보 수정 성공
   :statuscode 404: 강좌 정보 수정 실패

.. http:get:: /api/classes/(class_no)

   강좌 번호가 `class_no` 인 강좌
 
   **요청 예시**
 
   .. sourcecode:: http

      GET /api/classes/10464 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript

   :param class_no: 강좌 번호
   :request Accept: ``application/json`` 혹은 ``text/javascript``
 
   **응답 예시**
 
   .. sourcecode:: http

      HTTP/1.1 200 OK

      {
        "class_no": "10464",
        "course_no": "CSE409",
        "name": "시스템프로그래밍",
        "major": "MT303",
        "time": [
          {"start_time": 411, "end_time": 414, "room": "H77-0607"},
          {"start_time": 515, "end_time": 518, "room": "H77-0607"}
        ],
        "score": 3.00,
        "grade": 2,
        "instructor": "조인휘",
        "category": "전공핵심"
      }
 
   :resheader Content-Type: ``application/json``
   :statuscode 200: 강좌 정보 수정 성공
   :statuscode 404: 강좌 정보 수정 실패

.. http:delete:: /api/classes/(class_no)

   강좌 번호가 `class_no` 인 강좌 삭제
 
   **요청 예시**
 
   .. sourcecode:: http
 
      DELETE /api/classes/10464 HTTP/1.1
      Host: example.com
 
   :param class_no: 강좌 번호
 
   **응답 예시**
 
   .. sourcecode:: http
 
      HTTP/1.1 200 OK
 
   :statuscode 200: 강좌 삭제 성공
   :statuscode 404: 강좌 삭제 실패
