---
title: "LeetCode 1460 서브배열을 뒤집어 두 배열을 동일하게 만들기 쉬움 - Meta 인터뷰 질문"
description: ""
coverImage: "/assets/img/2024-08-04-LeetCode1460MakeTwoArraysEqualbyReversingSubarraysEasyMetaInterviewQuestion_0.png"
date: 2024-08-04 18:33
ogImage:
  url: /assets/img/2024-08-04-LeetCode1460MakeTwoArraysEqualbyReversingSubarraysEasyMetaInterviewQuestion_0.png
tag: Tech
originalTitle: "LeetCode 1460 Make Two Arrays Equal by Reversing SubarraysEasy Meta Interview Question"
link: "https://medium.com/@fatmaerturk/leetcode-1460-make-two-arrays-equal-by-reversing-subarrays-easy-meta-interview-question-7dbf3d211419"
isUpdated: true
---

두 개의 정수 배열인 target과 arr가 주어졌습니다. 두 배열의 길이는 같습니다. 한 번에 arr의 비어있지 않은 하위 배열을 선택하여 뒤집을 수 있습니다. 원하는 만큼 많은 단계를 수행할 수 있습니다.

arr을 target으로 변환할 수 있다면 true를 반환하고, 그렇지 않으면 false를 반환합니다.

예시 1:

```js
Input: target = [1,2,3,4], arr = [2,4,1,3]
Output: true
Explanation: arr을 target으로 변환하기 위해 다음 단계를 수행할 수 있습니다:
1- 하위 배열 [2,4,1]을 뒤집어, arr은 [1,4,2,3]이 됩니다.
2- 하위 배열 [4,2]을 뒤집어, arr은 [1,2,4,3]이 됩니다.
3- 하위 배열 [4,3]을 뒤집어, arr은 [1,2,3,4]가 됩니다.
arr을 target으로 변환할 수 있는 여러 방법이 있습니다. 이것이 유일한 방법은 아닙니다.
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

예시 2:

```js
입력: target = [7], arr = [7]
출력: true
설명: arr을 뒤집지 않고 target과 동일하다.
```

예시 3:

```js
입력: target = [3,7,9], arr = [3,7,11]
출력: false
설명: arr에는 값 9가 없고, target으로 변환할 수 없다.
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

제약 조건:

- target.length == arr.length
- 1 ≤ target.length ≤ 1000
- 1 ≤ target[i] ≤ 1000
- 1 ≤ arr[i] ≤ 1000

이 문제는 arr과 target이 동일한 요소를 포함하고 있는지에 따라 결정됩니다. 이를 판단하기 위해 두 배열을 정렬할 수 있습니다. 배열이 동일한 요소를 포함하면 정렬된 버전도 동일해야 합니다. 그렇지 않으면 정렬된 버전에서 적어도 하나의 값이 일부 색인 i에서 다르게 나타날 것입니다.

```js
class Solution {
    public boolean canBeEqual(int[] target, int[] arr) {
        Arrays.sort(target);
        Arrays.sort(arr);
        for(int i=0; i < target.length; i++){
            if(arr[i] != target[i]) return false;
        }
        return true;
    }
}
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
function canBeEqual(target: number[], arr: number[]): boolean {
  target.sort((a, b) => a - b);
  arr.sort((a, b) => a - b);
  for (let i = 0; i < target.length; i++) {
    if (target[i] != arr[i]) return false;
  }
  return true;
}
```

배열 target 및 arr의 크기를 N이라고 합시다.

시간 복잡도: O(N⋅logN)

공간 복잡도: O(logN)

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

![Image](/assets/img/2024-08-04-LeetCode1460MakeTwoArraysEqualbyReversingSubarraysEasyMetaInterviewQuestion_0.png)
