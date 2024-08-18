---
title: "연구 논문 리뷰 표 형식 데이터에서 트리 기반 모델이 여전히 딥러닝을 능가하는 이유"
description: ""
coverImage: "/assets/img/2024-07-07-ResearchpaperreviewWhydotreebasedmodelsstilloutperformdeeplearningontabulardata_0.png"
date: 2024-07-07 02:58
ogImage:
  url: /assets/img/2024-07-07-ResearchpaperreviewWhydotreebasedmodelsstilloutperformdeeplearningontabulardata_0.png
tag: Tech
originalTitle: "Research paper review: Why do tree based models still outperform deep learning on tabular data?"
link: "https://medium.com/@tracyrenee61/research-paper-review-why-do-tree-based-models-still-outperform-deep-learning-on-tabular-data-3bb9e9ff0846"
isUpdated: true
---

![Research Paper Review](/assets/img/2024-07-07-ResearchpaperreviewWhydotreebasedmodelsstilloutperformdeeplearningontabulardata_0.png)

요즘 저는 Kaggle 플레이그라운드 대회에서 sklearn의 ExtraTreesClassifier를 사용하여 코드 리뷰를 수행했습니다. 이 글은 여기에서 찾을 수 있습니다: [Medium 링크](https://medium.com/@tracyrenee61/sometimes-sklearn-outperforms-tensorflow-when-making-predictions-on-tabular-data-7fa997f662dc)

그러나 ExtraTreesClassifier를 사용하여 예측을 수행하기 전에 신경망을 시도했었습니다. 그러나 신경망의 문제는 정확도가 35%에 불과했다는 것이었습니다. ExtraTrees Classifier가 신경망을 능가하는 이유가 무엇인지 궁금했기 때문에 이것을 조사하기로 결정했습니다.

궁금증을 구글로 검색하다가 “Why do tree-based models still outperform deep learning on tabular data?”라는 제목의 연구 논문을 발견했습니다. 이 논문은 제 궁금증에 대한 답을 시도한 것이라고 생각해서 기계 및 딥러닝의 다이내믹스에 대한 추가 통찰을 얻을 수 있을지 확인하기 위해 읽어보기로 결정했습니다.

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

신문을 읽으신 후, 그 내용에 대한 요약을 아래에서 확인하실 수 있습니다:-

타부러 데이터에서 딥 러닝의 우수성은 명확하지 않습니다. 결과에서는 트리 기반 모델이 중간 규모의 데이터에서 여전히 최신 기술임이 나타났습니다. 그들의 뛰어난 속도를 감안하지 않은 상태에서도 결과가 나오더군요. 이 연구의 저자들은...
