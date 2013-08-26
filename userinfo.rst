사용자 정보
***********

이 페이지에서는 사용자 정보를 처리하는 API를 다룬다.

로그인하지 않은 사용자를 위한 API
=================================

.. http:post:: /api/users
   
   새로운 사용자를 생성.

   **요청 예시**:

   .. sourcecode:: http

      POST /api/users HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript

      {
        "email": "somebody@example.com",
        "password": "1234",
        "major": "MAJORCODE",
        "fb_id": "facebook.id"
      }

   :jsonparam string email: 사용자의 이메일 주소
   :jsonparam string password: 사용자의 암호
   :jsonparam string major: 사용자의 소속 전공
   :jsonparam string fb_id: 선택적인(optional) 사용자의 Facebook 계정 ID
   :reqheader Content-Type: ``application/json``
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 201 Created
      Location: http://example.com/api/users/me

   :statuscode 201: 사용자 생성 성공 및 로그인 완료.
                    ``Location`` 헤더에 새로운 사용자의 정보가 담긴 링크를 전송.
   :statuscode 400: 양식 데이터가 올바르지 않음

.. http:post:: /api/reset_password
   
   폼 파라미터로 주어진 이메일 주소의 계정이 존재하면, 이 주소로 암호를 재설정할 수 있는 메일을 전송.

   **요청 예시**:

   .. sourcecode:: http

      POST /api/reset_password HTTP/1.1
      Host: example.com
      Content-Type: application/x-www-form-urlencoded

      email=somebody%40example.com

   :formparam email: 사용자의 계정에 등록된 이메일 주소
   :reqheader Content-Type: ``application/x-www-form-urlencoded``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 계정 찾기에 성공하고 메일 전송을 완료함
   :statuscode 404: 계정 찾기에 실패함
   :statuscode 500: 계정 찾기에는 성공했으나 메일 전송에 실패함

.. http:post:: /api/login
   
   사용자를 로그인 시킴.

   **요청 예시**:

   .. sourcecode:: http

      POST /api/login HTTP/1.1
      Host: example.com
      Content-Type: application/x-www-form-urlencoded

      email=somebody%40example.com&password=1234

   :formparam email: 사용자의 계정에 등록된 이메일 주소
   :formparam password: 사용자의 계정의 암호
   :reqheader Content-Type: ``application/x-www-form-urlencoded``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Location: http://example.com/api/users/me

   :statuscode 200: 사용자 인증에 성공.
                    ``Location`` 헤더에 사용자의 정보가 담긴 링크를 전송.
   :statuscode 404: 사용자 인증에 실패

로그인한 사용자를 위한 API
==========================

.. http:get:: /api/users/me
   
   현재 로그인한 사용자의 정보.

   **요청 예시**:

   .. sourcecode:: http

      GET /api/users/me HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript
   
   :reqheader Accept: ``application/json`` 혹은 ``text/javascript``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json

      {
        "id": "123",
        "email": "somebody@example.com",
        "major": "MAJORCODE",
        "fb_id": "facebook.id"
      }

   :resheader Content-Type: ``application/json``
   :statuscode 200: 오류 없음
   :statuscode 404: 사용자가 로그인하지 않은 상태

.. http:put:: /api/users/me
   
   현재 로그인한 사용자의 정보를 편집.

   **요청 예시**:

   .. sourcecode:: http

      PUT /api/users/me HTTP/1.1
      Host: example.com
      Content-Type: application/json
      Accept: application/json, text/javascript

      {
        "email": "somebody@example.com",
        "password": "4321",
        "major": "MAJORCODE",
        "fb_id": "facebook.id"
      }

   :jsonparam string email: 사용자의 이메일 주소
   :jsonparam string password: 사용자의 암호
   :jsonparam string major: 사용자의 소속 전공
   :jsonparam string fb_id: 사용자의 Facebook 계정 ID
   :reqheader Content-Type: ``application/json``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Location: http://example.com/api/users/me

   :statuscode 200: 사용자 정보 편집 성공.
                    ``Location`` 헤더에 사용자의 정보가 담긴 링크를 전송.
   :statuscode 400: 양식 데이터가 올바르지 않음

.. http:delete:: /api/users/me
   
   현재 로그인한 사용자를 삭제.

   **요청 예시**:

   .. sourcecode:: http

      DELETE /api/users/me HTTP/1.1
      Host: example.com
      Content-Type: application/json

      {"password": "4321"}

   :jsonparam string password: 사용자의 암호
   :reqheader Content-Type: ``application/json``

   **응답 예시**:

   .. sourcecode:: http

      HTTP/1.1 200 OK

   :statuscode 200: 사용자 삭제 성공
   :statuscode 404: 암호가 틀림
