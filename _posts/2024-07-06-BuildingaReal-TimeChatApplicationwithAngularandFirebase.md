---
title: "Angular와 Firebase로 실시간 채팅 애플리케이션 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-06-BuildingaReal-TimeChatApplicationwithAngularandFirebase_0.png"
date: 2024-07-06 03:09
ogImage: 
  url: /assets/img/2024-07-06-BuildingaReal-TimeChatApplicationwithAngularandFirebase_0.png
tag: Tech
originalTitle: "Building a Real-Time Chat Application with Angular and Firebase"
link: "https://medium.com/@md.mollaie/building-a-real-time-chat-application-with-angular-and-firebase-89f70ebdd5a1"
isUpdated: true
---




![](/assets/img/2024-07-06-BuildingaReal-TimeChatApplicationwithAngularandFirebase_0.png)

현대 디지털 시대에 실시간 통신은 다양한 애플리케이션에 필수적인 기능이 되었습니다. 소셜 미디어 플랫폼부터 협업 도구에 이르기까지 실시간 상호작용은 사용자 경험을 향상시키는데 있어 즉각적인 업데이트와 상호작용을 제공합니다. 이 기사에서는 실시간 연결의 필요성을 탐구하고 다양한 솔루션을 비교하며 Angular와 Firebase를 사용하여 실시간 채팅 애플리케이션을 구축하는 과정에 대한 단계별 가이드를 제공합니다.

# 실시간 연결이 필요한 이유

## 실시간 통신이 필요한 시나리오들

<div class="content-ad"></div>

- 소셜 미디어 및 메신저 앱: WhatsApp, Facebook Messenger, Slack과 같은 플랫폼은 사용자들이 즉각적으로 소통할 수 있도록 실시간 업데이트에 의존합니다.
- 협업 도구: Google Docs, Trello와 같은 애플리케이션은 실시간 협업 기능을 제공하여 여러 사용자가 원활하게 함께 작업할 수 있습니다.
- 생중계 스포츠 및 뉴스 업데이트: 최신 점수, 뉴스 및 알림을 사용자에게 전달하는 데 있어서 실시간 업데이트가 중요합니다.
- 고객 지원: 실시간 채팅 시스템은 사용자에게 즉각적인 지원과 도움을 제공하여 고객 서비스를 향상시킵니다.

# 실시간 통신 솔루션 비교

여러 기술이 실시간 통신을 가능케하며, 각각 장단점이 있습니다:

- WebSockets: 단일 TCP 연결을 통해 전이중 통신 채널을 제공합니다. 저 지연 시간과 높은 주파수 메시지 교환을 필요로 하는 시나리오에 이상적입니다.
- Server-Sent Events (SSE): 서버가 클라이언트로 업데이트를 푸시할 수 있게 합니다. 양방향 통신 없이 실시간 업데이트가 필요한 애플리케이션에 적합합니다.
- 폴링: 클라이언트가 정기적으로 서버에서 업데이트를 요청합니다. 구현하기 쉽지만 효율적이지 않고 더 높은 지연 시간을 유발할 수 있습니다.
- Firebase Realtime Database: 실시간으로 데이터를 업데이트하는 NoSQL 클라우드 데이터베이스입니다. 구현을 간소화하지만 복잡한 쿼리에는 유연하지 않을 수 있습니다.
- Firebase Firestore: 더 고급 질의 기능과 실시간 데이터 동기화를 제공합니다. 확장 가능하며 복잡한 애플리케이션에 적합합니다.

<div class="content-ad"></div>

# Angular과 Firebase 프로젝트 설정하기

## 단계 1: Angular 프로젝트 설정하기

우선 Angular CLI를 사용하여 새 Angular 프로젝트를 만들어보세요:

```js
ng new real-time-chat
cd real-time-chat
```

<div class="content-ad"></div>

필요한 Firebase 패키지를 설치해주세요:

```js
ng add @angular/fire
ng add ngxtension
```

## 단계 2: Firebase 구성하기

Firebase 콘솔에서 Firebase 프로젝트를 만들고 Firebase 구성 객체를 가져옵니다. 이 구성을 Angular 환경 파일에 추가하세요.

<div class="content-ad"></div>

```js
// src/environments/environment.ts
export const environment = {
  production: false,
  firebase: {
    apiKey: "your-api-key",
    authDomain: "your-auth-domain",
    projectId: "your-project-id",
    storageBucket: "your-storage-bucket",
    messagingSenderId: "your-messaging-sender-id",
    appId: "your-app-id",
  },
};
```

## Step 3: Setting Up Firebase in Angular

Modify your `app.config.ts` to initialize Firebase:

```js
import { ApplicationConfig, provideZoneChangeDetection } from "@angular/core";
import { provideRouter, withComponentInputBinding } from "@angular/router";

import { routes } from "./app.routes";
import { provideClientHydration } from "@angular/platform-browser";
import { initializeApp, provideFirebaseApp } from "@angular/fire/app";
import { getAuth, provideAuth } from "@angular/fire/auth";
import { getFirestore, provideFirestore } from "@angular/fire/firestore";
import { environment } from "../environments/environment.development";

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes, withComponentInputBinding()),
    provideClientHydration(),
    provideFirebaseApp(() => initializeApp(environment.firebase)),
    provideAuth(() => getAuth()),
    provideFirestore(() => getFirestore()),
  ],
};
```

<div class="content-ad"></div>

# **실시간 채팅 기능 구현하기**

## **단계 4: 인증**

사용자 등록, 로그인 및 로그아웃을 처리하는 인증 스토어를 생성하세요:

```js
// src/app/stores/auth.store.ts

import { createInjectable } from 'ngxtension/create-injectable';
import { inject, signal } from '@angular/core';
import {
  Auth,
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
  signOut,
  User,
} from '@angular/fire/auth';
import { Firestore, doc, setDoc, getDoc } from '@angular/fire/firestore';

export interface AppUser {
  uid: string;
  email: string;
  displayName?: string;
}

export const useAuthStore = createInjectable(() => {
  const auth = inject(Auth);
  const firestore = inject(Firestore);
  const currentUser = signal<AppUser | null>(null);

  auth.onAuthStateChanged(async (user) => {
    if (user) {
      const appUser = await getUserFromFirestore(user.uid);
      currentUser.set(appUser);
    } else {
      currentUser.set(null);
    }
  });

  async function getUserFromFirestore(uid: string): Promise<AppUser | null> {
    const userDoc = await getDoc(doc(firestore, `users/${uid}`));
    if (userDoc.exists()) {
      return userDoc.data() as AppUser;
    }
    return null;
  }

  async function createUserInFirestore(user: User): Promise<void> {
    const newUser: AppUser = {
      uid: user.uid,
      email: user.email!,
      displayName: user.displayName || undefined,
    };
    await setDoc(doc(firestore, `users/${user.uid}`), newUser);
  }

  async function signUp(email: string, password: string, displayName: string) {
    try {
      const userCredential = await createUserWithEmailAndPassword(
        auth,
        email,
        password
      );
      await createUserInFirestore({ ...userCredential.user, displayName });
      const appUser = await getUserFromFirestore(userCredential.user.uid);
      currentUser.set(appUser);
    } catch (error: any) {
      console.error('Signup error:', error);
      switch (error.code) {
        case 'auth/email-already-in-use':
          throw new Error(
            '이메일이 이미 사용 중입니다. 다른 이메일을 시도해주세요.'
          );
        case 'auth/invalid-email':
          throw new Error('유효하지 않은 이메일 주소입니다.');
        case 'auth/weak-password':
          throw new Error(
            '비밀번호가 너무 약합니다. 강력한 비밀번호를 사용해주세요.'
          );
        default:
          throw new Error(
            '가입 중에 오류가 발생했습니다. 다시 시도해주세요.'
          );
      }
    }
  }

  async function login(email: string, password: string) {
    try {
      const userCredential = await signInWithEmailAndPassword(
        auth,
        email,
        password
      );
      const appUser = await getUserFromFirestore(userCredential.user.uid);
      currentUser.set(appUser);
    } catch (error: any) {
      console.error('Login error:', error);
      switch (error.code) {
        case 'auth/user-not-found':
        case 'auth/wrong-password':
          throw new Error('잘못된 이메일 또는 비밀번호입니다. 다시 시도해주세요.');
        case 'auth/invalid-email':
          throw new Error('유효하지 않은 이메일 주소입니다.');
        case 'auth/user-disabled':
          throw new Error(
            '이 계정은 비활성화되었습니다. 지원팀에 문의하세요.'
          );
        default:
          throw new Error('로그인 중에 오류가 발생했습니다. 다시 시도해주세요.');
      }
    }
  }

  async function logout() {
    try {
      await signOut(auth);
      currentUser.set(null);
    } catch (error) {
      console.error('Logout error:', error);
      throw new Error('로그아웃 중에 오류가 발생했습니다. 다시 시도해주세요.');
    }
  }

  return {
    currentUser,
    signUp,
    login,
    logout,
  };
});
```

<div class="content-ad"></div>

## 단계 5: Firestore 채팅 스토어

채팅 관련 작업을 관리하기 위한 채팅 스토어를 생성하세요:

```js
// src/app/stores/chat.store.ts

import { createInjectable } from 'ngxtension/create-injectable';
import { inject, signal, computed, effect } from '@angular/core';
import {
  Firestore,
  collection,
  doc,
  onSnapshot,
  addDoc,
  updateDoc,
  Timestamp,
  query,
  where,
  getDocs,
  orderBy,
  getDoc,
} from '@angular/fire/firestore';
import { useAuthStore, AppUser } from './auth.store';

export interface Chat {
  id: string;
  participants: string[];
  participantNames?: string[];
  lastMessage: string;
  lastMessageTimestamp: Timestamp;
}

export interface Message {
  id: string;
  chatId: string;
  senderId: string;
  text: string;
  timestamp: Timestamp;
}

export const useChatStore = createInjectable(() => {
  const firestore = inject(Firestore);
  const authStore = inject(useAuthStore);
  const chats = signal<Chat[]>([]);
  const currentChatId = signal<string | null>(null);
  const messages = signal<Message[]>([]);

  const currentChat = computed(() =>
    chats().find((chat) => chat.id === currentChatId())
  );

  effect(() => {
    const userId = authStore.currentUser()?.uid;
    if (userId) {
      listenToChats(userId);
    }
  });

  async function getUserName(userId: string): Promise<string> {
    const userDoc = await getDoc(doc(firestore, `users/${userId}`));
    if (userDoc.exists()) {
      const userData = userDoc.data() as AppUser;
      return userData.displayName || userData.email || 'Unknown User';
    }
    return 'Unknown User';
  }

  async function fetchParticipantNames(
    participantIds: string[]
  ): Promise<string[]> {
    const names = await Promise.all(participantIds.map(getUserName));
    return names;
  }

  function listenToChats(userId: string) {
    const chatsRef = collection(firestore, 'chats');
    const q = query(chatsRef, where('participants', 'array-contains', userId));
    return onSnapshot(q, async (snapshot) => {
      const updatedChats = await Promise.all(
        snapshot.docs.map(async (doc) => {
          const chatData = { id: doc.id, ...doc.data() } as Chat;
          chatData.participantNames = await fetchParticipantNames(
            chatData.participants
          );
          return chatData;
        })
      );
      chats.set(updatedChats);
    });
  }

  function listenToMessages(chatId: string) {
    currentChatId.set(chatId);
    const messagesRef = collection(firestore, `chats/${chatId}/messages`);
    const q = query(messagesRef, orderBy('timestamp', 'asc'));
    return onSnapshot(q, (snapshot) => {
      const updatedMessages = snapshot.docs.map(
        (doc) => ({ id: doc.id, ...doc.data() } as Message)
      );
      messages.set(updatedMessages);
    });
  }

  async function sendMessage(chatId: string, senderId: string, text: string) {
    const messagesRef = collection(firestore, `chats/${chatId}/messages`);
    const newMessage = {
      chatId,
      senderId,
      text,
      timestamp: Timestamp.now(),
    };
    await addDoc(messagesRef, newMessage);

    // 대화 문서의 마지막 메시지 업데이트
    const chatRef = doc(firestore, `chats/${chatId}`);
    await updateDoc(chatRef, {
      lastMessage: text,
      lastMessageTimestamp: Timestamp.now(),
    });
  }

  async function createNewChat(participantEmail: string) {
    const currentUser = authStore.currentUser();
    if (!currentUser) throw new Error('대화를 생성하려면 로그인해야 합니다.');

    // 주어진 이메일의 사용자 찾기
    const usersRef = collection(firestore, 'users');
    const q = query(usersRef, where('email', '==', participantEmail));
    const querySnapshot = await getDocs(q);

    if (querySnapshot.empty) {
      throw new Error('사용자를 찾을 수 없습니다.');
    }

    const participantUser = querySnapshot.docs[0].data() as AppUser;

    // 이 사용자 사이에 이미 대화가 있는지 확인
    const existingChatQuery = query(
      collection(firestore, 'chats'),
      where('participants', 'array-contains', currentUser.uid),
      where('participants', 'array-contains', participantUser.uid)
    );
    const existingChatSnapshot = await getDocs(existingChatQuery);

    if (!existingChatSnapshot.empty) {
      // 대화가 이미 존재하면 ID 반환
      return existingChatSnapshot.docs[0].id;
    }

    // 새로운 대화 문서 생성
    const chatsRef = collection(firestore, 'chats');
    const newChat = await addDoc(chatsRef, {
      participants: [currentUser.uid, participantUser.uid],
      lastMessage: '',
      lastMessageTimestamp: Timestamp.now(),
    });

    return newChat.id;
  }

  return {
    chats,
    currentChat,
    messages,
    listenToChats,
    listenToMessages,
    sendMessage,
    createNewChat,
    getUserName,
  };
});
```

## 단계 6: UI 구성 요소 생성하기

<div class="content-ad"></div>

- 회원 가입 구성 요소:

```js
// src / app / components / signup / signup.component.ts

import {Component, inject, signal} from '@angular/core';
import {FormsModule} from '@angular/forms';
import {Router, RouterModule} from '@angular/router';
import {useAuthStore} from '../stores/auth.store';

@Component({
    selector: 'app-signup',
    standalone: true,
    imports: [FormsModule, RouterModule],
    template: `
    <div
      class="min-h-screen bg-gray-100 flex flex-col justify-center py-12 sm:px-6 lg:px-8"
    >
      <div class="sm:mx-auto sm:w-full sm:max-w-md">
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          새 계정 만들기
        </h2>
      </div>

      <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
        <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
          @if (errorMessage()) {
          <div
            class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative"
            role="alert"
          >
            <span class="block sm:inline">{ errorMessage() }</span>
          </div>
          }

          <form (ngSubmit)="onSubmit()" class="space-y-6">
            <div>
              <label
                for="email"
                class="block text-sm font-medium text-gray-700"
              >
                이메일 주소
              </label>
              <div class="mt-1">
                <input
                  id="email"
                  name="email"
                  type="email"
                  required
                  [(ngModel)]="email"
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                />
              </div>
            </div>
            <div>
              <label
                for="displayName"
                class="block text-sm font-medium text-gray-700"
              >
                닉네임
              </label>
              <div class="mt-1">
                <input
                  id="displayName"
                  name="displayName"
                  type="text"
                  required
                  [(ngModel)]="displayName"
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                />
              </div>
            </div>

            <div>
              <label
                for="password"
                class="block text-sm font-medium text-gray-700"
              >
                비밀번호
              </label>
              <div class="mt-1">
                <input
                  id="password"
                  name="password"
                  type="password"
                  required
                  [(ngModel)]="password"
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                />
              </div>
            </div>

            <div>
              <button
                type="submit"
                class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              >
                가입
              </button>
            </div>
          </form>

          <div class="mt-6">
            <div class="relative">
              <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-gray-300"></div>
              </div>
              <div class="relative flex justify-center text-sm">
                <span class="px-2 bg-white text-gray-500"> 또는 </span>
              </div>
            </div>

            <div class="mt-6">
              <a
                routerLink="/login"
                class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-indigo-600 bg-white hover:bg-gray-50"
              >
                기존 계정으로 로그인
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
})
export class SignupComponent {
  email = '';
  password = '';
  displayName = '';
  errorMessage = signal<string | null>(null);
  private authStore = inject(useAuthStore);
  private router = inject(Router);

  async onSubmit() {
    try {
      console.log(this.displayName);
      await this.authStore.signUp(this.email, this.password, this.displayName);
      this.router.navigate(['/chat']);
      // 성공적으로 등록 후 채팅 목록으로 이동
    } catch (error) {
      console.error('회원 가입 오류:', error);
      if (error instanceof Error) {
        this.errorMessage.set(error.message);
      } else {
        this.errorMessage.set('예기치 않은 오류가 발생했습니다. 다시 시도해주세요.');
      }
    }
  }
}
```

로그인 구성 요소:

```js
// src / app / components / login / login.component.ts

import {Component, inject, signal} from '@angular/core';
import {FormsModule} from '@angular/forms';
import {Router, RouterModule} from '@angular/router';
import {useAuthStore} from '../stores/auth.store';

@Component({
    selector: 'app-login',
    standalone: true,
    imports: [FormsModule, RouterModule],
    template: `
    <div
      class="min-h-screen bg-gray-100 flex flex-col justify-center py-12 sm:px-6 lg:px-8"
    >
      <div class="sm:mx-auto sm:w-full sm:max-w-md">
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          계정에 로그인하기
        </h2>
      </div>

      <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
        <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
          @if (errorMessage()) {
          <div
            class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative"
            role="alert"
          >
            <span class="block sm:inline">{ errorMessage() }</span>
          </div>
          }

          <form (ngSubmit)="onSubmit()" class="space-y-6">
            <div>
              <label
                for="email"
                class="block text-sm font-medium text-gray-700"
              >
                이메일 주소
              </label>
              <div class="mt-1">
                <input
                  id="email"
                  name="email"
                  type="email"
                  required
                  [(ngModel)]="email"
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                />
              </div>
            </div>

            <div>
              <label
                for="password"
                class="block text-sm font-medium text-gray-700"
              >
                비밀번호
              </label>
              <div class="mt-1">
                <input
                  id="password"
                  name="password"
                  type="password"
                  required
                  [(ngModel)]="password"
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                />
              </div>
            </div>

            <div>
              <button
                type="submit"
                class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              >
                로그인
              </button>
            </div>
          </form>

          <div class="mt-6">
            <div class="relative">
              <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-gray-300"></div>
              </div>
              <div class="relative flex justify-center text-sm">
                <span class="px-2 bg-white text-gray-500"> 또는 </span>
              </div>
            </div>

            <div class="mt-6">
              <a
                routerLink="/signup"
                class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-indigo-600 bg-white hover:bg-gray-50"
              >
                새 계정 만들기
              </a>
            </div>
          </div>
        </div>
      </div>


<div class="content-ad"></div>

채팅 목록 컴포넌트:

// src/app/components/chat-list/chat-list.component.ts

import { Component, inject, OnInit } from '@angular/core';
import { Router, RouterModule } from '@angular/router';
import { Chat, useChatStore } from '../stores/chat.store';
import { useAuthStore } from '../stores/auth.store';
import { DatePipe } from '@angular/common';

@Component({
  selector: 'app-chat-list',
  standalone: true,
  imports: [RouterModule, DatePipe],
  template: `
    <div class="min-h-screen bg-gray-100">
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <div class="flex justify-between items-center mb-6">
            <h1 class="text-3xl font-bold text-gray-900">채팅</h1>
            <a
              routerLink="/new-chat"
              class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              새 채팅
            </a>
          </div>

          <div class="bg-white shadow overflow-hidden sm:rounded-md">
            <ul role="list" class="divide-y divide-gray-200">
              @for (chat of chatStore.chats(); track chat.id) {
              <li>
                <a
                  [routerLink]="['/chat', chat.id]"
                  class="block hover:bg-gray-50"
                >
                  <div class="px-4 py-4 sm:px-6">
                    <div class="flex items-center justify-between">
                      <p class="text-sm font-medium text-indigo-600 truncate">
                        { getChatName(chat) }
                      </p>
                      <div class="ml-2 flex-shrink-0 flex">
                        <p
                          class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800"
                        >
                          {
                            chat.lastMessageTimestamp.toDate() | date : 'short'
                          }
                        </p>
                      </div>
                    </div>
                    <div class="mt-2 sm:flex sm:justify-between">
                      <div class="sm:flex">
                        <p class="flex items-center text-sm text-gray-500">
                          { chat.lastMessage }
                        </p>
                      </div>
                    </div>
                  </div>
                </a>
              </li>
              }
            </ul>
          </div>

          @if (chatStore.chats().length === 0) {
          <p class="mt-4 text-gray-500">
            사용 가능한 채팅이 없습니다. 새 채팅을 시작하세요!
          </p>
          }

          <button
            (click)="logout()"
            class="mt-6 inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            로그아웃
          </button>
        </div>
      </div>
    </div>
  `,
})
export class ChatListComponent implements OnInit {
  chatStore = inject(useChatStore);
  authStore = inject(useAuthStore);
  router = inject(Router);

  ngOnInit() {
    const userId = this.authStore.currentUser()?.uid;
    if (userId) {
      this.chatStore.listenToChats(userId);
    } else {
      this.router.navigate(['/login']);
    }
  }

  getChatName(chat: Chat): string {
    if (chat.participantNames) {
      return chat.participantNames
        .filter((name) => name !== this.authStore.currentUser()?.displayName)
        .join(', ');
    }
    return '로드 중...';
  }

  logout() {
    this.authStore.logout();
    this.router.navigate(['/login']);
  }
}

채팅 상세 컴포넌트:

// src/app/components/new-chat/new-chat.component.ts

import { Component, inject, signal } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { Router } from '@angular/router';
import { useChatStore } from '../stores/chat.store';

@Component({
  selector: 'app-new-chat',
  standalone: true,
  imports: [FormsModule],
  template: `
    <div
      class="min-h-screen bg-gray-100 flex flex-col justify-center py-12 sm:px-6 lg:px-8"
    >
      <div class="sm:mx-auto sm:w-full sm:max-w-md">
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          새로운 채팅 시작하기
        </h2>
      </div>

      <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
        <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
          @if (errorMessage()) {
          <div
            class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative"
            role="alert"
          >
            <span class="block sm:inline">{ errorMessage() }</span>
          </div>
          }

          <form (ngSubmit)="onSubmit()" class="space-y-6">
            <div>
              <label
                for="email"
                class="block text-sm font-medium text-gray-700"
              >
                참여자 이메일
              </label>
              <div class="mt-1">
                <input
                  id="email"
                  name="email"
                  type="email"
                  required
                  [(ngModel)]="participantEmail"
                  class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                />
              </div>
            </div>

            <div>
              <button
                type="submit"
                class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              >
                채팅 시작
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  `,
})
export class NewChatComponent {
  participantEmail = '';
  errorMessage = signal<string | null>(null);
  private chatStore = inject(useChatStore);
  private router = inject(Router);

  async onSubmit() {
    try {
      const chatId = await this.chatStore.createNewChat(this.participantEmail);
      this.router.navigate(['/chat', chatId]);
    } catch (error) {
      console.error('New chat error:', error);
      if (error instanceof Error) {
        this.errorMessage.set(error.message);
      } else {
        this.errorMessage.set(
          '예기치 못한 오류가 발생했습니다. 다시 시도해주세요.'
        );
      }
    }
  }
}

<div class="content-ad"></div>

# 결론

이 글은 Angular와 Firebase를 사용하여 실시간 채팅 애플리케이션을 구축하는 포괄적인 가이드를 제공합니다. Firebase의 실시간 기능과 Angular의 강력한 프레임워크를 이용하여 개발자들은 사용자들을 위한 원활한 실시간 통신 경험을 만들어낼 수 있습니다. 제공된 코드 예제와 단계별 지침은 개발 프로세스를 원활히 하고 이러한 기술의 실용적인 구현을 보여주기 위해 노력하고 있습니다.

소셜 미디어 플랫폼, 협업 도구 또는 실시간 업데이트를 필요로 하는 어떤 애플리케이션을 구축하고 있든, 이 안내서는 프로젝트에 실시간 채팅 기능을 통합하는 데 유용한 자원으로 기능합니다. 이 프로젝트의 완전한 소스 코드를 GitHub에서 확인할 수 있습니다: Real-Time Chat Application.
```
