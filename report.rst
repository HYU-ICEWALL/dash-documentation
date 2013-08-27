신고
***********

이 페이지에서는 사용자 신고를 처리하는 API를 다룬다.

관리자를 위한 API
=================================

.. http:get:: /api/report
   
   신고 항목의 목록을 받아옴

   **요청 예시**:

   .. sourcecode:: http

      GET /api/report?type=1&page=1&limit=10 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript

   :query type: 오류의 종류 [ 1:강좌 , 2:서비스오류 , 3:건의사항 ]
   :query page: 페이지
   :query limit: 한 페이지당 오류 출력 갯수
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json
   
      [
         {
            "no": "1",
            "class_no": "10443",
            "type": "1",
            "content": "강의 시간이 맞지 않아요",
            "status": "0",
            "time": "2013-08-25T09:28:28+0000",
            "reporter": "1"   
         },
         {
            "no": "1",
            "class_no": "10417",
            "type": "1",
            "content": "강의 교실이 잘못됐어요",
            "status": "",
            "time": "2013-08-26T09:28:28+0000",
            "reporter": "23"
         }

   :statuscode 200: 신고 목록 받아오기 성공
   :statuscode 400: 양식 데이터가 올바르지 않음
   :statuscode 404: 사용자가 신고 목록을 받아올 권한이 없음

.. http:get:: /api/report/:id
   
   특정 리포팅의 정보 열람

   **요청 예시**:

   .. sourcecode:: http

      GET /api/report/4 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript
   
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
         "no": "4",
         "class_no": "10443",
         "type": "1",
         "content": "강의 시간이 맞지 않아요",
         "status": "0",
         "reporter": "1"
      }
      

   :statuscode 200: 신고사항 받아오기 성공
   :statuscode 400: 신고사항의 번호가 존재하지 않음
   :statuscode 404: 사용자가 신고사항의 정보를 받아올 권한이 없음


.. http:put:: /api/report/:id
   
   특정 리포팅의 정보 수정

   **요청 예시**:

   .. sourcecode:: http

      PUT /api/report/4 HTTP/1.1
      Host: example.com
      Content-Type: application/json
      Accept: application/json, text/javascript

      [
         {
            "no": "4",
            "class_no": "10443",
            "type": "1",
            "content": "강의 시간이 맞지 않아요",
            "status": "1",
            "time": "2013-08-26T09:28:28+0000"
            "reporter": "1"
         }
      ]

   :jsonparam string no: 신고번호
   :jsonparam string class_no: 수강번호
   :jsonparam string type: 오류의 종류 [ 1:강좌 , 2:서비스오류 , 3:건의사항 ]
   :jsonparam string status: 처리상황 [ 0:미처리 , 1:처리 ]
   :jsonparam string time: 처리시간
   :jsonparam string reporter: 신고한 회원번호
   :reqheader Content-Type: ``application/x-www-form-urlencoded``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Location: http://example.com/api/report/4

   :resheader Location: 신고사항이 성공적으로 수정되었을 때, 수정된 신고사항의 링크
   :statuscode 200: 신고 사항의 정보 편집 성공.   
   :statuscode 400: 양식 데이터가 올바르지 않음
   :statuscode 404: 사용자가 신고사항의 정보를 수정할 권한이 없음

.. http:delete:: /api/report/:id
   
   특정 신고 항목을 삭제함

   **요청 예시**:

   .. sourcecode:: http

      DELETE /api/report/4 HTTP/1.1
      Host: example.com
      Content-Type: application/json

   :reqheader Content-Type: ``application/json``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 신고사항 삭제 성공
   :statuscode 400: 신고사항의 번호가 존재하지 않음
   :statuscode 404: 사용자가 신고사항의 정보를 삭제할 권한이 없음

사용자를 위한 API
==========================

.. http:post:: /api/report
   
   사용자가 신고하기를 통해 신고내용 전송

   **요청 예시**:

   .. sourcecode:: http

      POST /api/report HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript
      
      {
            "class_no": "10443",
            "type": "1",
            "content": "강의 시간이 맞지 않아요",
            "reporter": "1"  
      }

   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``
   :jsonparam string class_no: 수강번호 (강좌에 대한 신고사항이 아닐 경우엔 빈칸으로)
   :jsonparam string type: 오류의 종류 [ 1:강좌 , 2:서비스오류 , 3:건의사항 ]
   :jsonparam string reporter: 신고한 회원번호


   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 오류 없음
   :statuscode 400: 양식데이터가 올바르지 않음
   :statuscode 401: 사용자가 로그인하지 않은 상태
   :statuscode 403: 강좌가 이미 신고됐을 경우