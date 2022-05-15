---
layout: post
title: 프로젝트 생성, 라이브러리 살펴보기, View 환경 설정
date: 2022-05-16
category: Spring
---

# 스프링 프로젝트 생성

[https://start.spring.io/](https://start.spring.io/)에서 스프링 프로젝트를 만들 수 있다.

- <div style="font-weight:600">Project: Maven 또는 Gradle</div>
  - 필요한 라이브러리를 가져와 주고, build라이프 사이클을 관리해주는 툴
  - Gradle사용
  <div style="margin-bottom:12px">
- <div style="font-weight:600">Language</div>
  - Java 선택
  <div style="margin-bottom:12px">
- <div style="font-weight:600">Spring Boot</div>
  - 2022.05.16 기준 2.6.7 다운로드
  <div style="margin-bottom:12px">
- <div style="font-weight:600">Project Metadata</div>
  - groupId: 보통 기업 도메인명
  - arifactId: 빌드되어 나온 결과물로 프로젝트명과 비슷하다.
  <div style="margin-bottom:12px">
- <div style="font-weight:600">Dependencies</div>
    - 스프링 부트 기반으로 프로젝트를 시작할 때, 어떤 라이브러리를 가져와 쓸 것인지 설정
  - Spring Web : 웹 프로젝트를 만들때 필수
  - Thymeleaf : HTML 템플릿을 만들어주는 라이브러리
  <div style="margin-bottom:12px">

# 기본 파일 구조

- .idea: intelliJ가 사용하는 설정파일 폴더
- gradle 및 wrapper: gradle이 사용하는 설정 파일 폴더
- src

  - main

    - java: 실제 패키지와 소스파일이 들어있다.
    - resources

  - test: 테스트코드와 관련된 파일

<!-- # Controller 컨트롤러

- Spring MVCfmf -->
