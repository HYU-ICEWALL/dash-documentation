전공
****

이 페이지에서는 전공 정보를 처리하는 API를 다룬다.

.. _major-object:

전공 객체
=========

전공들을 처리하는 API는 전공 정보가 포함된 객체 혹은 이것의 배열들을 사용한다. 전공 객체에는 다음과 같은 속성이 있다.

   * `name` – 전공 이름
   * `code` – 전공 코드

전공 API
========

.. http:get:: /api/majors
  
   해당 학교의 학과 목록
 
   **요청 예시**:
 
   .. sourcecode:: http
 
      GET /api/majors HTTP/1.1
      Host: example.com
      Accept: application/json, text/javascript
 
   :reqheader Accept: ``application/json` 혹은 `text/javascript``
 
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
 
   :resheader Content-Type: ``application/json``
   :statuscode 200: 전공 목록 받아오기 성공
   :statuscode 404: 전공 목록을 받아오는 데 실패함
 