---
title: "[flutter] http 패키지 사용하여 통신하기"
date: 2024-05-20 00:00:00 +0800
categories: [flutter, package]
tags: [http]
render_with_liquid: false
---

flutter http 패키지를 사용하여 API 호출하기<br>
패키지 : [http](https://pub.dev/packages/http)

캡스톤으로 진행하고 있는 학사관리 앱에서 http 통신을 이용하여 개인정보와 게시판을 관리 해보려고 한다.

## 설정


우선 __http__ 패키지를 설치한다.
```
flutter pub add http
```

__pubspec.yaml__ 파일에 __http__ 패키지를 추가한다.
```
dependencies:
  flutter:
    sdk: flutter
  http: ^0.13.3  # 최신 버전으로 변경
```

## 사용법

test