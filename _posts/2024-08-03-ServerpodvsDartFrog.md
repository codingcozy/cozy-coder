---
title: "Serverpod vs Dart Frog 2024년 최신 백엔드 프레임워크 비교"
description: ""
coverImage: "/assets/img/2024-08-03-ServerpodvsDartFrog_0.png"
date: 2024-08-03 20:30
ogImage: 
  url: /assets/img/2024-08-03-ServerpodvsDartFrog_0.png
tag: Tech
originalTitle: "Serverpod vs Dart Frog"
link: "https://medium.com/serverpod/serverpod-vs-dart-frog-2507df893273"
---


우리는 Dart 백엔드 프레임워크인 Serverpod와 다른 인기 있는 Dart Frog를 비교해 보기로 했어요. 이를 위해 Frog의 웹사이트에서 Todos 예제를 복제했어요.

![image](/assets/img/2024-08-03-ServerpodvsDartFrog_0.png)

# 데이터 모델

Dart Frog는 데이터 직렬화를 위한 네이티브 방식을 제공하지 않아요. 대신, json_serializable와 같은 서드파티 패키지를 활용하여 모델을 읽고 쓰도록 의존하고 있어요. 이것은 모델에 로직을 포함하고 싶을 때 유용할 수 있지만, 다른 컴퓨터 언어 간에 작동하지는 않아요. 이것은 Todos 예제의 모델이에요.

<div class="content-ad"></div>

```js
// Dart Frog - 데이터 모델

import 'package:equatable/equatable.dart';
import 'package:json_annotation/json_annotation.dart';
import 'package:meta/meta.dart';

part 'todo.g.dart';

@immutable
@JsonSerializable()
class Todo extends Equatable {
  Todo({
    this.id,
    required this.title,
    this.description = '',
    this.isCompleted = false,
  }) : assert(id == null || id.isNotEmpty, 'id는 비어 있을 수 없습니다.');

  final String? id;

  final String title;

  final String description;

  final bool isCompleted;

  Todo copyWith({
    String? id,
    String? title,
    String? description,
    bool? isCompleted,
  }) {
    return Todo(
      id: id ?? this.id,
      title: title ?? this.title,
      description: description ?? this.description,
      isCompleted: isCompleted ?? this.isCompleted,
    );
  }

  static Todo fromJson(Map<String, dynamic> json) => _\$TodoFromJson(json);

  Map<String, dynamic> toJson() => _\$TodoToJson(this);

  @override
  List<Object?> get props => [id, title, description, isCompleted];
}
```

서버팟은 YAML 파일을 사용하여 읽기 쉬운 형태로 모델을 정의합니다. 모델로부터 생성된 Dart 코드에는 직렬화 메서드와 copyWith 메서드가 포함되어 있습니다. 또한, 서버팟의 장점 중 하나는 모델이 서버팟의 ORM을 통해 데이터베이스에 매핑될 수 있다는 것입니다. (원하는 경우에는 사용자 정의 직렬화도 지원합니다.)

```js
# Serverpod - 데이터 모델

class: Todo

fields:
  id: UuidValue?
  title: String
  description: String?
  isCompleted: bool
```

# 엔드포인트


<div class="content-ad"></div>

다음으로는 다트 프로그와 Serverpod를 사용하여 엔드포인트와 메소드를 작성하는 방법을 살펴보겠습니다. 두 프레임워크는 매우 다른 접근 방식을 취합니다. 다트 프로그는 직렬화와 HTTP 요청을 수동으로 처리해야 하지만, Serverpod는 원격 메소드 호출을 사용합니다.

다음은 다트 프로그의 Todos 예제에서의 코드입니다. 먼저, 미들웨어를 생성해야 합니다.

```js
// Dart Frog - Create middleware

import 'package:dart_frog/dart_frog.dart';
import 'package:in_memory_todos_data_source/in_memory_todos_data_source.dart';

final _dataSource = InMemoryTodosDataSource();

Handler middleware(Handler handler) {
  return handler
      .use(requestLogger())
      .use(provider<TodosDataSource>((_) => _dataSource));
}
```

그 다음으로는 /todos 경로를 설정합니다.

<div class="content-ad"></div>

```js
// Dart Frog - /todos 경로

import 'dart:async';
import 'dart:io';

import 'package:dart_frog/dart_frog.dart';
import 'package:todos_data_source/todos_data_source.dart';

FutureOr<Response> onRequest(RequestContext context) async {
  switch (context.request.method) {
    case HttpMethod.get:
      return _get(context);
    case HttpMethod.post:
      return _post(context);
    case HttpMethod.delete:
    case HttpMethod.head:
    case HttpMethod.options:
    case HttpMethod.patch:
    case HttpMethod.put:
      return Response(statusCode: HttpStatus.methodNotAllowed);
  }
}

Future<Response> _get(RequestContext context) async {
  final dataSource = context.read<TodosDataSource>();
  final todos = await dataSource.readAll();
  return Response.json(body: todos);
}

Future<Response> _post(RequestContext context) async {
  final dataSource = context.read<TodosDataSource>();
  final todo = Todo.fromJson(
    await context.request.json() as Map<String, dynamic>,
  );

  return Response.json(
    statusCode: HttpStatus.created,
    body: await dataSource.create(todo),
  );
}
```

마지막으로, /todos/`id` 경로가 있습니다.

```js
// Dart Frog - /todos/<id> route

import 'dart:async';
import 'dart:io';

import 'package:dart_frog/dart_frog.dart';
import 'package:todos_data_source/todos_data_source.dart';

FutureOr<Response> onRequest(RequestContext context, String id) async {
  final dataSource = context.read<TodosDataSource>();
  final todo = await dataSource.read(id);

  if (todo == null) {
    return Response(statusCode: HttpStatus.notFound, body: 'Not found');
  }

  switch (context.request.method) {
    case HttpMethod.get:
      return _get(context, todo);
    case HttpMethod.put:
      return _put(context, id, todo);
    case HttpMethod.delete:
      return _delete(context, id);
    case HttpMethod.head:
    case HttpMethod.options:
    case HttpMethod.patch:
    case HttpMethod.post:
      return Response(statusCode: HttpStatus.methodNotAllowed);
  }
}

Future<Response> _get(RequestContext context, Todo todo) async {
  return Response.json(body: todo);
}

Future<Response> _put(RequestContext context, String id, Todo todo) async {
  final dataSource = context.read<TodosDataSource>();
  final updatedTodo = Todo.fromJson(
    await context.request.json() as Map<String, dynamic>,
  );
  final newTodo = await dataSource.update(
    id,
    todo.copyWith(
      title: updatedTodo.title,
      description: updatedTodo.description,
      isCompleted: updatedTodo.isCompleted,
    ),
  );

  return Response.json(body: newTodo);
}

Future<Response> _delete(RequestContext context, String id) async {
  final dataSource = context.read<TodosDataSource>();
  await dataSource.delete(id);
  return Response(statusCode: HttpStatus.noContent);
}
```

그래서, 이러한 작업을 Serverpod로 어떻게 수행합니까? Serverpod에서는 엔드포인트 메서드를 Endpoint에 추가합니다. Serverpod는 서버 코드를 분석하여 필요한 서버 측 코드를 자동으로 생성합니다. 이 예제에서는 Dart Frog 예제와 동일하게 InMemoryTodosDataSource를 사용합니다.


<div class="content-ad"></div>

```js
import 'package:serverpod/serverpod.dart';
import 'package:todo_server/src/business/in_memory_data_source.dart';
import 'package:todo_server/src/generated/protocol.dart';

final _dataSource = InMemoryTodosDataSource();

class TodoEndpoint extends Endpoint {
  Future<Todo> create(Session session, Todo todo) async {
    return _dataSource.create(todo);
  }

  Future<List<Todo>> getAll(Session session) async {
    return _dataSource.readAll();
  }

  Future<Todo?> get(Session session, UuidValue id) async {
    return _dataSource.read(id);
  }

  Future<Todo> update(Session session, Todo todo) async {
    return _dataSource.update(todo.id!, todo);
  }

  Future<void> delete(Session session, UuidValue id) async {
    return _dataSource.delete(id);
  }
}
```

Serverpod을 사용하는 장점 중 하나는 모든 것이 엄격히 유형화되어 있다는 것입니다. 또한 Serverpod는 이미 날짜, UUID, 이진 데이터와 같은 일반 유형을 직렬화하는 방법을 이미 알고 있으므로 별도의 설정을 하지 않아도 됩니다.

# Flutter에서 서버 호출

Dart Frog은 Flutter에서 서버를 호출할 때 거의 도움이 되지 않습니다. 이를 위해 HTTP 호출을 수행하고 데이터를 직렬화해야 합니다.

<div class="content-ad"></div>

```js
// Dart Frog - 클라이언트 코드

var todo = Todo(
  title: '내 할일',
  isCompleted: false,
);

var url = Uri.parse('http://localhost:8080/todos');
var headers = {'Content-Type': 'application/json'};
var body = jsonEncode(todo.toJson());

var response = await http.post(
  url,
  headers: headers,
  body: body,
);

var newTodo = Todo.fromJson(jsonDecode(response.body));
```

한편, Serverpod는 자동으로 클라이언트 패키지를 생성해줍니다. 따라서 엔드포인트 메소드를 호출하는 것은 로컬 메소드를 호출하는 것만큼 쉬워집니다. 사용자가 생성한 모델은 플러터 앱에서도 접근할 수 있습니다.

```js
// Serverpod - 클라이언트 코드

var todo = Todo(
  title: '내 할일',
  isCompleted: false,
);

var newTodo = await client.todo.create(todo);
```

# 결론


<div class="content-ad"></div>

Serverpod과 Dart Frog는 Dart 백엔드를 설계하고 구축하는 데 매우 다른 방식을 취합니다. Dart Frog는 서버 엔드포인트를 구조화하는 데 추가적인 유연성을 제공할 수 있지만, Serverpod는 백엔드를 최소한의 코드로 설정하는 실용적이고 견고한 방법을 제공합니다.