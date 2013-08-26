과목
********

이 페이지에서는 학부목록을 로드하고 과목정보를 받아오는 API를 다룬다.

과목 API
===========

.. http:get:: /api/major
  
  해당 학교의 학과 목록

  **요청 예시**:

  .. sourcecode:: http

    GET /api/major HTTP/1.1
    Host: example.com
    Accept: application/json, text/javascript

  :query name: 받아올 학과의 이름
  :query code: 받아올 학과의 코드
  :request Accept: ''application/json'' 혹은 ''text/javascript''

  **응답 예시**:

  .. sourcecode:: http

    HTTP/1.1 200 OK
    Content-Type: application/json

    [
      {
        "name": "컴퓨터공학부",
        "code": "MT302"
      },
      {
        "name": "컴퓨터전공",
        "code": "MT303"
      },
      {
        "name": "소프트웨어전공",
        "code": "MT304"
      }
    ]

  :resheader Content-Type: ''application/json''
  :statuscode 200: 학과목록 받아오기 성공
  :statuscode 404: 학과목록을 받아오는데 실패함

.. http:get:: /api/lecture
    
  해당 학교의 과목정보

  **요청 예시**:

  .. sourcecode:: http

    GET /api/lecture HTTP/1.1
    Host: example.com
    Accept: Application/json, text/javascript

  :query name: 해당 강의의 이름
  :query grade: 해당 강의의 학점
  :query major: 해당 강의에 해당하는 전공 이름
  :query class_no: 해당 강의의 수업코드
  :query course_no: 해당 강의의 학수 번호
  :query time: 해당 강의의 시간, 
  {start_time:xyy end_time:xzz} 형식으로 x는 요일(1부터 월요일, 즉 7은 일요일), yy는 시작교시, zz는 종료교시로 한다.
  :request Accept: ''application/json'' 혹은 ''text/javascript''

  **응답 예시**:

  .. sourcecode:: http

    HTTP/1.1 200 OK
    Content-Type=application/json

    [
      {
        class_no: "10462",
        course_no: "COE301",
        name:"공업수학",
        major:"컴퓨터전공",
        time:[{"start_time":106, "end_time":108, "room":H77-0207},
              {"start_time":415, "end_time":417, "room":H77-0207}
        ],
        score:"3학점",
        tutor:"최윤성",
        category:"전공핵심"
      },
      {
        class_no: "10464",
        course_no: "CSE409",
        name:"시스템프로그래밍",
        major:"컴퓨터전공",
        time:[{"start_time":411, "end_time":414, "room":H77-0607},
              {"start_time":515, "end_time":518, "room":H77-0607}
        ],
        score:"3학점",
        tutor:"조인휘",
        category:"전공핵심"
      },
      {
        class_no: "10465",
        course_no: "ECE308",
        name:"신호와시스템",
        major:"컴퓨터전공",
        time:[{"start_time":207, "end_time":209, "room":H77-0207},
              {"start_time":403, "end_time":405, "room":H77-0207}
        ],
        score:"3학점",
        tutor:"이상화",
        category:"전공핵심"
      },
      {
        class_no: "10474",
        course_no: "MAT203",
        name:"선형대수",
        major:"컴퓨터전공",
        time:[{"start_time":116, "end_time":118, "room":H77-0203},
              {"start_time":316, "end_time":318, "room":H77-0207}
        ],
        score:"3학점",
        tutor:"이병호",
        category:"기초필수"
      }
    ]

.. http:post:: /api/lecture

  기존 목록에 강의 추가

  **요청 예시**

  .. sourcecode:: http

    POST /api/lecture HTTP/1.1
    Host: example.com
    Content-Type: application/json

    {
      class_no: "10464",
      course_no: "CSE409",
      name:"시스템프로그래밍",
      major:"컴퓨터전공",
      time:[{"start_time":411, "end_time":414, "room":H77-0607},
            {"start_time":515, "end_time":518, "room":H77-0607}
      ],
      score:"3학점",
      tutor:"조인휘",
      category:"전공핵심"
    }

  :jsonparam string class_no: 추가할 강의의 수업 코드
  :jsonparam string lec_no: 추가할 강의의 학수번호
  :jsonparam string name: 추가할 강의의 이름
  :jsonparam string major: 추가할 강의의 설강학과
  :jsonparam array time: 추가할 강의의 수업 시간과 장소
                            수업 요일과 시간이 포함된 'start_time', 'end_time' 필드와 강의 장소에 대한 'room' 필드로 구성된 객체들의 배열이다.
  :jsonparam string score: 추가할 강의의 학점
  :jsonparam string tutor: 추가할 강의의 강의자 이름
  :jsonparam string category: 추가할 강의의 종류 ex) 전공핵심/필수교양/기초필수
  :reqheader Content-Type: ''application/json''

  **응답 예시**:

  .. sourcecode:: http

    HTTP/1.1 200 OK

  :statuscode 200: 강의 추가 성공
  :statuscode 404: 강의 추가 실패 ex) 중복될 경우

.. http:put:: /apt/lecture/(class_no)

  강의에 대한 정보 수정

  **요청 예시**

  .. sourcecode:: http

    PUT /api/lecture/(class_no) HTTP/1.1
    Host: example.com
    Content-Type: application/json

    {
      class_no: "10464",
      course_no: "CSE409",
      name:"시스템프로그래밍",
      major:"컴퓨터전공",
      time:[{"start_time":411, "end_time":414, "room":H77-0607},
            {"start_time":515, "end_time":518, "room":H77-0607}
      ],
      score:"3학점",
      tutor:"조인휘",
      category:"전공핵심"
    }

  :jsonparam string class_no: 추가할 강의의 수업 코드
  :jsonparam string lec_no: 추가할 강의의 학수번호
  :jsonparam string name: 추가할 강의의 이름
  :jsonparam string major: 추가할 강의의 설강학과
  :jsonparam array time: 추가할 강의의 수업 시간과 장소
                            수업 요일과 시간이 포함된 'start_time', 'end_time' 필드와 강의 장소에 대한 'room' 필드로 구성된 객체들의 배열이다.
  :jsonparam string score: 추가할 강의의 학점
  :jsonparam string tutor: 추가할 강의의 강의자 이름
  :jsonparam string category: 추가할 강의의 종류 ex) 전공핵심/필수교양/기초필수
  :reqheader Content-Type: ''application/json''
 
  **응답 예시**

  .. sourcecode:: http

    HTTP/1.1 200 OK

  :statuscode 200: 강의 정보 수정 성공
  :statuscode 404: 강의 정보 수정 실패

.. http:get:: /api/lecture/(class_no)

  과목 코드로 강의정보 받기

  **요청 예시**

  .. sourcecode:: http

    GET /api/lecture/(class_no) HTTP/1.1
    Host: example.com
    Accept: application/json, text/javascript

  :param class_no: 받아올 강의의 수업코드
  :request Accept: ''application/json'' 혹은 ''text/javascript''

  **응답 예시**

  .. sourcecode:: http

    HTTP/1.1 200 OK

    {
      class_no: "10464",
      course_no: "CSE409",
      name:"시스템프로그래밍",
      major:"컴퓨터전공",
      time:[{"start_time":411, "end_time":414, "room":H77-0607},
            {"start_time":515, "end_time":518, "room":H77-0607}
      ],
      score:"3학점",
      tutor:"조인휘",
      category:"전공핵심"
    }

  :resheader Content-Type: ''application/json''
  :statuscode 200: 강의 정보 수정 성공
  :statuscode 404: 강의 정보 수정 실패

.. http:delete:: /apit/lecture/(class_no)

  특정 강의에 대한 정보를 삭제

  **요청 예시**

  .. sourcecode:: http

    DELETE /api/lecture/(class_no) HTTP/1.1
    Host: example.com
    Accept: application/json, text/javascript

  :param class_no: 삭제하려는 강의의 수업코드

  **응답 예시**

  .. sourcecode:: http

    HTTP/1.1 200 OK

  :statuscode 200: 강의 삭제 성공
  :statuscode 404: 강의 삭제 실패