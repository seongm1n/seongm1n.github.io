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
<br><br>

GET 요청은 주어진 URL로부터 데이터를 가져오는 데 사용


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

