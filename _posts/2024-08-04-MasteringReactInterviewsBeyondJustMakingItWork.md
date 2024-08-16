---
title: "프론트엔드 기술 면접에서 반드시 알아야하는 React관련 질문"
description: ""
coverImage: "/assets/img/2024-08-04-MasteringReactInterviewsBeyondJustMakingItWork_0.png"
date: 2024-08-04 18:39
ogImage: 
  url: /assets/img/2024-08-04-MasteringReactInterviewsBeyondJustMakingItWork_0.png
tag: Tech
originalTitle: "Mastering React Interviews Beyond Just Making It Work"
link: "https://medium.com/the-javascript/mastering-react-interviews-beyond-just-making-it-work-3b5ee98debd6"
isUpdated: true
---





<img src="/assets/img/2024-08-04-MasteringReactInterviewsBeyondJustMakingItWork_0.png" />

소프트웨어 개발의 세계에서 특히 시니어 역할을 지향할 때 인터뷰는 기능적 코드 작성 이상의 도전을 제시할 수 있습니다. 최근에 내 친구 중 한 명이 시니어 리엑트 개발자 역할에 대한 면접 중 이러한 경험을 겪었습니다. 이 경험은 주어진 작업을 완료하는 것뿐만 아니라 확장성, 최적화 및 적절한 상태 관리와 같은 측면을 고려하는 중요성을 강조했습니다.

## 과제

면접 중 나의 친구는 React를 사용하여 시간 제한이 있는 작업을 수행해야 했습니다. 바 차트를 만들고 오름차순, 내림차순 및 기본(원래) 순서를 선택할 수 있는 드롭다운을 추가해야 했습니다. 바 차트의 데이터는 API에서 가져와야 했습니다. 초기 구현은 예상대로 작동했지만, 최종적으로 시니어 수준의 기대에 미치지 못해 거부당했습니다.

<div class="content-ad"></div>

## 초기 구현

면접 중에 제출된 코드가 여기 있어요:

```js
import React, { useState, useEffect } from 'react';

// 데이터를 가져오는 모의 함수
const fetchData = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve([
        { name: 'A', value: 30 },
        { name: 'B', value: 80 },
        { name: 'C', value: 45 },
        { name: 'D', value: 60 },
      ]);
    }, 1000);
  });
};

const BarChart = () => {
  const [data, setData] = useState([]);
  const [sortOrder, setSortOrder] = useState('default');

  useEffect(() => {
    fetchData().then((data) => setData(data));
  }, []);

  // 데이터를 정렬하는 함수
  const sortData = (order) => {
    let sortedData;
    if (order === 'default') {
      fetchData().then((data) => setData(data));
    } else {
      sortedData = [...data].sort((a, b) => {
        if (order === 'asc') {
          return a.value - b.value;
        } else {
          return b.value - a.value;
        }
      });
      setData(sortedData);
    }
  };

  return (
    <div>
      <select onChange={(e) => { setSortOrder(e.target.value); sortData(e.target.value); }} value={sortOrder}>
        <option value="default">Default</option>
        <option value="asc">Ascending</option>
        <option value="desc">Descending</option>
      </select>
      <div>
        {data.map((item, index) => (
          <div key={index} style={{ marginBottom: '10px' }}>
            <div>{item.name}</div>
            <div
              style={{
                height: '20px',
                width: `${item.value}px`,
                backgroundColor: 'blue',
              }}
            ></div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default BarChart;
```

코드는 올바르게 작동하지만, 신중한 고려 없이 확장성, 최적화, 그리고 적절한 상태 관리가 미흡합니다. 이 부분은 시니어 개발자 역할에 매우 중요합니다.

<div class="content-ad"></div>

## 향상된 구현

여기에는 고급 기술을 적용한 코드의 개선 버전이 있습니다:

```js
import React, { useState, useMemo, useCallback, useEffect, useRef } from 'react';

// Mock function to fetch data
const fetchData = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve([
        { name: 'A', value: 30 },
        { name: 'B', value: 80 },
        { name: 'C', value: 45 },
        { name: 'D', value: 60 },
      ]);
    }, 1000);
  });
};

const BarChart = () => {
  const [data, setData] = useState([]);
  const originalData = useRef([]);
  const [loading, setLoading] = useState(false);
  const [sortOrder, setSortOrder] = useState('default');

  useEffect(() => {
    setLoading(true);
    fetchData().then((fetchedData) => {
      setData(fetchedData);
      originalData.current = fetchedData;
      setLoading(false);
    });
  }, []);

  // 데이터를 정렬하는 함수
  const sortData = useCallback(() => {
    let sortedData;
    if (sortOrder === 'default') {
      sortedData = originalData.current;
    } else {
      sortedData = [...data].sort((a, b) => {
        if (sortOrder === 'asc') {
          return a.value - b.value;
        } else {
          return b.value - a.value;
        }
      });
    }
    setData(sortedData);
  }, [data, sortOrder]);

  useEffect(() => {
    sortData();
  }, [sortOrder, sortData]);

  return (
    <div>
      <select onChange={(e) => setSortOrder(e.target.value)} value={sortOrder}>
        <option value="default">기본</option>
        <option value="asc">오름차순</option>
        <option value="desc">내림차순</option>
      </select>
      {loading ? (
        <p>로딩 중...</p>
      ) : (
        <div>
          {data.map((item, index) => (
            <div key={index} style={{ marginBottom: '10px' }}>
              <div>{item.name}</div>
              <div
                style={{
                  height: '20px',
                  width: `${item.value}px`,
                  backgroundColor: 'blue',
                }}
              ></div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default BarChart;
```

## 향상 사항 설명

<div class="content-ad"></div>

- 확장성:

- 정렬 기능: 정렬 로직은 useCallback 훅에 캡슐화되어 있어, 의존성이 변경될 때만 새로 생성되도록 보장합니다.
- 데이터 처리: 정렬 함수는 데이터의 복사본 ([...data])에서 작동하여 의도하지 않은 부작용을 방지합니다.

2. 최적화 기술:

- useCallback 훅: 이를 이용하여 sortData 함수가 메모이제이션되어 데이터나 sortOrder가 변경될 때에만 재생성됩니다. 이렇게 해서 불필요한 다시 렌더링과 계산을 방지합니다.
- useMemo 훅: 직접적으로 사용되지는 않지만, 값비싼 계산을 메모이제이션하는 데 유용할 수 있습니다.

<div class="content-ad"></div>

3. 적절한 로딩 상태:

- 로딩 인디케이터: useState로 간단한 로딩 상태를 관리하고 데이터 가져오기 및 처리 중에 토글합니다. 이는 데이터 로딩 및 정렬 중에 피드백을 제공하여 사용자 경험을 향상시킵니다.

4. 고급 Hooks 활용:

- useCallback: sortData 함수가 매 렌더링마다 재생성되지 않도록 보장하여 대규모 데이터셋을 다룰 때 성능에 중요합니다.
- useRef: 원본 데이터를 저장하는 데 사용되며 초기 상태로 쉽게 재설정 할 수 있도록 하고 다시 렌더링을 유발하지 않습니다.
- useEffect: 컴포넌트가 마운트될 때 데이터 가져오기를 트리거하고 sortOrder가 변경될 때마다 정렬을 처리합니다.

<div class="content-ad"></div>

## 주요 포인트

내 친구는 이러한 고급 기술에 대해 알고 있었지만, 시간 제약으로 인해 구현하지 못했습니다. 이 경험을 통해 중요한 교훈을 깨닫게 되었습니다: 더 나은 해결책을 알고 있지만 시간 제약으로 완전히 구현할 수 없는 경우, 이를 면접관에게 명확히 전달하는 것이 중요합니다. 더 많은 시간이 주어진다면 어떤 개선 사항을 적용할 것인지 설명하세요. 이는 당신의 지식과 이해를 나타내어 면접관의 평가에 긍정적인 영향을 줄 수 있습니다.

고급급 인터뷰에서 중요한 것은 코드를 작동시키는 것뿐만 아니라 효율적이고 확장 가능하며 사용자 친화적인 코드를 작성하는 것입니다. 메모이제이션, 적절한 상태 관리 및 성능 고려와 같은 기술을 통합함으로써 기대치를 충족하고 초과할 수 있습니다.

기억하세요, 인터뷰는 문제를 해결하는 것뿐만 아니라 가능한 최상의 방식으로 해결하는 것입니다. 이러한 통찰력을 기억하면 다음 고급 개발자 인터뷰에 더 잘 준비될 것입니다.