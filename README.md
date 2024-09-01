# 딜리메이트

**딜리메이트 Web Page v1.0**

![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/main.png)
[딜리메이트](https://github.com/juseungpark97/Semiproject)


**개발기간**: 2024.06 ~ 2024.07

## 웹개발팀 인원

- 박주승, 이주빈, 오민혁, 정인석, 이은호, 강경호

## 프로젝트 소개

**딜리메이트**는 빠르고 효율적인 서비스 제공을 목표로 하는 웹 애플리케이션입니다. 

### 주요 기능

- 주문 생성
- 채팅방(고객, 라이더 1:1 채팅방)
- 목록 조회/상세 조회
- 회원가입/고객 및 라이더 로그인 (아이디/비밀번호 찾기)
- 파일 첨부
- 고객 문의
- 신고하기
- 긴급배송

### 사용된 기술 스택

- **프론트엔드**:
  - HTML5
  - CSS3
  - JavaScript
- **백엔드**:
  - Spring Framework(Version: 3.9.18.RELEASE)
- **데이터베이스**:
  - Oracle Database 21c Express Edition
- **개발 환경**:
  - Java(TM) 플랫폼: 11.0.21, 0.2
  - Oracle IDE: 23.1.1.345.2114
- **서버**:
  - Tomcat v9.0 Server

### 사용한 API
  - 카카오맵API
  - 카카오모빌리티API

### 담당 기능

- 회원가입
  ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/signup.png)
```javascript
// 사용자가 숫자 이외의 문자를 입력하면 자동으로 제거하고, 전화번호 양식에 맞게 하이픈이 자동으로 들어가는 코드입니다.
// 사용자가 보기 편하면서 DB에 보낼때는 원하는 길이의 값으로 매핑하는 코드입니다.
function formatPhoneNumber(input) {
    // 입력 값에서 숫자 이외의 모든 문자를 제거
    let cleaned = input.value.replace(/\D/g, ''); 
    let formatted = ''; // 형식화된 전화번호를 저장할 변수

    // 입력된 숫자의 길이가 3보다 크고 7 이하인 경우
    if (cleaned.length > 3 && cleaned.length <= 7) {
        // 첫 세 자리 숫자 뒤에 하이픈을 추가
        formatted = cleaned.replace(/(\d{3})(\d+)/, '$1-$2'); 
    } 
    // 입력된 숫자의 길이가 7보다 큰 경우
    else if (cleaned.length > 7) {
        // 첫 세 자리 숫자와 그 다음 네 자리 숫자 뒤에 하이픈을 추가
        formatted = cleaned.replace(/(\d{3})(\d{4})(\d+)/, '$1-$2-$3'); 
    } 
    // 입력된 숫자의 길이가 3 이하인 경우
    else {
        // 그대로 할당
        formatted = cleaned; 
    }

    // 입력 값에 형식화된 전화번호를 할당
    input.value = formatted; 
}

function cleanPhoneNumber() {
    // 'phone' 아이디를 가진 입력 요소를 선택
    let phoneInput = document.getElementById('phone'); 
    // 입력 값에서 모든 하이픈을 제거
    phoneInput.value = phoneInput.value.replace(/-/g, ''); 
}

// cleanPhoneNumber를 따로 사용한 이유는 DB에 Phone컬럼 크기가 11이기 때문입니다.
// 하이픈이 들어가게 되면 11을 초과하므로 DB에 올바르게 들어갈 수 없기 때문에 그것을 없애주면서 사용자가 보기에 편한 로직을 짰습니다.
<button class="tooltip" id="" onclick="cleanPhoneNumber()">회원가입</button>
```
  
- 주문 생성, 카카오 맵 API
  ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/주문생성.png)
```javascript
// 카카오맵 API와 카카오모빌리티 API를 이용해서 지도를 띄우고, 마커를 찍으며 마커사이의 길찾기 경로를 폴리라인으로 나타내는 코드입니다.
// 맵을 띄우는것과 마커를찍는것은 카카오맵API에서 가져오고, 길찾기는 카카오모빌리티 API를 이용했습니다.
// 사용자에게 위도, 경도를 보여줘도 의미가 없다고 생각해서 지오코더를 이용해 실제 주소로 사용자에게 뜨게 했습니다.
<script type="text/javascript"
	src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=API-KEY&libraries=services"></script>
// 'map' 아이디를 가진 HTML 요소를 선택
var mapContainer = document.getElementById('map'); 

// 지도 옵션 설정
var mapOption = {
    center: new kakao.maps.LatLng(33.450701, 126.570667), // 초기 중심 좌표 설정
    level: 3 // 지도 확대 레벨 설정
};

// 지도 생성
var map = new kakao.maps.Map(mapContainer, mapOption);

// 주소-좌표 변환 객체 생성
var geocoder = new kakao.maps.services.Geocoder();

// 브라우저에서 지오로케이션을 지원하는지 확인
if (navigator.geolocation) {
    // 지오로케이션을 통해 현재 위치를 얻어옴
    navigator.geolocation.getCurrentPosition(function(position) {
        var lat = position.coords.latitude; // 위도
        var lon = position.coords.longitude; // 경도
        var locPosition = new kakao.maps.LatLng(lat, lon); // 현재 위치의 좌표 객체 생성
        map.setCenter(locPosition); // 현재 위치를 지도 중심으로 설정
    }, function(error) {
        console.error(error); // 오류 발생 시 콘솔에 오류 메시지 출력
    });
} else {
    console.error("Geolocation is not supported by this browser."); // 지오로케이션을 지원하지 않는 경우
}

// 마커와 폴리라인 배열 초기화
var markers = [];
var polylines = [];

// 지도 클릭 이벤트 리스너 추가
kakao.maps.event.addListener(map, 'click', function(mouseEvent) {
    if (markers.length >= 2) {
        clearMap(); // 마커가 2개 이상인 경우 맵 초기화
    } else {
        var latlng = mouseEvent.latLng; // 클릭한 위치의 좌표 얻기
        var marker = new kakao.maps.Marker({
            position: latlng // 마커 위치 설정
        });
        marker.setMap(map); // 지도에 마커 추가
        markers.push(marker); // 마커 배열에 추가
        getAddressFromCoords(latlng); // 좌표로 주소 찾기
        if (markers.length === 2) {
            setTimeout(() => {}, 5000); // 5초 지연 (명시적 딜레이가 없는 빈 함수)
            findRouteAndDrawLine(); // 경로 찾고 선 그리기
        }
    }
});

// 두 마커 사이의 경로를 찾고 선을 그리는 함수
function findRouteAndDrawLine() {
    var start = markers[0].getPosition(); // 첫 번째 마커 위치
    var end = markers[1].getPosition(); // 두 번째 마커 위치
    
    // 경로 요청 URL 생성
    var url = `https://apis-navi.kakaomobility.com/v1/directions?origin=${start.getLng()},${start.getLat()}&destination=${end.getLng()},${end.getLat()}&waypoints=&priority=RECOMMEND&road_details=false`;
    
    // 경로 요청 API 호출
    fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': 'KakaoAK API-KEY' // 인증 키
        }
    })
    .then(response => response.json()) // 응답을 JSON으로 변환
    .then(data => {
        if (data.routes && data.routes.length > 0) {
            var route = data.routes[0]; // 첫 번째 경로 선택
            var linePath = []; // 경로를 저장할 배열
            route.sections.forEach(section => {
                section.roads.forEach(road => {
                    road.vertexes.forEach((vertex, index) => {
                        if (index % 2 === 0) {
                            linePath.push(new kakao.maps.LatLng(road.vertexes[index+1], road.vertexes[index])); // 경로 좌표 추가
                        }
                    });
                });
            });

            // 폴리라인 생성 및 지도에 추가
            var polyline = new kakao.maps.Polyline({
                path: linePath,
                strokeWeight: 5,
                strokeColor: '#86C1C6',
                strokeOpacity: 0.7,
                strokeStyle: 'solid'
            });

            polyline.setMap(map); // 지도에 폴리라인 추가
            polylines.push(polyline); // 폴리라인 배열에 추가

            var distance = polyline.getLength(); // 폴리라인의 길이 계산
            var distanceKm = (distance / 1000).toFixed(2); // 길이를 km 단위로 변환하여 소수점 2자리까지 표시
            
            var distanceMoney = 5000 + Math.floor(distanceKm * 4000); // 거리 기반 요금 계산
            document.getElementById("price").value = distanceMoney + "원"; // 계산된 요금 표시
            document.getElementById("dis").value = distanceKm + "km"; // 계산된 거리 표시
            console.log('두 마커 사이의 거리는 ' + distanceKm + ' km 입니다.');
        } else {
            console.log('경로를 찾을 수 없습니다.');
        }
    })
    .catch(error => console.log('경로 요청 중 오류 발생:', error)); // 오류 발생 시 콘솔에 출력
}
function getAddressFromCoords(coords) {
    // 좌표를 주소로 변환하는 함수 호출
    geocoder.coord2Address(coords.getLng(), coords.getLat(), function(result, status) {
        // 주소 변환이 성공한 경우
        if (status === kakao.maps.services.Status.OK) {
            var address = result[0].address.address_name; // 변환된 주소를 가져옴
            if (markers.length === 1) {
                // 마커가 하나일 때는 시작 지점의 주소로 설정
                document.getElementById('start-point').value = address;
            } else if (markers.length === 2) {
                // 마커가 두 개일 때는 끝 지점의 주소로 설정
                document.getElementById('end-point').value = address;
            }
        }
    });
}

```


- 주문상세보기
- ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/주문상세보기.png)

```javascript
// 주문상세보기는 새로운 마커를 찍을 필요가 없고, 기존의 마커와 길찾기를 똑같이 보여줍니다.
// 마커를 찍기위해 다시 지오코더를 이용해서 이번엔 주소를 위도와 경도로 바꿔줍니다.
// 바꾼 위도와 경도를 기준으로 마커를 다시 찍고 길찾기를 합니다.
// 길찾기는 주문생성 페이지에 있는 카카오 모빌리티를 재활용했습니다.
// 마커와 마커사이가 멀어질 것을 대비해 두 마커가 전부 보이는 중앙을 기점으로 지도를 확대, 축소합니다.
function initializeMap() {
    var mapContainer = document.getElementById('map'); // 'map' 아이디를 가진 HTML 요소를 선택
    var mapOption = {
        center: new kakao.maps.LatLng(33.450701, 126.570667), // 초기 지도 중심 좌표 설정
        level: 4 // 지도 확대 레벨 설정
    };

    var map = new kakao.maps.Map(mapContainer, mapOption); // 지도 객체 생성
    var geocoder = new kakao.maps.services.Geocoder(); // 주소-좌표 변환 객체 생성

    var startPoint = "${order.startPoint}"; // 시작 지점 주소
    var endPoint = "${order.endPoint}"; // 끝 지점 주소

    var markers = []; // 마커를 저장할 배열
    var polylines = []; // 폴리라인을 저장할 배열

    // 주소를 좌표로 변환하는 함수
    function geocodeAddress(address) {
        return new Promise(function(resolve, reject) {
            geocoder.addressSearch(address, function(result, status) {
                if (status === kakao.maps.services.Status.OK) {
                    resolve(new kakao.maps.LatLng(result[0].y, result[0].x)); // 변환된 좌표를 반환
                } else {
                    reject(status); // 실패 시 상태 반환
                }
            });
        });
    }

    // 시작 지점과 끝 지점의 주소를 좌표로 변환
    Promise.all([geocodeAddress(startPoint), geocodeAddress(endPoint)])
        .then(function(coords) {
            // 시작 지점 마커 생성 및 지도에 추가
            var startMarker = new kakao.maps.Marker({
                map: map,
                position: coords[0]
            });
            markers.push(startMarker); // 마커 배열에 추가

            // 끝 지점 마커 생성 및 지도에 추가
            var endMarker = new kakao.maps.Marker({
                map: map,
                position: coords[1]
            });
            markers.push(endMarker); // 마커 배열에 추가

            adjustMapBounds(); // 지도 범위 조정

            if (markers.length === 2) {
                findRouteAndDrawLine(); // 두 마커가 모두 존재하면 경로를 찾고 선을 그림
            }
        })
        .catch(function(error) {
            console.error('Geocode error:', error); // 주소 변환 실패 시 콘솔에 오류 출력
        });

    // 지도의 범위를 마커들이 모두 보이도록 조정하는 함수
    function adjustMapBounds() {
        if (markers.length === 2) {
            var bounds = new kakao.maps.LatLngBounds(); // 지도의 범위 객체 생성
            markers.forEach(marker => bounds.extend(marker.getPosition())); // 각 마커의 위치를 범위에 포함
            map.setBounds(bounds); // 지도의 범위를 설정
        }
    }
}


```

  
- 채팅방(고객, 라이더 1:1 채팅방)
  ![Example Image](https://github.com/juseungpark97/introduce/blob/main/image/채팅방.png)
```javascript
// stomp라이브러리와, SockJS를 이용한 채팅방 구현 코드
// 안정적인 연결을 위해 네트워크와, 브라우저 환경에 유연한 SockJs를 사용했고, 1:1채팅방을 만들기 위해 구독을 이용하는 stomp라이브러리를 이용했습니다.
// 추가로 채팅방이기 때문에 연결했을 때만 서로 채팅을 주고받는것이 아니라 다른곳에 갔다가 다시 채팅방에 왔을 경우에도 여전히 채팅내역은 남아있게끔 DB를 이용해서 만들었습니다.
    var stompClient = null; // STOMP 클라이언트를 저장할 변수

    function connect() {
        var chatRoomId = document.getElementById('chatRoomId').value; // 채팅방 ID 가져오기
        var socket = new SockJS('<%= request.getContextPath() %>/chat'); // SockJS 객체 생성
        stompClient = Stomp.over(socket); // STOMP 객체 생성
        stompClient.connect({}, function (frame) { // STOMP 서버에 연결
            console.log('Connected: ' + frame); // 연결 성공 시 콘솔에 출력
            // 특정 채팅방 ID에 해당하는 메시지 구독
            stompClient.subscribe('/topic/messages/' + chatRoomId, function (messageOutput) {
                console.log('Received message:', messageOutput.body); // 수신된 메시지 콘솔에 출력
                showMessage(JSON.parse(messageOutput.body)); // 메시지 화면에 표시
            });
        });
    }

    function sendMessage() {
        var from = document.getElementById('from').value; // 보내는 사람 ID 가져오기
        var text = document.getElementById('text').value; // 메시지 내용 가져오기
        var chatRoomId = document.getElementById('chatRoomId').value; // 채팅방 ID 가져오기

        console.log('Sending message:', from, text); // 메시지 전송 콘솔에 출력
        // 메시지 전송
        stompClient.send("/app/sendMessage", {}, JSON.stringify({
            'senderId': from, // 보내는 사람 ID
            'content': text, // 메시지 내용
            'writer' : '${nickName}', // 작성자 닉네임
            'chatRoomId': chatRoomId // 채팅방 ID
        }));

        document.getElementById('text').value = ''; // 메시지 전송 후 입력 필드 초기화
        scrollToBottom(); // 채팅창 스크롤을 맨 아래로 이동
    }

    function showMessage(message) {
        var response = document.getElementById('messages'); // 메시지 리스트 요소 선택
        var li = document.createElement('li'); // 새로운 리스트 아이템 생성
        // 메시지 내용 설정
        li.appendChild(document.createTextNode(`${message.writer}` + ": " + message.content));
        response.appendChild(li); // 메시지 리스트에 아이템 추가
        scrollToBottom(); // 채팅창 스크롤을 맨 아래로 이동
    }

    function scrollToBottom() {
        var chat = document.getElementById('chat'); // 채팅창 요소 선택
        chat.scrollTop = chat.scrollHeight; // 스크롤을 맨 아래로 이동
    }

    function handleKeyPress(event) {
        if (event.key === 'Enter') { // Enter 키를 누를 때
            sendMessage(); // 메시지 전송
        }
    }

    function exitChat() {
        window.history.back(); // 이전 페이지로 이동
    }

    connect(); // 페이지 로드 시 서버에 연결
```

```java
// 1:1채팅방이기 때문에 DB에서 중복되지 않는 primaryKey로 메세징 구독 처리를 했습니다. 이러면 채팅방끼리 서로 오송신이 일어나지 않습니다.
// 메세지를 전달하는순간 DB에 메세지가 저장되고, 다시 채팅방에 입장할때는 그 저장된 DB를 순서대로 불러와서 실제 채팅방 기능을 구현했습니다.
// 주문당 채팅방이 고객-라이더 이렇게 1개이기때문에 주문번호(고유값)를 이용해서 각 DB에 저장하기 때문에 소켓통신을 통한 채팅도 1:1이고, 다시 같은 채팅방 입장했을때 채팅내역도 1:1입니다.
@MessageMapping("/sendMessage")
public void sendMessage(Message message) {
    // 메시지의 senderId가 0이면 예외를 발생시킴 (로그인되지 않은 사용자)
    if (message.getSenderId() == 0) {
        throw new IllegalStateException("User is not logged in.");
    }

    // 메시지를 데이터베이스에 저장
    messageService.saveMessage(message);

    // 특정 채팅방의 주제로 메시지 전송
    messagingTemplate.convertAndSend("/topic/messages/" + message.getChatRoomId(), message);
}

@GetMapping("/messages")
@ResponseBody
public List<Message> getMessages(@RequestParam int chatRoomId) {
    // 주어진 채팅방 ID에 해당하는 메시지 목록을 반환
    return messageService.getMessagesByChatRoomId(chatRoomId);
}


```


## 연락처
프로젝트 관련 문의는 다음 이메일로 연락해주세요: [parkjuseung0313@gmail.com]

