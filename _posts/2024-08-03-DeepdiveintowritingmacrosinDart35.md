---
title: "Dart 35에서 매크로 작성 깊이 파헤치기"
description: ""
coverImage: "/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_0.png"
date: 2024-08-03 20:31
ogImage: 
  url: /assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_0.png
tag: Tech
originalTitle: "Deep dive into writing macros in Dart 35"
link: "https://medium.com/@alexey.inkin/deep-dive-into-writing-macros-in-dart-3-5-a1dd50914a7d"
---


한 달 동안 전업으로 마크로를 만지작거리며 보냈는데, 여기서 빠르게 시작할 수 있는 모든 것이 있어요.

첫 번째 부분에서는 Dart의 베타 버전을 설정하여 마크로를 실험하고, Dart 팀이 공개한 @JsonCodable 마크로를 시연용으로 사용해본 뒤 'hello-world' 마크로를 작성해봤어요.

두 번째 부분에서는 명령행 인수를 위한 파서를 생성하는 내가 만든 정식 마크로에 대해 설명할 거에요. 이를 통해 마크로를 작성하고 테스트하는 모든 측면을 설명하는 예제로 사용할 거예요.

이것은 첫 번째 부분을 읽었다는 것을 나타냅니다.

<div class="content-ad"></div>

경고: 이 전체 Dart 기능은 현재 미리보기 상태이며 성숙 단계에 이르기 전에 많은 변경사항이 있을 것입니다. 기다리기만 하기에는 너무 재미있었어요. 

# 시작할 코드 선택

코드 생성을 위해서는 원하는 생성 코드의 예시를 수동으로 작성한 다음 생성기를 작성하는 방식으로 시작하세요.

우리의 경우, 다음 클래스에 매크로를 적용하고 싶습니다:

<div class="content-ad"></div>

Markdown 형식으로 테이블 태그를 변경해 보겠습니다.

<div class="content-ad"></div>

# 매크로 적용 단계

클래스에 매크로를 적용할 때, 모든 코드를 한 번에 실행하고 생성할 수는 없습니다. 하나의 클래스를 수정하는 여러 매크로가 있을 수 있기 때문입니다. 하나의 매크로가 다른 매크로에 의해 생성된 코드를 볼 수 있고 수정할 수 있을까요? 어떤 순서로 발생할까요? 이러한 문제를 해결하기 위해 단계가 도입되었습니다. 총 3개의 단계가 있습니다: 형식 생성, 선언 및 정의입니다.

## 형식 생성 단계

이것이 첫 번째 단계입니다. 이 단계에서 매크로는 클래스, 믹스인, 열거형, typedef 등 새로운 형식을 소개하는 코드를 생성할 수 있습니다. 이 단계에서 매크로는 프로그램의 다른 형식을 볼 수 있지만, 검사할 수는 없습니다. 왜냐하면 다른 매크로에 의해 생성된 다른 형식에 의해 그 형식이 가려질 수 있기 때문입니다.

<div class="content-ad"></div>

## 선언 단계

선언 단계가 시작될 때에는 모든 유형이 생성되었고 새로운 유형을 도입할 수 없습니다. 이는 각 유형 이름이 정확히 어떤 것을 가리키는지 볼 수 있게 해줍니다. 이 능력으로 매크로는 이제 모든 클래스의 필드와 메소드를 확인할 수 있습니다. 기존 유형에 새로운 필드와 메소드를 생성할 수 있습니다. 그러나 여전히 프로그램에서 볼 수 없는 것들이 있습니다. 예를 들어 다음 코드에서 변수의 유형을 추론할 수 없습니다.

```js
final a = b;
```

만약 다른 매크로가 b라는 이름의 게터를 소개하면 여전히 b가 가려질 수 있습니다.

<div class="content-ad"></div>

## 정의 단계

정의 단계가 시작될 때 모든 유형의 모든 선언이 완료되고 더 이상 섀도우 처리할 수 있는 것이 없습니다. 이 시점에서 모든 매크로는 프로그램에 대한 최대 가시성을 갖게 됩니다. 그러나 그들이 할 수 있는 일은 기존 메서드의 본문 및 기존 변수의 초기화자를 대체하는 것 뿐입니다.

따라서 단계는 최소 가시성에서 최대 권한을 갖추어 가장 낮은 권한으로 실행됩니다. 이러한 제약 사항은 여러 매크로가 서로를 인식하지 못하게 하고 독립적으로 실행되어 실행 순서에 대해 대부분 무관하게 만듭니다.

매크로는 어떤 단계에서 실행하려는지 결정할 수 있습니다. 이를 위해 미리 정의된 인터페이스 중 하나 이상을 구현함으로써 수행합니다.

<div class="content-ad"></div>

# 단계 및 인터페이스 선택

우리의 경우 매크로는 다음과 같은 클래스에 적용될 수 있어야 합니다:

```js
@Args()
class HelloArgs {
  final String name;
  final int count;
}
```

이 클래스를 생성해야 합니다. HelloArgsParser로 ArgParser 객체를 표준 args 패키지에서 생성하고 각 필드에 대해 옵션을 생성해야합니다.

<div class="content-ad"></div>

이는 매크로가 3개의 가능한 단계 중 2개에서 실행되어야 함을 의미합니다:

- HelloArgsParser 클래스를 생성해야 하므로 타입 단계에서 실행되어야 합니다.
- 적용된 클래스의 필드를 자세히 살펴봐야 하므로 선언 또는 정의 단계에서도 실행되어야 합니다. 후자는 나중에 설명할 것이므로 선언 단계를 선택합니다.

이는 매크로 클래스가 다음 두 인터페이스를 구현해야 함을 의미합니다:

```js
macro class Args implements ClassTypesMacro, ClassDeclarationsMacro {
  @override
  Future<void> buildTypesForClass(
    ClassDeclaration clazz,
    ClassTypeBuilder builder,
  ) async {
    // 여기서 파서 클래스를 생성합니다.
  }

  @override
  Future<void> buildDeclarationsForClass(
    ClassDeclaration clazz,
    MemberDeclarationBuilder builder,
  ) async {
    // 옵션을 사용하여 파서를 채우고 데이터를 파싱합니다.
  }
}
```

<div class="content-ad"></div>

세 번째 단계를 위한 인터페이스는 ClassDefinitionsMacro라고 하며, 지금은 필요하지 않습니다.

만약 매크로를 enum에 적용하고 싶다면 EnumTypesMacro, EnumDeclarationsMacro, EnumDefinitionsMacro 인터페이스를 사용해야 합니다. 코드에서 다른 항목에 적용 가능한 매크로를 위한 많은 인터페이스도 있습니다. 이를 API에서 모두 찾을 수 있습니다.

각 메소드가 두 개의 매개변수를 가져야 함을 확인할 수 있습니다:

- 매크로가 적용된 클래스의 선언.
- 해당 단계에 대한 빌더 객체로, 프로그램을 검사하고 수정하는 방법입니다. 이것이 단계의 가시성과 권한이 부여되는 방식입니다.

<div class="content-ad"></div>

# 프로그래밍 방식으로 클래스 생성하기

간단합니다:

```js
@override
Future<void> buildTypesForClass(
  ClassDeclaration clazz,
  ClassTypeBuilder builder,
) async {
  final name = clazz.identifier.name;
  final parserName = _getParserName(clazz);

  builder.declareType(
    name,
    DeclarationCode.fromString('class $parserName {}\n'),
  );
}

String _getParserName(ClassDeclaration clazz) {
  final name = clazz.identifier.name;
  return '${name}Parser';
}
```

처음에는 이 클래스를 비어있는 상태로 만듭니다. 여기서 메서드와 속성을 생성할 수는 있지만, 우리는 두 번째 단계에서만 알 수 있는 데이터에 크게 의존하므로, 한 곳에서 모든 메서드를 생성하는 것이 더 타당합니다.

<div class="content-ad"></div>

# 클래스 확장

선언 단계에서는 다음을 수행해야 합니다:

- 현재 클래스의 필드에 대해 모두 학습합니다.
- 생성자를 추가합니다.
- 빈 구문 분석기 클래스를 확장하여 옵션을 구문 분석할 수 있도록 합니다.

```js
@override
Future<void> buildDeclarationsForClass(
  ClassDeclaration clazz,
  MemberDeclarationBuilder builder,
) async {
  final intr = await _introspect(clazz, builder);

  await _declareConstructor(clazz, builder);
  _augmentParser(builder, intr);
}
```

<div class="content-ad"></div>

자기 성찰은 긴 이야기에요. 우선, intr 변수에 필요한 필드 정보가 모두 있는 것으로 가정하고 다른 것들을 설정해 보죠.

# 생성자 추가하기

우리의 데이터 클래스에는 필드만 있고 생성자가 없어요. 하나 추가해 볼게요.

저는 그것을 처리하고 많은 특별한 경우를 신경 써주는 @Constructor() 매크로를 만들었어요. Args 매크로에서는 이것만 하면 돼요:

<div class="content-ad"></div>

```js
미래<void> _declareConstructor(
  ClassDeclaration clazz,
  MemberDeclarationBuilder builder,
) async {
  await const Constructor().buildDeclarationsForClass(clazz, builder);
}
```

이렇게 매크로가 직접 다른 매크로를 호출하여 작업의 일부를 수행할 수 있습니다. 해당 매크로에 동일한 빌더를 전달하므로, 생성하는 코드는 우리가 직접 생성한 것과 마찬가지로 좋습니다.

# 반복 작업 설정

필드를 둘 이상 반복해야합니다:  

<div class="content-ad"></div>

- 각 필드마다 표준 ArgParser에 옵션을 추가합니다.
- 옵션에 대한 구문 분석된 값으로 데이터 개체를 구성합니다.

각 반복에서 각 필드의 동작은 해당 유형에 따라 달라지므로 Visitor 패턴이 적절합니다. 이 패턴은 동작을 클래스로 잘 추출하고 필드를 반복할 때 항상 모든 유형을 올바르게 다루고 있다는 컴파일 시간 보장을 제공합니다.

우리는 이 방문자 클래스로 시작하겠습니다:

```js
abstract class ArgumentVisitor<R> {
  R visitInt(IntArgument argument);
  R visitString(StringArgument argument);
}
```

<div class="content-ad"></div>

그리고 두 가지 타입에 대한 두 클래스에 대한 매크로드를 사용해주세요:

```js
sealed class Argument {
  Argument({
    required this.intr,
    required this.optionName,
  });

  final FieldIntrospectionData intr;
  final String optionName;

  R accept<R>(ArgumentVisitor<R> visitor);
}

class IntArgument extends Argument {
  IntArgument({
    required super.intr,
    required super.optionName,
  });

  @override
  R accept<R>(ArgumentVisitor<R> visitor) {
    return visitor.visitInt(this);
  }
}

// StringArgument에 대해서도 동일합니다.
```

# 자기 소개

## 자기 설명을 돕는 도우미 객체 구축

<div class="content-ad"></div>

우리는 여러 가지 정보를 수집해야 합니다: 데이터 클래스의 필드, 코드를 생성할 때 사용할 표준 식별자 등등에 관한 정보입니다.

좋은 방법은 매크로가 필요로하는 모든 정보를 가진 클래스를 만들고, 그것을 별도로 넘기는 대신 우리의 메서드들에게 한 번에 단일 인수로 전달하는 것입니다.

이 클래스를 'IntrospectionData'라고 부를 것입니다:

```js
Future<IntrospectionData> _introspect(
  ClassDeclaration clazz,
  MemberDeclarationBuilder builder,
) async {
  final fields = await builder.introspectFields(clazz);
  final ids = await ResolvedIdentifiers.resolve(builder);
  final arguments = await _fieldsToArguments(fields, builder);

  return IntrospectionData(
    arguments: arguments,
    clazz: clazz,
    fields: fields,
    ids: ids,
  );
}
```

<div class="content-ad"></div>

데이터 클래스의 모든 필드를 검사하는 것으로 시작됩니다. 이 일을 제대로 처리하는 데는 많은 작업이 필요합니다. 이 작업은 일반적이고 지루해서 macro_util 패키지로 이 작업을 추출했습니다. args macro에서는 다음과 같이 한 줄만 필요하며 각 필드에 대한 자세한 정보를 얻을 수 있습니다:

```js
final fields = await builder.introspectFields(clazz);
```

이 확장 메소드가 어떻게 작동하는지 알아봅시다. 그러면 안심하고 패키지를 사용할 수 있습니다.

## 필드 반복하기

<div class="content-ad"></div>

만약 ClassDeclaration이 있으면 다음과 같이 필드를 반복할 수 있습니다:

```js
final List<FieldDeclaration> fields = await builder.fieldsOf(type);

for (final field in fields) {
  // 작업 수행.
}
```

## FieldDeclaration

이 클래스는 빌더에서 제공한 필드에 대한 정보를 포함합니다. 일반적으로 필드가 선언된 정보를 모두 가지고 있습니다. hasConst, hasFinal, hasStatic, hasInitializer 등과 같은 속성들이 있습니다. 우리에게 가장 중요한 것은 TypeAnnotation 객체를 반환하는 type 속성입니다.

<div class="content-ad"></div>

## TypeAnnotation

이는 전담 깊은 살펴보기 없이 필드 유형에 대해 알려진 것을 나타내는 베이스 클래스입니다. 이 클래스는 추상 클래스이며 필드 선언이 어떻게 보이는지에 따라 얻는 내용이 다릅니다.

Foo foo;와 같은 필드를 분석할 때에는 Foo에 대한 참조를 가지고 있는 NamedTypeAnnotation이 반환됩니다. 그것이 무엇인지는 알 수 없습니다.

var a = 1;와 같은 필드를 분석할 때에는 유용하지 않지만 세 번째 단계에 있을 때 유형을 추론할 수 있는 핸들인 OmittedTypeAnnotation이 반환됩니다.

<div class="content-ad"></div>

"final (a, b) = getRecord();"와 같은 특이한 선언을 위한 여러 하위 클래스가 많이 있지만, 우리는 신경 쓰지 않습니다.

그냥 각 필드의 유형이 NamedTypeAnnotation이라고 확인하고, 그렇지 않으면 중단합니다.

## NamedTypeAnnotation

NamedTypeAnnotation이 TypeAnnotation에 추가하는 가장 중요한 요소는 식별자 속성입니다. 
Foo foo;와 같은 선언에 대해 Identifier 객체를 반환합니다.

<div class="content-ad"></div>

## 식별자

이 클래스의 객체는 코드에서 각 식별자를 나타냅니다. 기본적으로, 이름과 무언가로 해석될 것을 함축합니다.

Foo foo; 라는 선언에서, namedTypeAnnotation.identifier는 "Foo"이며, 이름과 무언가를 가리키고 있다는 개념입니다.

print(); 와 같은 코드의 경우, 먼저 나오는 것은 함수를 가리키는 print 식별자입니다. 이는 코어 라이브러리의 함수를 참조합니다.

<div class="content-ad"></div>

신원자가 무엇인지 알고 계시네요.

그래서 이름이 "String" 또는 "int"인 식별자가있는 NamedTypeAnnotation을 볼 때, 우리가 원한 것을 찾은 것이죠, 최소한 첫 번째 버전에 해당하는 것입니다.

typedef가 관련된 경우에는 더 많은 내용이 있고, 나중에 지원할 계획입니다. 지금은 여기까지 하겠습니다. 다시 한번 요약하면:

![이미지](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_0.png)

<div class="content-ad"></div>

다시 한번, macro_util 패키지가 이 작업을 대신해줍니다.

# 식별자 해결하기

첫 번째 글에서 print 함수를 참조하기 위해 해야 했던 꼼수를 기억하십시오. 우리는 먼저 print라는 식별자를 코어 라이브러리에서 해결해야 했고, 그 후에야 우리가 생성한 코드에서 사용할 수 있었습니다.

여기서는 더 많은 것을 해야 합니다. 생성된 코드는 ArgParser, List, String 및 int 클래스를 참조해야 합니다. 모든 이러한 식별자를 하나의 구조체에 그룹화하는 것이 가장 쉽습니다:

<div class="content-ad"></div>

```js
class Libraries {
  static final arg_parser = Uri.parse('package:args/src/arg_parser.dart');
  static final core = Uri.parse('dart:core');
}

class ResolvedIdentifiers {
  ResolvedIdentifiers({
    required this.ArgParser,
    required this.int,
    required this.List,
    required this.String,
  });

  final Identifier ArgParser;
  final Identifier int;
  final Identifier List;
  final Identifier String;

  static Future<ResolvedIdentifiers> resolve(
    MemberDeclarationBuilder builder,
  ) async {
    final (
      ArgParser,
      int,
      List,
      String,
    ) = await (
      builder.resolveIdentifier(Libraries.arg_parser, 'ArgParser'),
      builder.resolveIdentifier(Libraries.core, 'int'),
      builder.resolveIdentifier(Libraries.core, 'List'),
      builder.resolveIdentifier(Libraries.core, 'String'),
    ).wait;

    return ResolvedIdentifiers(
      ArgParser: ArgParser,
      int: int,
      List: List,
      String: String,
    );
  }
}
```

우리는 이상적으로 식별자를 해결할 때 공개 라이브러리를 사용하고 싶지만, ArgParser에 대해 `package:args/args.dart`를 사용할 수 없는 버그가 있어서, 우리는 여기서 비공개 라이브러리를 사용해야 합니다: `package:args/src/arg_parser.dart`

이 긴 ResolvedIdentifiers 클래스를 보고 '진짜 이것이 모두 필요한가?'라고 궁금해할 수 있습니다. 필드들과 모두 나열된 생성자, 그리고 모든 필드에 대해 똑같은 작업을 하는 것이 있습니다. 이 사용 사례가 익숙한가요? 단지...

<img src="/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_1.png" />


<div class="content-ad"></div>

그래! 지금 배우고 있는 내용을 적용해서 지금 하는 일을 간단하게 만들어 봅시다!

이렇게 간소화할 수 있는 매크로를 만들었어요:

```js
@ResolveIdentifiers()
class ResolvedIdentifiers {
  @Resolve('package:args/args.dart')
  final Identifier ArgParser;

  final Identifier int;
  final Identifier List;
  final Identifier String;
}

// ...

final ids = ResolvedIdentifiers.resolve(builder);
```

이것은 같은 macro_util 패키지에 있어요. 하지만 문제가 있어요. int, List, String 같은 코어 타입에 대한 사전은 만들 수 있지만, ArgParser와 같은 사용자 정의 패키지의 경우에는 명시적인 패키지 링크와 매크로가 아직 필드에 적용된 주석을 읽을 수 없어요.

<div class="content-ad"></div>

요즘은 단순한 것들에 @ResolveIdentifiers() 매크로를 사용할 수는 있지만, 지금은 사용할 수 없어요. 우리는 ResolvedIdentifiers 클래스의 전체 중복 코드로 돌아가야 해요. 하지만 미래에는 훨씬 간단해질 거라 다행이에요.

# 필드를 Argument 객체로 변환하기

방문자(visitor) 클래스들이 작동하도록 하려면 StringArgument와 IntArgument 객체를 생성해야 해요:

```js
Map<String, Argument> _fieldsToArguments(
  Map<String, FieldIntrospectionData> fields,
  DeclarationBuilder builder,
) {
  return {
    for (final entry in fields.entries)
      entry.key: _fieldToArgument(
        entry.value as ResolvedFieldIntrospectionData,
        builder: builder,
      ),
  };
}

Argument _fieldToArgument(
  ResolvedFieldIntrospectionData fieldIntr, {
  required DeclarationBuilder builder,
}) {
  final typeDecl = fieldIntr.deAliasedTypeDeclaration;
  final optionName = _camelToKebabCase(fieldIntr.name);
  final typeName = typeDecl.identifier.name;

  switch (typeName) {
    case 'int':
      return IntArgument(
        intr: fieldIntr,
        optionName: optionName,
      );

    case 'String':
      return StringArgument(
        intr: fieldIntr,
        optionName: optionName,
      );
  }

  throw Exception();
}
```

<div class="content-ad"></div>

# 파서 확장

이제 파서 클래스에 많은 코드를 추가해야 합니다. 이를 시작해봅시다:

```js
void _augmentParser(
  MemberDeclarationBuilder builder,
  IntrospectionData intr,
) {
  final parserName = _getParserName(intr.clazz);

  builder.declareInLibrary(
    DeclarationCode.fromParts([
      //
      'augment class $parserName {\n',
      '  final parser = ', intr.ids.ArgParser, '();\n',
      ..._getConstructor(intr.clazz),
      ...AddOptionsGenerator(intr).generate(),
      ...ParseGenerator(intr).generate(),
      '}\n',
    ]),
  );
}
```

여기에는 나중에 생성된 코드에 연결될 코드 부품 목록을 반환하는 몇 가지 메서드가 있습니다. 첫 번째 글에서 기억하시죠? 코드 부분은 문자열 리터럴과 식별자의 혼합입니다.

<div class="content-ad"></div>

먼저, 우리는 곧 채울 표준 구문 분석기의 인스턴스를 보유할 **parser** 공개 필드를 선언합니다. 그리고나서 더 흥미로운 내용들이 있어요.

## 구문 분석기의 옵션으로 채우기

**AddOptionsGenerator**는 인수에 대한 방문자입니다. **_addOptions()** 메서드를 생성하는 비공개 `_addOptions()` 메서드를 만듭니다:

```js
List<Object> _getConstructor(ClassDeclaration clazz) {
  final parserName = _getParserName(clazz);

  return [
    //
    parserName, '() {\n',
    '  _addOptions();\n',
    '}\n',
  ];
}

class AddOptionsGenerator extends ArgumentVisitor<List<Object>> {
  AddOptionsGenerator(this.intr);

  final IntrospectionData intr;

  List<Object> generate() {
    return [
      //
      'void _addOptions() {\n',
      for (final argument in intr.arguments.values)
        ...[...argument.accept(this), '\n'],
      '}\n',
    ];
  }

  @override
  List<Object> visitInt(IntArgument argument) =>
      _visitStringInt(argument);

  @override
  List<Object> visitString(StringArgument argument) =>
      _visitStringInt(argument);

  List<Object> _visitStringInt(Argument argument) {
    return [
      //
      'parser.addOption(\n',
      '  ${jsonEncode(argument.optionName)},\n',
      '  mandatory: true,\n',
      ');\n',
    ];
  }
}
```

<div class="content-ad"></div>

_table_ 태그를 Markdown 형식으로 변경하십시오.

## 파싱된 데이터로 데이터 클래스 채우기

이 _addOptions() 메서드는 이 파서 클래스의 생성자에서 호출됩니다.

이것이 ParseGenerator 클래스가 수행하는 것입니다, 또 다른 인수 방문자:

```js
class ParseGenerator extends ArgumentVisitor<List<Object>> {
  ParseGenerator(this.intr);

  final IntrospectionData intr;

  List<Object> generate() {
    final name = intr.clazz.identifier.name;
    final ids = intr.ids;

    return [
      //
      '$name parse(', ids.List, '<', ids.String, '> argv) {\n',
      '  final wrapped = parser.parse(argv);\n',
      '  return $name(\n',
      for (final argument in intr.arguments.values) ...[
        ...argument.accept(this),
        ',\n',
      ],
      '  );\n',
      '}\n',
    ];
  }

  @override
  List<Object> visitInt(IntArgument argument) {
    return [
      argument.intr.name,
      ': ',
      intr.ids.int,
      '.parse(wrapped.option(${jsonEncode(argument.optionName)})!)',
    ];
  }

  @override
  List<Object> visitString(StringArgument argument) {
    return [
      argument.intr.name,
      ': wrapped.option(${jsonEncode(argument.optionName)})!',
    ];
  }
}
```

<div class="content-ad"></div>

이제 마크로를 사용할 수 있어요!

main.dart 파일을 생성하세요:

```js
import 'macro.dart';

@Args()
class HelloArgs {
  final String name;
  final int count;
}

void main(List<String> argv) {
  final parser = HelloArgsParser(); // 생성된 클래스.
  final HelloArgs args = parser.parse(argv);

  for (int n = 0; n < args.count; n++) {
    print('Hello, ${args.name}!');
  }
}
```

그리고 실행해보세요:

<div class="content-ad"></div>

```js
$ dart run --enable-experiment=macros lib/main.dart --name=Alexey --count=3
Hello, Alexey!
Hello, Alexey!
Hello, Alexey!
```

그리고 엣지 케이스와 기능들로 인한 미친 폭풍이 시작되었습니다!

# 오류 처리

우리의 매크로는 성공적인 시나리오만 처리하며, 뭔가를 좋아하지 않는다면 암호화된 오류를 발생시킵니다. 지원하지 않는 유형의 필드를 데이터 클래스에 추가해보세요:

<div class="content-ad"></div>

```js
@Args()
class HelloArgs {
  final Map map;
}
```

`_fieldToArgument` 함수는 매크로를 중단시킬 예외를 throw합니다. 결과적으로 클래스에 더 이상 생성자가 없어서 더 많은 오류가 발생합니다.

매크로의 작동 원리가 사용자에게 전혀 알려져 있지 않기 때문에 매끄럽게 실패하는 것이 중요합니다. 이를 위해 두 가지 습관이 필요합니다:

- 매크로에서 예외를 throw하지 말고 대신 컴파일러 진단 메시지를 보고합니다.
- 오류를 보고할 때도 구문적으로 정확한 코드를 생성하여 부차적인 오류를 피합니다. 


<div class="content-ad"></div>

## 컴파일러 진단 메시지 보고

다음은 진단 메시지를 보고하는 방법입니다:

```js
builder.report(
  Diagnostic(
    DiagnosticMessage(
      'My error',
      target: fieldDeclaration.asDiagnosticTarget,
    ),
    Severity.error,
  ),
);
```

이는 컴파일을 중단시키고 사용자에게 메시지를 보여줍니다. 내장 컴파일러 오류와 비슷하게 보입니다.

<div class="content-ad"></div>

```js
$ dart run --enable-experiment=macros lib/min.dart --name=Alexey --count=3
lib/min.dart:5:16: 에러: 내 오류
  final String name;
               ^
lib/min.dart:6:13: 에러: 내 오류
  final int count;
            ^
```

macro_util 패키지에는 더 간결하게 만들 수 있는 확장 메서드가 있습니다:

```js
builder.reportError(
  '내 오류',
  target: fieldDeclaration.asDiagnosticTarget,
);
```

## 우리가 다루고 싶은 사례들

<div class="content-ad"></div>

- 지원되지 않는 유형에 대한 오류 표시.
- 생략된 유형에 대한 오류 표시.
- int 및 String을 가리는 클래스 필드에 대한 오류 표시.
- _private 필드에 대한 오류 표시.
- 초기화기가 있는 필드에 대한 오류 표시.

이와 같은 모든 사항을 처리하는 향상된 버전은 여기에 있습니다. 변경된 내용을 확인하기 위해 이 두 브랜치를 diff해보는 것을 제안합니다:

![이미지](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_2.png)

변경사항을 살펴보겠습니다.

<div class="content-ad"></div>

## 지원되지 않는 유형에 대한 오류 음소거

모든 공개 필드를 named parameter로 사용하는 생성자가 있습니다. 만약 필드의 값을 구문 분석할 수 없는 경우에도 생성자에 무언가를 전달해 주어야 합니다. 그렇지 않으면 생성자 호출 시 문법 오류가 발생하며, 사용자에게 표시해서는 안 됩니다.

우린 이 변수를 만들어 컴파일러를 속이고 있어요:

```js
static var _silenceUninitializedError;
```

<div class="content-ad"></div>

동적으로 생성되는 형식이므로 컴파일 오류 없이 모든 생성자 매개변수로 전달할 수 있습니다. 실행하면 오류가 발생할 것이지만, 실행되지 않습니다. 왜냐하면 잘못된 필드에 대해 실행을 방지하는 오류 진단이 생성되기 때문입니다.

dynamic 대신에 null 같은 값을 전달할 수도 있지만, 이는 동적 식별자를 해결해야하기 때문에 원하지 않았습니다.

다음으로, 만약 StringArgument와 IntArgument만 있다면 이 더미 값을 생성자에 전달해야 하는 방법을 어떻게 알 수 있을까요? 우리는 전용 클래스 InvalidTypeArgument를 만들어야 하며 (int와 String 위에 유효한 argument 클래스를 만들어야 합니다):

![image](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_3.png)

<div class="content-ad"></div>

```js
class InvalidTypeArgument extends Argument {
  InvalidTypeArgument({
    required super.intr,
  }) : super(
          optionName: '',
        );

  @override
  R accept<R>(ArgumentVisitor<R> visitor) {
    return visitor.visitInvalidType(this);
  }
}
```

그런 다음 두 방문자에 대한 처리기를 추가합니다. 파서에 옵션을 추가할 때는 아무것도 해줄 필요가 없습니다:

```js
class AddOptionsGenerator extends ArgumentVisitor<List<Object>> {
  // ...

  @override
  List<Object> visitInvalidType(InvalidTypeArgument argument) {
    return const [];
  }
}
```

데이터 객체를 구성할 때는 이러한 필드에 저그러한 서언자를 전달해줍니다:

<div class="content-ad"></div>

```js
class ParseGenerator extends ArgumentVisitor<List<Object>> {
  //...

  @override
  List<Object> visitInvalidType(InvalidTypeArgument argument) {
    return [
      argument.intr.name,
      ': _silenceUninitializedError',
    ];
  }
}
```

그런 다음, 에러가 발생할 때마다 InvalidTypeArgument의 인스턴스를 생성해야 하며, 이런 경우가 많습니다:

```js
Argument _fieldToArgument(
  FieldIntrospectionData fieldIntr, {
  required DeclarationBuilder builder,
}) async {
  final field = fieldIntr.fieldDeclaration;
  final target = field.asDiagnosticTarget;
  final type = field.type;

  void reportError(String message) {
    builder.reportError(message, target: target);
  }

  void unsupportedType() {
    if (type is OmittedTypeAnnotation) {
      reportError('여기에는 명시적으로 선언된 유형이 필요합니다.');
      return;
    }

    reportError('허용된 유형은 다음과 같습니다: String, int.');
  }

  if (fieldIntr is! ResolvedFieldIntrospectionData) {
    unsupportedType();
    return InvalidTypeArgument(intr: fieldIntr);
  }

  final typeDecl = fieldIntr.deAliasedTypeDeclaration;
  final optionName = _camelToKebabCase(fieldIntr.name);

  if (field.hasInitializer) {
    reportError('인수 필드에 대한 이니셜라이저는 허용되지 않습니다.');
    return InvalidTypeArgument(intr: fieldIntr);
  }

  if (typeDecl.library.uri != Libraries.core) {
    unsupportedType();
    return InvalidTypeArgument(intr: fieldIntr);
  }

  final typeName = typeDecl.identifier.name;

  switch (typeName) {
    case 'int':
      return IntArgument(
        intr: fieldIntr,
        optionName: optionName,
      );

    case 'String':
      return StringArgument(
        intr: fieldIntr,
        optionName: optionName,
      );
  }

  unsupportedType();
  return InvalidTypeArgument(intr: fieldIntr);
}
```

이 코드는 비공개 필드를 제외한 모든 에러를 공손하게 처리합니다. 곧 비공개 필드에 대해 설명하겠습니다. 그 전에 방금 사용한 유형 선언에 대한 간략한 투어를 진행하겠습니다.


<div class="content-ad"></div>

# 필드의 TypeDeclaration 확인하기

내장 int 유형인 필드를 그림자가 생긴 것과 구분해야 합니다:

```js
class int {}

@Args()
class HelloArgs {
  final int count; // 내장 'int'가 아닙니다.
}
```

이는 타입의 이름만 확인하는 것으로 충분하지 않다는 것을 의미합니다. 라이브러리를 알아봐야 합니다. dart:core라면 괜찮지만, 그 외의 경우에는 오류를 표시해야 합니다. 위의 코드는 macro_util 패키지가 그 작업을 대신합니다. 어떻게 하는지 보겠습니다.

<div class="content-ad"></div>

지금까지 타입에 대한 메모를 기억하고 계신가요? 관련 정보가 더 있어요:

![이미지](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_4.png)

## TypeDeclaration

'문자열' 또는 '정수'를 포함하고 있던 식별자에 대해 기억하시나요? 그 이름만을 담고 있던 것이 아니라, 해당 라이브러리에서 선언된 실제 타입 선언을 끌어올 수 있는 핸들이기도 했어요. 그 타입 선언을 가져오는 방법은 다음과 같아요:

<div class="content-ad"></div>

```js
const typeDecl = await builder.typeDeclarationOf(
  namedTypeAnnotation.identifier,
);
```

이것은 TypeDeclaration을 반환합니다. TypeAnnotation과는 달리 이것은 단순히 핸들(handle)이 아니라 라이브러리에서 선언된 실제 타입입니다. 이 클래스는 추상 클래스이며, 타입에 따라 하위 클래스를 얻을 수 있습니다.

가장 간단한 경우에는 ClassDeclaration을 얻게 됩니다. 기억하세요, 이것은 매크로가 클래스에 적용될 때 우리가 시작한 클래스입니다. 이번에는 필드용 ClassDeclaration을 얻게 됩니다. 이 긴 여정을 재귀적으로 반복하면 프로그램의 거의 모든 것을 내부적으로 검사할 수 있습니다.

저희에게 도움이 되는 또 다른 하위 클래스는 EnumDeclaration입니다. 필드에 대해 enums를 지원하고 사용자가 명령행 옵션에 대해 고정된 집합에서만 값을 제공할 수 있도록 하려고 합니다. 하지만 enums는 복잡하니 우선 단순한 유형들이 작동하도록 해봅시다.


<div class="content-ad"></div>

우리는 typedef에 대한 TypeAliasDeclaration도 얻을 수 있습니다. 이 경우에는 별칭이 지정된 타입을 살펴보고 여전히 지원하는 타입에 도달할 수 있기 때문에 프로세스를 반복해야 합니다. 디별리징 전체 프로세스는 다음과 같습니다:

```js
TypeDeclaration typeDecl = await builder.typeDeclarationOf(
  namedTypeAnnotation.identifier,
);

while (typeDecl is TypeAliasDeclaration) {
  final aliasedType = typeDecl.aliasedType;
  if (aliasedType is! NamedTypeAnnotation) {
    // 오류. typedef가 레코드나 함수와 같은 이상한 것으로 우리를 이끌었습니다.
    throw Exception('...');
  }
  typeDecl = await builder.typeDeclarationOf(aliasedType.identifier);
}
```

이것이 이전에 확인하던 필드가 FieldIntrospectionData.deAliasedTypeDeclaration라고 불리는 이유입니다.

## 라이브러리

<div class="content-ad"></div>

TypeDeclaration 이 있는 경우, 해당 선언이로드된 라이브러리에 링크됩니다. 이것이 내장 int 와 사용자 정의 그림자 int를 구분할 수 있는 방법입니다.

모든 것은 macro_util 패키지에 의해 수행됩니다. 모든 것이 원활하게 해결되면 별칭이 지정된 형식으로 ResolvedFieldIntrospectionData를 가져옵니다. 그렇지 않으면 TypeAnnotation만 포함된 FieldIntrospectionData를 가져옵니다.

이것이 바로 ResolvedTypeArgument라는 기반 유효한 인수 형식을 명명한 이유입니다.

# 개인 필드 처리

<div class="content-ad"></div>

만약 데이터 클래스에 private 필드가 있는 경우 어떻게 될까요?

```js
@Args()
class HelloArgs {
  final String _name;
}
```

Dart에서는 함수에 대한 명명된 매개변수가 언더스코어로 시작할 수 없기 때문에 사용한 생성자 매크로는 명명된 매개변수가 아니라 위치 매개변수로 변환합니다.

데이터 클래스에 private 필드를 사용하는 것은 의미가 없으므로 해당 필드에 대한 오류를 표시해야 합니다.

<div class="content-ad"></div>

먼저, 이러한 필드에 대한 InvalidTypeArgument를 생성하고 진단을 표시해야 합니다:

```js
Argument _fieldToArgument(
  FieldIntrospectionData fieldIntr, {
  required DeclarationBuilder builder,
}) {
  final field = fieldIntr.fieldDeclaration;
  final target = field.asDiagnosticTarget;

  if (fieldIntr.name.contains('_')) {
    builder.reportError(
      '인수 필드 이름에는 밑줄을 포함할 수 없습니다.',
      target: target,
    );
    return InvalidTypeArgument(intr: fieldIntr);
  }

  // ...
```

그런 다음 명명된 매개변수와 위치 매개변수를 분리해야 합니다. 위치 매개변수에 대해 모든 매개변수에 우리의 사일렌서를 전달합니다:

```js
class ParseGenerator extends ArgumentVisitor<List<Object>> {
  ParseGenerator(this.intr);

  final IntrospectionData intr;

  List<Object> generate() {
    final name = intr.clazz.identifier.name;
    final ids = intr.ids;

    final arguments = intr.arguments.values.where(
      (a) =>
          a.intr.constructorHandling ==
          FieldConstructorHandling.namedOrPositional,
    );

    return [
      //
      name, ' parse(', ids.List, '<', ids.String, '> argv) {\n',
      '  final wrapped = parser.parse(argv);\n',
      '  return $name(\n',
      for (final param in _getPositionalParams()) ...[...param, ',\n'],
      for (final argument in arguments) ...[
        ...argument.accept(this),
        ',\n',
      ],
      '  );\n',
      '}\n',
    ];
  }

  List<List<Object>> _getPositionalParams() {
    final result = <List<Object>>[];
    final fields = intr.fields.values.where(
      (f) => f.constructorHandling == FieldConstructorHandling.positional,
    );

    for (final _ in fields) {
      result.add([
        '_silenceUninitializedError',
      ]);
    }

    return result;
  }
```

<div class="content-ad"></div>

여기서는 편리한 getter를 사용합니다.
FieldIntrospectionData.constructorHandling

이는 두 가지 중 하나입니다:

- 필드가 언더스코어(_)로 시작하여 생성자의 위치 매개변수로만 사용될 수 있는 경우 positional입니다.
- 그렇지 않은 경우 namedOrPositional입니다.

이 변경으로 매크로는 모든 오류를 안정하게 처리할 수 있게 되었습니다. 자체 진단만을 보고 다른 컴파일 오류를 보고하지 않습니다.

<div class="content-ad"></div>

# 리스트 지원 및 유형 매개변수 검사

우리의 매크로는 int와 String의 리스트 및 세트를 모두 지원할 것입니다. 이를 위해 인수 유형을 다음과 같이 확장할 것입니다:

![image](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_5.png)

해당 기능을 구현한 전체 코드는 여기에 있습니다. 변경된 내용을 확인하려면 이전 브랜치와 비교해보세요:

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_6.png)

대부분의 변경 내용은 인수 클래스 및 방문자를 추가하는 것이므로 해당 부분은 건너 뜁니다. 여기에는 매크로 관점에서 알아야 할 내용이 있습니다.

리스트 또는 세트를 만나면 typeArguments를 확인합니다. 이는 NamedTypeAnnotation을 다룰 때만 사용할 수 있으므로 해당 사항을 확인하고 다른 경우는 잘못된 인수로 처리합니다.

타입 인수를 얻으면 별칭 해제 및 타입 검사 프로세스를 실행하고 작업을 마칩니다:


<div class="content-ad"></div>

```js
Future<Argument> _fieldToArgument(
  FieldIntrospectionData fieldIntr, {
  required DeclarationBuilder builder,
}) async {
  // ...

  if (type is! NamedTypeAnnotation) {
    unsupportedType();
    return InvalidTypeArgument(intr: fieldIntr);
  }

  // ...

  switch (typeName) {
    // ...

    case 'List':
    case 'Set':
      final paramType = type.typeArguments.firstOrNull;
      if (paramType == null) {
        reportError(
          'A $typeName requires a type parameter: '
          '$typeName<String>, $typeName<int>.',
        );

        return InvalidTypeArgument(intr: fieldIntr);
      }

      if (paramType.isNullable) {
        reportError(
          'A $typeName type parameter must be non-nullable because each '
          'element is either parsed successfully or breaks the execution.',
        );

        return InvalidTypeArgument(intr: fieldIntr);
      }

      if (paramType is! NamedTypeAnnotation) {
        unsupportedType();
        return InvalidTypeArgument(intr: fieldIntr);
      }

      final paramTypeDecl = await builder.deAliasedTypeDeclarationOf(paramType);

      if (paramTypeDecl.library.uri != Libraries.core) {
        unsupportedType();
        return InvalidTypeArgument(intr: fieldIntr);
      }

      switch (paramTypeDecl.identifier.name) {
        case 'int':
          return IterableIntArgument(
            intr: fieldIntr,
            iterableType: IterableType.values.byName(typeName.toLowerCase()),
            optionName: optionName,
          );

        case 'String':
          return IterableStringArgument(
            intr: fieldIntr,
            iterableType: IterableType.values.byName(typeName.toLowerCase()),
            optionName: optionName,
          );
      }

    // ...
```

# Supporting Enums

열거형 인수는 사용자가 지정된 집합에서만 값으로 지정할 수 있게 합니다:

```js
@Args()
class HelloArgs {
  final Fruit fruit;
}

enum Fruit { apple, banana, mango, orange }
```

<div class="content-ad"></div>

해당 필드를 자세히 살펴보면 ClassDeclaration 대신 EnumDeclaration이 반환되어야 합니다. 그러나 현재 구현되어 있지 않습니다. 여전히 ClassDeclaration이 반환됩니다. Dart 팀이 해당 부분을 구현할 때까지 기다리거나 제가 한 것처럼 해결책을 사용해야 합니다.

이것은 큰 주황색 깃발인데, 실제로 EnumDelcaration을 반환하도록 API가 변경될 때 클래스로 한 해결책이 갑자기 동작하지 않을 수 있습니다. 따라서 여기서는 놀이터로만 사용하세요.

해결책은 가지고 있는 클래스가 내장 Enum 인터페이스를 구현했는지를 확인하는 것입니다. 매크로는 이를 단순히 이전에 본 내성 작업으로 수행할 수 없으므로 다른 방법을 살펴보고 메모에 추가해 봅시다:

![이미지](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_7.png)

<div class="content-ad"></div>

## StaticType

이는 다른 StaticType 객체들과의 상속을 비교할 수 있도록 하는 타입의 표현입니다. 왜 다른 클래스가 필요했는지, 왜 ClassDeclaration 객체를 상속을 비교하지 못하는지 정확히 말하기 어렵습니다. 아마도 StaticType은 더 비싼 내부 검사를 필요로 하는 다른 종류의 타입을 처리하기 때문일 것입니다. 그러나 이것은 이전에 있던 것을 대체하지는 않습니다.

NamedTypeAnnotationCode를 해결하기 위해 빌더에게 StaticType를 요청하여 얻을 수 있습니다. NamedTypeAnnotationCode는 단순히 타입 이름의 식별자를 담고 있는 컨테이너입니다:

```js
final staticType = await builder.resolve(
  NamedTypeAnnotationCode(name: namedTypeAnnotation.identifier),
);

if (await staticType.isSubtypeOf(enumStaticType)) {
  // ...
}
```

<div class="content-ad"></div>

이제 아이디어를 이해했으니 enums를 지원하는 버전을 살펴보세요. 이전 버전과 비교해 변경 사항을 확인하세요:

![diff image](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_8.png)

변경 사항을 자세히 살펴봅시다.

## 정적 유형 해결

<div class="content-ad"></div>

`macro_utils` 패키지는 각 필드에 대해 StaticType 객체를 해결하는 작업을 처리합니다. Enum 클래스에 대해 다른 필드와 비교하기 위해 해당 객체를 해결해야 합니다. 

우리는 ResolvedIdentifiers 클래스와 동일한 접근 방식을 취합니다. 이 클래스에는 매크로가 필요로 할 모든 식별자가 포함되어 있습니다. 우리는 하나의 정적 타입만 가지고 있지만, 이 클래스에 정적 타입을 저장합니다:

```js
import 'package:macros/macros.dart';

import 'resolved_identifiers.dart';

class StaticTypes {
  StaticTypes({
    required this.Enum,
  });

  final StaticType Enum;

  static Future<StaticTypes> resolve(
    MemberDeclarationBuilder builder,
    ResolvedIdentifiers ids,
  ) async {
    final Enum = await builder.resolve(NamedTypeAnnotationCode(name: ids.Enum));

    return StaticTypes(
      Enum: Enum,
    );
  }
}
```

이 객체를 최상위 내부 검사 데이터 클래스에 추가합니다:

<div class="content-ad"></div>

```js
class IntrospectionData {
  IntrospectionData({
    required this.arguments,
    required this.clazz,
    required this.fields,
    required this.ids,
    required this.staticTypes, //                      NEW
  });

  final Map<String, Argument> arguments;
  final ClassDeclaration clazz;
  final Map<String, FieldIntrospectionData> fields;
  final ResolvedIdentifiers ids;
  final StaticTypes staticTypes; //                    NEW
}
```

그리고 해결된 데이터로 채워봅시다:

```js
Future<IntrospectionData> _introspect(
  ClassDeclaration clazz,
  MemberDeclarationBuilder builder,
) async {
  final ids = await ResolvedIdentifiers.resolve(builder);

  final (fields, staticTypes) = await (
    builder.introspectFields(clazz),
    StaticTypes.resolve(builder, ids), //                  NEW
  ).wait;

  final arguments = await _fieldsToArguments(
    fields,
    builder: builder,
    staticTypes: staticTypes, //                           NEW
  );

  return IntrospectionData(
    arguments: arguments,
    clazz: clazz,
    fields: fields,
    ids: ids,
    staticTypes: staticTypes, //                           NEW
  );
}
```

## 열거형 인수 생성하기

<div class="content-ad"></div>

비핵심 타입의 필드를 발견하면 이제 열거형일 수도 있으므로 해당 사항을 확인하세요:

```js
Future<Argument> _fieldToArgument(
  FieldIntrospectionData fieldIntr, {
  required DeclarationBuilder builder,
  required StaticTypes staticTypes,
}) async {
  // ...

  if (typeDecl.library.uri != Libraries.core) {
    if (await fieldIntr.nonNullableStaticType.isSubtypeOf(staticTypes.Enum)) {
      return EnumArgument(
        enumIntr:
            await builder.introspectEnum(fieldIntr.deAliasedTypeDeclaration),
        intr: fieldIntr,
        optionName: optionName,
      );
    }

    unsupportedType();
    return InvalidTypeArgument(intr: fieldIntr);
  }

  // ...

  switch (typeName) {
    // ...

    case 'List':
    case 'Set':
      // ...

      if (paramTypeDecl.library.uri != Libraries.core) {
        final paramStaticType = await builder.resolve(paramType.code);
        if (await paramStaticType.isSubtypeOf(staticTypes.Enum)) {
          return IterableEnumArgument(
            enumIntr: await builder.introspectEnum(paramTypeDecl),
            intr: fieldIntr,
            iterableType: IterableType.values.byName(typeName.toLowerCase()),
            optionName: optionName,
          );
        }

        unsupportedType();
        return InvalidTypeArgument(intr: fieldIntr);
      }

      // ...
```

## 열거형 값 검사

위 코드에서 빌더의 확장 메서드를 사용했습니다:

<div class="content-ad"></div>

```js
class EnumIntrospectionData {
  EnumIntrospectionData({
    required this.deAliasedTypeDeclaration,
    required this.values,
  });

  final TypeDeclaration deAliasedTypeDeclaration;
  final List<EnumConstantIntrospectionData> values;
}

class EnumConstantIntrospectionData {
  const EnumConstantIntrospectionData({
    required this.name,
  });

  final String name;
}

extension EnumIntrospectionExtension on DeclarationBuilder {
  Future<EnumIntrospectionData> introspectEnum(
    TypeDeclaration deAliasedTypeDeclaration,
  ) async {
    final fields = await fieldsOf(deAliasedTypeDeclaration);
    final values = (await Future.wait(fields.map(introspectEnumField)))
        .nonNulls
        .toList(growable: false);

    return EnumIntrospectionData(
      deAliasedTypeDeclaration: deAliasedTypeDeclaration,
      values: values,
    );
  }

  Future<EnumConstantIntrospectionData?> introspectEnumField(
    FieldDeclaration field,
  ) async {
    final type = field.type;

    if (type is NamedTypeAnnotation) {
      return null;
    }

    return EnumConstantIntrospectionData(
      name: field.identifier.name,
    );
  }
}
```

보통은 안전한 전용 메서드 builder.valuesOf(enumDeclaration)을 사용해야 하지만, EnumDeclaration을 얻을 수 없어서 클래스의 모든 필드를 가져오는 단순한 접근 방식을 사용합니다. 다행히 모든 enum 상수가 클래스의 필드로 반환됩니다. 값 컬렉션을 건너뛰기만 하면 되기 때문에, 필드의 타입이 NamedTypeAnnotation인지 확인하여(values에 해당되지 않음) 비어있는 값을 반환합니다. 이 방법은 enum에 다른 데이터가 포함되어 있으면 예상치 못한 결과를 초래할 수 있지만, 일시적인 해결책을 더 잘 맞추기 위해 너무 꼼꼼하지는 않습니다.

또한 이것은 신뢰할만한 것이 아니기 때문에 macro_util에 포함되어 있지 않습니다.

어쨌든, EnumIntrospectionData 객체를 얻어서 EnumArgument에 전달합니다:


<div class="content-ad"></div>

```js
class EnumArgument extends ResolvedTypeArgument {
  EnumArgument({
    required super.intr,
    required super.optionName,
    required this.enumIntr,
  });

  final EnumIntrospectionData enumIntr;

  @override
  R accept<R>(ArgumentVisitor<R> visitor) {
    return visitor.visitEnum(this);
  }
}
```

그런 다음 옵션을 채우는 중에 값을 사용합니다:

```js
class AddOptionsGenerator extends ArgumentVisitor<List<Object>> {
  // ...

  @override
  List<Object> visitEnum(EnumArgument argument) {
    final values =
        argument.enumIntr.values.map((v) => v.name).toList(growable: false);

    return [
      //
      'parser.addOption(\n',
      '  ${jsonEncode(argument.optionName)},\n',
      '  allowed: ${jsonEncode(values)},\n',
      '  mandatory: true,\n',
      ');\n',
    ];
  }

  // ...
```

데이터 구문 분석하기는 쉽습니다:


<div class="content-ad"></div>

```js
class ParseGenerator extends ArgumentVisitor<List<Object>> {
  // ...

  @override
  List<Object> visitEnum(EnumArgument argument) {
    final valueGetter = _getOptionValueGetter(argument);

    return [
      argument.intr.name,
      ': ',
      argument.intr.deAliasedTypeDeclaration.identifier,
      '.values.byName($valueGetter!)',
    ];
  }

  // ...
```

IterableEnumArgument에 대해서도 크게 달라진 것이 없습니다.

# 필드 초기화 및 null 필드

우리는 인자에 대한 기본값을 지원하기를 원합니다. 이를 표현하는 좋은 방법 중 하나는 기본값이 있는 필드입니다.

<div class="content-ad"></div>


```js
@Args()
class HelloArgs {
  final int count = 1;
}
```

요청사항의 사양에 따르면 이와 같이 초기화자를 확장할 수 있어야 합니다:

```js
augment class HelloArgs {
  augment final int count = _parse_count(augmented);

  static int _parse_count(int defaultValue) {
    // 파싱 처리를 합니다.
  }
}
```

그러나 작성 시점에서 아직 이것이 구현되지 않았으므로 초기화자를 확장할 수 없습니다. 따라서 적절한 해결책을 기다리는 대신에 다시 한 번 해결책을 사용하겠습니다.


<div class="content-ad"></div>

다음으로 좋은 방법은 해당 필드에 대한 최종 값을 포기하고 필수 필드만을 포함하는 객체를 생성한 다음 기본값이 재정의된 필드를 덮어쓰는 것입니다.

이 방법으로는 const 생성자를 사용할 수 없기 때문에 이것은 깔끔하지 못합니다. 초기화 프로그램을 확장할 수 있게 되면이를 변경해야 합니다.

그러나 더 큰 문제가 있습니다. 사용법 도움말 텍스트에 기본값이 나타나도록 하려면 기본값에 액세스해야 합니다. 필드 초기화 값을 Code 개체로 읽을 수 있다면 이 작업을 수행할 수 있습니다. 그러나 그렇게 할 수는 없습니다. API는 초기화 프로그램이 있는지 확인할 수 있도록 허용하지만 읽을 수 있도록 허용하지는 않습니다. 초기화 프로그램을 읽는 것이 유용하다고 생각한다면이 기능 요청에 투표해 주십시오.

그래서 해결책은 다음과 같습니다:

<div class="content-ad"></div>

- 기본값이 없는 필드만 있는 두 번째 생성자를 만듭니다.
- 이 생성자를 사용하여 mock 객체를 만듭니다. 이때 초기화값이 있는 필드는 기본값을 유지합니다. 필요한 필드에는 더미 값이 들어가도록 합니다.
- 해당 객체의 필드를 읽고 addOption(defaultsTo: ...)에 전달합니다.

또한, 이제 빈 값을 처리하는 방법으로 초기화 프로그램과 Nullability를 모두 지원할 수 있습니다. 빈 값을 처리하는 두 가지 방법을 모두 한 번에 구현하는 것이 더 쉽습니다.

기본값과 Nullable 옵션을 처리하는 이 버전을 참고하세요. 변경된 내용을 이전 버전과 비교하여 참조하세요:

![이미지](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_9.png)

<div class="content-ad"></div>

변경 사항을 살펴보겠습니다.

## 다른 생성자 추가하기

MockDataObjectGenerator 클래스를 추가하고 역할을 맡겠습니다:

```js
Future<void> _declareConstructors(
  ClassDeclaration clazz,
  MemberDeclarationBuilder builder,
) async {
  await Future.wait([
    //
    const Constructor().buildDeclarationsForClass(clazz, builder),
    MockDataObjectGenerator.createMockConstructor(clazz, builder), // NEW
  ]);
}
```

<div class="content-ad"></div>

```js
const _constructorName = 'withDefaults';

class MockDataObjectGenerator {
  /// 데이터 클래스에서 초기화 항목을 가진 필드에 대한 매개변수가 없는 생성자를 만들어
  /// 덮어쓰기를 방지합니다.
  static Future<void> createMockConstructor(
    ClassDeclaration clazz,
    MemberDeclarationBuilder builder,
  ) async {
    return const Constructor(
      name: _constructorName,
      skipInitialized: true,
    ).buildDeclarationsForClass(clazz, builder);
  }
}
```

우리가 사용한 @Constructor 매크로는 필요한 기능을 수행하기 위한 선택적 매개변수를 가지고 있습니다. 두 번째 생성자를 withDefaults로 지정하고 초기화 항목이 있는 필드를 추가하지 않습니다. 이렇게 하면 덮어쓰기되지 않습니다.

## 목 객체 생성하기

이 새로운 클래스를 세 번째 ArgumentVisitor로 변환해봅시다.


<div class="content-ad"></div>

```js
/// 데이터 클래스의 인스턴스를 생성하는 코드를 생성합니다.
/// 초기화된 필드를 덮어쓰지 않습니다.
///
/// 이 가짜 객체는 전달되지 않은 옵션들에 대한 데이터의 원본으로 사용됩니다.
///
/// 필수 필드는 각각의 유형에 대한 더미 값으로 채워집니다.
/// 이 값들은 실제 값이 구성 단계에서 파싱될 때 사용되지 않기 때문에 사용되지 않습니다.
class MockDataObjectGenerator extends ArgumentVisitor<List<Object>>
    with PositionalParamGenerator {
  // ...

  List<Object> generate() {
    final name = intr.clazz.identifier.name;
    final arguments = intr.arguments.values.where(
      (a) =>
          a.intr.constructorOptionality ==
              FieldConstructorOptionality.required &&
          a.intr.constructorHandling ==
              FieldConstructorHandling.namedOrPositional,
    );

    return [
      'static final $fieldName = $name.$_constructorName(\n',
      for (final param in getPositionalParams()) ...[...param, ',\n'],
      for (final parts in arguments.map((argument) => argument.accept(this)))
        ...[...parts, ',\n'],
      ');\n',
    ];
  }

  @override
  List<Object> visitEnum(EnumArgument argument) {
    return [
      argument.intr.name,
      ': ',
      argument.intr.deAliasedTypeDeclaration.identifier,
      '.values.first',
    ];
  }

  @override
  List<Object> visitInt(IntArgument argument) {
    return [
      argument.intr.name,
      ': 0',
    ];
  }

  @override
  List<Object> visitInvalidType(InvalidTypeArgument argument) {
    return [
      argument.intr.name,
      ': _silenceUninitializedError',
    ];
  }

  @override
  List<Object> visitIterableEnum(IterableEnumArgument argument) =>
      _visitIterable(argument);

  @override
  List<Object> visitIterableInt(IterableIntArgument argument) =>
      _visitIterable(argument);

  @override
  List<Object> visitIterableString(IterableStringArgument argument) =>
      _visitIterable(argument);

  @override
  List<Object> visitString(StringArgument argument) {
    return [
      argument.intr.name,
      ': ""',
    ];
  }

  List<Object> _visitIterable(IterableArgument argument) {
    switch (argument.iterableType) {
      case IterableType.list:
        return [
          argument.intr.name,
          ': const []',
        ];
      case IterableType.set:
        return [
          argument.intr.name,
          ': const {}',
        ];
    }
  }
}
```

지금 두 개의 인수 방문자에서 위치 매개변수를 가져와야 하므로 해당 메서드를 PositionalParamGenerator mixin으로 추출했습니다.

사용자가 관련없는 컴파일 오류로 방해받지 않고 저희 진단에 집중할 수 있도록 유효한 코드를 생성하기 위해 이렇게 많은 노력을 기울입니다. 이것이 매크로에서 가장 중요한 부분 중 하나입니다.

이후에 매크로에서 이것을 호출합니다:


<div class="content-ad"></div>

```js
_void _augmentParser(
  MemberDeclarationBuilder builder,
  IntrospectionData intr,
) {
  final parserName = _getParserName(intr.clazz);

  builder.declareInLibrary(
    DeclarationCode.fromParts([
      //
      'augment class $parserName {\n',
      '  final parser = ', intr.ids.ArgParser, '();\n',
      '  static var _silenceUninitializedError;\n',
      ...MockDataObjectGenerator(intr).generate(), //     NEW
      ..._getConstructor(intr.clazz),
      ...AddOptionsGenerator(intr).generate(),
      ...ParseGenerator(intr).generate(),
      '}\n',
    ]),
  );
}
```

## 기본 값 처리

먼저, 표준 ArgParser에 기본 값을 알려줍시다. 옵션을 추가하는 각 메서드는 다음과 같이 변경되어야 합니다:

```js
class AddOptionsGenerator extends ArgumentVisitor<List<Object>> {
  // ...

  List<Object> _visitStringInt(Argument argument) {
    final field = argument.intr.fieldDeclaration;

    return [
      //
      'parser.addOption(\n',
      '  "${argument.optionName}",\n',
      if (field.hasInitializer) ...[            // CHANGED
        '  defaultsTo: ',                       // CHANGED
        MockDataObjectGenerator.fieldName,      // CHANGED
        '.',                                    // CHANGED
        argument.intr.name,                     // CHANGED
        '.toString()',                          // CHANGED
        ',\n',                                  // CHANGED
      ] else if (!field.type.isNullable)        // CHANGED
        '  mandatory: true,\n',                 // CHANGED
      ');\n',
    ];
  }

  // ...
```

<div class="content-ad"></div>

그러면 모든 구문 분석 방법은 null 값을 고려해야 합니다:

```js
class ParseGenerator extends ArgumentVisitor<List<Object>>
    with PositionalParamGenerator {
  // ...

  @override
  List<Object> visitInt(IntArgument argument) {
    final valueGetter = _getOptionValueGetter(argument);

    return [
      argument.intr.name,
      ': ',
      if (argument.intr.fieldDeclaration.type.isNullable)  // 변경된 부분
        '$valueGetter == null ? null : ',                  // 변경된 부분
      intr.ids.int,
      '.parse($valueGetter!)',
    ];
  }

  // ...
```

데이터 객체에서 기본값을 덮어쓸 값이 전달되었는지 확인할 필요가 없습니다. 표준 ArgParser는 명시적으로 전달된 값이나 이전에 제공한 기본값을 반환할 뿐입니다.

## 더 나은 오류 진단

<div class="content-ad"></div>

이제 한 필드에 여러 오류를 갖을 수 있습니다. 예를 들어, 이 예시에서 많은 것이 잘못되었습니다:

```js
@Args()
class HelloArgs {
  final List<int?>? list = [];
}
```

이 코드에 대해 한 번에 세 가지 오류 진단을 표시해야 합니다:

- 초기화자가 있는 필드는 인수를 구문 분석할 때 덮어써져야 하므로 final일 수 없습니다.
- List는 이름이 지정된 옵션이 전달되지 않을 때에는 단순히 빈 상태이기 때문에 null일 수 없습니다.
- List 유형 매개변수는 각 요소가 성공적으로 구문 분석되거나 실행이 중단될 수 있으므로 non-nullable이어야 합니다.

<div class="content-ad"></div>

첫 번째 오류에서 멈추면 사용자는 그것을 수정하기만 해서 또 다른 오류에 당황해하고, 그리고 또 다른 오류에 빠지게 됩니다. 그들은 오류가 무한하고 매크로가 미흡하며 요구사항이 예측할 수 없다고 생각할 것입니다. 그래서 매크로의 기술에는 모든 잘못된 것을 미리 보여주는 것이 포함되어야 합니다. 따라서 사용자가 코드를 수정하는 동안

```js
@Args()
class HelloArgs {
  List<int> list = [];
}
```

오류가 하나씩 사라지면서 매우 만족스러운 경험을 할 수 있습니다.

이를 가능하게 하기 위해서는 다음을 해야 합니다:

<div class="content-ad"></div>

- 인수를 구문 분석할 때 isValid 로컬 플래그를 만드세요.
- 잘못된 점이 있을 때 진단을 보고하고 플래그를 false로 설정하세요.
- 마지막에 플래그를 확인하여 InvalidTypeArgument 또는 유효한 것을 생성하세요.

여기 몇 가지 변경사항이 있습니다. 많기 때문에 전체 차이를 확인하세요:

```js
bool isValid = true;

if (field.hasInitializer && field.hasFinal) {
  reportError(
    '초기화 프로그램이 있는 필드는 final이 될 수 없습니다. '
    '인수를 구문 분석할 때 덮어쓰여져야 하기 때문입니다.',
  );

  isValid = false;
}

// ...
```

좋은 매크로는 오류에 따라 복잡한 분기로 발전합니다.

<div class="content-ad"></div>

# 도움말 지원 메시지

--help 플래그를 사용하여 명령을 실행하면 사용법이 출력되어야 합니다. 그를 위해서 각 필드에 대한 도움말 텍스트가 필요합니다. 그것을 어떻게 저장해야 할까요?

문법적으로, 최상의 선택은 문서 주석입니다:

```js
@Args()
class HelloArgs {
  /// 출력할 이름입니다.
  final String name;

  /// 이름을 출력할 횟수입니다.
  final int count = 1;
}
```

<div class="content-ad"></div>


$ dart run --enable-experiment=macros lib/min.dart --help
사용법: [인수]
    --name (필수)    출력할 이름.
    --count          (기본값: "1") 이름을 출력할 횟수.
-h, --help           이용 가능한 옵션을 출력합니다.

안타깝게도 매크로는 필드의 문서 주석을 읽을 수 없습니다. 이 기능 요청을 좋아하신다면 추천해주세요.

다음으로는 필드 주석에 이 도움말을 전달하는 것이 좋습니다:

```js
@Args()
class HelloArgs {
  @Arg(help: '출력할 이름.')
  final String name;

  @Arg(help: '이름을 출력할 횟수.')
  final int count = 1;
}
```

<div class="content-ad"></div>

아직 이렇게 주석이 있는 것이 아니지만, 앞으로는 옵션 이름을 덮어쓸 수 있고, 정수의 범위를 제한할 수 있는 등의 주석이 지원되어야 합니다.

하지만 매크로는 아직 코드에서 주석을 읽을 수 없습니다.

제가 생각해 낸 해결책은 도움말 텍스트를 별도의 정적 필드에 저장하는 것입니다:

```js
@Args()
class HelloArgs {
  final String name;
  static const _nameHelp = '프린트할 이름을 입력하세요.';

  int count = 1;
  static const _countHelp = '이름을 몇 번 프린트할지 입력하세요.';
}
```

<div class="content-ad"></div>

여기는 이를 지원하는 버전입니다. 이전 버전과 비교해 보세요:

![Image](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_10.png)

변경 사항은 간단합니다:

- 정적 필드를 무시합니다(이전 버전에서는 끊어진 상태였습니다).
- 상수 필드의 이름을 코드에서 직접 참조합니다.

<div class="content-ad"></div>

# 유형 추론

이렇게 작성하는 것이 유형 추론을 지원하면 유혹적일 수 있습니다:

```js
@Args()
class HelloArgs {
  final String name;
  var count = 1; // 추론된 유형
}
```

필드를 final로 만들 수 있는 보완 기능이 완전히 출시되면 더욱 유혹스러울 것입니다:

<div class="content-ad"></div>

```js
@Args()
class HelloArgs {
  final String name;
  final count = 1; // 추론된 타입
}
```

타입 추론은 매크로 응용 프로그램의 세 번째 단계에서만 가능합니다. 불행하게도, 두 번째 단계에서는 매크로가 다음과 같은 글로벌 코드를 작성할 수 있지만

```js
builder.declareInLibrary(
  DeclarationCode.fromParts([
    'augment class $parserName {',
    // ...
  ]),
);
```

세 번째 단계에서는 매크로가 적용된 클래스의 메서드 본문과 필드 초기화자를 대체하는 데 제한됩니다.

<div class="content-ad"></div>

두 가지 접근 방식을 취할 수 있습니다:

- 새로운 매크로인 @ArgsParser와 같은 이름의 매크로를 생성하고 주요 매크로가 생성한 파서 클래스에 적용하도록 설정할 수 있습니다. 그런 다음 대부분의 작업을 이 매크로로 이동해야 합니다. 사양으로는 가능하지만 아직 구현되지 않았습니다. 단점은 이 매크로가 의미 있는 기능이 없는 라이브러리의 내보내진 심볼이 될 것입니다.
- 별도의 파서 클래스를 포기하고 모든 작업을 데이터 클래스의 정적 메서드로 변환할 수 있습니다:

```js
@Args()
class HelloArgs {
  final String name;
  final int count;
}

void main(List<String> argv) {
  final args = HelloArgs.parse(argv);
}
```

이것은 지금 당장 할 수 있지만, 여기에 혼재된 관심사가 마음에 들지 않습니다. 데이터 객체의 클라이언트는 생성한 구문 분석 메서드에 대한 액세스 권한을 갖지 않아야 합니다.

<div class="content-ad"></div>

세 번째로 떠오른 아이디어가 있어요:  
첫 번째 접근 방식을 사용하되 두 개의 매크로를 사용하고 같은 @Args 매크로를 재사용하여 파서 클래스에 적용될 때 다른 작업을 할 수 있게 해 보세요. 이 방법은 조금 복잡할 수 있지만 사용자에게 제공되는 API는 최대한 깔끔하게 유지할 수 있어요.  

아직 확실하지 않아서 최종 매크로 API가 어떻게 보일지 확인해봐야 해요. 현재는 이 문제를 해결하지 않고 사용자가 명시적인 유형을 지정하도록 강제할 거예요.  

더 깔끔한 경우가 있고 유형을 추론하고 싶다면 다음과 같이 수행할 수 있어요:  

```js
typeAnnotation = await builder.inferType(omittedTypeAnnotation);
```

<div class="content-ad"></div>

# 매크로 테스트

## 단위 테스트

가장 먼저 취할 수 있는 접근 방법은 단위 테스팅입니다:

- Mock ClassDeclaration 및 주어진 단계에 대한 빌더를 사용합니다.
- 매크로를 일반 객체로 인스턴스화하고 인터페이스 메서드를 호출합니다.
- 인보케이션이 원하는 코드를 생성하기 위해 필요한 모든 빌더 메서드 호출에 결과가 나왔는지 확인합니다.

<div class="content-ad"></div>

이 작업에는 많은 노력이 필요합니다. 가짜 빌더는 식별자, StaticType 등을 해결해야 합니다. 아마도 Dart 팀에서 테스트 스위트를 기다려서 간소화하는 것이 좋을 것 같습니다.

## 통합 테스트

@Args 매크로의 경우, 다음과 같이 간단합니다:

- 명령줄 인수를 예상하는 다양한 프로그램 작성
- 이들을 실행
- 출력 및 오류 메시지 확인

<div class="content-ad"></div>

args_macro 패키지의 주 브랜치를 확인해보세요. 거기서 테스트가 어떻게 만들어졌는지 살펴봅시다.

그 버전은 아직 빠진 멋진 기능들인 이중 인수, 부울 플래그, 그리고 더 읽기 쉬운 오류 메시지를 추가했어요. 이들은 매크로와는 그리 많은 관계가 없으니, 여기선 넘어갈게요.

변경사항을 확인해보세요:

![diff](/assets/img/2024-08-03-DeepdiveintowritingmacrosinDart35_11.png)

<div class="content-ad"></div>

모든 테스트는 args_macro_test.dart에 있습니다.

예제/lib에는 성공적으로 실행하거나 실패할 것으로 예상되는 많은 샘플 프로그램이 있습니다.

## 런타임 오류 테스트

main.dart는 모든 기능과 패키지가 지원하는 30가지 다른 인수를 포함하는 아주 긴 파일입니다.

<div class="content-ad"></div>

명령줄 인수가 잘못 설정되면 성공적으로 실행되거나 오류가 발생할 수 있습니다. 이를 테스트하는 코드는 다음과 같습니다.

```js
const _executable = 'lib/main.dart';
const _experiments = ['macros'];
const _workingDirectory = 'example';
const _usageExitCode = 64;

// ...

group('int', () {
  test('missing required', () async {
    final arguments = {..._arguments};
    arguments.remove(_requiredInt);

    final result = await dartRun(
      [_executable, ...arguments.values],
      experiments: _experiments,
      workingDirectory: _workingDirectory,
      expectedExitCode: _usageExitCode,
    );

    expect(
      result.stderr,
      'Option "$_requiredInt" is mandatory.\n\n$_helpOutput',
    );
  });

  // ...
```

이러한 테스트는 매크로에 특정되어 있지 않기 때문에 그것들을 자세히 살펴보지는 않겠습니다. 그러나 Dart 프로그램을 쉽게 실행하고 예상대로 작동하는지 확인하기 위해 만든 도우미인 'dartRun' 함수를 사용하고 있습니다. 이 함수는 'test_util' 패키지에 있습니다.

## 매크로 오류 진단 테스트

<div class="content-ad"></div>

다음은 더 흥미로운 테스트입니다:

```js
const _compileErrorExitCode = 254;

// ...

test('error_iterable_nullable', () async {
  await dartRun(
    ['lib/error_iterable_nullable.dart'],
    experiments: _experiments,
    workingDirectory: _workingDirectory,
    expectedExitCode: _compileErrorExitCode,
    expectedErrors: const [
      ExpectedFileErrors('lib/error_iterable_nullable.dart', [
        ExpectedError(
          'A List cannot be nullable because it is just empty '
          'when no options with this name are passed.',
          [7],
        ),
        ExpectedError(
          'A Set cannot be nullable because it is just empty '
          'when no options with this name are passed.',
          [8],
        ),
      ]),
    ],
  );
});
```

동일한 dartRun 함수를 사용하여 특정 행 번호에서 특정 컴파일 오류 메시지를 예상할 수 있습니다.

컴파일러의 기계 판독 가능한 출력을 사용하지 않고 정규 출력을 분석하기 때문에 매우 신뢰성이 떨어집니다. 일부 예외 케이스나 형식이 변경된 경우 실패할 수 있습니다.

<div class="content-ad"></div>

기억해야 할 몇 가지 사항이 있어요:

- 프로그램을 실행할 때 컴파일러는 최대 10개의 오류 메시지를 출력합니다. 이는 각 샘플 프로그램을 최대 9개의 메시지로 테스트할 수 있다는 것을 의미합니다 (10개가 있으면 11번째 메시지가 버려졌는지 알 수 없습니다). 이것은 우리가 많은 작은 프로그램을 테스트하는 이유 중 하나에요.
- 안정되지 않은 기능인 매크로와 같은 기능들 때문에 분석기의 결과와 컴파일러의 출력을 사용해야 해요. 이것은 그들이 지금과 같이 다를 수 있기 때문에 중요해요. 이 버그를 예로 들어요. 이것은 분석기의 결과가 언어 서버 프로토콜로 잘 정리되어 있고 10개의 오류로 제한되어 있지 않다는 것을 보여줍니다.

# 결론

이것은 제품 출시 준비가 된 상태가 아닙니다. API가 변경될 뿐만 아니라 규격에 맞게 구현되지 않은 것도 많이 있어요.

<div class="content-ad"></div>

당신이 일한 창의성의 효과를 연습한 것으로 생각해주세요.

이것과 노는 시간을 즐겼습니다. 하지만 한 달이 걸릴 거라는 사실을 알았더라면, 이로운 일이나 하려고 했을 겁니다. 그래도 적어도 여러분은 이를 통해 무언가를 배웠을지도 모르겠네요.

스토리를 놓치지 마세요. 제 주요 매체인 텔레그램에서 저를 팔로우해주세요. 그리고 트위터, 링크드인, 그리고 깃허브도요.

# 좋은 정보들 요약

<div class="content-ad"></div>

## Dart **리소스**:

- **매크로 특징 명세**.
- **증대 특징 명세**.
- **매크로 패키지**.
- **json 패키지** 및 **@JsonCodable** 예제 매크로.
- **매크로 API**.

## **내 패키지**:

- **args_macro** — 이 글의 주제.
- **macro_util** — 필드 내부 검사.
- **show_augmentation** — 만약 당신의 IDE가 표시하지 않는다면 증대 표시.
- **test_util** — dartRun 기능.
- **common_macros** — **@Constructor** 매크로.
- **enum_map** — 다른 좋은 매크로 예제, v0.4+로, 현재 사전 릴리스 중.