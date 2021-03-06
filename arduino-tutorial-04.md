# Thực hành với cảm biến siêu âm HC-SR05
## Giới thiệu

### Cảm biến siêu âm HC-SR05
![alt](images/tutorial-04/hc-sr05.png)

![alt](images/tutorial-04/hc-sr05-01.png)

## Linh Kiện
### Cảm biến siêu âm HC-SR05
Cảm biến siêu âm HC-SR05 (cảm biến đo khoảng cách) sử dụng rất phổ biến để xác định khoảng cách. HC-SR05 sử dụng sóng siêu âm và có thể đo khoảng cách trong khoảng từ 2 -> 300 cm, với độ chính xác gần như chỉ phụ thuộc vào cách lập trình.

### Nguyên lý tính khoảng cách
Tốc độ của âm thanh trong không khí là 340 m/s (hằng số vật lý), tương đương với 29,412 microSeconds/cm (106 / (340*100)). Khi đã tính được thời gian, ta sẽ chia cho 29,412 để nhận được khoảng cách.

```c
unsigned long duration; // biến đo thời gian
int distance;           // biến lưu khoảng cách
/* Phát xung từ chân trig */
digitalWrite(trig,0);   // tắt chân trig
delayMicroseconds(2);
digitalWrite(trig,1);   // phát xung từ chân trig
delayMicroseconds(5);   // xung có độ dài 5 microSeconds

digitalWrite(trig,0);   // tắt chân trig

/* Tính toán thời gian */
// Đo độ rộng xung HIGH ở chân echo.
duration = pulseIn(echo,HIGH);

// Tính khoảng cách đến vật.
distance = int(duration/2/29.412);
```

* Các thông số chính
  - Nguồn làm việc: 3.3V – 5V (chuẩn 5V)
  - Dòng tiêu thụ : 2mA
  - Tín hiệu đầu ra xung: HIGH (5V) và LOW (0V)
  - Khoảng cách đo: 2cm – 300cm
  - Độ chính xác: 0.5cm

* Sơ đồ chân của HC-SR05 gồm 4 chân:
  - VCC –> pin 5V Arduino.
  - trig –> chân digital (OUTPUT), đây là chân sẽ phát tín hiệu từ cảm biến.
  - echo –> chân digital (INPUT), đây là chân sẽ nhận lại tín hiệu được phản xạ từ vật cản
  - GND —> GND Arduino.

## Lắp mạch
### Sơ đồ ráp mạch
![alt](images/tutorial-04/assemble-hc-sr04.png)