---
title: "Angular로 오프라인 우선 애플리케이션 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-OfflineFirstApplicationswithAngular_0.png"
date: 2024-07-07 03:01
ogImage: 
  url: /assets/img/2024-07-07-OfflineFirstApplicationswithAngular_0.png
tag: Tech
originalTitle: "Offline First Applications with Angular"
link: "https://medium.com/@md.mollaie/offline-first-applications-with-angular-845bedaef51c"
---


<img src="/assets/img/2024-07-07-OfflineFirstApplicationswithAngular_0.png" />

# 오프라인 모드의 필요성

오늘날, 네트워크 상태에 관계없이 응용 프로그램의 가용성을 보장하는 것이 중요합니다. 오프라인 지원은 특히 불안정한 인터넷 연결이나 자주 이동하는 사용자를 위해 사용자 경험을 크게 향상시킬 수 있습니다. 오프라인 가능한 응용 프로그램은 다음과 같은 이점을 제공합니다:

- 향상된 사용자 경험: 사용자는 네트워크 문제로 중단되지 않고 계속해서 응용 프로그램과 상호 작용할 수 있습니다.
- 데이터 신뢰성: 오프라인에서 생성 또는 수정된 데이터는 연결이 복구되면 서버와 동기화될 수 있습니다.
- 성능 향상: 로컬 저장소 및 처리는 지연 시간을 줄여 더 빠른 응답 시간으로 이어집니다.

<div class="content-ad"></div>

# 오프라인 솔루션 비교

웹 애플리케이션에서 오프라인 기능을 구현하는 여러 가지 방법이 있습니다. 여기서는 일부 인기 있는 솔루션을 비교해보겠습니다: 서비스 워커 및 프로그레시브 웹 앱(PWAs), 파이어베이스 SDK, RxDB, 그리고 Dexie.js와 사용자 지정 데이터 서비스 레이어를 활용한 사용자 정의 솔루션.

# 서비스 워커 및 PWAs

개요: 서비스 워커는 웹 애플리케이션과 네트워크 사이에서 프록시 역할을 하며, 네트워크 요청을 가로채고 캐시된 응답을 제공하여 오프라인 기능을 활성화합니다. PWAs는 서비스 워커를 활용하여 네이티브 앱과 유사한 경험을 제공합니다.

<div class="content-ad"></div>

장점:

- 네이티브 앱과 유사한 경험
- 오프라인 지원 및 푸시 알림
- 캐싱을 통한 성능 향상

단점:

- 캐시 및 서비스 워커 라이프사이클 관리의 복잡성
- IndexedDB와 비교하여 저장 용량 한계

<div class="content-ad"></div>

# Firebase SDK

개요: Firebase는 오프라인 지원을 통해 Firestore 데이터베이스를 통해 웹 및 모바일 애플리케이션을 구축하기 위한 포괄적인 도구 세트를 제공합니다.

장점:

- Firebase의 생태계와 매끄럽게 통합
- 실시간 데이터 동기화
- 오프라인 지속성을 위한 간단한 API

<div class="content-ad"></div>

단점:

- 벤더 락인
- 맞춤형 솔루션에 비해 제한된 맞춤화 가능성

# RxDB

개요: RxDB(반응형 데이터베이스)는 자바스크립트 애플리케이션을 위한 실시간 NoSQL 데이터베이스로, 오프라인 우선 기능을 제공합니다.

<div class="content-ad"></div>

장점:

- RxJS를 사용한 반응형 데이터 스트림
- 실시간 데이터 동기화
- 스키마 기반 데이터 유효성 검사

단점:

- 반응형 프로그래밍 패러다임으로 인한 학습 곡선
- Firebase와 비교하여 커뮤니티 지원이 제한적

<div class="content-ad"></div>

# Dexie.js 및 사용자 정의 데이터 서비스 계층을 활용한 맞춤형 솔루션

개요: Dexie.js는 IndexedDB를 간단히 다룰 수 있는 래퍼입니다. Dexie.js와 사용자 정의 데이터 서비스 계층을 결합하면 맞춤형 오프라인 솔루션을 구현할 수 있습니다.

장점:

- 데이터 처리 및 동기화 로직에 대한 완전한 제어권
- 기존 아키텍처와 통합할 수 있는 유연성
- 공급업체에 대한 의존성 제거

<div class="content-ad"></div>

단점:

- 개발 노력이 더 필요합니다.
- 오프라인 동기화를 관리하는 데 높은 복잡성이 필요합니다.

# Angular를 활용한 사용자 정의 오프라인 우선 애플리케이션 구축

## Angular 애플리케이션 설정하기

<div class="content-ad"></div>

먼저, 최신 버전을 사용하여 독립 구성으로 새 Angular 애플리케이션을 만들어 보겠습니다.

```js
ng new todo-app --standalone
cd todo-app
ng add @angular/pwa
```

angular.json 파일을 구성하여 PWA 기능을 위한 서비스 워커를 활성화합니다.

# ToDo 페이지 만들기

<div class="content-ad"></div>

두 페이지를 만들어보겠습니다: 할 일 목록(List)과 상세(Detail) 페이지입니다. 할 일(Task) 모델을 정의하는데, 제목(title), 설명(description), 마지막 동작(last action), 그리고 사용자 ID(user ID) 필드가 있습니다.

```js
// src/app/models/task.model.ts
export interface Task {
  id: string;
  title: string;
  description: string;
  lastAction: 'in-progress' | 'done';
  userId: string;
}
```

# 핵심 API 서비스

데이터 작업 및 동기화를 처리하는 Core API 서비스를 정의했습니다.

<div class="content-ad"></div>

```js
import { HttpClient, HttpParams } from '@angular/common/http';
import { inject, Injectable } from '@angular/core';
import { map, Observable, switchMap, catchError, from, of, throwError } from 'rxjs';
import { environment } from 'src/environments/environment';
import { v4 as uuidv4 } from 'uuid';
import { DataService, QueryOptions } from 'src/app/services';

@Injectable({
  providedIn: 'root',
})
export abstract class CoreApiService<T> {
  http = inject(HttpClient);
  dataService = inject(DataService);

  url = environment.backendUrl;

  abstract dbPath: string;

  create(data: T & { [x: string]: any }): Observable<T> {
    const id = uuidv4();
    (data as any)['createdAt'] = new Date().toISOString();
    (data as any)['id'] = id;
    const endpoint = `${this.url}/${this.dbPath}`;

    if (navigator.onLine) {
      return this.http.post<T>(endpoint, data).pipe(
        switchMap((response) =>
          this.dataService.postData<T>(this.dbPath, endpoint, response).pipe(map(() => response)),
        ),
        catchError((error) =>
          this.dataService.saveTransaction(this.dbPath, id, data, 'POST', endpoint).pipe(
            map(() => {
              throw error;
            }),
          ),
        ),
      );
    } else {
      return this.dataService.saveTransaction(this.dbPath, id, data, 'POST', endpoint).pipe(
        switchMap((data: T | null) => {
          if (data) {
            return new Observable<T>((subscriber) => {
              subscriber.next(data);
            });
          } else {
            return new Observable<T>((subscriber) => {
              subscriber.error('Error');
            });
          }
        }),
      );
    }
  }

  update(id: string, data: any): Observable<T> {
    (data as any)['editedAt'] = new Date().toISOString();
    const endpoint = `${this.url}/${this.dbPath}/${id}`;

    if (navigator.onLine) {
      return this.http.put<T>(endpoint, data).pipe(
        switchMap((response) =>
          this.dataService.putData<T>(this.dbPath, endpoint, id, response).pipe(map(() => response)),
        ),
        catchError((error) =>
          this.dataService.saveTransaction(this.dbPath, id, data, 'PUT', endpoint).pipe(
            map(() => {
              throw error;
            }),
          ),
        ),
      );
    } else {
      return this.dataService.saveTransaction(this.dbPath, id, data, 'PUT', endpoint);
    }
  }

  // Other CRUD operations as in the original code...
}

```

```js
export interface WhereCondition {
  field: string;
  operator: '==' | '<' | '<=' | '>' | '>=' | 'array-contains' | 'in' | 'array-contains-any' | 'not-in';
  value: string | string[] | number | number[] | boolean;
}

export interface QueryOptions {
  limit?: number;
  orderBy?: string;
  startAt?: string;
  // Other fields as in the original code...
}
```

# 데이터 서비스

Dexie.js를 사용하여 로컬 스토리지를 관리하는 데이터 서비스를 생성하세요.

<div class="content-ad"></div>

```js
npm install dexie
```

```js
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, from, of, lastValueFrom } from 'rxjs';
import { switchMap, map, catchError } from 'rxjs/operators';
import Dexie from 'dexie';
import * as CryptoJS from 'crypto-js';
import { v4 as uuidv4 } from 'uuid';

@Injectable({
  providedIn: 'root',
})
export class DataService<T> {
  private secretKey = 'CHART-PAPER-SCISSORS-ROCK';
  private db!: Dexie;
  private INSTALLATION_KEY = 'installation_id';

  constructor(private http: HttpClient) {
    this.initializeDatabase();
    window.addEventListener('online', () => this.syncTransactions());
  }

  private initializeDatabase() {
    this.db = new Dexie('MyDatabase');
    this.db.version(1).stores({
      todos: 'key',
      app_meta: 'key',
      transactions: '++id, collection, key, data, timestamp, method, endpoint, payload',
    });

    this.checkInstallationId();
  }

  private async checkInstallationId() {
    const installationId = await this.db.table('app_meta').get(this.INSTALLATION_KEY);

    if (!installationId) {
      await this.setInstallationId();
    } else {
      // This means the app has been reinstalled or data was wiped
      await this.wipeAllData();
      await this.setInstallationId();
    }
  }

  private async setInstallationId() {
    const installationId = uuidv4();
    await this.db.table('app_meta').put({ key: this.INSTALLATION_KEY, value: installationId });
  }

  getData<T>(collection: string, key: string, endpoint: string, params?: any): Observable<T | null> {
    return from(this.getLocalData<T>(collection, key)).pipe(
      switchMap((localData) => {
        if (navigator.onLine) {
          return this.fetchFromApiAndStore<T>(collection, key, endpoint, params).pipe(
            catchError(() => of(localData)), // In case of error, return local data
          );
        } else {
          return of(localData);
        }
      }),
    );
  }

  fetchFromApiAndStore<T>(collection: string, key: string, endpoint: string, params?: any): Observable<T> {
    return this.http.get<T>(endpoint, { params }).pipe(
      switchMap((data) => this.setLocalData(collection, key, data).pipe(map(() => data))),
      switchMap((data) => this.saveTransaction(collection, key, data, 'GET', endpoint, params).pipe(map(() => data))), // Save snapshot of the transaction
    );
  }

  postData<T>(collection: string, endpoint: string, data: any): Observable<T> {
    if (navigator.onLine) {
      return this.http
        .post<T>(endpoint, data)
        .pipe(
          switchMap((response) =>
            this.saveTransaction(collection, null, response, 'POST', endpoint, data).pipe(map(() => response)),
          ),
        );
    } else {
      return this.saveTransaction(collection, null, data, 'POST', endpoint);
    }
  }

  putData<T>(collection: string, endpoint: string, key: string, data: any): Observable<T> {
    if (navigator.onLine) {
      return this.http
        .put<T>(endpoint, data)
        .pipe(
          switchMap((response) =>
            this.saveTransaction(collection, key, response, 'PUT', endpoint, data).pipe(map(() => response)),
          ),
        );
    } else {
      return this.saveTransaction(collection, key, data, 'PUT', endpoint);
    }
  }

  deleteData<T>(collection: string, endpoint: string, key: string): Observable<T | null> {
    if (navigator.onLine) {
      return this.http
        .delete<T>(endpoint)
        .pipe(
          switchMap((response) =>
            this.saveTransaction(collection, key, response, 'DELETE', endpoint).pipe(map(() => response)),
          ),
        );
    } else {
      return this.saveTransaction<T>(collection, key, null, 'DELETE', endpoint);
    }
  }

  setLocalData<T>(collection: string, key: string, data: T): Observable<T> {
    const encryptedData = CryptoJS.AES.encrypt(JSON.stringify(data), this.secretKey).toString();
    return from(this.db.table(collection).put({ key, value: encryptedData })).pipe(map(() => data));
  }

  getLocalData<T>(collection: string, key: string): Promise<T | null> {
    return this.db
      .table(collection)
      .get({ key })
      .then((record) => {
        if (record && record.value) {
          const bytes = CryptoJS.AES.decrypt(record.value, this.secretKey);
          const decryptedData = JSON.parse(bytes.toString(CryptoJS.enc.Utf8));
          return decryptedData as T;
        }
        return null;
      })
      .catch((error) => {
        console.log('Error getting data from storage', error);
        return null;
      });
  }

  clearData(collection: string, key: string): Observable<void> {
    return from(this.db.table(collection).delete(key));
  }

  clearAllData(collection: string): Observable<void> {
    return from(this.db.table(collection).clear());
  }

  wipeAllData(): Promise<void[]> {
    const collections = [
      'todos',
      'transactions',
    ];
    return Promise.all(collections.map((collection) => this.db.table(collection).clear()));
  }

  saveTransaction<T>(
    collection: string,
    key: string | null,
    data: T | null,
    method: 'GET' | 'POST' | 'PUT' | 'DELETE',
    endpoint: string,
    payload?: any,
  ): Observable<T | null> {
    const transaction = {
      collection,
      key,
      data,
      method,
      endpoint,
      payload,
      timestamp: new Date().toISOString(),
    };
    return from(this.db.table('transactions').add(transaction)).pipe(map(() => data));
  }

  async syncTransactions(): Promise<void> {
    const transactions = await this.db.table('transactions').toArray();
    for (const transaction of transactions) {
      try {
        let response;
        if (transaction.method === 'POST') {
          response = await lastValueFrom(this.http.post(transaction.endpoint, transaction.payload));
        } else if (transaction.method === 'PUT') {
          response = await lastValueFrom(this.http.put(transaction.endpoint, transaction.payload));
        } else if (transaction.method === 'DELETE') {
          response = await lastValueFrom(this.http.delete(transaction.endpoint));
        }
        if (response) {
          await this.db.table('transactions').delete(transaction.id); // Remove transaction after successful sync
        }
      } catch (error) {
        console.log('Error syncing transaction', error);
        // If there's an error, keep the transaction for retry
      }
    }
  }

  async saveMultipleData(dataMap: { [key: string]: any[] }): Promise<void> {
    const savePromises: Promise<void>[] = [];

    for (const collection in dataMap) {
      const dataArray = dataMap[collection];
      for (const data of dataArray) {
        const id = data.id || uuidv4();
        savePromises.push(lastValueFrom(this.setLocalData(collection, id, data)).then(() => {}));
      }
    }

    await Promise.all(savePromises);
  }

  async getMultipleData(collectionKeys: string[]): Promise<{ [key: string]: any[] }> {
    const getPromises = collectionKeys.map((collection) =>
      this.db
        .table(collection)
        .toArray()
        .then((records) => ({
          collection,
          records: records.map((record) => {
            const bytes = CryptoJS.AES.decrypt(record.value, this.secretKey);
            return JSON.parse(bytes.toString(CryptoJS.enc.Utf8));
          }),
        })),
    );

    const results = await Promise.all(getPromises);
    const dataMap: { [key: string]: any[] } = {};
    for (const result of results) {
      dataMap[result.collection] = result.records;
    }
    return dataMap;
  }
}
```

# 할 일 서비스 및 모델

Core API 서비스를 확장하고 Task 모델을 정의하는 ToDo 서비스를 생성하십시오.


<div class="content-ad"></div>

```js
import { Injectable } from '@angular/core';
import { CoreApiServiceV2 } from './core-api.service';
import { Task } from '../models/task.model';

@Injectable({
  providedIn: 'root',
})
export class ToDoService extends CoreApiServiceV2<Task> {
  dbPath = 'tasks';
}
```

# UI 구현

ToDo 목록과 상세 페이지를 위한 간단한 UI를 만들어보세요.

```js
<!-- src/app/todo-list/todo-list.component.html -->
<div>
  <h1>ToDo 목록</h1>
  <ul>
    <li *ngFor="let task of tasks" (click)="viewTask(task.id)">
      { task.title } - { task.lastAction }
    </li>
  </ul>
  <button (click)="addTask()">할 일 추가</button>
</div>
```

<div class="content-ad"></div>

```js
// src/app/todo-list/todo-list.component.ts
import { Component, OnInit } from '@angular/core';
import { ToDoService } from '../services/todo.service';
import { Task } from '../models/task.model';

@Component({
  selector: 'app-todo-list',
  templateUrl: './todo-list.component.html',
})
export class TodoListComponent implements OnInit {
  tasks: Task[] = [];
  todoService = inject(ToDoService);

  ngOnInit() {
    this.loadTasks();
  }

  loadTasks() {
    this.todoService.getAll({}).subscribe((tasks) => (this.tasks = tasks));
  }

  viewTask(id: string) {
    // Navigate to detail page
  }

  addTask() {
    // Navigate to add task page
  }
}
```

```js
<!-- src/app/todo-detail/todo-detail.component.html -->
<div *ngIf="task">
  <h1>{ task.title }</h1>
  <p>{ task.description }</p>
  <p>Status: { task.lastAction }</p>
  <button (click)="editTask()">Edit Task</button>
</div>
```

```js
// src/app/todo-detail/todo-detail.component.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { ToDoService } from '../services/todo.service';
import { Task } from '../models/task.model';

@Component({
  selector: 'app-todo-detail',
  templateUrl: './todo-detail.component.html',
})
export class TodoDetailComponent implements OnInit {
  task: Task | null = null;
  taskId = input<string>();
  todoService = inject(ToDoService);

  ngOnInit() {
    if (this.taskId()) {
      this.loadTask(this.taskId());
    }
  }

  loadTask(id: string) {
    this.todoService.get(id).subscribe((task) => (this.task = task));
  }

  editTask() {
    // Navigate to edit task page
  }
}
```

# 결론

<div class="content-ad"></div>

Angular, Dexie.js 및 사용자 정의 데이터 서비스 레이어를 활용하여 네트워크 상태와 관계없이 원활한 사용자 경험을 제공하는 견고한 오프라인 우선 응용 프로그램을 만들 수 있습니다. 이 접근 방식은 데이터 처리 및 동기화에 대한 유연성과 제어를 제공하여 복잡한 응용 프로그램에 대한 실용적인 해결책으로 작용합니다.