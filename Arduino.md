# Arduino 元件應用



## AHT10
```arduino=
#include <SPI.h>
#include <Wire.h>
#include <AHT10.h>

//-----------------------------------------------------

uint8_t readStatus = 0;
AHT10 myAHT10(AHT10_ADDRESS_0X38);
const uint64_t pipe = 0x00FF; // 設定發送端 pipe 名稱
 
void setup(void) {
  Serial.begin(9600);
  while (myAHT10.begin() != true)
  {
    Serial.println(F("AHT10 not connected...")); 
    delay(5000);
  }
  Serial.println(F("AHT10 initial OK"));
}

 
void loop(void) {
    double t = myAHT10.readTemperature();    //讀取 AHT10 溫度
    double h = myAHT10.readHumidity();   //讀取 AHT10 濕度
    Serial.print("Temperatura: ");
    Serial.print(t);
    Serial.print("   Humidity: ");
    Serial.println(h);
}
```


## LCD

https://youtu.be/b_EbN71kp-Q

```arduino=
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);

void setup() {
lcd.init();
lcd.backlight();
Serial.begin(9600);
lcd.clear();
lcd.setCursor(0,0);
lcd.print("  Hello World!");
lcd.setCursor(6,1);
lcd.print("^_^");
delay(1000);
}
void loop() {
lcd.setCursor(0,0);
lcd.print("Are You Ready?");
delay(1000);
for(int i=0;i<18;i++)
lcd.setCursor(i,1);
lcd.print("GO");
delay(500);
}
```







## LED
https://youtu.be/zOiw8cNzU78

```arduino=
void setup() {
pinMode(13, OUTPUT);
}
void loop() {
digitalWrite(13, HIGH);
delay(1000);
digitalWrite(13, LOW);
delay(1000);
}

```



## 可變電阻
https://youtu.be/iZT_zpbKRsk
```arduino=
#define pin A0
void setup() {
pinMode(pin,INPUT);
Serial.begin(9600);
}
void loop() {
Serial.println(analogRead(pin));
delay(100);
}

```




## RFID
https://youtu.be/l_DqxqUhu0I

```arduino=
#include <SPI.h>
#include <MFRC522.h> // 需下載MFRC522函式庫
#define RST_PIN 9 // RST接到9
#define NSS_PIN 10 // NSS接到10

MFRC522 mfrc522 (NSS_PIN,RST_PIN);
void setup() {
Serial.begin(9600); // 通訊 baud 值
SPI.begin();
mfrc522.PCD_Init(); // 初始化 MFRC522 讀卡機模組
Serial.print(F("Reader ")); // 印出文字
Serial.print(F(": ")); // 印出文字
mfrc522.PCD_DumpVersionToSerial(); // 顯示讀卡設備的版本
}


void loop() {
if (mfrc522.PICC_IsNewCardPresent() && mfrc522.PICC_ReadCardSerial()) {
  // 檢查是否讀到卡片
  // 顯示卡片內容
  Serial.print(F("Card UID:")); // 印出'Card UID:'
  byte *id = mfrc522.uid.uidByte; // 取得卡片的UID
  byte idSize = mfrc522.uid.size; // 取得UID的長度
  Serial.println();
  for (byte i = 0; i < idSize; i++){
  Serial.print(id[i]); // 印出卡片UID
  Serial.print(",");
  }

  Serial.println(); // 換行
  byte Card[4] = {55,227,194,26}; // 正確的卡片UID
  byte Cord[4] = {113,216,200,39}; // 正確的卡片UID
  byte Cerd[4] = {161,51,75,31}; // 正確的卡片UID
  for (byte i = 0; i < 4; i++){
  if (id[i] != Card[i] && id[i]!=Cord[i]&& id[i]!=Cerd[i])
  { // 判斷卡片UID符合
    Serial.print("拒絕進入"); // 拒絕進入
    Serial.println(); // 換行
    break;
    }

  if (i == 3){
    Serial.print("歡迎回來"); // 歡迎回來
    Serial.println(); // 換行
    }
  }
  mfrc522.PICC_HaltA(); // 讓卡片進入停止模式
  }
}

```


---

## Servo Motor 
https://youtu.be/kyXFV8ddGaM

```arduino=
#include <Servo.h>
#define pin 9
Servo servo9;
void setup() {
servo9.attach(pin);
}
void loop() {
servo9.write(0); // 旋轉到0度
delay(500);
servo9.write(90); // 旋轉到90度
delay(500);
servo9.write(180); // 旋轉到180度
delay(500);
}
```


---



## DHT11
https://youtu.be/qpd6e-_cvW8
```arduino=
#include <DHT.h> 
#define pin 6

DHT dht (pin, DHT11); // 宣告物件
void setup() {
  Serial.begin(9600);  // 初始化序列埠
  dht.begin();
}

void loop() {

  Serial.print(dht.readTemperature());
  Serial.print(" c , ");    
  Serial.print(dht.readHumidity());
  Serial.println(" % "); 

  delay(1); // 每2秒讀一次
}
```




---




## 超音波模組
https://youtu.be/MbmKsgL6y7w
```arduino=
#define trigPin 12
#define echoPin 11

double ultrasonic() {
digitalWrite(trigPin, LOW);
delay(2);
digitalWrite(trigPin, HIGH);
delay(10);
digitalWrite(trigPin, LOW);
double duration = pulseIn(echoPin, HIGH);  
double distance= duration*0.034/2;
return distance;
}
```


---



```arduino=
void setup() {
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
Serial.begin(9600);
}
void loop() {
  
double dist = ultrasonic();
Serial.print("Distance: ");
Serial.print(dist);
Serial.println(" cm");
delay(1);
}
```





---






# Arduino 作品



---





## 門禁卡模組

```arduino=
#include <SPI.h>
#include <MFRC522.h> // 需下載MFRC522函式庫
#define RST_PIN 9 // RST接到9
#define NSS_PIN 10 // NSS接到10
#define buzzer 6
MFRC522 mfrc522 (NSS_PIN,RST_PIN);
void setup() {
pinMode(buzzer, OUTPUT);
Serial.begin(9600); // 通訊 baud 值
SPI.begin();
mfrc522.PCD_Init(); // 初始化 MFRC522 讀卡機模組
Serial.print(F("Reader ")); // 印出文字
Serial.print(F(": ")); // 印出文字
mfrc522.PCD_DumpVersionToSerial(); // 顯示讀卡設備的版本
}

void loop() {
if (mfrc522.PICC_IsNewCardPresent() && mfrc522.PICC_ReadCardSerial()) {
  // 檢查是否讀到卡片
  // 顯示卡片內容
  Serial.print(F("Card UID:")); // 印出'Card UID:'
  byte *id = mfrc522.uid.uidByte; // 取得卡片的UID
  byte idSize = mfrc522.uid.size; // 取得UID的長度
  Serial.println();
  for (byte i = 0; i < idSize; i++){
  Serial.print(id[i]); // 印出卡片UID
  Serial.print(",");
  }

  Serial.println(); // 換行
  byte Card[4] = {55,227,194,26}; // 正確的卡片UID
  byte Cord[4] = {113,216,200,39}; // 正確的卡片UID
  byte Cerd[4] = {161,51,75,31}; // 正確的卡片UID
  for (byte i = 0; i < 4; i++){
  if (id[i] != Card[i] && id[i]!=Cord[i]&& id[i]!=Cerd[i]){ // 判斷卡片UID符合
    Serial.print("拒絕進入"); // 拒絕進入
    Serial.println(); // 換行
    for(int j=900;j<=10000;j++)
    tone(buzzer, j);
    delay(10);
    noTone(buzzer);
    for(int j=900;j<=10000;j++)
    tone(buzzer, j);
    delay(10);
    noTone(buzzer);
    for(int j=900;j<=10000;j++)
    tone(buzzer, j);
    delay(10);
    noTone(buzzer);
    for(int j=900;j<=10000;j++)
    tone(buzzer, j);
    delay(10);
    noTone(buzzer);
    break;
    }


  if (i == 3){
    Serial.print("歡迎回來"); // 歡迎回來
    Serial.println(); // 換行
    
    tone(buzzer, 2000);
    delay(100);
    noTone(buzzer);
    
    }
  }
  mfrc522.PICC_HaltA(); // 讓卡片進入停止模式
  }
}

```


---

## 溫溼度計
https://youtu.be/BVxNymGERNY
```arduino=
#include <DHT.h> 
#include <LiquidCrystal_I2C.h>
#define pin 5

LiquidCrystal_I2C lcd(0x27,16,2);    //(型號,16行,2列)
DHT dht (pin, DHT11); // 宣告物件
void setup() {
  Serial.begin(9600);  // 初始化序列埠
  dht.begin();
  lcd.init(); 
  lcd.backlight(); 
}
void loop() {

  lcd.clear(); 
  lcd.setCursor(0,0);
  lcd.print("Temp: ");
  lcd.print(dht.readTemperature());
  lcd.print(" C   ");
  lcd.setCursor(0,1);
  lcd.print("Humi: ");
  lcd.print(dht.readHumidity());
  lcd.print(" %");
  
  Serial.print(dht.readTemperature());
  Serial.print(" , ");    
  Serial.println(dht.readHumidity());

  delay(2000); // 每2秒讀一次
}


```


---

## 測距器
https://youtu.be/WHfEapgjuvE
```arduino=
#define trigPin 12
#define echoPin 11
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);

double ultrasonic() {
digitalWrite(trigPin, LOW);
delay(2);
digitalWrite(trigPin, HIGH);
delay(10);
digitalWrite(trigPin, LOW);
double duration = pulseIn(echoPin, HIGH);  
double distance= duration*0.034/2;
return distance;
}
void setup() {
lcd.init();
lcd.backlight();
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
Serial.begin(9600);
}

void loop() {
  lcd.clear();
double dist = ultrasonic();


lcd.print("   Distance : ");
lcd.setCursor(4,1);
lcd.print(dist);
lcd.print(" cm");

Serial.print("Distance: ");
Serial.print(dist);
Serial.println(" cm");
delay(1000);
}
```


---

## 伺服馬達調整器
https://youtu.be/zDBcNvZXDsU
```arduino=
#include <Servo.h>
Servo servo; //建立一個servo

void setup() {
  pinMode(A0,INPUT); //設定A0為輸入腳位
  servo.attach(9); //將servo物件連接到pin 9
  Serial.begin(9600); //設定傳輸速度
}


void loop() {
  float angle=analogRead(A0); //讀取A0腳位的值(0-1023)
  angle=(angle/1023*180)+1; 
  servo.write(angle); //讓servo轉到指定角度
  Serial.println(angle); //可透過序列埠視窗監控角度值
  delay(100); //延遲0.1秒讓servo轉到指定角度
}
```
--------------------------------------
## 溫控開關
```arduino=
#include <DHT.h> 
#include <LiquidCrystal_I2C.h>
#define pin 5

LiquidCrystal_I2C lcd(0x27,16,2);    //(型號,16行,2列)
DHT dht (pin, DHT11); // 宣告物件
void setup() {
  Serial.begin(9600);  // 初始化序列埠
  dht.begin();
  lcd.init(); 
  lcd.backlight();
  pinMode(13, OUTPUT);
}
void loop() {
  
  float t=dht.readTemperature(),settemp,n;
  n=analogRead(A0);
  settemp=n/1023*25+25;
  if(t>settemp){
    digitalWrite(13, HIGH);
  }
  else{
    digitalWrite(13, LOW);
  }
  lcd.clear(); 
  lcd.setCursor(0,0);
  lcd.print("Now : ");
  lcd.print(dht.readTemperature());
  lcd.print(" C   ");
  lcd.setCursor(0,1);
  lcd.print("Set : ");
  lcd.print(settemp);
  lcd.print(" C");
  
  Serial.print(dht.readTemperature());
  Serial.print(" , ");    
  Serial.println(dht.readHumidity());

  delay(1000); // 每2秒讀一次
}





//-----------------------------------------------------------------------





#include <DHT.h> 
#include <LiquidCrystal_I2C.h>
#define pin 5

float h=10.0,l=99.9;
LiquidCrystal_I2C lcd(0x27,16,2);
DHT dht (pin, DHT11); 

void setup() {
  Serial.begin(9600);  
  dht.begin();
  lcd.init(); 
  lcd.backlight();
  pinMode(13, OUTPUT);

  
  
}

void loop() {
  
  float t=dht.readTemperature(),settemp,n;
  n=analogRead(A0);
  settemp=n/1023*25+25;

  h=max(h,t);
  l=min(l,t);
  
  if(t>settemp){
    digitalWrite(13, HIGH);
  }
  else{
    digitalWrite(13, LOW);
  }
  

  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("n: ");
  lcd.print(t);
  lcd.setCursor(7,0);
  lcd.print(" s: ");
  lcd.print(settemp);
  lcd.setCursor(15,0);
  lcd.print(" ");
  //--------------------
  lcd.setCursor(0,1);
  lcd.print("h: ");
  lcd.print(h);
  lcd.setCursor(7,1);
  lcd.print(" l: ");
  lcd.print(l);
  lcd.setCursor(15,1);
  lcd.print(" ");
  //---------------
  Serial.println("Temp  | Humi  |  Set  | High  |  Low  ");    
  Serial.println("------------------------------------");
  Serial.print(dht.readTemperature());
  Serial.print(" | ");    
  Serial.print(dht.readHumidity());
  Serial.print(" | ");
  Serial.print(settemp);
  Serial.print(" | ");
  Serial.print(h);
  Serial.print(" | ");
  Serial.println(l);
  Serial.println("------------------------------------");
  Serial.println("  ");
  
  delay(1000); // 每2秒讀一次
}












//-------------------------------
//OLED____________________-
#include <SPI.h> 
#include <Wire.h> 
#include <Adafruit_GFX.h> 
#include <Adafruit_SSD1306.h> 
#include <DHT.h> 
//#include <LiquidCrystal_I2C.h>
#define pin 5

#define SCREEN_WIDTH 128 // OLED 寬度像素
#define SCREEN_HEIGHT 64 // OLED 高度像素

float h=10.0,l=99.9;
//LiquidCrystal_I2C lcd(0x27,16,2);
DHT dht (pin, DHT11); 

// 設定OLED
#define OLED_RESET     -1 // Reset pin # (or -1 if sharing Arduino reset pin)
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);


void testdrawstyles() {

  
  float t=dht.readTemperature(),settemp,n;
  n=analogRead(A0);
  settemp=n/1023*25+25;

  h=max(h,t);
  l=min(l,t);


   if(t>settemp){
    digitalWrite(13, HIGH);
  }
  else{
    digitalWrite(13, LOW);
  }

  
  display.clearDisplay();
  display.setTextSize(2);          
  display.setTextColor(1);         
  display.setCursor(0,0);
  display.print("now :");         
  display.print(t);     
  display.setCursor(0,16); 
  display.print("set :");       
  display.print(settemp);       
  display.setCursor(0,32); 
  display.print("high:");      
  display.print(h);        
  display.setCursor(0,48); 
  display.print("low :");      
  display.print(l); 
  display.display();              
  delay(1000);
}




void setup() {
  Serial.begin(9600);
  dht.begin();
  //lcd.init(); 
  //lcd.backlight();
  pinMode(13, OUTPUT);
  
  // 偵測是否安裝好OLED了
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { // 一般1306 OLED的位址都是0x3C
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  }

  display.clearDisplay(); // 清除畫面
  delay(1000);
}

void loop() {
  

  
 
  testdrawstyles();    // 測試文字
  
}



//---------------------------------------





#include <SPI.h> 
#include <Wire.h> 
#include <Adafruit_GFX.h> 
#include <Adafruit_SSD1306.h> 
#include <DHT.h> 
#define pin 5
#define SCREEN_WIDTH 128 // OLED 寬度像素
#define SCREEN_HEIGHT 64 // OLED 高度像素
float h=10.0,l=99.9;
int s=0;
DHT dht (pin, DHT11); 
#define OLED_RESET     -1 // Reset pin # (or -1 if sharing Arduino reset pin)
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void test() {

  float t=dht.readTemperature();
  h=max(h,t);
  l=min(l,t);
  if(t>=43){
    digitalWrite(13, HIGH);
  }
  else if(t<40){
    digitalWrite(13, LOW);
  }
  
  if(s>=3600)
      digitalWrite(3, HIGH);
      
  
  
  display.clearDisplay();
  display.setTextSize(2);          
  display.setTextColor(1);         
  display.setCursor(0,0);
  display.print("now :");         
  display.print(t);     
  display.setCursor(0,16); 
  display.print("high:");       
  display.print(h);       
  display.setCursor(0,32); 
  display.print("low :");      
  display.print(l);
  display.setCursor(0,48);
  display.print(s/(3600*24));
  display.print(":");
  display.print((s/3600)%24);
  display.print(":"); 
  display.print((s/60)%60);
  display.print(":");     
  display.print(s%60); 
  display.display();              
  delay(925.78);
  s++;
}




void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(13, OUTPUT);
  pinMode(3, OUTPUT);
  digitalWrite(3, LOW);
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { 
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  }
  display.clearDisplay(); 
  
}
void loop() {
  test();  
  } 
```


## AHT10xMicroSD
```arduino=
/*  Lab15 SD記憶卡 程式碼*/
/*SD卡
CS   --> to Arduino pin4 
SCK  --> to Arduino pin13
MOSI --> to Arduino pin11
MISO --> to Arduino pin12
VCC  --> to Arduino 5V
GND  --> to Arduino GND
*/
double maxt=0,mint=1000,maxh=0,minh=1000;

//#include <SPI.h>
#include <Wire.h>
#include <AHT10.h>

//-----------------------------------------------------

uint8_t readStatus = 0;
AHT10 myAHT10(AHT10_ADDRESS_0X38);
const uint64_t pipe = 0x00FF; // 設定發送端 pipe 名稱
 
#include <SPI.h>
#include <SD.h>
File myFile;
String filename = "test.csv";     //要寫入的檔案名稱

void setup() {
  pinMode(10,OUTPUT);             //保留pin10, SD Library需要使用
  pinMode(7,OUTPUT);
  pinMode(2,OUTPUT);
  digitalWrite(7, LOW);
  while (!SD.begin(4)) {}
  Serial.begin(9600);
  //-----------------
  //Serial.begin(9600);
  while (myAHT10.begin() != true)
  {
    Serial.println(F("AHT10 not connected...")); 
    delay(5000);
  }
  Serial.println(F("AHT10 initial OK"));
}

void loop()
{
    double t = myAHT10.readTemperature();    //讀取 AHT10 溫度
    double h = myAHT10.readHumidity();   //讀取 AHT10 濕度
    maxt=max(t,maxt);
    mint=min(t,mint);
    maxh=max(h,maxh);
    minh=min(t,minh);
    Serial.print("Temperatura: ");
    Serial.print(t);
    Serial.print("   Humidity: ");
    Serial.println(h);

    //-------------------------------------

  
  
  myFile = SD.open(filename, FILE_WRITE);
  if (myFile) {
    digitalWrite(2, HIGH);
    Serial.print("Writing to test.txt...");
    myFile.print(t);           //寫入時間 
    myFile.print(",");                  //之後每個數據之間加入逗號","
    myFile.print(h);         //要寫偵測數據時，這一行請換掉
    myFile.println();
    myFile.close();                     //關閉檔案
    Serial.println("done.");
  } else {
    Serial.println("error opening test.txt");
    digitalWrite(7, HIGH);
    

  }
  delay(60000);
  digitalWrite(2, LOW);
}
```
