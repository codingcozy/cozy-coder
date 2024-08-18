---
title: "TypeScript 작성할 때 알아두면 유용한 꿀팁 5가지"
description: ""
coverImage: "/assets/img/2024-08-13-5ProTipsfromaStaffEngineersTypeScriptCode_0.png"
date: 2024-08-13 11:08
ogImage:
  url: /assets/img/2024-08-13-5ProTipsfromaStaffEngineersTypeScriptCode_0.png
tag: Tech
originalTitle: "5 Pro Tips from a Staff Engineers TypeScript Code"
link: "https://medium.com/gitconnected/5-pro-tips-from-a-staff-engineers-typescript-code-1680adb4eaf8"
isUpdated: true
updatedAt: 1723863029963
---

![이미지](/assets/img/2024-08-13-5ProTipsfromaStaffEngineersTypeScriptCode_0.png)

저는 최근에 스탭 소프트웨어 엔지니어인 동료와 프로젝트를 같이 진행했는데, 그 분의 TypeScript 코딩 스타일에서 몇 가지 기술을 발견해서 눈에 띄었습니다. 이러한 관행들은 초보자나 중급 개발자와 경험있는 전문가가 코딩에 접근하는 방식을 명확하게 보여주었습니다.

스태프 엔지니어와 함께 일하는 것은 깨우치는 경험이 될 수 있습니다. 이 프로젝트에서 나는 그의 방대한 경험을 가진 사람과 내 코드를 비교했을 때 기대했지만 조금은 우려스러웠습니다. 프로젝트에 깊이 들어가면서, 그의 코드를 차별화하는 여러 기술과 관행들이 점차 명확해졌습니다. 그래서, 저는 그의 TypeScript 코드에서 발견한 이 5가지 핵심 관행을 공유하고자 합니다. 이것들을 사용하여 전문 프로그래머가 되는 데 도움이 될 수 있습니다!

먼저 코드가 어떻게 보였는지 살펴보고, 그 다음으로는 그 5가지 핵심 포인트를 확인하고, 코드를 작성할 때 다음 번에 유용하게 활용할 수 있도록 간단히 설명하겠습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
// 학생 시험 결과를 처리하는 processStudentExamResults 함수는 평균 점수, 고유 학점, 과목 및 통과한, 실패한 및 전체 학생에 대한 요약을 생성합니다. 코드 스니펫은 다음과 같습니다:

// Types 정의
type ExamResult = {
  subject: string,
  score: number,
  grade: string,
  comments?: string,
};

type Student = {
  name: string,
  age: number,
  results: ExamResult[],
};

type ProcessedStudentDetails = Pick<Student, "name" | "age"> & {
  averageScore: number,
  allGrades: string,
  allSubjects: string,
  comments: string[],
};

type StudentSummary = {
  totalStudents: number,
  averageScore: number,
  allGrades: string,
  allSubjects: string,
  comments: string[],
};

const createGradeChecker = (failingGrade: string) => (grade: string) => grade !== failingGrade;
const passingGrade = createGradeChecker("F");

// 평균 점수 계산 함수
const calculateAverageScore = (results: ExamResult[]): number =>
  results.reduce((acc, result) => acc + result.score, 0) / results.length;

// 학생 세부 정보를 순차적으로 생성하는 생성기 함수
function* studentDetailsGenerator(students: Student[]): Generator<Student> {
  for (const student of students) {
    yield student;
  }
}

// 학생 결과 처리 메인 함수
const processStudentExamResults = (students: Student[]) => {
  // 통과, 실패한 학생들 필터링
  const passedStudents = students.filter((student) => student.results.some((result) => passingGrade(result.grade)));
  const failedStudents = students.filter((student) => student.results.every((result) => !passingGrade(result.grade)));

  // 생성된 학생 세부 정보를 대략적으로 처리하는 함수
  const processStudentDetail = (student: Student): ProcessedStudentDetails => {
    const { name, age, results } = student;
    const averageScore = calculateAverageScore(results);
    const allGrades = [...new Set(results.map((result) => result.grade))].join(", ");
    const allSubjects = [...new Set(results.map((result) => result.subject))].join(", ");
    const comments = results.flatMap((result) => result.comments || []);

    return {
      name,
      age,
      averageScore,
      allGrades,
      allSubjects,
      comments,
    };
  };

  // 결과 요약 함수
  const summarizeDetails = (processedDetails: ProcessedStudentDetails[]): StudentSummary => {
    const totalStudents = processedDetails.length;
    const averageScore = calculateAverageScore(
      processedDetails.map((student) => ({
        subject: "",
        score: student.averageScore,
        grade: "",
      }))
    );
    const allGrades = [...new Set(processedDetails.flatMap((student) => student.allGrades.split(", ")))].join(", ");
    const allSubjects = [...new Set(processedDetails.flatMap((student) => student.allSubjects.split(", ")))].join(", ");
    const comments = processedDetails.flatMap((student) => student.comments);

    return {
      totalStudents,
      averageScore,
      allGrades,
      allSubjects,
      comments,
    };
  };

  // 통과한 학생들 처리
  const passedDetails: ProcessedStudentDetails[] = [];
  for (const student of studentDetailsGenerator(passedStudents)) {
    passedDetails.push(processStudentDetail(student));
  }

  // 실패한 학생들 처리
  const failedDetails: ProcessedStudentDetails[] = [];
  for (const student of studentDetailsGenerator(failedStudents)) {
    failedDetails.push(processStudentDetail(student));
  }

  // 모든 학생 처리
  const allDetails: ProcessedStudentDetails[] = [];
  for (const student of studentDetailsGenerator(students)) {
    allDetails.push(processStudentDetail(student));
  }

  return {
    passedSummary: summarizeDetails(passedDetails),
    failedSummary: summarizeDetails(failedDetails),
    allSummary: summarizeDetails(allDetails),
  };
};

// 함수에 샘플 데이터를 사용하여 호출한 결과는 다음과 같았습니다:

// 샘플 학생 입력
const students: Student[] = [
  {
    name: "Alice",
    age: 20,
    results: [
      { subject: "Math", score: 85, grade: "A", comments: "Excellent" },
      { subject: "English", score: 78, grade: "B", comments: "Good" },
      { subject: "Science", score: 92, grade: "A", comments: "Outstanding" },
    ],
  },
  {
    name: "Bob",
    age: 22,
    results: [
      { subject: "Math", score: 65, grade: "C", comments: "Needs Improvement" },
      { subject: "English", score: 88, grade: "A", comments: "Very Good" },
      { subject: "Science", score: 75, grade: "B", comments: "Good" },
    ],
  },
  {
    name: "Charlie",
    age: 21,
    results: [
      { subject: "Math", score: 50, grade: "F", comments: "Failed" },
      { subject: "English", score: 45, grade: "F", comments: "Failed" },
      { subject: "Science", score: 55, grade: "F", comments: "Failed" },
    ],
  },
];

// 함수 호출
const processedStudentSummary = processStudentExamResults(students);
console.info(processedStudentSummary);
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

주어진 입력 데이터에 대한 출력 결과는 합격한 학생, 불합격한 학생 및 전반적인 성적 개요를 반환했습니다:

```js
{
  passedSummary: {
    totalStudents: 2,
    averageScore: 80.5,
    allGrades: 'A, B, C',
    allSubjects: '수학, 영어, 과학',
    comments: [
      '우수함',
      '좋음',
      '뛰어남',
      '향상이 필요함',
      '매우 좋음',
      '좋음'
    ]
  },
  failedSummary: {
    totalStudents: 1,
    averageScore: 50,
    allGrades: 'F',
    allSubjects: '수학, 영어, 과학',
    comments: [ '불합격', '불합격', '불합격' ]
  },
  allSummary: {
    totalStudents: 3,
    averageScore: 70.33333333333333,
    allGrades: 'A, B, C, F',
    allSubjects: '수학, 영어, 과학',
    comments: [
      '우수함',
      '좋음',
      '뛰어남',
      '향상이 필요함',
      '매우 좋음',
      '좋음',
      '불합격',
      '불합격',
      '불합격'
    ]
  }
}
```

이제 코드를 보셨는데, 처음에는 조금 어려울 수 있습니다. 하지만, 한 번 따라가 보세요. 그러면 끝까지 쉽게 다가갈 거예요.

## 1. "고차 함수"의 광범위한 활용

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

가장 먼저 눈에 띄는 점은 코드에서 map, reduce, filter, some, every 등과 같은 함수들이 굉장히 많이 사용된다는 것입니다. 이러한 함수들은 고차 함수라고 불리며, 종종 HOFs로도 알려져 있습니다.

이러한 함수들은 다른 함수를 인수로 받거나 함수를 반환하는 형태입니다. 이러한 실천은 함수형 프로그래밍에서 일반적이며 더 추상적이고 재사용 가능한 코드를 개발하는 데 도움이 됩니다. 코드에서 HOFs가 사용된 부분을 가리킬 수 있는 몇 가지 예시는 다음과 같습니다:

```js
// 'reduce'를 사용한 평균 점수 계산
const calculateAverageScore = (results: ExamResult[]): number =>
  results.reduce((acc, result) => acc + result.score, 0) / results.length;
```

```js
// 'some'과 'every' HOFs를 사용한 학생 필터링
const passedStudents = students.filter((student) => student.results.some((result) => passingGrade(result.grade)));
const failedStudents = students.filter((student) => student.results.every((result) => !passingGrade(result.grade)));
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
// 'map'과 'flatMap' HOFs를 사용한 파싱 세부 사항
const allGrades = [...new Set(results.map((result) => result.grade))].join(', ');
const allSubjects = [...new Set(results.map((result) => result.subject))].join(', ');
const comments = results.flatMap((result) => result.comments || []);

## 2. “Generator function”의 실용적인 사용

아마도 코드에서 function* studentDetailsGenerator()를 주목했나요?

// 한 번에 하나의 학생 세부 정보를 생성하는 제너레이터 함수
function* studentDetailsGenerator(students: Student[]): Generator<Student> {
  for (const student of students) {
    yield student;
  }
}

...
// 모든 학생 처리
const allDetails: ProcessedStudentDetails[] = [];
for (const student of studentDetailsGenerator(students)) {
  allDetails.push(processStudentDetail(student));
}
...

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

실제 코드에서 yield를 사용하는 사람들이 누구인지 궁금했던 적이 있었어요. 기본적인 방법으로 비슷한 결과를 얻을 수 있는데, 이를 왜 사용하는지 의문이었죠. 그러나 무지하다고 하더라도, 실제로 실용적인 응용 프로그램에서 사용된다는 것을 발견했어요!

그러니 생성자(generator) 함수는 필요에 따라 값을 생성하여 실행을 일시 중단하고 상태를 유지한 다음 이전 yield 실행 이후에 다시 실행을 재개하는 방식으로 동작해요. 이 방법은 데이터셋이 매우 큰 경우 값의 게으른(lazy) 평가에 특히 유용하며, 사용자 정의 반복자(iterators)를 구현하거나 명확하게 비동기 코드를 작성할 때 사용돼요.

## 3. Spread 연산자와 객체 비구조화

Spread 연산자 ... 와 객체 비구조화는 현대 JavaScript 기술로, 객체와 배열을 간결하고 표현적으로 조작할 수 있는 기술이에요. 이들이 코드를 깨끗하고 가독성 있게 만들어주며, 데이터 처리가 더 쉬워지는 방식을 정말 좋아해요. 이를 몇 줄의 코드로 보는 우아한 방법을 살펴봐요:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

// Set 및 spread 연산자를 사용하여 고유한 성적 및 과목 목록을 만드는 예제
const allGrades = [...new Set(results.map((result) => result.grade))].join(', ');
const allSubjects = [...new Set(results.map((result) => result.subject))].join(', ');

// 객체 해체를 사용하여 학생 속성 가져오기
const { name, age, results } = student;

경험이 적거나 초보 수준의 개발자는 이러한 기법에 대한 지식이 부족하여 동일한 코드를 다음과 같이 작성할 수 있습니다:

// 반복문과 배열 사용해 고유한 성적 및 과목 목록 만들기
const allGradesArray = [];
const allSubjectsArray = [];

for (let i = 0; i < results.length; i++) {
    const grade = results[i].grade;
    const subject = results[i].subject;

    if (!allGradesArray.includes(grade)) {
        allGradesArray.push(grade);
    }

    if (!allSubjectsArray.includes(subject)) {
        allSubjectsArray.push(subject);
    }
}

const allGrades = allGradesArray.join(', ');
const allSubjects = allSubjectsArray.join(', ');

// 객체 해체를 사용하지 않고 학생 속성에 액세스
const name = student.name;
const age = student.age;
const results = student.results;

결과는 동일하지만 코드 줄 수 및 성능 면에서 효율성이 중요합니다. 이 모든 것은 경험에서 오는 것이죠. 걱정하지 마세요! 블로그를 통해 이에 대한 도움을 드릴게요 ;)

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 4. 고급 유형 정의

이미 알고 계시다시피 TypeScript는 모두 '유형(types)'에 관한 것입니다. 따라서 코드의 견고성과 유지 관리성을 향상시키기 위해 유형을 정의하고 처리하는 방법을 알아야 합니다.

// 유형 정의
type ExamResult = {
  subject: string;
  score: number;
  grade: string;
  comments?: string;
};

...

type ProcessedStudentDetails = Pick<Student, 'name' | 'age'> & {
  averageScore: number;
  allGrades: string;
  allSubjects: string;
  comments: string[];
};

...

코드에서 ProcessedStudentDetails 및 ExamResult와 같은 모든 복잡한 유형은 명확하고 명시적인 유형 정의를 제공하기 위해 정의되어 있어 잠재적인 오류를 줄일 수 있습니다. 이는 정말 좋은 점입니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

하지만 더 좋은 것은 Pick 유틸리티 타입이 기존 타입에서 선택한 속성을 기반으로 새로운 타입을 생성하는 방식입니다! 언어의 고급 기능을 알면 소프트웨어 엔지니어링 경력을 전진시키고 기술을 향상시키는 데 중요합니다.

## 5. 커링 기술의 사용

이 코드는 학생들의 성적을 확인하는 함수를 생성하기 위해 인기있는 커링 기술도 활용합니다.

// 등급 확인 기본 함수를 생성하는 커링 함수
const createGradeChecker = (failingGrade: string) => (grade: string) => grade !== failingGrade;

// 합격 등급을 확인하는 기본 함수
const passingGrade = createGradeChecker('F');

...
  // 불합격 학생 필터링
  const failedStudents = students.filter(student =>
    student.results.every(result => !passingGrade(result.grade))
  );

...

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

코드 스니펫에서 createGradeChecker 함수를 보셨을 겁니다. 단순하게 표시하기 위해 여기에는 passingGrade 함수만 표시되어 있습니다. 해당 함수는 학생이 통과했는지 여부를 확인하고 반환합니다.
하지만, 우리는 마찬가지로 서로 다른 맥락에 대해 서로 다른 등급을 정의할 수 있습니다. 다음과 같이:

// 총점이 Distinction 또는 Outstanding인 학생을 확인합니다
const distinctionGrade = createGradeChecker('O');

이것이 커링의 아름다움입니다. 이는 함수를 일련의 함수로 변환하여 매 단일 인수를 취하게 만드는 것을 허용합니다. 그리고 위에서 보신 것처럼, 이는 함수를 부분적으로 적용하도록 함으로써 재사용성과 가독성을 도입합니다.

좋아요! 이 블로그는 여기까지입니다. 적어도 이 중 1개 또는 2개의 기술이 여러분의 레벨 업에 도움이 될 것을 희망합니다. 이러한 최선의 방법을 수용하면 여러분의 코딩 기술을 향상시키고 돋보이는 전문가 수준의 코드를 작성할 수 있게 됩니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그러니 다음 프로젝트를 할 때는 코드에 이러한 고급 기술을 활용해보고 코드 리뷰어들을 감동시켜보세요 ;)

최고의 실천 방법을 배우고 기술을 향상시키는 것에 관심이 있다면, 다음으로 읽어볼 것을 추천합니다:

![이미지](https://miro.medium.com/v2/resize:fit:292/0*4IT9_pPBwe7zobBW.gif)

만약 이 글이 유용했다면, 이 게시물에 박수👏🏼 50번과 내 블로그를 팔로우 ❤️를 꼭 눌러주세요.
그리고 다음 게시물을 직접 이메일로 받아보기 위해 구독해주세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

행복한 학습되세요! 🚀
```
