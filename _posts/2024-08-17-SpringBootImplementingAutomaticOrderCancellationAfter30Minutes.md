---
title: "Spring Boot 30분 후 자동 주문 취소 기능 구현 방법"
description: ""
coverImage: "/assets/img/2024-08-17-SpringBootImplementingAutomaticOrderCancellationAfter30Minutes_0.png"
date: 2024-08-17 00:22
ogImage:
  url: /assets/img/2024-08-17-SpringBootImplementingAutomaticOrderCancellationAfter30Minutes_0.png
tag: Tech
originalTitle: "Spring Boot Implementing Automatic Order Cancellation After 30 Minutes"
link: "https://medium.com/stackademic/spring-boot-implementing-automatic-order-cancellation-after-30-minutes-e020292ea6e2"
isUpdated: true
updatedAt: 1723863800995
---

<img src="/assets/img/2024-08-17-SpringBootImplementingAutomaticOrderCancellationAfter30Minutes_0.png" />

결제 관련 비즈니스 시나리오에서 주문이 생성된 후 일정 시간 내에 사용자가 결제를 완료하지 않으면 시스템이 주문을 자동으로 취소하는 기능을 구현해야 하는 경우가 많습니다.

이 글에서는 Spring Boot에서 30분 내에 자동 주문 취소를 구현하는 여러 방법과 예시 코드에 대해 다루겠습니다.

# 방법 1: 예약된 작업

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

@Scheduled 주석을 사용하면 주기적으로 주문 레코드를 스캔하는 예약된 작업을 쉽게 구현할 수 있습니다.

지불되지 않은 주문이 30분 이상 지난 경우, 해당 주문은 종료됩니다.

```java
@Component
public class OrderSchedule {
    @Autowired
    private OrderService orderService;

    @Scheduled(cron = "0 0/1 * * * ?")
    public void cancelUnpaidOrders() {
        LocalDateTime now = LocalDateTime.now();
        List<Integer> idList = new ArrayList<Integer>();
        List<OrderEntity> orderList = orderService.getOrderList();
        orderList.forEach(order -> {
            if (order.getWhenCreated().plusMinutes(30).isBefore(now)) {
                idList.add(order.getId());
            }
        });
        orderService.cancelOrderList(idList);
    }
}
```

# 방법 2: 지연된 큐

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

메시지 대기열을 사용할 때 주문이 생성되면 주문 ID가 30분의 만료 시간으로 설정된 지연 대기열에 푸시됩니다.

대기 시간이 만료되면 메시지가 소비되고 주문 상태가 확인됩니다. 주문이 아직 지불되지 않았다면, 주문이 취소됩니다.

```java
@Service
public class OrderService {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    public void createOrder(Order order) {
        // 주문을 데이터베이스에 저장합니다
        saveOrder(order);

        // 주문 ID를 지연 대기열로 푸시합니다
        rabbitTemplate.convertAndSend("orderDelayExchange", "orderDelayKey", order.getId(), message -> {
            message.getMessageProperties().setDelay(30 * 60 * 1000); // 대기 시간을 설정합니다
            return message;
        });
    }
}

@Component
public class OrderDelayConsumer {

    @Autowired
    private OrderService orderService;

    @RabbitHandler
    @RabbitListener(queues = "orderDelayQueue")
    public void cancelOrder(String orderId) {
        // 주문을 취소합니다
        orderService.cancelOrder(orderId);
    }
}
```

# 방법 3: Redis 만료 이벤트

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

Redis 키 만료 이벤트를 사용하면 주문이 생성될 때 Redis에 30분 만료 시간을 갖는 키가 저장됩니다.

키가 만료되면 Redis가 주문 취소를 유도하는 이벤트 알림을 트리거합니다.

```java
@Service
public class OrderService {

    @Autowired
    private StringRedisTemplate redisTemplate;

    public void createOrder(Order order) {
        // 주문을 데이터베이스에 저장
        saveOrder(order);

        // 30분 만료 시간을 갖는 키를 Redis에 저장
        redisTemplate.opsForValue().set("order:" + order.getId(), order.getId(), 30, TimeUnit.MINUTES);
    }

    // 이 메서드는 키가 만료될 때 자동으로 호출됩니다 (Redis가 만료 이벤트 알림을 위해 구성되어 있어야 함)
    public void onOrderKeyExpired(String orderId) {
        cancelOrder(orderId);
    }
}
```

참고: Redis 키 만료 알림은 전형적인 발행-구독 패턴을 따릅니다.

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

Redis에서는 특정 이벤트, 예를 들어 키 만료와 같은 이벤트에 구독할 수 있습니다.

그러나 이 기능을 사용하려면 Redis 서버를 해당 설정으로 구성해야 합니다. 이에 대한 구성 방법에 대한 자세한 내용은 나중에 다른 기사에서 공유될 예정입니다.

# 최종 요약:

세 가지 접근 방식 모두 주문이 30분 이내에 지불되지 않으면 자동으로 취소되는 요구 사항을 충족할 수 있습니다.

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

특정 비즈니스 요구 사항, 시스템 부하 및 기타 요소에 따라 시스템에 가장 잘 맞는 구현 방법을 선택할 수 있습니다.

각 접근 방식에는 장단점이 있으며, 올바른 선택을하기 위해 신중한 고려가 필요합니다.
