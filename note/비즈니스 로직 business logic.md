---
title: 비즈니스 로직 business logic
date: 2022-10-12T17:44:51+09:00
last_modified_at: 2022-10-12T17:55:02+09:00
---
비즈니스 로직 또는 도메인 로직이라고 부른다. 데이터가 어떻게 생성되고, 저장되고, 변경되어야 하는지에 대한 real world 비즈니스 규칙을 작성하는 프로그램 일부를 말한다. 3 레이어 아키텍처에서 중간에 해당한다.[^wiki]

[^wiki]: https://en.wikipedia.org/wiki/Business_logicBusiness_logicBusiness_logic

![3 Layered](https://softwareontheroad.com/static/122dab3154cb7e417bbb210bbce7ca01/62eec/server_layers.jpg)

![3 Layered as Express](https://softwareontheroad.com/static/1a21f74cfc4c965f00324afd39642b9f/384f8/server_layers_2.webp)

3 layered architecture 에서, 관심사를 기준으로 나누면 다음과 같은듯.

레이어|관심사
--:|:--
Presentation|Req, Res
Business Logic|req, res, DB 접근 없는 순수 비즈니스 로직
Data Access |DB 접근

