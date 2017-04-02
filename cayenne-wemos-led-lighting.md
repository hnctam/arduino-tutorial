# Thực hành bật tắt đèn LED với Cayenne và mạch WeMos D1 R2 (8266)
## Giới thiệu

### Cayenne
Cayenne là dự án Internet of Thing (IOT), hỗ trợ quản lý các thiết bị, giám sát cũng như công tác điều khiển cho các thiết bị này.
![alt](images/tutorial-05/cayenne.png)

Với Cayenne, các em có thể xây dựng các màn hình giám sát thiết bị hoặc điều khiển thiết bị từ xa thông qua Cayenne.

### WeMos D1 R2
WEMOS D1 R2 là kit phát triển phiên bản mới nhất từ WeMos, kit được thiết kế với hình dáng tương tự Arduino Uno nhưng trung tâm lại là module wifi Soc ESP8266EX được build lại firmware để có thể chạy với chương trình Arduino.  Kit thích hợp và dễ dàng thực hiện các ứng dụng thu thập dữ liệu và điều khiển qua Wifi.

![alt](images/tutorial-05/wemos-d1-r2.png)

KIT ESP8266 WeMos D1 R2 có hình dáng giống Arduino Uno, sử dụng chip wifi ESP8266-12, có thể được nạp Firmware để viết chương trình như Arduino

* Có 11 chân vào/ra số
* 1 chân Analog (chỉ đọc được điện áp 1V)
* Kết nối USB bằng cổng MicroUSB
* Điện áp hoạt động: 5V (từ cổng USB) hoặc 7-12V (từ cổng DC)
* Tương thích Arduino IDE và NodeMCU

[Tài liệu tham khảo](https://www.wemos.cc/tutorial)

### Cấu hình Arduino IDE để làm việc với Cayenne và WeMos
* Thiết lập Arduino IDE với WeMos
Trên menu ứng dụng Arduino IDE, chọn: ```Tools > Boards : "[Board type]" > Boards Manager...```

![alt](images/tutorial-05/arduino-ide-8266.png)

Em nhập 8266 vào ô nhập ```Type```, sau đó chọn ```Install```. Hình minh họa trên là sau khi em đã cài đặt thành công nhé.

* Thiết lập Arduino IDE với Cayenne

Một cách tương tự. em sẽ cài dặt các ví dụ về Cayenne cũng như các thư viện liên quan.
Trên menu ứng dụng của Arduino IDE, em chọn ```Sketch > Include Library > Manage Libraries...```. Sau đó nhập ```Cayenne``` vào ô nhập ```Topic``` như hình dưới đây:

![alt](images/tutorial-05/arduino-ide-cayenne.png)

Hình trên là hiển thị của Arduino IDE sau khi cài đặt hoàn thành.

## Thiết kế và lập trình mạch bật tắt đèn LED
Tương tự Arduino, WeMos dễ dàng để lập trình và thiết kế mạch. Mạch sau dùng để bật tắt đèn LED theo chân D7.

### Đăng ký tài khoản Cayenne

### Tạo mới một thiết bị trên Cayenne

Hai bước sau dùng để đăng ký thiết bị mới với Cayenne:
* Tạo mới thiết bị:

![alt](images/tutorial-05/cayenne-add-new-device.png)

* Ghi nhớ mã an ninh dùng để kết nối với Cayenne

![alt](images/tutorial-05/cayenne-got-authen-token.png)
### Ráp mạch:
Hình sau mô tả mạch ráp để bật đèn LED. Pin D7 sẽ được dùng để kích nguồn cho đèn.
![alt](images/tutorial-05/wemos-board-led.jpg)

### Mã chương trình
* Khai báo các thư viên liên quan

Các thư viện Cayenne và 8266 được khai báo để sử dụng trong ứng dụng:

```C
#define CAYENNE_DEBUG
#define CAYENNE_PRINT Serial

#include "CayenneDefines.h"
#include "BlynkSimpleEsp8266.h"
#include "CayenneWiFiClient.h"
#include "ESP8266WiFi.h"
```

* Khai báo các biến được sử dụng trong chương trình
```C
// Thông tin bảo mật dùng để kết nối với đám mây của Cayenne
char token[] = "abcdedghij";
// Thông tin WIFI kết nối
char ssid[] = "SSID";
char password[] = "Password";

unsigned long lastTempUpdate = 0;

// Khai báo PIN ảo trên Cayenne
#define VIRTUAL_PIN V4
```

* Phần chương trình cấu hình
```C
void setup()
{
  // Khai báo PIN 2 là chân xuất.
  pinMode(2, OUTPUT);

  // Khai báo chân D7 là chân xuất, ta sẽ dùng chân D7 để bật tắt đèn LED.
  pinMode(D7, OUTPUT);

  Serial.begin(9600);
  // Kết nối với Cayenne thông qua thông tin cấu hình WIFI
  Cayenne.begin(token, ssid, password);
}
```

* Phần chương trình thực thi
```C
void loop()
{
  Cayenne.run();
}

// This function will be called every time a Dashboard widget writes a value to Virtual Pin 2.
CAYENNE_IN(V2)
{
  CAYENNE_LOG("Got a value: %s", getValue.asStr());
  int i = getValue.asInt();
  
  if (i == 0)
  {
    digitalWrite(2, HIGH);
  }
  else
  {
    digitalWrite(2, LOW);
  }  
}

CAYENNE_IN(V7)
{
  CAYENNE_LOG("Got a value: %s", getValue.asStr());
  int i = getValue.asInt();
  
  if (i == 0)
  {
    digitalWrite(D7, HIGH);
  }
  else
  {
    digitalWrite(D7, LOW);
  }  
}
```