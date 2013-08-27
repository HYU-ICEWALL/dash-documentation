신고
****

이 페이지에서는 사용자의 신고 항목들을 처리하는 API를 다룬다.

.. _report-object:

신고 항목 객체
==============

신고 항목들을 처리하는 API는 신고 항목의 정보가 포함된 객체 혹은 이것의 배열들을 사용한다. 신고 항목 객체에는 다음과 같은 속성이 있다.

   * `id` – 신고 항목의 ID
   * `type` – 신고 항목의 종류. 다음 중 하나의 값을 갖는다.

      * ``classinfo`` – 강좌 정보 오류
      * ``service_err`` – 기타 서비스 오류
      * ``idea`` – 건의 사항

   * `class_no` – 선택적임. `type` 이 ``classinfo`` 인 경우, 오류가 있는 강좌의 번호.
   * `content` – 신고 항목의 내용
   * `status` – 신고 항목의 처리 상태. 다음 중 하나의 값을 갖는다.

      * ``added`` – 신고 항목이 추가됨. 관리자가 아직 접수하지 않은 상태.
      * ``accepted`` – 신고 항목이 관리자에 의해 접수됨. 관리자가 아직 처리하지 않은 상태.
      * ``solved`` – 신고 항목이 관리자에 의해 해결됨.
      * ``rejected`` – 신고 항목이 관리자에 의해 거부됨.

   * `created_by` – 신고 항목을 생성한 사용자의 ID
   * `created_time` – 신고 항목을 생성한 날짜와 시각. ISO-8601 date-time을 포함하는 문자열이다.

관리자를 위한 API
=================================

.. http:get:: /api/reports
   
   신고 항목의 목록을 받아옴

   **요청 예시**:

   .. sourcecode:: http

      GET /api/reports?type=1&page=1&limit=10 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript

   :query type: 선택적임. 받아올 신고 항목의 종류. 가능한 값은 :ref:`report-object` 의 `type` 참조.
   
   :query limit: 페이지 당 신고 항목의 개수
   :query page: 받아올 신고 항목 목록의 페이지 번호
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json
   
      [
        {
           "id": "123",
           "class_no": "10443",
           "type": "classinfo",
           "content": "강의 시간이 맞지 않아요",
           "status": "added",
           "created_time": "2013-08-25T09:28:28+0000",
           "created_by": "12"   
        },
        {
           "id": "124",
           "type": "idea",
           "content": "강의평가 커뮤니티가 있으면 좋겠어요",
           "status": "accepted",
           "created_time": "2013-08-26T04:28:28+0000",
           "created_by": "21"
        }
      ]

   :statuscode 200: 신고 목록 받아오기 성공
   :statuscode 400: 쿼리 파라미터가 올바르지 않음
   :statuscode 404: 신고 목록을 받아올 권한이 없음

.. http:get:: /api/reports/(id)
   
   ID가 `id` 인 신고 항목

   **요청 예시**:

   .. sourcecode:: http

      GET /api/reports/123 HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript
   
   :param id: 신고 항목의 ID
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "id": "123",
        "class_no": "10443",
        "type": "classinfo",
        "content": "강의 시간이 맞지 않아요",
        "status": "added",
        "created_time": "2013-08-25T09:28:28+0000",
        "created_by": "12"
      }
      

   :statuscode 200: 신고 항목 받아오기 성공
   :statuscode 404: 신고 항목의 정보를 받아올 권한이 없음


.. http:put:: /api/reports/(id)
   
   특정 신고 항목의 정보 수정

   **요청 예시**:

   .. sourcecode:: http

      PUT /api/reports/124 HTTP/1.1
      Host: example.com
      Content-Type: application/json

      {
        "id": "124",
        "type": "idea",
        "content": "강의평가 커뮤니티가 있으면 좋겠어요",
        "status": "rejected",
        "created_time": "2013-08-26T04:28:28+0000"
        "created_by": "21"
      }

   JSON 파라미터에 대한 정보는 :ref:`report-object` 참조.

   :param id: 신고 항목의 ID
   :reqheader Content-Type: ``application/x-www-form-urlencoded``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Location: http://example.com/api/reports/124

   :resheader Location: 신고 항목이 성공적으로 수정되었을 때, 수정된 신고 항목의 링크
   :statuscode 200: 신고 항목의 정보 편집 성공
   :statuscode 400: 신고 항목의 정보가 올바르지 않음
   :statuscode 404: 신고 항목의 정보를 수정할 권한이 없음

.. http:delete:: /api/reports/(id)
   
   특정 신고 항목을 삭제함

   **요청 예시**:

   .. sourcecode:: http

      DELETE /api/reports/124 HTTP/1.1
      Host: example.com

   :param id: 신고 항목의 ID

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 신고 항목 삭제 성공
   :statuscode 400: 신고 항목의 ID가 올바르지 않음
   :statuscode 404: 신고 항목을 삭제할 권한이 없음

사용자를 위한 API
==========================

.. http:post:: /api/reports
   
   신고 항목 생성

   **요청 예시**:

   .. sourcecode:: http

      POST /api/reports HTTP/1.1
      Host: example.com
      Content-Type: application/json
      
      {
        "type": "classinfo",
        "class_no": "10443",
        "content": "강의 시간이 맞지 않아요",
        "created_by": "12"
      }

   요청 바디에는 `type`, `class_no`, `content`, `created_by` 속성을 가진 JSON 객체를 전달한다.
   단, `type` 이 ``classinfo`` 가 아닌 경우에는 `class_no` 속성이 존재하지 않아야 한다.
   각 속성에 대한 정보는 :ref:`report-object` 참조.

   :reqheader Content-Type: ``application/json``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 신고 항목 생성 성공
   :statuscode 400: 신고 항목 정보가 올바르지 않음
   :statuscode 403: `type` 이 ``classinfo`` 인 경우, 
                    다른 사용자가 이미 같은 강좌에 대해 문제를 신고함
   :statuscode 404: 신고 항목을 생성할 권한이 없음
