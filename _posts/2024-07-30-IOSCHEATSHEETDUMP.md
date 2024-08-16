---
title: "아이폰 사용자 필독 필수 치트 시트 모음 2024"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-30 16:45
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "IOS CHEAT SHEET DUMP"
link: "https://medium.com/programming1/ios-cheat-sheet-dump-ade3e5b160dc"
---


# 포괄적인 iOS 개발 치트시트

## 일반 정보

- 프로그래밍 언어: Swift, Objective-C
- 개발 환경: Xcode
- UI 프레임워크: UIKit, SwiftUI

## Xcode 기초

<div class="content-ad"></div>

- 새 프로젝트: 파일 - 새로 만들기 - 프로젝트
- 프로젝트 실행: Cmd + R
- 프로젝트 중지: Cmd + .
- 프로젝트 빌드: Cmd + B
- 빌드 폴더 정리: Cmd + Shift + K
- 어시스턴트 편집기 열기: Cmd + Option + Return
- 프로젝트 네비게이터 열기: Cmd + 1
- 소스 제어 네비게이터 열기: Cmd + 2
- 디버그 영역 열기: Cmd + Shift + Y
- 콘솔 열기: Cmd + Shift + C
- 정의로 이동: Cmd + 클릭
- 설명서 보기: Option + 클릭

## Swift 기초

변수 선언:

var 변수명 = 값
let 상수명 = 값

<div class="content-ad"></div>

데이터 유형:

```swift
let integer: Int = 10
let double: Double = 10.5
let boolean: Bool = true
let string: String = "Hello"
let array: [String] = ["A", "B", "C"]
let dictionary: [String: Int] = ["A": 1, "B": 2]
```

함수:

```swift
func functionName(parameterName: ParameterType) -> ReturnType {
    // 함수 본문
}
```

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 바꿔주세요.


| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |


<div class="content-ad"></div>

루프:

```js
for item in collection {     // 루프 내용 }  
while condition {     // 루프 내용 }  
repeat {     // 루프 내용 } 
while condition
```

옵셔널:

```js
var optionalString: String? = "안녕" 
print(optionalString!) // 강제 언래핑
if let unwrappedString = optionalString {     
    print(unwrappedString) // 옵셔널 바인딩
}
```

<div class="content-ad"></div>

## 에러 처리:

```js
enum CustomError: Error {
    case runtimeError(String)
}

func canThrowError() throws {
    throw CustomError.runtimeError("오류가 발생했습니다")
}

do {
    try canThrowError()
} catch {
    print(error)
}
```

# SwiftUI 기본 사항

뷰 구조:

<div class="content-ad"></div>

```kotlin
struct ContentView: View {     var body: some View {         Text("Hello, World!")     } }
```

Common Views:

```kotlin
Text("Hello, World!") Image(systemName: "star") Button(action: {     // action }) {     Text("Tap me!") }
```

Modifiers:

<div class="content-ad"></div>


```js
Text("Hello, World!")     .font(.largeTitle)     .foregroundColor(.blue)
```


Stacks:


```js
VStack {     Text("First")     Text("Second") }  HStack {     Text("First")     Text("Second") }  ZStack {     Text("First")     Text("Second") }
```


Navigation:


<div class="content-ad"></div>


NavigationView {     NavigationLink(destination: AnotherView()) {         Text("다른 뷰로 이동")     } }


Lists:


List(items) { item in     Text(item.name) }


Forms:

<div class="content-ad"></div>

```js
Form {     TextField("이름을 입력하세요", text: $name)     Toggle("알림 활성화", isOn: $isEnabled) }
```

바인딩:

```js
@State private var name: String = ""  var body: some View {     TextField("이름을 입력하세요", text: $name) }
```

ObservableObject:

<div class="content-ad"></div>

```swift
class ViewModel: ObservableObject {  
    @Published var text: String = "Hello" 
}

struct ContentView: View {  
    @ObservedObject var viewModel = ViewModel() 
  
    var body: some View {         
        Text(viewModel.text)     
    } 
}
```

## UIKit Basics

View Controllers:

```swift
class ViewController: UIViewController {     
    override func viewDidLoad() {         
        super.viewDidLoad()         
        // setup code     
    } 
}
```

<div class="content-ad"></div>

일반적인 UI 요소:

```swift
let label = UILabel()
label.text = "Hello, World!"

let button = UIButton(type: .system)
button.setTitle("Tap me!", for: .normal)
button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)

@objc func buttonTapped() {
    // 동작
}
```

내비게이션:

```swift
let viewController = AnotherViewController()
navigationController?.pushViewController(viewController, animated: true)
```

<div class="content-ad"></div>

테이블 뷰:

```swift
class ViewController: UIViewController, UITableViewDataSource {
    let tableView = UITableView()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.dataSource = self
        view.addSubview(tableView)
    }
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        cell.textLabel?.text = items[indexPath.row].name
        return cell
    }
}
```

컬렉션 뷰:

```swift
class ViewController: UIViewController, UICollectionViewDataSource {
    let collectionView: UICollectionView
    
    override func viewDidLoad() {
        super.viewDidLoad()
        collectionView.dataSource = self
        view.addSubview(collectionView)
    }
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return items.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "cell", for: indexPath)
        // 셀 구성
        return cell
    }
}
```

<div class="content-ad"></div>

스토리보드:

```js
// IBOutlet 및 IBAction 연결
@IBOutlet weak var label: UILabel!
@IBAction func buttonTapped(_ sender: UIButton) {
    // 액션
}
```

## 네트워킹

- URLSession:

<div class="content-ad"></div>

```js
let url = URL(string: "https://api.example.com/data")!
let task = URLSession.shared.dataTask(with: url) { data, response, error in
    guard let data = data, error == nil else {
        print("에러: \(error?.localizedDescription ?? "알 수 없는 오류")")
        return
    }
    // 데이터 처리
}
task.resume()
```

JSON 디코딩:

```js
struct Item: Codable {
    let id: Int
    let name: String
}
let decoder = JSONDecoder()
let items = try? decoder.decode([Item].self, from: data)
```

## Core Data


<div class="content-ad"></div>

Core Data 스택 설정하기:

```swift
import CoreData  

lazy var persistentContainer: NSPersistentContainer = {     
    let container = NSPersistentContainer(name: "ModelName")     
    container.loadPersistentStores { description, error in         
        if let error = error {             
            fatalError("Core Data 스택을 불러오는 데 실패했습니다: \(error)")         
        }     
    }     
    return container 
}()
```

페치 요청:

```swift
let fetchRequest: NSFetchRequest<EntityName> = EntityName.fetchRequest() 
let items = try? context.fetch(fetchRequest)
```

<div class="content-ad"></div>

저장된 컨텍스트:

```swift
do {
    try context.save()
} catch {
    print("컨텍스트 저장 실패: \(error)")
}
```

## 조합

발행자와 구독자:

<div class="content-ad"></div>

```kotlin
import Combine class ViewModel: ObservableObject { @Published var text: String = "Hello" } let viewModel = ViewModel() let cancellable = viewModel.$text.sink { newValue in print("Text changed to \(newValue)") }

## Common Shortcuts

- Comment/Uncomment Line: Cmd + /
- Show/Hide Navigator: Cmd + 0
- Show/Hide Debug Area: Cmd + Shift + Y
- Add New File: Cmd + N
- Refactor: Cmd + Shift + J
- Toggle Breakpoint: `Cmd + \`

Will Add More Overtime
```