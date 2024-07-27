---
title: "Angular 18과 NET 8 SignalR 사용법 실시간 웹 앱 개발 가이드"
description: ""
coverImage: "/assets/img/2024-07-10-Angular18NET8SignalRUsage_0.png"
date: 2024-07-10 00:38
ogImage: 
  url: /assets/img/2024-07-10-Angular18NET8SignalRUsage_0.png
tag: Tech
originalTitle: "Angular 18   .NET 8 SignalR Usage"
link: "https://medium.com/@ferhatblnk/angular-18-net-8-signalr-usage-6d0186906946"
---


<img src="/assets/img/2024-07-10-Angular18NET8SignalRUsage_0.png" />

안녕하세요! 이번 글에서는 Angular 18과 .NET 8을 사용하여 SignalR 연결을 설정하는 방법을 단계별로 살펴보겠습니다. 먼저, .NET 8 프로젝트를 만들고 프로젝트에 SignalR 라이브러리를 추가합니다. 그런 다음 Angular 18 프로젝트를 시작하고 Angular 프로젝트에 SignalR 클라이언트 라이브러리를 포함시킵니다. .NET 측에서는 Hub 클래스를 만들고 필요한 메서드를 정의합니다. Angular 측에서는 SignalR 연결을 초기화하는 서비스를 만듭니다. 예를 들어, .NET에서 "ChatHub" 클래스를 만들고 "SendMessage"라는 메서드를 정의합니다. Angular에서는 이러한 메서드를 호출하기 위해 "chat.service.ts" 파일에서 연결 설정을 구성합니다. Angular의 라이프사이클 메서드를 사용하여 연결을 설정하고 관리하는 것이 중요합니다. 이러한 예제를 통해 Angular 18 및 .NET 8 프로젝트에서 SignalR을 사용하여 실시간 데이터 통신을 쉽게 구현하는 방법을 배울 수 있습니다.

SignalR 연결을 설정하기 위해 먼저 .NET 8 프로젝트에 필요한 SignalR 패키지를 설치해야 합니다. 이를위해 NuGet을 통해 Microsoft.AspNetCore.SignalR 패키지를 추가합니다. 그런 다음 SignalR 허브 클래스를 만듭니다:

```js
public class ChatHub : Hub
{
    public async Task SendMessage(string user, string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }
```

<div class="content-ad"></div>

이 클래스는 SignalR 허브의 기본 기능을 포함하고 있어요. SendMessage 메서드는 받은 메시지를 모든 연결된 클라이언트에게 보냅니다. 다음으로, 이 허브를 사용하는 엔드포인트를 생성하기 위해 Program.cs 파일을 업데이트합니다:

```js
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

var builder = WebApplication.CreateBuilder(args);

// 컨테이너에 서비스 추가
builder.Services.AddControllersWithViews();
builder.Services.AddSignalR(); // SignalR 서비스 추가

var app = builder.Build();

// HTTP 요청 파이프라인 구성
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
else
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();

app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
app.MapHub<ChatHub>("/chathub"); // ChatHub 추가

app.Run();
```

.NET 쪽 구성을 완료한 후, Angular 18 프로젝트로 이동합니다. 첫 번째 단계는 Angular 프로젝트에 @microsoft/signalr 패키지를 추가하는 것이에요:

```js
npm install @microsoft/signalr
```

<div class="content-ad"></div>

다음으로, 우리는 SignalR 연결을 관리하는 서비스를 생성합니다. chat.service.ts 파일에서 연결을 설정하고 메시지를 보내는 함수들을 정의합니다:

```js
import { Injectable } from '@angular/core';
import * as signalR from '@microsoft/signalr';
```

```js
@Injectable({
  providedIn: 'root'
})
export class ChatService {
  private hubConnection: signalR.HubConnection;
  constructor() {
    this.hubConnection = new signalR.HubConnectionBuilder()
      .withUrl('/chathub')
      .build();
    this.hubConnection.on('ReceiveMessage', (user, message) => {
      console.log(`User: ${user}, Message: ${message}`);
    });
    this.hubConnection.start()
      .catch(err => console.error(err));
  }
  sendMessage(user: string, message: string): void {
    this.hubConnection.invoke('SendMessage', user, message)
      .catch(err => console.error(err));
  }
}
```

이 서비스는 SignalR 연결을 설정하고 들어오는 메시지를 수신합니다. 또한 sendMessage 메서드를 제공하여 메시지를 전송합니다. 이제 Angular 컴포넌트에서 이 서비스를 사용하여 사용자 인터페이스에서 메시지를 보내고 수신 메시지를 표시할 수 있습니다:

<div class="content-ad"></div>

```js
import { Component } from '@angular/core';
import { ChatService } from './chat.service';
```

```js
@Component({
  selector: 'app-chat',
  template: `
    <div>
      <input [(ngModel)]="user" placeholder="User" />
      <input [(ngModel)]="message" placeholder="Message" />
      <button (click)="sendMessage()">Send</button>
    </div>
    <div *ngFor="let msg of messages">
      {msg}
    </div>
  `,
})
export class ChatComponent {
  user = '';
  message = '';
  messages: string[] = [];
  constructor(private chatService: ChatService) {}
  sendMessage(): void {
    this.chatService.sendMessage(this.user, this.message);
    this.messages.push(`You: ${this.message}`);
    this.message = '';
  }
}
```

이 예제를 통해 Angular 구성 요소에서 메시지를 보내고 들어오는 메시지를 나열할 수 있습니다. 이러한 단계를 통해 SignalR을 사용하여 Angular 18 및 .NET 8 프로젝트에서 실시간 통신을 구현하는 것이 쉬워집니다.

# 장점

<div class="content-ad"></div>

- 실시간 통신: SignalR은 클라이언트와 서버 간의 저레이턴시가 낮은 실시간 통신을 제공합니다. 채팅 애플리케이션, 게임 및 실시간 데이터 피드와 같이 실시간 업데이트가 필요한 애플리케이션에 중요합니다.
- 쉬운 통합: .NET 및 Angular과 쉽게 통합될 수 있습니다. SignalR은 .NET 플랫폼에서 네이티브 솔루션을 제공하면서 Angular용 강력한 SignalR 클라이언트 라이브러리도 제공합니다.
- 지원되는 프로토콜: SignalR은 WebSockets, Server-Sent Events (SSE), Long Polling 등 다양한 전송 프로토콜을 지원합니다. 이는 애플리케이션이 실행되는 환경의 기능에 따라 가장 적합한 프로토콜이 자동으로 선택될 수 있도록 합니다.
- 확장성: SignalR은 Redis, Azure 서비스 버스, SQL Server와 같은 확장 옵션을 지원합니다. 이를 통해 애플리케이션이 증가하는 부하를 처리하기 쉽게 만듭니다.

# 단점

- 요청 수 증가: 실시간 통신은 서버로의 요청 수를 상당히 증가시킬 수 있습니다. 특히 대규모 사용자를 보유한 애플리케이션의 경우 서버 리소스가 빠르게 고갈될 수 있습니다. 높은 트래픽을 처리하기 위해서는 더 많은 리소스와 복잡한 인프라 솔루션이 필요할 수 있습니다.
- 서버 부하: 지속적인 연결은 서버에 추가 부하를 줍니다. WebSockets와 같은 프로토콜을 사용할 때 특히 더 많이 드러납니다. 이 추가 부하를 처리하기 위해 서버에 대한 추가 하드웨어 또는 최적화가 필요할 수 있습니다.
- 보안: 실시간 데이터 통신은 보안 위험을 증가시킬 수 있습니다. 사용자 데이터의 안전한 전송과 무단 접근을 방지하기 위해 추가 보안 조치를 취해야 합니다.
- 브라우저 호환성: 모든 브라우저가 SignalR의 모든 기능을 지원하는 것은 아닐 수 있습니다. 오래된 브라우저나 일부 모바일 브라우저는 WebSockets와 같은 현대적인 프로토콜을 지원하지 않을 수 있어 호환성 문제가 발생할 수 있습니다.
- 개발 및 유지보수 비용: 실시간 애플리케이션을 개발하고 유지하는 것은 표준 HTTP 기반 애플리케이션보다 복잡합니다. 이는 개발 및 유지보수 단계 모두에 더 많은 시간과 비용이 필요할 수 있습니다.

# 요약

<div class="content-ad"></div>

SignalR은 실시간 통신이 필요한 애플리케이션에 강력하고 유연한 솔루션을 제공합니다. 그러나 요청 수와 서버 부하 증가 등 단점을 고려하면 적절한 구성 및 확장 전략을 도입해야 합니다. 이렇게 하면 SignalR의 장점을 최대한 활용하면서 단점의 영향을 최소화할 수 있습니다.