# 코드 리뷰

## 프로젝트 개요
실시간 채팅 기능을 제공하는 웹 기능입니다. 
Spring Framework를 사용하여 WebSocket을 구성하고, 사용자 간의 메시지를 주고받는 기능을 구현했습니다.


## 주요 파일 설명

### WebSocketConfig.java
이 파일은 WebSocket을 설정하는 구성 파일입니다. WebSocket 메시지 브로커를 활성화하고, STOMP 엔드포인트를 등록합니다.
```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic");
        config.setApplicationDestinationPrefixes("/app");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/chat").withSockJS();
    }
}

