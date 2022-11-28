## 저수준 I/O

### I/O 포트

회로를 구성할 때에는 부품마다 스펙이 다르므로 손상와 사고를 방지하기 위해 데이터시트를 참고합니다.

[[Sample] - Vishay Pi 3mm LED datasheet](https://www.vishay.com/docs/83001/tldr4400.pdf)

### 버튼을 눌러라

금속 케이블로 연결된 부품 간에는 스위치가 눌리지 않았음에도 미세전류로 인해 HIGH 와 LOW 상태가 왔다갔다하는 플로팅(floating) 현상이 발생할 수 있습니다. 이는 회로 작동에 오류를 불러일으키는 요인이 될 수 있기에 이를 방지할 방법 중 하나로 풀업 저항을 사용합니다.
<br>
<br>

#### 풀업 저항

<img src='https://t1.daumcdn.net/cfile/tistory/9991FB435A9CD4A814'>
<br>
<br>

5V 핀에서 흘러들어오는 전류는 스위치가 열려있는 이상 저항을 거쳐 입력핀으로 흐릅니다. 그래서 스위치가 눌리지 않은 평소에는 2번 입력핀은 HIGH 신호를 받습니다.
하지만 버튼을 누르면 전압이 미세전류가 흐르는 2번 입력핀이 아닌, 전압 0 인 Ground 핀으로 전류가 흐르게 됩니다.
<br>

이처럼 스위치를 누르지 않은 상태에 HIGH 를 유지하고 누르면 LOW 로 상태변화를 주는 저항 회로구성을 풀업 저항이라고 합니다.
<br>
<br>

### 직렬통신

- BPS (Beat per Second) <br>
  초당 보낼 수 있는 비트의 수
- Baud Rate <br>
  초당 보낼 수 있는 단위 신호 수. 초당 일어난 신호 변조의 횟수.
  <br>
  <br>

#### UART (Universal Asyncronous Receiver-Transmitter)

<img src='https://cdn.rohde-schwarz.com/pws/solution/research___education_1/educational_resources_/oscilloscope_and_probe_fundamentals/05_Understanding-UART_01_w640_hX.png'>
<br>
<br>
수신용 핀 RX, 송신용 핀 TX 핀을 활용해서 간단하게 구현할 수 있는 양방향 통신 프로토콜입니다. RX, TX 핀은 클럭을 공유하지 않으므로 Baud Rate 를 선언해서 공유해야합니다. 일반적으로 4800, 9600, 192000, 1152000 등이 사용됩니다.
<br>
<br>

#### I2C (Inter-Integrated Circuit)

데이터 전송용 핀 SDA, 클럭 동기용 핀 SCL 핀으로 구성된 양방향 통신 프로토콜입니다. 하나의 데이터 선으로 양방향 데이터 송수신이 이루어지기 때문에 풀업저항 구성이 필수적입니다.
<br>
<br>

#### SPI

1-N 통신에 특화된 동기식 통신 프로토콜입니다. 데이터 송수신선이 구분되어있기 때문에 송수신이 빠릅니다.

- MOSI (Master Out, Slave In)
- MISO (Master In, Slave Out)
- SCK (Select Clock)
- SS (Select Slave)
  <br>
  <br>

#### USB (Universal Serial Bus)

엔드포인트의 컨트롤러에 따라 범용적으로 사용할 수 있는 버스입니다. 컨트롤러에 따라 USB-UART, USB-I2C 등 여러 형태로 사용할 수 있습니다.
<br>
<br>

## 네트워킹

- Internet
  Inter + Network. 네트워크들로 이뤄진 네트워크. 즉, 여러 LAN 을 하나로 연결해주는 WAN.

- Ethernet
  최초의 이더넷은 반이중 시스템. 컴퓨터를 포함한 모든 주변기기가 같은 선에 연결되었습니다. 때문에 MAC(Media Access Control) 주소로 장치를 구분했고 일치하지않는 장치의 데이터가 송수신 될 경우, 무시가 되는 구조였습니다. <br>
  현재는 라우터(Router) 가 MAC 주소를 기억하고 데이터를 전달하도록 개선되었습니다.
  <br>
  <br>

## 인터넷

### TCP/IP (Transmission Control Protocol/Internet Protocol)

패킷 전송 단위인 데이터그램(Datagram) 을 옮겨주는 IP 레이어 위에 TCP 가 구성되어있고, 두 프로토콜은 대부분 같이 사용되기 때문에 붙여서 부르곤 합니다. <br>

<img src='https://t1.daumcdn.net/cfile/tistory/995EFF355B74179035'>
<br>
<br>

데이터의 신뢰성(연속성)을 보장하지않는 UDP(User Datagram Protocol) 와는 다르게 데이터의 송수신 여부를 확인하는 Handshake/Response 단계를 거칩니다. 하지만 이런 절차 때문에 UDP 보다는 속도가 느립니다.

<br>

TCP/IP 의 신뢰성 특징 때문에 그 위에 만들어진 프로토콜이 여럿 있습니다.

- FTP (File Transfer Protocol)
- SMTP (Simple Mail Transfer Protocol)
- HTTP (Hyper-Text Transfor Protocol)
- DHCP (Dynamic Host Configuration Protocol)
  <br>
  etc..
  <br>
  <br>

## 아날로그 처리 방법

### 디지털을 아날로그로 변환

1비트는 수준이 2 이므로 LED 에 적용했을 때 표현할 수 있는 경우는 [켜짐], [꺼짐] 의 두 가지 뿐입니다. 더욱 다양한 경우를 표현하기 위해 비트를 더 붙였다고 가정해봅시다. 2비트만 되어도 표현할 수 있는 경우가 2 가지 추가됩니다. 하나는 [꺼짐] 과 [완전히 켜짐] 에 추가해 [낮은 밝기로 켜짐] 과 [높은 밝기로 켜짐] 으로 표현할 수 있겠습니다.
<br>
이렇게 디지털 신호를 비트를 더 활용해 수준을 나누고 표현하는 방식으로 아날로그 표현을 모방할 수 있습니다.
<br>
<br>

### 아날로그를 디지털로 변환

디지털 입장에서 아날로그 신호는 일정하지 않고 흔들리는 데이터 덩어리입니다. 때문에 단위시간 동안 아날로그 신호를 샘플링하고 문턱값(Treshold) 과 비교하는 작업을 거칩니다.
<br>
<br>

#### 음원, 이미지, 비디오

변환 방식은 유사합니다. 샘플링 데이터와 나열의 방식 차이뿐입니다.
음원은 시간-헤르츠, 이미지는 픽셀 위치-색, 비디오는 앞서 말한 것을 혼합한 형태입니다.
