---
title: "Angular 18과 ASPNET Core 80으로 프로젝트 개발하는 방법 3가지"
description: ""
coverImage: "/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_0.png"
date: 2024-07-07 13:36
ogImage: 
  url: /assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_0.png
tag: Tech
originalTitle: "ANGULAR 18 AND ASP.NET CORE 8.0 PROJECT DEVELOPMENT 3"
link: "https://medium.com/@ahmetbilgic81/angular-18-and-asp-net-core-8-0-project-development-3-d9aa15a4c3d5"
isUpdated: true
---





![2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_0](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_0.png)

이 글에서는 Angular 프로젝트 개발에 대해 배울 것입니다.

더 많은 세부 정보는 우리의 비디오 강좌에서 확인하실 수 있습니다:

[Angular 18 and ASP.NET 8.0 Project Development for Beginners](https://www.udemy.com/course/angular-18-and-aspnet-80-project-development-for-beginners/)

<div class="content-ad"></div>

아래는 Markdown 형식으로 변경되었습니다:
![2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_1.png](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_1.png)

또한, 프로젝트 리포지토리에 다음 GitHub 링크로 접속할 수 있습니다:

이전에 언급했듯이, 우리는 Visual Studio에서 만든 클라이언트 측 프로젝트 템플릿을 생성했습니다.

이제 이 프로젝트에서 몇 가지 개발을 진행할 예정입니다.

<div class="content-ad"></div>

먼저, 해결 탐색기에서 angularapplication.client 프로젝트를 엽니다.

![이미지](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_2.png)

src/app/ 위치에서 app.component.ts 파일을 엽니다.

이 파일에 다음 코드를 추가하세요:

<div class="content-ad"></div>

```js
import { HttpClient } from '@angular/common/http';
import { Component, OnInit } from '@angular/core';

interface Product {
  id: number
  name: string;
  price: number;
}

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent implements OnInit {
  public products: Product[] = [];
  public apiUrl: string = 'http://localhost:5252/api/Product';
  isShowSaveBtn = false;
  isShowUpdateBtn = false;

  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.getproducts();
  }
  displayStyle = "none";

  openPopup() {
    this.displayStyle = "block";
    this.isShowSaveBtn = true;
    this.isShowUpdateBtn = false;
    (document.getElementById("ProductModalLabel") as HTMLInputElement).innerHTML = "Save Product";
  }
  openPopupUpdate() {
    this.displayStyle = "block";
    this.isShowSaveBtn = false;
    this.isShowUpdateBtn = true;
    (document.getElementById("ProductModalLabel") as HTMLInputElement).innerHTML = "Update Product";
  }
  closePopup() {
    this.displayStyle = "none";
  }


  getproducts() {
    this.http.get<Product[]>(this.apiUrl).subscribe(
      (result) => {
        this.products = result;
      },
      (error) => {
        console.error(error);
      }
    );
  }
  getProduct(val: any) {
    this.isShowSaveBtn = false;
    this.isShowUpdateBtn = true;
    this.http.get<Product>(this.apiUrl + '/' + val).subscribe(
      (result) => {
        (document.getElementById("ProductId") as HTMLInputElement).value = result.id.toString();
        (document.getElementById("ProductName") as HTMLInputElement).value = result.name;
        (document.getElementById("Price") as HTMLInputElement).value = result.price.toString();
      },
      (error) => {
        console.error(error);
      }
    );
  }

  updateProduct() {
    let Product = {
      id: (document.getElementById("ProductId") as HTMLInputElement).value,
      name: (document.getElementById("ProductName") as HTMLInputElement).value,
      price: (document.getElementById("Price") as HTMLInputElement).value
    }
    return this.http.put(this.apiUrl, Product).subscribe(response => {
      if (response) {
        alert("Product is Updated");
      }
      else {
        alert("Product is not updated");
      }
    });
  }

  deleteProduct(val: any) {

    return this.http.delete(this.apiUrl + '/' + val).subscribe(response => {
      if (response) {
        alert("Product is deleted");

      }
      else {

        alert("Product is not deleted");
      }
    });
  }


  createProduct() {

    let Product = {
      name: (document.getElementById("ProductName") as HTMLInputElement).value,
      price: (document.getElementById("Price") as HTMLInputElement).value
    }

    this.http.post(this.apiUrl, Product)
      .subscribe(response => {
        if (response) {
          alert("Product is Saved");
        }
        else {
          alert("Product is not saved");
        }
      });
  }

  title = 'angularapplication.client';
}
```

여기서, 웹 API 프로젝트의 엔드포인트에 따라 apiUrl = ‘http://localhost:5252/api/Product`를 업데이트해야합니다. 엔드포인트를 확인하려면 스웨거 페이지를 사용하여 웹 API 프로젝트의 엔드포인트를 확인할 수 있습니다.위의 코드를 app.component.html 파일에 추가하십시오.

<div class="content-ad"></div>


이제 필요한 모든 코드를 Angular 프로젝트에 추가했습니다.

프로젝트를 실행하면 다음처럼 출력됩니다:

![이미지](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_4.png)


<div class="content-ad"></div>

우리는 "제품 추가" 버튼을 클릭하고 팝업 창이 열립니다. 제품 정보를 입력하고 이 화면에 저장합니다.

![이미지](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_5.png)

저장 버튼을 클릭한 후 페이지를 새로고침하면 제품이 다음과 같이 페이지에 나타납니다:

![이미지](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_6.png)

<div class="content-ad"></div>

업데이트 버튼을 클릭하면 화면에 업데이트 팝업 모달이 표시됩니다.

![업데이트 모달](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_7.png)

제품에 일부 변경을 가하면 제품 정보가 업데이트됩니다.

![제품 정보 업데이트](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_8.png)

<div class="content-ad"></div>

다음으로, 우리는 삭제 버튼을 클릭하면 제품이 삭제됩니다.

![이미지](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_9.png)

# 결론:

종합하면, Angular와 ASP.NET Core를 사용하여 프로젝트를 개발하면 현대 웹 애플리케이션을 구축하기 위한 견고하고 확장 가능하며 유지보수가 쉬운 솔루션을 제공합니다. 이 기술 스택은 프론트엔드 기술과 백엔드 기술의 강점을 활용하여 개발자에게 강력한 도구, 커뮤니티 지원 및 다양한 프로젝트 요구 사항을 충족하기 위한 유연성을 제공합니다. Angular의 풍부한 클라이언트 측 기능과 ASP.NET Core의 강력한 서버 측 기능을 효과적으로 활용함으로써, 개발자는 현대 웹 개발 모베스트 프랙티스를 준수하는 반응성, 안전성 및 성능이 우수한 응용 프로그램을 제공할 수 있습니다.

<div class="content-ad"></div>

감사합니다. 이 기사를 읽어주셔서 감사합니다.

Angular에 대한 더 자세한 내용을 알아보려면 아래 링크에서 Udemy 코스를 확인해보세요:

[Udemy 코스 바로가기](https://www.udemy.com/course/angular-18-and-aspnet-80-project-development-for-beginners/)

![Angular 개발 프로젝트 이미지](/assets/img/2024-07-07-ANGULAR18ANDASPNETCORE80PROJECTDEVELOPMENT3_10.png)