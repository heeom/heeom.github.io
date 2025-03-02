---
layout: post
title: Spring Cloud Stream으로 Kafka 메시지 처리하기 - 1
---


## Spring Cloud Stream?
- 마이크로 서비스 아키텍처에서 이벤트 기반 메시징을 쉽게 처리할 수 있도록 도와주는 프레임워크
- Kafka, RabbitMQ, AWS SQS 와 같은 메세지 브로커와의 통합을 단순화하고, 프로듀서와 컨슈머를 쉽게 구현할 수 있도록 지원한다. 


## Spring Cloud Stream 구성 요소
- Binder : 특정 메세지 브로커와 연결을 담당한다. Spring cloud stream 의 바인더를 사용하면 메세지를 발행하고 소비하기 위해 플랫폼마다 별도의 라이브러리와 API를 제공하지 않고도 메세징을 사용할 수 있다.
  - Kafka, Kafka Stream, AWS SQS, RabbitMQ 등 다양한 바인더가 구현되어 있다.
  - 메세지 브로커가 변경돼도 코드 수정을 최소화 하면서 바인더만 교체가능하고, 하나의 프로젝트에서 여러 종류의 메세지 브로커를 사용하더라도 동일한 형태로 구현할 수 있다.  
  

- Bindings : Application과 브로커간 메세지를 주고 받는 방식을 정의한 개체 (Producer & Consumer)  


- Message Channel : 메시지 생산자와 소비자가 메시지를 발행하거나 소비한 후 메시지를 보관할 큐를 추상화한 것이다. 코드에서는 큐 이름을 직접 사용하지 않고 채널 이름을 사용한다.  


- Producer & Consumer
  - Producer (Source) : 메세지를 생성해서 Output Channel을 통해 브로커로 보낸다.
  - Consumer (Sink) : 브로커에서 메세지를 받아서 Input Channel을 통해 처리한다.  

![img_1.png](/assets/images/kafka_streams.png)
  
## 참고: Spring Cloud Stream vs Spring Kafka vs Kafka-Client

먼저 결론 멀티브로커 환경에서는 Spring Cloud Stream이 유리하고, Kafka만 사용할거면 가벼운 Spring kafka가 낫다.  


|  | Spring cloud stream | Spring kafka | Kafka clients |
| --- | --- | --- | --- |
| 추상화 수준 | 고수준 추상화 (다양한 메세지 브로커 지원) | 중간 수준 추상화 | 낮음 |
| 오프셋 관리 | 자동관리 (Spring cloud Stream이 처리) | 자동관리 | 수동처리 commit 필요 |
| 멀티브로커 (서로 다른 메세지 브로커 지 동시지원) | Kafka, RabbitMQ, Aws SQS 등 다양한 브로커 지원 | Kafka 전용 | Kafka 전용 |
| 브로커 변경 가능성 | 설정 변경으로 다른 메세지 브로커로 이동 가능 | Kafka 이외의 브로커 추가, 변경 불가 | 변경 불가 |
| 유연성 및 성능 최적화 | Spring Cloud Stream 방식에 따라야 함 | 디테일한 Kafka 설정 가능 | Kafka 기본 API를 다룰 수 있어서 가장 유연 |




