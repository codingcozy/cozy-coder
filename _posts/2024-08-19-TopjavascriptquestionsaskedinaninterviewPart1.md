---
title: "기술면접에서 가장 많이 물어보는 자바스크립트 질문"
description: ""
coverImage: "/assets/img/2024-08-19-TopjavascriptquestionsaskedinaninterviewPart1_0.png"
date: 2024-08-19 02:07
ogImage: 
  url: /assets/img/2024-08-19-TopjavascriptquestionsaskedinaninterviewPart1_0.png
tag: Tech
originalTitle: "Top javascript questions asked in an interview  Part 1"
link: "https://medium.com/@mhetreayush1719/top-javascript-questions-asked-in-an-interview-part-1-2e4d6ee63127"
isUpdated: true
updatedAt: 1724034926287
---



![이미지](/assets/img/2024-08-19-TopjavascriptquestionsaskedinaninterviewPart1_0.png)

**주의:** 이 기사의 질문은 다음 LinkedIn 게시물에서 직접 가져온 것입니다: 링크

저는 친절하게 Alpana P.님이 이 질문에 대한 해결책을 공유할 수 있도록 허용해 주신 데 대해 감사드립니다. 모든 코드와 테스트는 관련 GitHub 저장소에서 확인하실 수 있습니다: 링크.

중요: 총 21개의 질문이 있으므로 해결책을 두 부분으로 나누었습니다. 두 번째 부분에 대한 링크는 이 글의 끝에서 확인하실 수 있습니다.


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

# 1. 주어진 문장에서 가장 긴 단어를 찾는 프로그램입니다.

```js
export const findLongestWord = (sentence: string): string => {
  let currentMaxLen = 0;
  let currentAns = "";
  sentence.split(" ").forEach((word) => {
    const wordLength = word.length;
    if (wordLength > currentMaxLen) {
      currentAns = word;
      currentMaxLen = wordLength;
    }
  });
  return currentAns;
};
```

- 문장을 단어로 분할합니다.
- 각 단어를 반복하며 검사합니다.
- 가장 긴 단어의 길이와 해당 단어를 추적합니다.
- 더 긴 단어가 발견되면 가장 긴 단어를 업데이트합니다.
- 가장 긴 단어를 반환합니다.

# 2. 주어진 문자열이 회문인지 확인합니다.

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
영문을 소문자로 변환하고 공백을 제거합니다.
문자열의 시작 지점에 있는 왼쪽 포인터와 끝 지점에 있는 오른쪽 포인터를 초기화합니다.
왼쪽과 오른쪽 포인터의 문자를 비교합니다.
왼쪽 포인터를 오른쪽으로 이동하고 오른쪽 포인터를 왼쪽으로 이동합니다.
일치하지 않는 문자가 발견되면 false를 반환합니다.
모든 문자가 일치하면 true를 반환합니다.

# 3. 배열에서 중복 항목을 제거하는 프로그램을 작성하세요.

export const removeDuplicates = (arr: any[]): any[] => {
  return [...new Set(arr)];
};

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

- 입력 배열에서 중복을 자동으로 제거하여 새 Set을 만듭니다.
- Spread 구문 (...)을 사용하여 Set을 다시 배열로 변환합니다.
- 중복이 제거된 새 배열을 반환합니다.

# 4. 빌트인 메소드를 사용하지 않고 문자열의 역을 찾는 프로그램.

export const reverseString = (str: string): string => {
  // 내장 reverse 메소드를 사용하지 않고 문자열을 뒤집습니다.

  let ans = "";

  for (var i = str.length - 1; i >= 0; i--) {
    ans += str[i];
  }

  return ans;
};

- 역으로 저장할 빈 문자열 ans를 초기화합니다.
- 입력 문자열 str을 마지막 문자부터 처음까지 반복합니다.
- 각 문자를 ans 문자열에 추가합니다.
- 역으로 된 문자열 ans를 반환합니다.

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

# 5. 배열에서 연속으로 나타나는 1의 최대 개수를 찾아보세요.

export const findMaxConsecutiveOnes = (arr: number[]): number => {
  let ans = 0;
  let currentMax = 0;
  for (var i = 0; i < arr.length; i++) {
    if (arr[i] == 1) currentMax++;
    else {
      ans = Math.max(ans, currentMax);
      currentMax = 0;
    }
  }
  ans = Math.max(ans, currentMax);
  return ans;
};

- ans와 currentMax 변수를 0으로 초기화하여 연속되는 최대 1의 개수와 현재 연속되는 1의 개수를 추적합니다.
- 입력 배열 arr을 반복합니다.
- 현재 요소가 1이면 currentMax를 증가시킵니다.
- 현재 요소가 0이면:
  - ans를 ans와 currentMax 중 최대값으로 업데이트합니다.
  - currentMax를 0으로 재설정합니다.
- 반복문이 종료된 후, 나머지 연속되는 1을 고려하여 ans를 마지막으로 업데이트합니다.
- 배열에서 연속으로 나타나는 1의 최대 개수를 나타내는 ans를 반환합니다.

# 6. 주어진 숫자의 팩토리얼을 찾아보세요.

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

export const factorial = (n: number): number => {
  if (n < 0) throw new Error("n은 양수여야 합니다.");

  let dp = new Array(n + 1);
  dp[0] = 1;

  for (let i = 1; i <= n; i++) {
    dp[i] = dp[i - 1] * i;
  }

  return dp[n];
};

- 잘못된 입력 처리: 입력 값 n이 음수인 경우 오류를 발생시킵니다.
- DP 배열 초기화: 팩토리얼 값을 저장할 크기가 n + 1인 배열 dp를 생성합니다. dp[0]를 1로 설정합니다 (0의 팩토리얼은 1입니다).
- 팩토리얼 계산: 1부터 n까지 반복하면서 이전 팩토리얼을 사용하여 각 팩토리얼 값을 계산합니다: dp[i] = dp[i - 1] * i.
- 결과 반환: n의 팩토리얼을 반환하며, 이 값은 dp[n]에 저장됩니다.

# 7. 정렬된 두 배열 [0,3,4,31]과 [4,6,30]이 주어졌을 때, 이를 병합하고 [0,3,4,4,6,30,31]로 정렬합니다.

export const mergeSortedArrays = (arr1: number[], arr2: number[]): number[] => {
  let ptr1 = 0;
  let ptr2 = 0;
  let arr1Size = arr1.length;
  let arr2Size = arr2.length;
  let ans: number[] = [];
  while (ptr1 < arr1Size && ptr2 < arr2Size) {
    if (arr1[ptr1] < arr2[ptr2]) {
      ans.push(arr1[ptr1]);
      ptr1++;
    } else {
      ans.push(arr2[ptr2]);
      ptr2++;
    }
  }

  while (ptr1 < arr1Size) {
    ans.push(arr1[ptr1]);
    ptr1++;
  }

  while (ptr2 < arr2Size) {
    ans.push(arr2[ptr2]);
    ptr2++;
  }

  return ans;
};

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

- ptr1과 ptr2를 각각 arr1과 arr2의 시작 지점으로 초기화합니다.
- 병합된 결과를 저장할 빈 배열 ans를 초기화합니다.
- ptr1과 ptr2의 요소를 비교합니다:
  - 작은 요소를 ans에 추가합니다.
  - 해당 포인터를 증가시킵니다.
- 두 배열 중 하나가 소진될 때까지 계속합니다.
- 소진되지 않은 배열에서 남은 요소를 ans에 추가합니다.
- 병합 및 정렬된 배열 ans를 반환합니다.

# 8. 두 배열 arr1과 arr2를 인수로 받는 함수를 작성하세요. 함수는 arr1의 각 값이 arr2에서 해당하는 값의 제곱임을 확인해야 합니다. 값의 빈도는 동일해야 합니다.

export const correspondingSquare = (
  arr1: number[],
  arr2: number[]
): boolean => {
  var arr1Size = arr1.length;
  var arr2Size = arr2.length;

  if (arr1Size != arr2Size) return false;

  for (var i = 0; i < arr1Size; i++) {
    if (Math.pow(arr1[i], 2) != arr2[i]) return false;
  }

  return true;
};

- 입력 배열 arr1과 arr2의 길이가 같은지 확인합니다. 그렇지 않으면 false를 반환합니다.
- arr1의 각 요소를 반복합니다.
- arr1의 현재 요소의 제곱을 arr2의 해당 요소와 비교합니다. 일치하지 않으면 false를 반환합니다.
- 모든 요소가 일치하면 true를 반환합니다.

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

# 9. 두 개의 문자열을 주어진다. 한 문자열이 다른 문자열의 문자를 재배열하여 형성될 수 있는지 찾습니다.

const formatString = (str: string): string => {
  str = str.replace(/ /g, "");
  str = str.toLowerCase();
  return str;
};

export const otherStringFromFirst = (
  source: string,
  target: string
): boolean => {
  source = formatString(source);
  target = formatString(target);

  const sourceLength = source.length;
  const targetLength = target.length;

  if (sourceLength != targetLength) return false;

  let sourceFreqMap: Record<string, number> = {};
  let targetFreqMap: Record<string, number> = {};

  for (var i = 0; i < sourceLength; i++) {
    sourceFreqMap[source[i]] = (sourceFreqMap[source[i]] || 0) + 1;
    targetFreqMap[target[i]] = (targetFreqMap[target[i]] || 0) + 1;
  }

  for (const char in sourceFreqMap) {
    if (sourceFreqMap[char] !== targetFreqMap[char]) {
      return false;
    }
  }

  return true;
};

문자열 전처리:

- formatString 함수는 공백을 제거하고 입력 문자열을 일관된 비교를 위해 소문자로 변환합니다.
- 소스 및 대상 문자열은 formatString을 사용하여 전처리됩니다.

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

길이 확인:

- source와 target의 길이가 같지 않은 경우, 길이가 같아야 동일한 문자로 형성될 수 없기 때문에 함수는 즉시 false를 반환합니다.

빈도 매핑:

- 두 빈도 맵, sourceFreqMap과 targetFreqMap이 생성되어 각 문자의 수를 저장합니다.
- 함수는 두 문자열을 반복하면서 빈도 맵을 업데이트합니다.

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

빈번도 비교:

- 이 함수는 sourceFreqMap의 키(문자)를 반복합니다.
- 각 문자에 대해 sourceFreqMap에서의 빈도가 targetFreqMap에서의 빈도와 같은지 확인합니다. 그렇지 않은 경우 false를 반환합니다.

결과:

- 모든 문자 빈도가 일치하는 경우, 대상 문자열이 소스 문자열의 문자로 형성될 수 있다는 것을 나타내기 위해 함수는 true를 반환합니다.

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

# 10. 아래 배열에서 고유한 객체를 얻는 논리를 작성하십시오.
입력: ['name: “sai”', 'name:”Nang”', 'name: “sai”', 'name:”Nang”', 'name: “111111”'];

# 출력: ['name: “sai”', 'name:”Nang”', 'name: “111111”']

export const uniqueObjectsFromArray = (
  arr: Record<any, any>[]
): Record<any, any>[] => {
  const objStrings = arr.map((obj) => JSON.stringify(obj));
  const unique = [...new Set(objStrings)];
  const parsed = unique.map((str) => JSON.parse(str));

  return parsed;
};

- 객체를 문자열로 변환: map 함수를 사용하여 입력 배열 arr의 각 객체를 JSON.stringify(obj)를 사용하여 JSON 문자열로 변환합니다. 이를 통해 객체를 쉽게 비교할 수 있습니다.
- 고유한 문자열 집합 생성: JSON 문자열의 배열(objStrings)로부터 새로운 Set을 생성합니다. Set은 자동으로 중복을 제거하여 원본 객체의 고유한 JSON 문자열 표현만 남깁니다.
- 고유한 문자열을 다시 객체로 변환: 전개 구문 (...)은 Set을 고유한 JSON 문자열의 배열로 변환하는 데 사용됩니다. 그런 다음 다른 map 함수를 사용하여 JSON 문자열을 JSON.parse(str)를 사용하여 다시 객체로 변환합니다.
- 고유한 객체 반환: 함수는 파싱된 객체 배열(parsed)을 반환합니다. 이제 이 배열은 원본 입력 배열 arr에서만 고유한 객체만 포함하게 됩니다.

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

# Part 2로 이동하기: [링크](Link)

매일 프로젝트에서 사용하여 FullStack 웹 개발, TypeScript, DevOps를 탐구 중이며 YouTube(짧은 및 장폼 콘텐츠), Instagram Reels, LinkedIn 게시물, Medium 기사, X(이전에는 Twitter)의 콘텐츠, 동료 및 선배들을 통해 다양한 플랫폼을 통해 학습합니다. 그러므로 모든 의견/피드백/교정은 환영합니다. 
이 글을 즐기시고 새로운 것을 배워 갈 수 있기를 바랍니다!

반드시 읽어보세요: TypeScript에서 디스크리미네이트된 유니언

코딩 즐기세요!