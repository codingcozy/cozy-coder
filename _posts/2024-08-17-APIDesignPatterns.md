---
title: "프론트엔드 개발자가 알아야 할 10가지 API 디자인 패턴(2024년)"
description: ""
coverImage: "/assets/img/2024-08-17-APIDesignPatterns_0.png"
date: 2024-08-17 01:02
ogImage: 
  url: /assets/img/2024-08-17-APIDesignPatterns_0.png
tag: Tech
originalTitle: "API Design Patterns"
link: "https://medium.com/@vikas.taank_40391/how-api-evolves-using-java-completable-future-a29966d3740b"
isUpdated: true
updatedAt: 1723863555208
---



![API evolution](/assets/img/2024-08-17-APIDesignPatterns_0.png)

API 발전.

## 문제 설명: 주어진 제품의 가격을 검색할 수 있는 API를 설계하십시오.

# 아래 API는 동기 작업으로 가격을 검색합니다.


<div class="content-ad"></div>


![API Design Patterns](/assets/img/2024-08-17-APIDesignPatterns_1.png)

```js
import java.util.random.RandomGenerator;

public class SynchronousAPI {
    public double getPrice(String product) {
        return calculatePrice(product);
    }

    private double calculatePrice(String product) {
        delay();
        return RandomGenerator.getDefault().nextDouble() * product.charAt(0) + product.charAt(1);
    }

    public static void delay() {
        try {
            Thread.sleep(1000L);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

# Asynchronous API:

이제 동일한 코드를 비동기 방식으로 가격 검색이 이루어지게 하려면 CompletableFuture를 사용하여 API 호출을 별도의 스레드에서 실행하고 supplyAsync 메서드를 사용할 수 있습니다.


<div class="content-ad"></div>


<img src="/assets/img/2024-08-17-APIDesignPatterns_2.png" />

```js
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.Future;
import java.util.random.RandomGenerator;

public class AsynchronousPriceAPI {

    public Future<Double> getPriceAsync(String product) {
        CompletableFuture<Double> futurePrice = new CompletableFuture<>();
        new Thread( () -> {
            double price = calculatePrice(product);
            futurePrice.complete(price);
        }).start();
        return futurePrice;
    }

    public Future<Double> getPriceAsyncSA(String product) {
        return CompletableFuture.supplyAsync(() -> calculatePrice(product));
    }
    public double getPrice(String product) {
        return calculatePrice(product);
    }
    private static double calculatePrice(String product) {
        delay();
        return RandomGenerator.getDefault().nextDouble() * product.charAt(0) + product.charAt(1);
    }
    public static void delay() {
        try {
            Thread.sleep(1000L);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

# 병렬 처리, 병렬성 활용하기.

이제 다른 상점에서 가격을 받아 오고 병렬로 작업을 진행하려면 어떻게 해야 할까요?


<div class="content-ad"></div>


<img src="/assets/img/2024-08-17-APIDesignPatterns_3.png" />

```js
import java.util.Arrays;
import java.util.List;
import java.util.random.RandomGenerator;

import static java.util.stream.Collectors.toList;

public class ParallelAPI {

    static List<Shop> shops;
    static {
        shops= Arrays.asList(new Shop("BestPrice"),
                new Shop("LetsSaveBig"),
                new Shop("MyFavoriteShop"),
                new Shop("BuyItAll"));
    }
    public List<String> getPrice(String product) {
        return shops.stream()
                .map(shop -> String.format("%s price is %.2f",
                        shop.getName(), calculatePrice(product)))
                .collect(toList());
    }

    private static double calculatePrice(String product) {
        delay();
        return RandomGenerator.getDefault().nextDouble() * product.charAt(0) + product.charAt(1);
    }
    public static void delay() {
        try {
            Thread.sleep(1000L);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

좋았다면 공유하거나 박수를 부탁드려요.
