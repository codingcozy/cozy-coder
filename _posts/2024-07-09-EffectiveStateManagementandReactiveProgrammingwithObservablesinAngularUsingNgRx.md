---
title: "NgRx를 사용한 Angular에서의 효율적인 상태 관리와 Reactive 프로그래밍 방법"
description: ""
coverImage: "/assets/img/2024-07-09-EffectiveStateManagementandReactiveProgrammingwithObservablesinAngularUsingNgRx_0.png"
date: 2024-07-09 10:31
ogImage: 
  url: /assets/img/2024-07-09-EffectiveStateManagementandReactiveProgrammingwithObservablesinAngularUsingNgRx_0.png
tag: Tech
originalTitle: "Effective State Management and Reactive Programming with Observables in Angular Using NgRx"
link: "https://medium.com/@bouguern.mohamed/effective-state-management-and-reactive-programming-with-observables-in-angular-using-ngrx-08880461e089"
---


## 1. 소개

현대 웹 개발에서는 상태 관리와 응용 프로그램 내에서 데이터 흐름을 원활하게 유지하는 것이 어려운 작업일 수 있습니다. Angular는 동적 웹 애플리케이션을 구축하기 위한 인기 있는 프레임워크로, 이러한 도전 과제들을 처리할 수 있는 강력한 도구를 제공합니다. Angular에서 상태 관리를 위한 가장 강력한 해결책 중 하나는 NgRx입니다. 이는 옵저버블과 반응형 프로그래밍의 강력함을 활용하는 반응형 상태 관리 라이브러리입니다. 이 블로그에서는 NgRx를 사용하여 Angular에서 옵저버블을 활용한 효과적인 상태 관리와 반응형 프로그래밍을 탐구하며, 효율적이고 유지보수가 가능한 응용 프로그램을 구축하기 위해 이 도구들을 활용하는 포괄적인 가이드를 제공합니다.

<img src="/assets/img/2024-07-09-EffectiveStateManagementandReactiveProgrammingwithObservablesinAngularUsingNgRx_0.png" />

## 2. NgRx란 무엇인가요?

<div class="content-ad"></div>

NgRx은 Angular용 반응형 라이브러리 스위트입니다. React 애플리케이션에서 흔히 사용되는 Redux 패턴에 영감을 받았습니다. 이는 Angular 애플리케이션의 상태를 예측 가능하고 일관된 방식으로 관리하기 위한 강력한 프레임워크를 제공합니다. NgRx에는 @ngrx/store, @ngrx/effects, @ngrx/entity 및 @ngrx/store-devtools 등 여러 핵심 라이브러리가 포함되어 있으며 각각이 상태 관리 및 부수 효과 처리를 간소화하는 독특한 기능을 제공합니다. 단방향 데이터 흐름을 채택함으로써 NgRx는 응용 프로그램 상태가 항상 동기화되어 디버깅 및 추론 작업을 더욱 쉽게 만들어줍니다.

- @ngrx/store는 NgRx 스위트의 핵심 라이브러리로서 Angular 애플리케이션의 상태를 관리하는 역할을 합니다. Redux 패턴을 구현하여 응용 프로그램 상태를 보유하는 중앙 집중식 저장소를 제공합니다.
- @ngrx/effects는 Angular 애플리케이션에서 부수 효과를 처리하기 위해 설계된 라이브러리입니다. 부수 효과란 응용 프로그램 상태에 영향을 주는 HTTP 요청, 로깅 및 기타 비동기 작업과 같은 작업을 의미합니다.
- @ngrx/entity는 상태에서 엔티티 컬렉션을 관리하는 프로세스를 간소화합니다. 컬렉션을 효율적으로 조작하기 위한 유틸리티 함수를 제공합니다.
- @ngrx/store-devtools는 NgRx 애플리케이션의 상태 변경을 디버깅하고 모니터링하는 데 도움이 되는 개발자 도구 세트입니다.

<div class="content-ad"></div>

## 3. Angular에서 반응형 프로그래밍이란 무엇인가요?

반응형 프로그래밍은 비동기 데이터 스트림과 변경 사항의 전파에 중점을 둔 패러다임입니다. Angular에서 반응형 프로그래밍은 주로 RxJS(JavaScript용 Reactive Extensions)를 통해 지원됩니다. 이 라이브러리를 사용하면 개발자가 선언적인 방식으로 비동기 이벤트와 데이터 스트림을 처리할 수 있습니다. Angular에서 반응형 프로그래밍은 RxJS의 핵심 개념 인 옵저버블을 사용하여 HTTP 요청, 사용자 입력 등과 같은 비동기 작업을 처리하는 데 중점을 두고 있습니다. 이 접근 방식을 통해 개발자는 비동기 데이터 흐름과 부작용을 효율적으로 관리하여 반응성이 뛰어나고 견고한 응용 프로그램을 구축할 수 있습니다.

## 4. Angular에서 옵저버블은 무엇인가요?

옵저버블은 RxJS의 중요한 기능으로, Angular의 반응형 프로그래밍 모델에서 중요한 역할을 합니다. 옵저버블은 시간이 지남에 따라 여러 값이 방출될 수 있는 데이터 스트림으로, 관찰하고 반응할 수 있습니다. 옵저버블은 개발자가 선언적으로 복잡한 이벤트 시퀀스를 구성하고 관리할 수 있는 강력한 방법을 제공합니다. Angular에서는 옵저버블이 서비스, 컴포넌트 및 다양한 Angular 모듈에서 사용되어 비동기 데이터 흐름과 상태 변경을 원할하게 관리합니다.

<div class="content-ad"></div>

## 5. NgRx의 이점

- 예측 가능한 상태 관리: NgRx는 Redux 패턴을 따르며, 데이터 흐름과 애플리케이션 상태에 대한 단일 진실의 소스를 강제합니다. 이러한 예측 가능성은 디버깅을 간소화하고 상태 변경을 보다 투명하고 추적하기 쉽게 만듭니다.
- 불변성: NgRx는 불변성을 장려하여 상태가 직접 변조되지 않도록 합니다. 대신 새로운 상태 객체가 생성되며, 이는 명확하고 일관된 응용 프로그램 상태를 유지하고 예상치 못한 부작용의 가능성을 줄이는 데 도움이 됩니다.
- 확장성: 애플리케이션이 복잡해질수록 상태와 부작용을 관리하는 것이 점점 어려워집니다. NgRx는 모듈식 설계를 통해 복잡한 상태 상호 작용 및 부작용을 다룰 수 있는 확장 가능한 아키텍처를 제공하여 대규모 애플리케이션에 적합합니다.
- 개선된 디버깅: @ngrx/store-devtools와 같은 도구를 사용하면 개발자들이 상태 변경을 검사하고 다시 실행할 수 있어 문제를 식별하고 해결하기가 더 쉬워집니다. 시간 여행 디버깅을 통해 개발자들은 상태 변경을 따라가면서 액션이 상태에 어떤 영향을 미치는지 명확히 이해할 수 있습니다.

- 리액티브 데이터가 필요한 복잡한 애플리케이션에 확장 가능하며 적합함 (예: 실시간 채팅 애플리케이션, 쇼핑 카트 등).

## 6. 전제조건

<div class="content-ad"></div>

- NgRx를 시작하기 전에, 반응형 프로그래밍, Observables 및 NgRx 개념에 기본적인 이해가 필요합니다.
- Angular CLI가 설치되어 있는지 확인하세요:

```js
npm install -g @angular/cli
```

## 7. 새 Angular 프로젝트 생성

```js
ng new angular-state-management-ngrx
cd angular-state-management-ngrx
```

<div class="content-ad"></div>

## 8. NgRx 및 관련 의존성 설치하기

NgRx는 다음과 같이 NPM을 사용하여 설치할 수 있습니다:

```js
npm install @ngrx/store@15 @ngrx/effects@15 @ngrx/entity@15 @ngrx/st
ore-devtools@15 --save
```

## 9. 이제 앱을 빌드해봅시다

<div class="content-ad"></div>


![image](/assets/img/2024-07-09-EffectiveStateManagementandReactiveProgrammingwithObservablesinAngularUsingNgRx_3.png)

9.1) product.actions.ts: NgRx에서 actions는 응용 프로그램에서 상태로 데이터를 보내는 정보의 페이로드입니다. 일반적으로 사용자 상호 작용이나 이벤트에 응답으로 디스패치됩니다. @ngrx/store의 createAction 함수를 사용하면 이러한 actions를 쉽게 만들 수 있습니다:

```js
import { createAction, props } from '@ngrx/store';
import { Product } from 'src/app/models/Product';

// 제품 불러오기
export const loadProducts = createAction('[Product] Load Products');
export const loadProductsSuccess = createAction('[Product] Load Products Success', props<{ products: Product[] }>());
export const loadProductsFailure = createAction('[Product] Load Products Failure', props<{ error: any }>());

// 제품 추가
export const addProduct = createAction('[Product] Add Product', props<{ product: Product }>());
export const addProductSuccess = createAction('[Product] Add Product Success', props<{ products: Product[] }>());
export const addProductFailure = createAction('[Product] Add Product Failure', props<{ error: any }>());

// 제품 업데이트
export const updateProduct = createAction('[Product] Update Product', props<{ product: Product }>());
export const updateProductSuccess = createAction('[Product] Update Product Success', props<{ products: Product[] }>());
export const updateProductFailure = createAction('[Product] Update Product Failure', props<{ error: any }>());

// 제품 삭제
export const deleteProduct = createAction('[Product] Delete Product', props<{ productId: number }>());
export const deleteProductSuccess = createAction('[Product] Delete Product Success', props<{ products: Product[] }>());
export const deleteProductFailure = createAction('[Product] Delete Product Failure', props<{ error: any }>());
```

- createAction: actions를 정의하는 함수입니다.
- props: actions의 payload(속성)을 정의하는 유틸리티입니다.
- Product: 제품 모델을 나타내는 인터페이스입니다.
- loadProducts: 제품을 불러오기 위한 동작을 초기화하기 위해 디스패치되는 action입니다. 페이로드를 전달하지 않습니다.
- loadProductsSuccess: 제품이 성공적으로 불러와지면 디스패치되는 action입니다. 불러온 제품들의 payload를 가지고 있습니다.
- loadProductsFailure: 제품을 불러오는 중에 오류가 발생하면 디스패치되는 action입니다. 오류의 payload를 가지고 있습니다.
- addProduct: 새 제품을 추가하기 위해 디스패치되는 action입니다. 추가할 제품의 payload를 가지고 있습니다.
- addProductSuccess: 제품이 성공적으로 추가되면 디스패치되는 action입니다. 업데이트된 제품들의 payload를 가지고 있습니다.
- addProductFailure: 제품을 추가하는 중에 오류가 발생하면 디스패치되는 action입니다. 오류의 payload를 가지고 있습니다.
- updateProduct: 기존 제품을 업데이트하기 위해 디스패치되는 action입니다. 업데이트할 제품의 payload를 가지고 있습니다.
- updateProductSuccess: 제품이 성공적으로 업데이트되면 디스패치되는 action입니다. 업데이트된 제품들의 payload를 가지고 있습니다.
- updateProductFailure: 제품을 업데이트하는 중에 오류가 발생하면 디스패치되는 action입니다. 오류의 payload를 가지고 있습니다.
- deleteProduct: 제품을 삭제하기 위해 디스패치되는 action입니다. 삭제할 제품의 ID를 payload로 가지고 있습니다.
- deleteProductSuccess: 제품이 성공적으로 삭제되면 디스패치되는 action입니다. 업데이트된 제품들의 payload를 가지고 있습니다.
- deleteProductFailure: 제품을 삭제하는 중에 오류가 발생하면 디스패치되는 action입니다. 오류의 payload를 가지고 있습니다.



<div class="content-ad"></div>

9.2) product.state.ts: 이 파일은 NgRx의 Entity Adapter를 사용하여 제품을 관리하는 데 필요한 상태 구조체를 정의합니다. Entity Adapter를 사용하면 엔터티의 컬렉션을 간단하게 처리할 수 있습니다.

```js
import { EntityState, createEntityAdapter } from "@ngrx/entity";
import { Product } from "src/app/models/Product";

export interface ProductState extends EntityState<Product> {
  loading: boolean;
  error: string | null; 
}

export const productAdapter = createEntityAdapter<Product>();

export const initialState: ProductState = productAdapter.getInitialState({
  //products: [],
  //selectedProductId: null,
  loading: false,
  error: null
});
```

- EntityState: 엔터티의 컬렉션을 관리하기 위해 NgRx 저장소의 상태를 확장하는 인터페이스입니다.
- createEntityAdapter: 엔터티 컬렉션을 관리하는 유틸리티 메서드를 제공하는 엔터티 어댑터를 생성하는 함수입니다.
- Product: 제품 모델을 나타내는 인터페이스입니다.
- ProductState: 제품 관리에 특화된 추가 상태 속성을 포함하기 위해 EntityState를 확장합니다.
- loading: 현재 제품을 로드하는 중인지를 나타내는 부울 플래그입니다.
- error: 제품 작업 중 발생하는 모든 오류를 나타내는 문자열 또는 null입니다.
- productAdapter: Product 모델을 위해 생성된 엔터티 어댑터입니다. 이 어댑터는 제품 컬렉션을 관리하기 위한 추가, 업데이트, 제거 엔터티와 같은 유틸리티 메서드를 제공합니다.
- getInitialState: 엔터티 어댑터가 엔터티 컬렉션의 초기 상태를 가져오기 위해 제공하는 메서드입니다. 이 메서드는 상태에 대한 추가 속성을 정의하는 데 객체를 사용할 수 있습니다.
- 초기 상태는 엔터티 어댑터의 getInitialState 메서드를 사용하여 구성됩니다. 이 메서드는 상태를 다음과 같은 속성으로 초기화합니다:
   - loading: 현재 제품을 로드하는 중이 아님을 나타내는 false로 설정됩니다.
   - error: 초기에는 오류가 없음을 나타내기 위해 null로 설정됩니다.

9.3) product.selectors.ts: 이 파일은 NgRx 저장소 내에서 제품 상태의 다른 슬라이스에 액세스하기 위한 다양한 셀렉터를 정의합니다.

<div class="content-ad"></div>

```js
import { createFeatureSelector, createSelector } from '@ngrx/store';
import { ProductState } from './product.state';

import { selectAll } from './product.reducer';

// 기능 상태를 선택하는 함수
export const selectProductState = createFeatureSelector<ProductState>('products');

// 제품 엔티티를 위한 셀렉터
export const selectAllProducts = createSelector(
  selectProductState,
  selectAll
);

export const selectProductEntities = createSelector(
  selectProductState,
  selectAll,
  (state, products) => products
);

export const selectProductIds = createSelector(
  selectProductState,
  selectAll,
  (state, products) => products
);

export const selectProductById = (productId: number) => createSelector(
  selectProductEntities,
  entities => entities[productId]
);

// 로딩 및 오류 상태를 위한 셀렉터
export const selectLoading = createSelector(
  selectProductState,
  state => state.loading
);

export const selectError = createSelector(
  selectProductState,
  state => state.error
);
```

- createFeatureSelector: 저장소에서 기능 상태를 선택하는 함수입니다.
- createSelector: 상태 조각에 대한 선택기를 작성하는 함수입니다.
- ProductState: 제품 상태 구조를 나타내는 인터페이스입니다.
- selectAll: 모든 제품을 선택하는 엔티티 어댑터에서 제공하는 선택기입니다.
- selectProductState: 이 선택기는 키 products를 사용하여 저장소에서 전체 제품 상태 슬라이스를 가져옵니다.
- selectAllProducts: 이 선택기는 selectProductState 선택기와 selectAll 선택기를 결합하여 제품 상태에서 모든 제품 엔티티를 가져옵니다.
- selectProductEntities: 이 선택기는 상태에서 모든 제품 엔티티를 가져옵니다. selectAllProducts와 겹치지만 특정 사용 사례에 사용될 수 있습니다.
- selectProductIds: selectProductEntities와 유사하게 모든 제품 엔티티를 가져오지만 필요한 경우 제품 ID를 추출하기위해 사용됩니다.
- selectProductById: 이 선택기는 제품 ID를 기반으로 단일 제품 엔티티를 가져옵니다. productId를 인수로 사용하고 엔티티 컬렉션에서 제품을 찾아 반환하는 선택기를 반환합니다.
- selectLoading: 이 선택기는 상품 상태에서 로딩 상태를 가져와 현재 제품이 로드되고 있는지를 나타냅니다.
- selectError: 이 선택기는 상품 상태에서 오류 상태를 가져와 제품 작업 중에 발생한 오류 메시지를 제공합니다.

9.4) product.reducer.ts: 이 파일은 NgRx 저장소에서 제품 관련 작업을 처리하는 리듀서를 정의합니다:

```js
import { createReducer, on } from '@ngrx/store';
import { ProductState, initialState, productAdapter } from './product.state';
import * as ProductActions from './product.actions';

export const productReducer = createReducer(
  initialState,

  on(ProductActions.loadProducts, state => ({
    ...state,
    loading: true,
    error: null
  })),
  on(ProductActions.loadProductsSuccess, (state, { products }) =>
    productAdapter.setAll(products, { ...state, loading: false })
  ),
  on(ProductActions.loadProductsFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error: error.message || 'Load products failed'
  })),

  on(ProductActions.addProductSuccess, (state, { products }) =>
    productAdapter.setAll(products, { ...state })
  ),
  on(ProductActions.addProductFailure, (state, { error }) => ({
    ...state,
    error: error.message || 'Add product failed'
  })),

  on(ProductActions.updateProductSuccess, (state, { products }) =>
    productAdapter.setAll(products, { ...state })
  ),
  on(ProductActions.updateProductFailure, (state, { error }) => ({
    ...state,
    error: error.message || 'Update product failed'
  })),

  on(ProductActions.deleteProductSuccess, (state, { products }) =>
    productAdapter.setAll(products, { ...state })
  ),
  on(ProductActions.deleteProductFailure, (state, { error }) => ({
    ...state,
    error: error.message || 'Delete product failed'
  }))
);

// 어댑터에서 엔티티 셀렉터 내보내기
export const {
  selectAll,
  selectEntities,
  selectIds,
} = productAdapter.getSelectors();

export const selectLoading = (state: ProductState) => state.loading;
export const selectError = (state: ProductState) => state.error;
```

<div class="content-ad"></div>

- createReducer: 리듀서 함수를 간결하게 정의하는 함수입니다.
- on: 리듀서 내에서 특정 액션을 처리하는 함수입니다.
- ProductState: 제품 상태의 구조를 나타내는 인터페이스입니다.
- initialState: 제품 슬라이스의 초기 상태입니다.
- productAdapter: 제품에 대한 엔티티 어댑터로, 제품 컬렉션을 관리하는 유틸리티 메서드를 제공합니다.
- ProductActions: 제품과 관련된 액션의 집합입니다.
- productReducer: 제품 상태에 대한 리듀서 함수로, ProductActions에서 정의된 여러 액션을 처리하고 상태를 업데이트합니다.
- loadProducts: loadProducts 액션이 디스패치되면 상태가 업데이트되어 로딩을 true로 설정하고 오류를 null로 설정합니다.
- loadProductsSuccess: loadProductsSuccess 액션이 디스패치되면, 엔티티 어댑터의 setAll 메서드를 사용하여 제품을 설정하고 로딩을 false로 설정합니다.
- loadProductsFailure: loadProductsFailure 액션이 디스패치되면, 상태가 업데이트되어 로딩을 false로 설정하고 제공된 오류 메시지를 오류로 설정합니다.
- addProductSuccess: addProductSuccess 액션이 디스패치되면, 새 제품 목록을 설정하도록 상태가 업데이트됩니다.
- addProductFailure: addProductFailure 액션이 디스패치되면, 상태가 업데이트되어 제공된 오류 메시지를 오류로 설정합니다.
- updateProductSuccess: updateProductSuccess 액션이 디스패치되면, 새 제품 목록을 설정하도록 상태가 업데이트됩니다.
- updateProductFailure: updateProductFailure 액션이 디스패치되면, 상태가 업데이트되어 제공된 오류 메시지를 오류로 설정합니다.
- deleteProductSuccess: deleteProductSuccess 액션이 디스패치되면, 새 제품 목록을 설정하도록 상태가 업데이트됩니다.
- deleteProductFailure: deleteProductFailure 액션이 디스패치되면, 상태가 업데이트되어 제공된 오류 메시지를 오류로 설정합니다.
- selectAll: 모든 제품을 얻기 위한 셀렉터입니다.
- selectEntities: 제품 엔티티를 사전 형태로 모두 얻기 위한 셀렉터입니다.
- selectIds: 모든 제품 ID를 얻기 위한 셀렉터입니다.
- selectLoading: 로딩 상태를 얻기 위한 셀렉터입니다.
- selectError: 오류 상태를 얻기 위한 셀렉터입니다.

9.5) product.effects.ts: 이 파일은 NgRx 스토어에서 제품 관련 액션에 대한 부수 효과를 처리하는 이펙트를 정의합니다:

```js
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { catchError, map, mergeMap } from 'rxjs/operators';
import { of } from 'rxjs';
import * as ProductActions from './product.actions';
import { ProductService } from '../../services/product.service';

@Injectable() 
export class ProductEffects {
  constructor(private actions$: Actions, private productService: ProductService) {}

  loadProducts$ = createEffect(() => this.actions$.pipe(
    ofType(ProductActions.loadProducts),
    mergeMap(() => this.productService.getAllProducts().pipe(
      map(products => ProductActions.loadProductsSuccess({ products })),
      catchError(error => of(ProductActions.loadProductsFailure({ error })))
    ))
  ));

  addProduct$ = createEffect(() => this.actions$.pipe(
    ofType(ProductActions.addProduct),
    mergeMap(action => this.productService.addProduct(action.product).pipe(
      map(products => ProductActions.addProductSuccess({ products })),
      catchError(error => of(ProductActions.addProductFailure({ error })))
    ))
  ));

  updateProduct$ = createEffect(() => this.actions$.pipe(
    ofType(ProductActions.updateProduct),
    mergeMap(action => this.productService.updateProduct(action.product).pipe(
      map(products => ProductActions.updateProductSuccess({ products })),
      catchError(error => of(ProductActions.updateProductFailure({ error })))
    ))
  ));

  deleteProduct$ = createEffect(() => this.actions$.pipe(
    ofType(ProductActions.deleteProduct),
    mergeMap(action => this.productService.deleteProduct(action.productId).pipe(
      map(products => ProductActions.deleteProductSuccess({ products })),
      catchError(error => of(ProductActions.deleteProductFailure({ error })))
    ))
  ));
}
```

- Injectable: 클래스를 제공 및 의존성 주입 가능하게 하는 데코레이터입니다.
- Actions: 디스패치된 액션의 스트림입니다.
- createEffect: 이펙트를 생성하는 함수입니다.
- ofType: 액션 스트림에서 특정 액션 타입을 선택하기 위한 필터입니다.
- catchError, map, mergeMap: 부수 효과 및 변환을 처리하는 RxJS 연산자입니다.
- of: 정적 값에서 Observable을 만드는 함수입니다.
- ProductActions: 제품과 관련된 액션의 집합입니다.
- ProductService: 제품 관련 작업을 처리하는 서비스입니다.
- ProductEffects: 제품 관련 액션에 대한 이펙트를 정의하는 클래스입니다.
- actions$: NgRx에 의해 주입된 디스패치된 액션의 스트림입니다.
- productService: 의존성 주입으로 주입된 제품 관련 작업을 처리하는 서비스입니다.
- loadProducts$: loadProducts 액션을 처리하는 이펙트입니다.
- ofType(ProductActions.loadProducts): loadProducts 액션을 필터링합니다.
- mergeMap: 액션을 observable에 매핑하는데 사용됩니다. 이 경우 productService.getAllProducts()에서 반환되는 observable에 매핑합니다.
- map(products =` ProductActions.loadProductsSuccess(' products ')): 응답을 loadProductsSuccess 액션으로 매핑합니다.
- catchError(error =` of(ProductActions.loadProductsFailure(' error '))): 모든 오류를 catch하고 loadProductsFailure 액션으로 매핑합니다.
- addProduct$: addProduct 액션을 처리하는 이펙트입니다.
- ofType(ProductActions.addProduct): addProduct 액션을 필터링합니다.
- mergeMap(action =` this.productService.addProduct(action.product).pipe(…): 액션을 productService.addProduct에서 반환되는 observable에 매핑합니다.
- map(products =` ProductActions.addProductSuccess(' products ')): 응답을 addProductSuccess 액션으로 매핑합니다.
- catchError(error =` of(ProductActions.addProductFailure(' error '))): 모든 오류를 catch하고 addProductFailure 액션으로 매핑합니다.
- updateProduct$: updateProduct 액션을 처리하는 이펙트입니다.
- ofType(ProductActions.updateProduct): updateProduct 액션을 필터링합니다.
- mergeMap(action =` this.productService.updateProduct(action.product).pipe(…): 액션을 productService.updateProduct에서 반환되는 observable에 매핑합니다.
- map(products =` ProductActions.updateProductSuccess(' products ')): 응답을 updateProductSuccess 액션으로 매핑합니다.
- catchError(error =` of(ProductActions.updateProductFailure(' error '))): 모든 오류를 catch하고 updateProductFailure 액션으로 매핑합니다.
- deleteProduct$: deleteProduct 액션을 처리하는 이펙트입니다.
- ofType(ProductActions.deleteProduct): deleteProduct 액션을 필터링합니다.
- mergeMap(action =` this.productService.deleteProduct(action.productId).pipe(…): 액션을 productService.deleteProduct에서 반환되는 observable에 매핑합니다.
- map(products =` ProductActions.deleteProductSuccess(' products ')): 응답을 deleteProductSuccess 액션으로 매핑합니다.
- catchError(error =` of(ProductActions.deleteProductFailure(' error '))): 모든 오류를 catch하고 deleteProductFailure 액션으로 매핑합니다.

<div class="content-ad"></div>

9.6) ProductService: 제품 목록을 관리하기 위해 설계되었습니다. 비동기 데이터 스트림을 처리하는 데 RxJS 라이브러리를 사용합니다:

```js
import { Injectable } from '@angular/core';
import { Observable, of, throwError } from 'rxjs';
import { Product } from '../models/Product';

@Injectable({
  providedIn: 'root'
})
export class ProductService {

  private products: Product[] = [
    { id: 1, name: 'p 1', price: 95 },
    { id: 2, name: 'p 2', price: 745 },
    { id: 3, name: 'p 3', price: 58 },
    { id: 4, name: 'p 4', price: 9 },
  ];

  constructor() { }

  getAllProducts(): Observable<Product[]> {
    return of([...this.products]); // Products 배열의 사본 반환
  }

  addProduct(product: Product): Observable<Product[]> {
    this.products = [...this.products, product]; // 새 제품을 새 배열에 추가
    return of([...this.products]);
  }

  updateProduct(product: Product): Observable<Product[]> {
    const index = this.products.findIndex(p => p.id === product.id);
    if (index !== -1) {
      this.products = [
        ...this.products.slice(0, index),
        product,
        ...this.products.slice(index + 1)
      ];
      return of([...this.products]);
    } else {
      return throwError('제품을 찾을 수 없습니다');
    }
  }

  deleteProduct(productId: number): Observable<Product[]> {
    this.products = this.products.filter(product => product.id !== productId);
    return of([...this.products]);
  }

}
```

- Observable, of, throwError, catchError: 비동기 작업과 오류를 처리하기 위한 RxJS 유틸리티입니다.

- getAllProducts()

<div class="content-ad"></div>

- 설명: 전체 제품 목록의 observable을 반환합니다.
- 구현: [...this.products]를 사용하여 제품 배열의 사본을 발행하여 변경 불가능성을 보장합니다.

2. addProduct(product: Product)

- 설명: 새 제품을 제품 목록에 추가합니다.
- 구현: 기존 제품과 새 제품이 포함된 새 배열을 생성하여 원래 배열이 변조되지 않도록합니다. 그런 다음 업데이트된 제품 목록을 발행합니다.

3. updateProduct(product: Product)

<div class="content-ad"></div>

- 설명: 제품 목록에서 기존 제품을 업데이트합니다.
- 구현:

1. 업데이트할 제품의 인덱스를 찾습니다.
   
2. 제품을 찾으면, 기존 제품을 대체하는 업데이트된 제품이 있는 새 배열을 생성하여 불변성을 보장합니다.
   
3. 업데이트된 제품 목록을 반환합니다.

<div class="content-ad"></div>

만약 제품을 찾을 수 없다면 `throwError`를 사용하여 observable 에러를 반환합니다.

4. deleteProduct(productId: number)

- 설명: 제품 ID를 통해 제품 목록에서 제품을 삭제합니다.
- 구현:

제품 배열에서 지정된 ID를 가진 제품을 제외한 상품들을 필터링합니다.

<div class="content-ad"></div>

`제품 목록을 업데이트하고 원본 배열이 변경되지 않도록 합니다.

추가 기능과 코드 전체를 상세히 보려면 GitHub 저장소를 방문해주세요. 여기에는 필요한 코드 조각과 설명이 제공되어 있지만 저장소에서 전체 구현을 찾을 수 있습니다. 피드백과 기여를 환영합니다!

```js
https://github.com/bouguern/angular-state-management-ngrx.git
```

## 결론

<div class="content-ad"></div>

Angular에서 NgRx를 사용하여 상태 관리를 구현하는 개요에서 우리는 액션, 상태, 리듀서, 셀렉터 및 이펙트의 생성과 사용을 다루었습니다. NgRx는 응용 프로그램에서 상태를 효과적으로 관리하는 강력한 방법을 제공하여 데이터 흐름을 일방향으로 유지하고 응용 프로그램 상태의 예측 가능성을 향상시킵니다. 액션을 사용하여 상태 변경을 트리거하고, 리듀서를 사용하여 해당 변경을 처리하며, 셀렉터를 사용하여 상태를 쿼리하고, 이펙트를 사용하여 부작용을 관리함으로써 견고하고 확장 가능한 응용 프로그램을 구축할 수 있습니다. 이 접근 방식은 깨끗한 아키텍처를 유지하며, 응용 프로그램을 시간이 지남에 따라 디버그, 테스트 및 확장하기 쉽게 만들어줍니다.

감사합니다!