시간표
******

이 페이지에서는 사용자가 보관한 시간표를 처리하는 API를 다룬다.

.. _timetable-object:

시간표 객체
===========

시간표들을 처리하는 API는 시간표의 정보가 포함된 객체 혹은 이것의 배열들을 사용한다. 시간표 객체에는 다음과 같은 속성이 있다.

   * `id` – 시간표의 ID
   * `name` – 시간표의 이름
   * `created_by` – 시간표를 생성한 사용자의 ID. :ref:`user-object` 참조.
   * `created_time` – 시간표를 생성한 날짜와 시각. ISO-8601 date-time을 포함하는 문자열이다.
   * `classes` – 시간표에 포함된 강좌의 목록. :ref:`class-object` 의 배열로 이루어져 있다.
                 단, :http:post:`/api/timetables` 에서는 :ref:`class-object` 의 속성 중에서 
                 `course_no` 와 `class_no` 속성만 필요하다.

시간표 API
==========

.. http:get:: /api/timetables
   
   사용자가 보관하고 있는 시간표의 목록

   **요청 예시**:

   .. sourcecode:: http

      GET /api/timetables?user_id=me&offset=0&limit=5 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript

   :query user_id: 시간표의 목록을 받아올 사용자의 ID. ``me`` 는 현재 로그인한 사용자를 가리킨다.
   :query limit: 받아올 시간표의 최대 개수
   :query offset: 시간표를 받아오기 시작할 위치. 최신순 정렬 기준이다. 즉, `offset` 이 0이면 가장 최근에 수정한 시간표들부터 받아온다.
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      [
        {
          "id": "123",
          "name": "이번 학기 최종 시간표",
          "created_by": "1",
          "created_time": "2013-08-25T09:28:28+0000",
          "classes": [
            {
              "course_no": "ITE231",
              "class_no": "10443",
              "title": "컴퓨터구조론",
              "instructor": "이인환",
              "score": 3.00,
              "time": [
                {"start_time": 216, "end_time": 218, "room": "H77-0207"},
                {"start_time": 514, "end_time": 516, "room": "H77-0207"}
              ]
            },
            {
              "course_no": "ITE316",
              "class_no": "10415",
              "title": "데이터베이스시스템",
              "instructor": "김상욱",
              "score": 3.00,
              "time": [
                {"start_time": 115, "end_time": 117, "room": "H77-0813"},
                {"start_time": 315, "end_time": 317, "room": "H77-0813"}
              ]
            },
            {
              "course_no": "SYH003",
              "class_no": "10130",
              "title": "비즈니스리더십(HELP3)",
              "instructor": null,
              "score": 2.00,
              "time": [
                {"start_time": 607, "end_time": 610, "room": "H"}
              ]
            },
            {
              "course_no": "CSE406",
              "class_no": "10407",
              "title": "소프트웨어공학",
              "instructor": "유인경",
              "score": 3.00,
              "time": [
                {"start_time": 213, "end_time": 215, "room": "H93-0811"},
                {"start_time": 306, "end_time": 308, "room": "H93-0811"}
              ]
            },
            {
              "course_no": "ELE429",
              "class_no": "10400",
              "title": "컴파일러",
              "instructor": "임을규",
              "score": 3.00,
              "time": [
                {"start_time": 303, "end_time": 305, "room": "H77-0813"},
                {"start_time": 505, "end_time": 507, "room": "H77-0507"}
              ]
            },
            {
              "course_no": "ENE419",
              "class_no": "10410",
              "title": "컴퓨터네트워크",
              "instructor": "조인휘",
              "score": 3.00,
              "time": [
                {"start_time": 418, "end_time": 420, "room": "H77-0203"},
                {"start_time": 512, "end_time": 514, "room": "H77-0501"}
              ]
            },
            {
              "course_no": "GEN606",
              "class_no": "10417",
              "title": "특허법의이해",
              "instructor": "장의선",
              "score": 2.00,
              "time": [
                {"start_time": 205, "end_time": 208, "room": "H77-0813"}
              ]
            }
          ]
        }
      ]

   :ref:`timetable-object` 의 배열로 이루어져 있다.

   :resheader Content-Type: ``application/json``
   :statuscode 200: 시간표들 받아오기 성공
   :statuscode 404: 사용자 `user_id` 가 보관하고 있는 시간표의 목록을 받아올 권한이 없음

.. http:get:: /api/timetables/(tt_id)
   
   ID가 `tt_id` 인 시간표

   **요청 예시**:

   .. sourcecode:: http

      GET /api/timetables/123 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript

   :param tt_id: 시간표의 ID
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "id": "123",
        "name": "이번 학기 최종 시간표",
        "created_by": "1",
        "created_time": "2013-08-25T09:28:28+0000",
        "classes": [
          {
            "course_no": "ITE231",
            "class_no": "10443",
            "title": "컴퓨터구조론",
            "instructor": "이인환",
            "score": 3.00,
            "time": [
              {"start_time": 216, "end_time": 218, "room": "H77-0207"},
              {"start_time": 514, "end_time": 516, "room": "H77-0207"}
            ]
          },
          {
            "course_no": "ITE316",
            "class_no": "10415",
            "title": "데이터베이스시스템",
            "instructor": "김상욱",
            "score": 3.00,
            "time": [
              {"start_time": 115, "end_time": 117, "room": "H77-0813"},
              {"start_time": 315, "end_time": 317, "room": "H77-0813"}
            ]
          },
          {
            "course_no": "SYH003",
            "class_no": "10130",
            "title": "비즈니스리더십(HELP3)",
            "instructor": null,
            "score": 2.00,
            "time": [
              {"start_time": 607, "end_time": 610, "room": "H"}
            ]
          },
          {
            "course_no": "CSE406",
            "class_no": "10407",
            "title": "소프트웨어공학",
            "instructor": "유인경",
            "score": 3.00,
            "time": [
              {"start_time": 213, "end_time": 215, "room": "H93-0811"},
              {"start_time": 306, "end_time": 308, "room": "H93-0811"}
            ]
          },
          {
            "course_no": "ELE429",
            "class_no": "10400",
            "title": "컴파일러",
            "instructor": "임을규",
            "score": 3.00,
            "time": [
              {"start_time": 303, "end_time": 305, "room": "H77-0813"},
              {"start_time": 505, "end_time": 507, "room": "H77-0507"}
            ]
          },
          {
            "course_no": "ENE419",
            "class_no": "10410",
            "title": "컴퓨터네트워크",
            "instructor": "조인휘",
            "score": 3.00,
            "time": [
              {"start_time": 418, "end_time": 420, "room": "H77-0203"},
              {"start_time": 512, "end_time": 514, "room": "H77-0501"}
            ]
          },
          {
            "course_no": "GEN606",
            "class_no": "10417",
            "title": "특허법의이해",
            "instructor": "장의선",
            "score": 2.00,
            "time": [
              {"start_time": 205, "end_time": 208, "room": "H77-0813"}
            ]
          }
        ]
      }

   JSON 파라미터에 대한 정보는 :ref:`timetable-object` 참조.

   :resheader Content-Type: ``application/json``
   :statuscode 200: 시간표 받아오기 성공
   :statuscode 404: 시간표 `tt_id` 를 받아올 권한이 없음

.. http:post:: /api/timetables
   
   새로운 시간표를 생성

   **요청 예시**:

   .. sourcecode:: http

      POST /api/timetables HTTP/1.1
      Host: example.com
      Content-Type: application/json

      {
        "name": "이번 학기 최종 시간표",
        "classes": [
          {"course_no": "ITE231", "class_no": "10443"},
          {"course_no": "ITE316", "class_no": "10415"},
          {"course_no": "SYH003", "class_no": "10130"},
          {"course_no": "CSE406", "class_no": "10407"},
          {"course_no": "ELE429", "class_no": "10400"},
          {"course_no": "ENE419", "class_no": "10410"},
          {"course_no": "GEN606", "class_no": "10417"}
        ]
      }

   JSON 파라미터에 대한 정보는 :ref:`timetable-object` 참조.
   
   :reqheader Content-Type: ``application/json``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Location: http://example.com/api/timetables/124

   :resheader Location: 시간표가 성공적으로 생성되었을 때, 생성된 시간표의 링크
   :statuscode 200: 시간표 생성 성공
   :statuscode 400: 시간표 생성 실패

.. http:delete:: /api/timetables/(tt_id)
   
   시간표 `tt_id` 를 삭제

   **요청 예시**:

   .. sourcecode:: http

      DELETE /api/timetables/123 HTTP/1.1
      Host: example.com

   :param tt_id: 시간표의 ID

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 시간표 삭제 성공
   :statuscode 404: 시간표 `tt_id` 를 삭제할 권한이 없음
