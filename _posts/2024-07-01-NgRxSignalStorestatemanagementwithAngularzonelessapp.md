---
title: "Zoneless Angular 앱에서 NgRx SignalStore 상태 관리 방법"
description: ""
coverImage: "/assets/img/2024-07-01-NgRxSignalStorestatemanagementwithAngularzonelessapp_0.png"
date: 2024-07-01 20:39
ogImage: 
  url: /assets/img/2024-07-01-NgRxSignalStorestatemanagementwithAngularzonelessapp_0.png
tag: Tech
originalTitle: "NgRx SignalStore state management with Angular zoneless app."
link: "https://medium.com/@serhiizhydetskyi/ngrx-signalstore-state-management-with-angular-zoneless-app-01ddc7e78571"
---


![2024-07-01-NgRxSignalStorestatemanagementwithAngularzonelessapp_0.png](/assets/img/2024-07-01-NgRxSignalStorestatemanagementwithAngularzonelessapp_0.png)

여러분은 이미 새로운 Angular 도구인 Signals에 대해 알고 있을 것입니다. 내 의견으로는, 이는 매우 강력하고 편리한 도구로, 컨트롤러의 코드 양을 줄이고 개발 과정을 더 빠르게 만들어줍니다. 동시에, NgRx가 제안하는 상태 관리를 위한 또 다른 도구에 대해 이야기해보고 싶습니다. 이 방식을 사용하면 뷰 컨트롤러에 코드가 적어지며 페이지 효율성이 높아지고, 컨트롤러에서 부작용을 처리할 필요가 없으며 zoneless입니다. 또한 상태 관리가 만병통치약이 아니라는 것을 기억해야 하며, 필요한 프로젝트에서만 사용해야 합니다. 그렇지 않으면 페이지/프로젝트에 복잡성만 더할 수 있습니다.

# 사례 설명

서버에서 가져온 책 목록이 있는 상황을 상상해보겠습니다. 이들을 상태로 유지하고, 제거하거나 책을 추가할 수 있는 페이지에 표시하는 작업을 수행해야 합니다(NgRx 문서에서 가져온 케이스입니다).

<div class="content-ad"></div>

우리가 할 수 있는 일은 무엇일까요? 우리는 모든 복잡성을 갖춘 NgRx 스토어를 사용할 수 있고, ngneat/elf store와 같은 더 간단한 솔루션을 사용할 수도 있어요. 이러한 기술을 사용하면 여전히 데이터 스트림(옵저버블)과 zone.js와 함께 작업할 것이지만, Angular의 마지막 18번째 버전은 zoneless change detection 접근 방식을 제안하고 있어요. 네, 현재는 실험적인 부분이라고 할 수 있지만, Ivy의 경우를 상기해보세요. Angular 8에서는 선택 사항이었지만, Angular 9에서는 표준이 되었고 View Engine을 사용하려면 다시 거슬러 올라가야 했어요. 그래서, 지금부터는 이 공개된(NgRx 팀이 발표한) 새로운 도구를 사용해 보려고 해요 (이 글을 작성하는 시점에는 RC 상태에요). 이제부터는 더 많은 코드와 간결한 설명으로 진행할게요 :)

이 글에서 설명된 방법은 NgRx SignalStore 문서에서 영감을 받았어요.

# 가짜 DataService 구현

서버에서 책 목록을 반환, 추가, 삭제하는 DataService가 있어요. 우리의 경우에는 서버 요청 수명을 흉내내기 위해 하드 코딩된 책 목록과 지연된 응답이 있는 서비스가 있어요.

<div class="content-ad"></div>

```js
import {Injectable} from "@angular/core";
import {delay, map, Observable, of} from "rxjs";
import {IBook} from "../interfaces/book.interface";

export interface IBook {
    id: number,
    name: string,
    pageCount: number
}

@Injectable({providedIn: "root"})
export class DataService {

    private books: IBook[] = [
        {
            id: 0,
            name: 'Book 0',
            pageCount: 0
        },
        {
            id: 1,
            name: 'Book 1',
            pageCount: 0
        },
        {
            id: 2,
            name: 'Book 2',
            pageCount: 0
        },
        {
            id: 3,
            name: 'Book 22',
            pageCount: 0
        },
        {
            id: 4,
            name: 'Book 222',
            pageCount: 0
        }
    ];

    getAllBooks(): Observable<IBook[]> {
        return of<IBook[]>(this.books).pipe(
            delay(2000)
        );
    }

    addBook(book: IBook): Observable<IBook> {
        this.books.push(book);
        return of(book).pipe(
            delay(2000)
        );
    }

    removeBook(id: number): Observable<number> {
        const index = this.books.findIndex(x => x.id === id);
        if(index >= 0) {
            this.books.splice(index, 1);
        }

        return of(id).pipe(
            delay(2000)
        )
    }
}
```

# BooksStore implementation

Let’s start creating simple SignalStore.

```js
export const BooksStore = signalStore(
    withEntities<IBook>()
)
```

<div class="content-ad"></div>

지금은 상태를 유지하기에 충분합니다. 이것을 어떤 컴포넌트에 주입하여 사용할 수 있습니다.

```js
@Component({
    selector: 'app-root',
    standalone: true,
    template: `
      <div>
          @for (book of booksStore.entities(); track book.id) {
              <div>
                  <div>{ book.name }</div>
              </div>
          }
      </div>
  `,
    providers: [BooksStore]
})
export class App {
    booksStore = inject(BooksStore);
}
```

작업은 매우 간단해 보이지만, 미들웨어로 만들면 훨씬 강력해질 것입니다. 이는 BooksStore가 데이터(우리의 경우 책 목록)를 서버와 동기화하는 책임을 져야 한다는 것을 의미합니다. DataService는 어디에서나 사용하면 안 되며 오직 BooksStore에서만 사용되어야 합니다. 이를 위해 해야 할 일을 설명해 보겠습니다.

- BooksStore에 모든 책을 가져오는 함수 추가
- BooksStore에 책을 추가하는 함수 추가
- BooksStore에서 책을 제거하는 함수 추가

<div class="content-ad"></div>

이 예시에서는 스토어에 있는 모든 책을 가져오는 것을 제안합니다. 이는 BooksStore가 Angular 컴포넌트에 주입될 때 서버에서 데이터를 가져오기 시작한다는 것을 의미합니다. (엄격하게 판단하지 마세요, SignalStore 훅을 보여주기 위해 이렇게 작성되었지만, 단순히 훅에서 메서드로 이동할 수 있습니다. 책을 추가하거나 제거하는 기능처럼).

onInit 훅에서 모든 책을 가져오는 BooksStore

```js
export const BooksStore = signalStore(
    withEntities<IBook>(),
    withHooks({
        onInit(store, dataService = inject(DataService)) {
            dataService.getAllBooks().pipe(
                takeUntilDestroyed(),
                tap(books => patchState(store, setEntities(books))), 
            ).subscribe();
        },
        onDestroy(store) {
            console.log('Book store destroy', store);
        },
    }),
)
```

이제, 책을 추가하고 제거하는 기능을 추가해봅시다.

<div class="content-ad"></div>

```js
export const BooksStore = signalStore(
    withEntities<IBook>(),
    withHooks({
        onInit(store, dataService = inject(DataService)) {
            dataService.getAllBooks().pipe(
                takeUntilDestroyed(), 
                // 서버에서 새 책 목록으로 상점을 패치하는 부분
                tap(books => patchState(store, setEntities(books))),
            ).subscribe();
        },
        onDestroy(store) {
            console.log('BooksStore destroy', store);
        },
    }),
    withMethods((store, dataService = inject(DataService)) => ({
        
        // 책 추가 함수
        add: rxMethod<IBook>(
            pipe(
                switchMap(book => {
                    return dataService.addBook(book).pipe(
                        // 응답 처리를 단순화하기위한 특수 NgRx 연산자
                        tapResponse({
                            next: (book) => {
                                // 상태 패치
                                // 책이 추가 또는 업데이트 될 것임 
                                patchState(store, addEntity(book))
                            },
                            error: console.error,
                            finalize: () => console.log,
                        })
                    )
                })
            )
        ),
        
        // 책 삭제 함수
        remove: rxMethod<IBook>(
            pipe(
                switchMap(book => {
                    return dataService.removeBook(book.id).pipe(
                        // 응답 처리를 단순화하기위한 특수 NgRx 연산자
                        tapResponse({
                            next: (book) => {
                                // 상태 패치
                                // 책이 제거될 것임
                                patchState(store, removeEntity(book))
                            },
                            error: console.error,
                            finalize: () => console.log,
                        })
                    )
                })
            )
        ),
    }))
)
```

SignalStore와 거의 끝났어요. 하지만, 데이터를 가져오는 동안 로딩 표시를 해야 한다면 어떻게 할까요? 상점 안에 객체가 있는 경우 (리스트가 아닌 경우) isLoading 속성을 추가하여 객체의 모든 속성이 시그널 (DeepSignal)이 되므로, 이 isLoading 속성에 신호 속성으로 액세스할 수 있습니다. 그러나 엔터티 목록이 있는 경우, SignalStore는 엔터티의 상점에서 추가 신호 속성을 추가하는 기능을 제공하지 않습니다. 이를 통해 로딩과 같은 추가 상태 또는 엔터티 목록이있는 상점의 정보에 액세스 할 수 있습니다.

객체가 있는 상점을 살펴보겠습니다. 여기서는 isLoading을 신호 속성으로 추가하고 계산된 신호 속성으로 count를 추가할 수 있습니다.

```js
type BooksState = {
    books: IBook[];
// 로딩을 나타내기 위한 객체의 추가 속성.
    isLoading: boolean;
};

const initialState: BooksState = {
    books: [],
    isLoading: false,
};

export const BooksStore = signalStore(
    // withEntities 대신 withState를 사용하여 생성된 상점
    withState(initialState),

    // 주입된 BookStore의 계산된 신호 속성 추가
    withComputed(({ booksStore }) => ({
        // booksCount는 BookStore의 주입된 인스턴스에서 계산된 신호 속성으로 사용 가능
        booksCount: computed(() => booksStore().entities().length),
    })),

    withMethods((store, dataService = inject(DataService)) => ({
        load: rxMethod<void>(
            pipe(
                distinctUntilChanged(),
                //
                tap(() => patchState(store, { isLoading: true })),
                switchMap(() => {
                    return dataService.getAllBooks().pipe(
                        tapResponse({
                            next: (books) => {
                                patchState(store, { books })
                            },
                            error: console.error,
                            finalize: () => patchState(store, { isLoading: false }),
                        })
                    );
                })
            )
        )
    }))
);
```

<div class="content-ad"></div>

빙고를 외쳐 보세요! 우리는 SignalStore를 entities가 아닌 books라는 속성을 가진 객체와 함께 사용할 수 있습니다. 이 속성은 책의 배열인데요. 하지만 안타깝게도, 만약 add(book: IBook) 또는 update(book: IBook) 메소드를 추가한다면, books 배열에서 단일 책 항목을 패치할 수 없게 됩니다. 그래서 우리는 새로운 필터링되거나 확장된 책 배열로 booksStore.books 시그널 속성을 덮어쓸 수밖에 없습니다.

다행히도, JavaScript 절을 다시 살펴본 뒤 이 문제를 해결할 수 있습니다. 다행히도 SignalStore 아키텍처를 사용하면 이 문제를 해결하고 BooksStore의 @Injectable() 동작을 유지할 수 있습니다.

따라서, 이 기사를 위한 BooksStore의 최종 버전은 다음과 같이 보일 것입니다:

<div class="content-ad"></div>

```js
import {patchState, signalStore, withHooks, withMethods} from "@ngrx/signals";
import {addEntity, removeEntity, setEntities, withEntities} from "@ngrx/signals/entities";
import {IBook} from "../interfaces/book.interface";
import {inject, signal} from "@angular/core";
import {DataService} from "../services/data.service";
import {rxMethod} from "@ngrx/signals/rxjs-interop";
import {pipe, switchMap, tap} from "rxjs";
import {tapResponse} from "@ngrx/operators";
import {takeUntilDestroyed} from "@angular/core/rxjs-interop";

// 새로운 인스턴스가 생성될 때마다 isLoading 플래그를 가진 signalStore 컨텍스트에 isLoading을 추가하는 클래스 정의
export const BooksStore = function() {
    let isLoading = signal(false);

    return signalStore(
        withEntities<IBook>(),
        withHooks({
            onInit(store, dataService = inject(DataService)) {
                isLoading.set(true);
                dataService.getAllBooks().pipe(
                    takeUntilDestroyed(),
                    tap(books => patchState(store, setEntities(books))),
                    tap(() => isLoading.set(false))
                ).subscribe();
            },
            onDestroy(store) {
                console.log('count on destroy', store);
            },
        }),
        withMethods((store, dataService = inject(DataService)) => ({
            isLoading,
            add: rxMethod<IBook>(
                pipe(
                    tap(() => isLoading.set(true)),
                    switchMap(book => {
                        return dataService.addBook(book).pipe(
                            tapResponse({
                                next: (book) => {
                                    patchState(store, addEntity(book));
                                },
                                error: console.error,
                                finalize: () => isLoading.set(false),
                            })
                        );
                    })
                )
            ),
            remove: rxMethod<IBook>(
                pipe(
                    tap(() => isLoading.set(true)),
                    switchMap(book => {
                        return dataService.removeBook(book.id).pipe(
                            tapResponse({
                                next: (book) => {
                                    patchState(store, removeEntity(book));
                                },
                                error: console.error,
                                finalize: () => isLoading.set(false),
                            })
                        );
                    })
                )
            ),
        }))
    );
}();
```

BooksStore 클래스 작성이 완료되었습니다. 이제 사용 방법을 알아보고 controller 내 코드 양이 줄어드는지 확인해봅시다.

# Controller 구현

```js
@Component({
    selector: 'app-root',
    standalone: true,
    template: `
      @if (bookStore.isLoading()) {
          <div class="backdrop">
              <div class="spinner"></div>
          </div>
      }
      <div>
          @for (book of booksStore.entities(); track book.id) {
              <div>
                  <div>{ book.name }</div>
                  <button (click)="booksStore.remove(book)">remove book</button>
              </div>
          }
          <button (click)="addBook()">Add book</button>
      </div>
  `,
    providers: [BooksStore]
})
export class App {
    booksStore = inject(BooksStore);

    addBook() {
        this.booksStore.add({
            id: RandomHelper.getRandomInt(10000, 99999),
            name: RandomHelper.getRandomString(5),
            pageCount: 0
        });
    }
}
```

<div class="content-ad"></div>

보시다시피 상점 사용법은 꽤 간단합니다. 프로젝트 전반에 걸쳐 데이터 저장소를 사용해야 할 경우, 그것은 꽤 좋고 안정적이며 빠르게 사용할 수 있는 방법입니다. 페이지의 변경 사항은 Angular의 존리스(Zoneless) 접근 방식을 통해 자동으로 적용됩니다.

```js
bootstrapApplication(AppComponent, {
    providers: [
        provideExperimentalZonelessChangeDetection()
    ]
}).then();
```

# 그리고 한 가지 더 :)

일부 Observable을 구독할 때 하는 것과 같이 변경 사항에 코드를 실행해야 할 필요가 있다면, Angular Signal 효과로 쉽게 할 수 있습니다.

<div class="content-ad"></div>

```js
@Component({
    selector: 'app-root',
    standalone: true,
    template: `
      <div>
          @for (book of booksStore.entities(); track book.id) {
              <div>
                  <div>{ book.name }</div>
                  <button (click)="booksStore.remove(book)">remove book</button>
              </div>
          }
          <button (click)="addBook()">Add book</button>
      </div>
  `,
    providers: [BooksStore]
})
export class App {
    booksStore = inject(BooksStore);

    constructor() {
        effect(() => {
            const isLoading = this.booksStore.isLoading();

            if (isLoading) {
                console.log('Books loading')
            } else {
                console.log('Books loaded', untracked(this.booksStore.entities()))
            }
        });
    }

    addBook() {
        this.booksStore.add({
            id: RandomHelper.getRandomInt(10000, 99999),
            name: RandomHelper.getRandomString(5),
            pageCount: 0
        })
    }
}
```

effect()에 전달된 화살표 함수는 this.booksStore.isLoading()의 변경 사항마다 실행됩니다. untracked()로 꾸며진 this.booksStore.entities() 변경은 무시됩니다.

시간 내어주셔서 감사합니다. 이 글이 누군가에게 유용하길 바랍니다. 이것이 제 첫 번째 글입니다 :)
의견이나 질문이 있다면 댓글이나 직접 메시지로 자유롭게 남겨주세요.

다음 글이 제가 작성한 다양한 형식과 데이터 서비스로 프로젝트 간에 사용할 수 있는 추상화된 스토어 로직에 관한 글이 필요하시다면 알려주세요.

<div class="content-ad"></div>

# 예시 ( git & stackblitz )

- git: [https://github.com/SerhiiZhydetskyi/stackblitz-rxjs-signals-approach](https://github.com/SerhiiZhydetskyi/stackblitz-rxjs-signals-approach)

- stackblitz: [https://stackblitz.com/~/github.com/SerhiiZhydetskyi/stackblitz-rxjs-signals-approach?file=tsconfig.json](https://stackblitz.com/~/github.com/SerhiiZhydetskyi/stackblitz-rxjs-signals-approach?file=tsconfig.json)

![GIF](https://miro.medium.com/v2/resize:fit:1200/1*7FTwCxciVW1J0WIMOenfpA.gif)

<div class="content-ad"></div>

# 유용한 링크:

- ngneat/elf store: [여기를 클릭하세요](https://ngneat.github.io/elf/docs/store)
- Angular zoneless: [여기를 클릭하세요](https://angular.dev/guide/experimental/zoneless)
- NgRx SignalStore 문서: [여기를 클릭하세요](https://ngrx.io/guide/signals/signal-store)

<div class="content-ad"></div>

DeepSignal: [https://ngrx.io/api/signals/DeepSignal](https://ngrx.io/api/signals/DeepSignal)

Angular Signal effect: [https://angular.dev/guide/signals#effects](https://angular.dev/guide/signals#effects)

Author's LinkedIn: [https://www.linkedin.com/in/serhii-zhydetskyi-80a7789b/](https://www.linkedin.com/in/serhii-zhydetskyi-80a7789b/)