---
title: "Varclus 상세 설명"
description: ""
coverImage: "/assets/img/2024-07-09-VarclusDetailedExplanation_0.png"
date: 2024-07-09 11:32
ogImage: 
  url: /assets/img/2024-07-09-VarclusDetailedExplanation_0.png
tag: Tech
originalTitle: "Varclus Detailed Explanation"
link: "https://medium.com/@venkatsaib/varclus-detailed-explanation-22d7d3c866e5"
isUpdated: true
---





안녕하세요 여러분,

오늘 이 글에서는 Varclus(Variable Clustering)를 간단하고 명확하게 단계별로 설명해 드리겠습니다. Varclus가 무엇인지 이해할 뿐만 아니라 해석하는 방법도 알 수 있을 거에요. 더불어, Excel을 사용하여 샘플 데이터의 Varclus를 찾는 실전 예제도 살펴볼 거에요.

변수 클러스터링(Varclus)은 다변량 통계에서 사용되는 기술로, 변수들을 유사점에 기초하여 클러스터로 그룹화하는 것입니다. 이 기술은 주로 변수/특성 축소에 사용됩니다.

Varclus를 계산하기 위한 기본 단계는 다음과 같아요. 각 단계를 예제와 함께 자세히 설명해 드릴 거에요.
1. 상관 행렬 계산
2. 초기 클러스터링
3. 클러스터 수 결정
4. Rsquare 계산

<div class="content-ad"></div>

Varclus를 더 잘 이해할 수 있도록 각 단계를 설명해 드리겠습니다. 따라 갈 수 있도록 6개 열과 6개 행으로 구성된 샘플 데이터 세트를 만들었습니다. 아래 링크를 통해 이 데이터에 액세스할 수 있습니다. 가능하다면, MS Excel을 사용하여 아래 스프레드시트를 여시기 바랍니다.

- Microsoft Excel: 링크
- Google Drive: 링크

![Varclus Detailed Explanation](/assets/img/2024-07-09-VarclusDetailedExplanation_0.png)

단계 1: 상관 행렬 계산
데이터 세트 내 변수 간 유사성을 평가하기 위해 상관 행렬을 계산할 수 있습니다. 이 행렬은 각 변수 쌍 사이의 상관 계수를 보여줍니다. 상관 계수가 1에 가까울수록 강한 양의 관골을 나타내며, -1에 가까울수록 강한 음의 관골을 나타냅니다. 이러한 계수를 계산하기 위해 Excel의 CORREL 함수를 사용할 수 있습니다. 예를 들어, Hours_of_Study(열 C)와 Student(열 B) 사이의 상관관계는 다음과 같은 공식을 사용하여 찾을 수 있습니다: =CORREL($B$2:$B$7,C2:C7)

<div class="content-ad"></div>

Step 2: Initial Clustering

Clustering is a powerful machine-learning technique that groups similar data points together based on their characteristics. Its goal is to organize data into clusters where points within a cluster share more similarities with each other compared to points in different clusters. There are many clustering algorithms available, but for now, let’s focus on Hierarchical Clustering.

While Microsoft Excel doesn’t offer built-in functionality for Hierarchical Clustering, I’ve used the XLSTAT add-on to perform this analysis in the attached Excel sheet. The resulting dendrogram, which is a tree-like structure, visually represents the hierarchical relationships between the data points.

<div class="content-ad"></div>

A dendrogram plot is a visual representation of hierarchical clustering that shows the arrangement of clusters formed at each step of the algorithm. Here’s a brief interpretation of a dendrogram plot and what the height of the tree signifies:

**Dendrogram Interpretation:**

- **Height of the Tree:** The height where two branches merge indicates the distance or dissimilarity between the clusters uniting. Lower height suggests more similarity between clusters, while higher height implies greater dissimilarity.
- **Cutting the Dendrogram:** Drawing a horizontal line across the dendrogram at a specific height helps determine the number of clusters. The number of vertical lines crossed by the horizontal line indicates the clusters at that dissimilarity level.

**Step 3: Determine the number of clusters**
XLSTAT provides an initial suggestion for the optimal number of clusters based on the structure of the dendrogram and the clusters themselves. Yet, we can also utilize the dendrogram to visually define our cluster boundaries. In this scenario, we will go along with XLSTAT's proposal for four clusters:
Here are the four clusters
- Cluster 1: GPA and Hours_of_Study
- Cluster 2: SAT_score and ACT_score
- Cluster 3: Extracurricular Activities
- Cluster 4: Student

<div class="content-ad"></div>

한 클러스터 내 변수 값들의 평균을 계산함으로써 각 클러스터 내의 값들을 요약하는 한 가지 방법이 있습니다. 이를 통해 각 클러스터에 대한 중심 경향 값을 얻을 수 있습니다. 아래는 클러스터 값들입니다.

![Cluster Values](/assets/img/2024-07-09-VarclusDetailedExplanation_3.png)

Step 4: Rsquare 계산하기
같은 클러스터 내 변수들은 상호 상관 관계가 있기 때문에, 각 클러스터에서 하나의 대표 변수를 선택하는 것이 좋을 수 있습니다. 이 대표 변수는 이상적으로는 자기 자신 클러스터(중심) 내 다른 변수들과 높은 상관 관계가 있어야 하며, 가장 가까운 이웃 클러스터의 변수들과는 약한 상관 관계를 가져야 합니다.

이러한 대표 변수를 식별하는 한 가지 방법은 R-제곱 값을 계산하는 것입니다. R-제곱은 한 변수로부터 다른 변수로의 설명되는 분산의 비율을 나타냅니다. 변수와 자기 자신 클러스터(중심) 사이의 R-제곱 값과 같은 변수와 가장 가까운 이웃 클러스터 사이의 R-제곱 값을 계산할 수 있습니다.

<div class="content-ad"></div>

If you want to identify the key variable that best represents its own cluster and stands out from the neighboring cluster, focus on the one with a high R-squared value internally and a low R-squared value externally. 🌟

To calculate the R-squared value, you can use the following formula in your analysis:


R-squared = 1 - (SSR/SST)


⭐ Step 4.1: Finding the Closest Cluster
Before diving into the R-squared calculations, it's crucial to determine the closest cluster to each cluster in your dataset. The Euclidean distance method is a reliable approach to measure the similarity between different data points. 🔍

<div class="content-ad"></div>

Varclus의 맥락에서, 각 클러스터의 중심 (대표점) 사이의 유클리드 거리를 계산할 수 있습니다. 주어진 클러스터에 대해 가장 작은 유클리드 거리를 가진 클러스터가 해당 클러스터의 가장 가까운 이웃으로 간주됩니다.

![이미지](/assets/img/2024-07-09-VarclusDetailedExplanation_5.png)

**단계 4.2 R 제곱 계산**
다행히도, Excel은 "RSQ"라는 편리한 기능을 제공하여 R 제곱 값을 계산할 수 있습니다. 우리는 이 기능을 활용하여 각 변수와 해당 클러스터 중심간의 R 제곱 값을 계산하고, 해당 변수와 가장 가까운 이웃 클러스터 간의 R 제곱 값을 계산할 수 있습니다.

![이미지](/assets/img/2024-07-09-VarclusDetailedExplanation_6.png)

<div class="content-ad"></div>

Step 4.3. 1-R 제곱 비율 계산하기

우리는 각 클러스터에 대표적인 변수를 선택하는 것을 목표로 하고 있습니다. 이 변수는 자신의 클러스터(중심점) 내 다른 변수들과 높은 상관 관계를 가져야 하며 가장 가까운 이웃 클러스터 내 변수들과는 약한 상관 관계를 가져야 합니다.

이와 같은 변수를 식별하기 위해 1-R 제곱 비율을 활용할 수 있습니다. 1-R 제곱 비율이 가장 낮은 변수는 해당 클러스터를 잘 대표하는 변수일 가능성이 높습니다. 이는 자신의 클러스터와 최대 상관 관계를 가지고 다음 클러스터와는 최소 상관 관계를 가진다는 것을 의미합니다.

![이미지 설명](/assets/img/2024-07-09-VarclusDetailedExplanation_7.png)

![이미지 설명](/assets/img/2024-07-09-VarclusDetailedExplanation_8.png)

<div class="content-ad"></div>

1-R-Squared 비율을 분석함으로써, 클러스터 1 내에서 GPA가 다른 변수와의 상관 관계가 Hours_of_Study보다 더 강한 것을 볼 수 있습니다. 이는 GPA가 클러스터 1 내의 공통점을 더 잘 대표하며, 가장 가까운 이웃 클러스터의 변수들과의 관계가 상대적으로 약한 것을 시사합니다. 따라서 이 분석을 바탕으로 GPA를 클러스터 1의 대표 변수로 선택할 수 있으며, 이로써 변수의 수를 두 개에서 하나로 줄일 수 있습니다.

참고
Varclus 설명에서 PCA(주성분 분석)를 사용하지 않은 점이 자주 물어지는 질문입니다. PCA가 차원 축소에 유용한 도구일 수 있지만, Varclus 과정에서 선택 사항임을 강조해야 합니다. 또한, PCA와 Varclus가 어떻게 함께 작동하는지 이해하는 것은 더 복잡할 수 있습니다.

의문이 있으시면 언제든지 댓글을 남겨주시면 최대한 빨리 답변 드리겠습니다.
독자 여러분, 감사합니다.
Venkat Sai