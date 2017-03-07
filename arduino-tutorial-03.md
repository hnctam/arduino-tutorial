# Thực hành Đổi Màu Đèn LED
## Giới thiệu
Trong bài học sau đây, em sẽ làm quen với việc điều khiển đổi mày đèn LED. Em sẽ học cách sử dụng hàm ```analogWrite()``` để điều khiển màu sắc của đèn LED.
![alt](images/tutorial-03/learn_arduino_project_3_on_breadboard.jpg)

Điều đầu tiên cần phải biết là đèn LED đổi màu tương tự như 1 đèn LED bình thường và nó đơn giản có ba bóng với ba màu ĐỎ (RED), XANH LÁ (GREEN) và XANH DƯƠNG (Blue). Bằng việc điều khiển độ sáng từng bóng, các em có thể phối trộn cho ra bất cứ màu gì mình thích.
Để làm được điều đó, em cần học cách điều khiển độ sáng tối của đèn bằng các điện trở khác nhau.

## Linh Kiện
Đèn LED ba màu
![alt](images/tutorial-03/learn_arduino_rgb_cc_10mm.jpg)

Điện trở 270 Ohm
![alt](images/tutorial-03/learn_arduino_R-270_web.jpg)

Bảng mạch
![alt](images/tutorial-03/learn_arduino_breadboard_half_web.jpg)

Dây cắm
![alt](images/tutorial-03/learn_arduino_jumpers_web.jpg)

## Thiết Kế Mạch
![alt](images/tutorial-03/learn_arduino_fritzing.jpg)

## Màu Sắc
Em có thể pha trộn các màu cơ bản RGB để tạo ra các màu sắc theo ý muốn. Hình sau mô tả nguyên lý phối màu.
![alt](images/tutorial-03/learn_arduino_rgb_color.png)

* Nếu em pha trộn các màu với cùng độ sáng, màu thu được sẽ là màu trắng.
* Nếu em tắt đèn màu xanh dương, bật đèn đỏ và xanh lá cùng độ sáng, màu thu được là màu vàng.

## Chương Trình
```C
// Khai báo chân 11 cho màu đỏ, chân 10 cho màu xanh lá, chân 9 cho màu xanh dương
int redPin = 11;
int greenPin = 10;
int bluePin = 9;
 
void setup()
{
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);  
}
 
void loop()
{
  setColor(255, 0, 0);  // red
  delay(1000);
  setColor(0, 255, 0);  // green
  delay(1000);
  setColor(0, 0, 255);  // blue
  delay(1000);
  setColor(255, 255, 0);  // yellow
  delay(1000);  
  setColor(80, 0, 80);  // purple
  delay(1000);
  setColor(0, 255, 255);  // aqua
  delay(1000);
}
 
void setColor(int red, int green, int blue)
{
  analogWrite(redPin, red);
  analogWrite(greenPin, green);
  analogWrite(bluePin, blue);  
}
```