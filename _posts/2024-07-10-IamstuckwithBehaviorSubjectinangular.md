---
title: "Angular에서 BehaviorSubject를 사용하는 방법 배우기"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:51
ogImage:
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "I am stuck with BehaviorSubject in angular"
link: "https://medium.com/@fixitblog/solved-i-am-stuck-with-behaviorsubject-in-angular-4a089342d2e3"
isUpdated: true
---

Angular v17에서 BehaviorSubject를 사용하여 중앙 오류 처리 서비스를 생성했어요. 하지만 예상한 대로 작동하지 않아요!

문제가 발생한 곳은 다음과 같아요: NotificationService → 중앙 오류 처리기, NotificationComponent → 재사용 가능한 사용자 친화적 오류 및 진행 메시지가 표시되는 팝업 모달. 직접 Appcomponent에 추가했어요. Surrender Pet Component → 옵션을 표시하기 위해 Notification을 사용하려고 하는 곳이에요.

제가 예상한 방식으로 데이터를 방출하지 않는 것 같아요.

NotificationService:

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
import { Injectable } from "@angular/core";
import { BehaviorSubject } from "rxjs";

@Injectable({
  providedIn: "root",
})
export class NotificationService {
  successMessageSubject = (new BehaviorSubject() < string) | (null > null);
  errorMessageSubject = (new BehaviorSubject() < string) | (null > null);

  successMessageAction$ = this.successMessageSubject.asObservable();
  errorMessageAction$ = this.errorMessageSubject.asObservable();

  setSuccessMessage(message: string) {
    this.successMessageSubject.next(message);
  }

  setErrorMessage(message: string) {
    this.errorMessageSubject.next(message);
    console.log(this.errorMessageSubject.getValue());
  }

  clearSuccessMessage() {
    this.successMessageSubject.next(null);
  }

  clearErrorMessage() {
    this.errorMessageSubject.next(null);
  }

  clearAllMessages() {
    this.clearSuccessMessage();
    this.clearErrorMessage();
  }
}
```

NotificationComponent:

```js
import { Component, OnInit } from '@angular/core';
import { NotificationService } from '../../../core/services/notifiaction/notification.service';
import { tap } from 'rxjs';

@Component({
    selector: 'app-notification',
    templateUrl: './notification.component.html',
    styleUrls: ['./notification.component.scss']
})
export class NotificationComponent implements OnInit {
    private notificationService: NotificationService;

    constructor(notificationService: NotificationService) {
        this.notificationService = notificationService;
    }

    successMessage$ = this.notificationService.successMessageAction$.pipe(
        tap((message) => {
            if (message) {
                console.log('clicked');
                setTimeout(() => {
                    this.notificationService.clearAllMessages();
                }, 5000);
            }
        })
    );

    errorMessage$ = this.notificationService.errorMessageAction$.pipe(
        tap((message) => {
            console.log(message);
            if (message) {
                console.log('clicked');
                setTimeout(() => {
                    this.notificationService.clearAllMessages();
                }, 5000);
            }
        })
    );

    ngOnInit(): void {
        console.log("initialized");
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
import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';
import { RouterLink } from '@angular/router';
import { ButtonComponent } from '../../../shared/components/button/button.component';
import { NgClass, NgIf, } from '@angular/common';
import { Component, inject } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';
import { PetsAdopteService } from '../../../core/services/pets-adopte/pets-adopte.service';
import { SurrenderPet } from '../../../core/models/surrenderPet.model';
import { NotificationService } from '../../../core/services/notifiaction/notification.service';

@Component({
  selector: 'app-surrender-pet',
  standalone: true,
  imports: [
    ReactiveFormsModule,
    NgClass,
    RouterLink,
    NgIf,
    ButtonComponent,
    HttpClientModule
  ],
  providers:[PetsAdopteService,NotificationService],
  templateUrl: './surrender-pet.component.html',
  styleUrl: './surrender-pet.component.scss'
})
export class SurrenderPetComponent {

  private petAdopteService=inject(PetsAdopteService);
  private notificationService=inject(NotificationService);

  submitted:boolean = false;

  registerPet= new FormGroup({
    name: new FormControl<string>('',[Validators.required]),
    phoneNo:new FormControl<string>('',[Validators.required]),
    petType:new FormControl<string> ('',[Validators.required]),
    location:new FormControl<string>('',[Validators.required]),
    otherDetails:new FormControl<string>('',[Validators.required])
  })

  onSubmit(){
      this.submitted = true;
      if(this.registerPet.valid){
       this.petAdopteService.sendPetSurrender_Request(this.registerPet.value as SurrenderPet).subscribe(
        {
          next:(data)=>{
         console.log(data);
        }

        }
       )
      }
  }
}
```

Notification Component html template:

```js
<div class="z-40">
   <div class="z-40" *ngIf="successMessage$ | async as successMessage" id="toast-success" class=" fixed fixed bottom-5 right-5 flex items-center w-full max-w-xs p-4 mb-4 text-gray-500 bg-white rounded-lg shadow dark:text-gray-400 dark:bg-gray-800" role="alert">
       <div class="inline-flex items-center justify-center flex-shrink-0 w-8 h-8 text-green-500 bg-green-100 rounded-lg dark:bg-green-800 dark:text-green-200">
           <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 20 20">
               <path d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5Zm3.707 8.207-4 4a1 1 0 0 1-1.414 0l-2-2a1 1 0 0 1 1.414-1.414L9 10.586l3.293-3.293a1 1 0 0 1 1.414 1.414Z"/>
           </svg>
           <span class="sr-only">Success icon</span>
       </div>
       <div class="ms-3 text-sm font-normal">{successMessage}</div>
       <button type="button" class="ms-auto -mx-1.5 -my-1.5 bg-white text-gray-400 hover:text-gray-900 rounded-lg focus:ring-2 focus:ring-gray-300 p-1.5 hover:bg-gray-100 inline-flex items-center justify-center h-8 w-8 dark:text-gray-500 dark:hover:text-white dark:bg-gray-800 dark:hover:bg-gray-700" data-dismiss-target="#toast-success" aria-label="Close">
           <span class="sr-only">Close</span>
           <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
               <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
           </svg>
       </button>
   </div>

   <div *ngIf="errorMessage$ | async as errorMessage" id="toast-failure" class=" fixed fixed bottom-5 right-5 flex items-center w-full max-w-xs p-4 mb-4 text-gray-500 bg-white rounded-lg shadow dark:text-gray-400 dark:bg-gray-800" role="alert">
       <div class="inline-flex items-center justify-center flex-shrink-0 w-8 h-8 text-green-500 bg-green-100 rounded-lg dark:bg-green-800 dark:text-green-200">
           <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 20 20">
               <path d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5Zm3.707 8.207-4 4a1 1 0 0 1-1.414 0l-2-2a1 1 0 0 1 1.414-1.414L9 10.586l3.293-3.293a1 1 0 0 1 1.414 1.414Z"/>
           </svg>
           <span class="sr-only">Success icon</span>
       </div>
       <div class="ms-3 text-sm font-normal">{errorMessage}</div>
       <button type="button" class="ms-auto -mx-1.5 -my-1.5 bg-white text-gray-400 hover:text-gray-900 rounded-lg focus:ring-2 focus:ring-gray-300 p-1.5 hover:bg-gray-100 inline-flex items-center justify-center h-8 w-8 dark:text-gray-500 dark:hover:text-white dark:bg-gray-800 dark:hover:bg-gray-700" data-dismiss-target="#toast-success" aria-label="Close">
           <span class="sr-only">Close</span>
           <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
               <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
           </svg>
       </button>
   </div>
</div>
```

The PetsAdopteService where I called the setMessages functions!

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
import { HttpClient } from '@angular/common/http';
import { Injectable, inject } from '@angular/core';
import { EMPTY, Observable, catchError, tap } from 'rxjs';
import { SurrenderPet } from '../../models/surrenderPet.model';
import { environment } from '../../../../environments/environment.development';
import { petsSurrenderEndpoints } from '../../constants/APIEndPoints/petsAdopte.EndPoints';
import { NotificationService } from '../notifiaction/notification.service';

@Injectable({
  providedIn: 'root'
})
export class PetsAdopteService {

constructor() { }

private http:HttpClient=inject(HttpClient);
private notificationService=inject(NotificationService);

sendPetSurrender_Request(payload:SurrenderPet):Observable<SurrenderPet>{
 return this.http.post<SurrenderPet>(environment.apiUrl+petsSurrenderEndpoints?.createSurrenderRequest,payload).pipe(
    tap((data)=>{
    this.notificationService.setSuccessMessage('Your request sent successfully')
  }),
  catchError((error)=>{
    console.log(error)
     this.notificationService.setErrorMessage("eROR");
     return EMPTY;
  })
 )
 }
}
```

해결 방법  
알겠어요. 마침내 해결책을 찾았어요. 이 이상한 동작은 standalone components의 providers 배열에 서비스를 추가했기 때문에 발생했던 겁니다. 이미 루트 레벨에서 제공되었었어요. 제가 그것들을 제거한 후에 로직이 정상 작동했어요.

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

위 문구를 친근하게 번역해 드리겠습니다.

답변 - 베를린 존스(M)  
답변 확인 - 세나이다 (수정자원 봉사자)

이 답변은 스택 오버플로우에서 가져온 것으로, cc by-sa 2.5, cc by-sa 3.0, cc by-sa 4.0의 라이센스를 따릅니다.
