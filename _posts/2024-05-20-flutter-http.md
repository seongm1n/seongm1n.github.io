---
title: "[flutter] http 패키지 사용하여 통신하기"
date: 2024-05-20 00:00:00 +0800
categories: [flutter, package]
tags: [http]
render_with_liquid: false
---

flutter http 패키지를 사용하여 API 호출하기<br>
패키지 : [http](https://pub.dev/packages/http)

캡스톤으로 진행하고 있는 학사관리 앱 d에서 http 통신을 이용하여 개인정보와 게시판을 관리 해보려고 한다.

<br><br>

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
<br><br><br>
## 사용법

### GET 요청

```dart
class GetDataWidget extends StatefulWidget {
  @override
  _GetDataWidgetState createState() => _GetDataWidgetState();
}

class _GetDataWidgetState extends State<GetDataWidget> {
  String data = "Loading...";

  @override
  void initState() {
    super.initState();
    fetchData();
  }

  Future<void> fetchData() async {
    final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1'));

    if (response.statusCode == 200) {
      setState(() {
        data = json.decode(response.body)['title'];
      });
    } else {
      setState(() {
        data = "Failed to load data";
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Text(data);
  }
}
```

GET 요청은 주어진 URL로부터 데이터를 가져오는 데 사용

<br><br>

### POST 요청

```dart
class PostDataWidget extends StatefulWidget {
  @override
  _PostDataWidgetState createState() => _PostDataWidgetState();
}

class _PostDataWidgetState extends State<PostDataWidget> {
  String responseMessage = "No Response";

  Future<void> sendData() async {
    final response = await http.post(
      Uri.parse('https://jsonplaceholder.typicode.com/posts'),
      headers: <String, String>{
        'Content-Type': 'application/json; charset=UTF-8',
      },
      body: jsonEncode(<String, String>{
        'title': 'foo',
        'body': 'bar',
        'userId': '1',
      }),
    );

    if (response.statusCode == 201) {
      setState(() {
        responseMessage = "Success: ${response.body}";
      });
    } else {
      setState(() {
        responseMessage = "Failed: ${response.body}";
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        Text(responseMessage),
        ElevatedButton(
          onPressed: sendData,
          child: Text('Send POST Request'),
        ),
      ],
    );
  }
}

```

POST 요청은 서버에 데이터를 전송하는 데 사용

## 적용
```dart
import 'package:http/http.dart' as http;
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

const String server = "";
const String server2 = "";

class UserProfile {
  final String userName;
  final String department;
  final String year;
  final String studentId;
  final int graduationScore;
  final int maxScore;
  final List<String> skills;
  final Map<String, bool> subjects;
  final String? profileImage;

  UserProfile({
    required this.userName,
    required this.department,
    required this.year,
    required this.studentId,
    required this.graduationScore,
    required this.maxScore,
    required this.skills, // 스킬 넣는거임 ex) 파이썬 이런거
    required this.subjects, // 내가 수강한 과목 넣기!
    required this.profileImage,
  });

  // JSON 데이터를 UserProfile 객체로 변환하기 위한 factory constructor 추가
  factory UserProfile.fromJson(Map<String, dynamic> json) {
    return UserProfile(
      userName: json['userName'] ?? '',
      department: json['department'] ?? '',
      year: json['year'] ?? '',
      studentId: json['studentId'] ?? '',
      graduationScore: json['graduationScore'] ?? 0,
      maxScore: json['maxScore'] ?? 0,
      skills: List<String>.from(json['skills'] ?? ["파이썬"]),
      subjects: Map<String, bool>.from(
          json['subjects'] ?? {"자료구조": true, "알고리즘": true, "캡스톤디자인": true}),
      profileImage: json['profileImage'],
    );
  }
}

// 전역 UserProfile 인스턴스
UserProfile userProfile = UserProfile(
  userName: '김성민',
  department: '컴퓨터공학과',
  year: '4-1',
  studentId: '20180595',
  graduationScore: 30,
  maxScore: 1000,
  skills: ["파이썬"],
  subjects: {"자료구조": true, "알고리즘": true, "캡스톤디자인": true},
  profileImage: null,
);

Future<bool> sendLoginRequest(String studentId, String password) async {
  final url = Uri.parse('$server2/member/login');
  final headers = {"Content-Type": "application/json"};
  final body = jsonEncode({"studentId": studentId, "memberPassword": password});

  final response = await http.post(url, headers: headers, body: body);

  if (response.statusCode == 200) {
    print('Login successful');
    final responseBody = json.decode(utf8.decode(response.bodyBytes));
    print('response : $responseBody');

    final token = response.headers['authorization'];
    if (token != null) {
      final SharedPreferences prefs = await SharedPreferences.getInstance();
      await prefs.setString('token', token);
      print('Token saved: $token');
    }
    userProfile = UserProfile.fromJson(responseBody['useData']);
    return true; // 로그인 성공 시 true 반환
  } else {
    print('Login failed: ${response.statusCode}');
    print('Response body: ${response.body}');
    return false; // 로그인 실패 시 false 반환
  }
}
```
json으로 데이터를 받아올 때 한글이 깨지는 에러가 생겨 decode로 고쳤다.

로그인 정보를 보내고 사용자 정보를 서버에서 받아왔다. 로그인할때 토큰도 받아와서 다른 정보를 요청할때 사용할 수 있는데 서버를 공부해야 완벽하게 이해 할 수 있을 것 같다.