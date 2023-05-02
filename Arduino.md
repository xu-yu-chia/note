# Arduino 元件應用


## Buzzer

:::spoiler 

```Arduino=
#define tonepin 9
viod setup(){
    pinMode(tonepin,OUTPUT);
}
void loop(){
    Tone(tonepin,500);
    delay(500);
    noTone(tonepin);
}
```
:::


## 遊戲搖桿模組

:::spoiler 

```arduino=
int xposPin = A0;         // 雙軸按鍵搖桿 VRx 接 Arduino Analog pin A0
int yposPin = A1;         // 雙軸按鍵搖桿 VRy 接 Arduino Analog pin A1
int Xpos = 0;             // 定義X軸伺服器位址參數
int Ypos = 0;             // 定義丫軸伺服器位址參數
int buttonPin = 7;        // 搖桿按鍵輸出 SW 接 Arduino pin 7
int buttonPress = 0;      // 定義 Arduino 從搖桿按鍵 SW 讀入值為 buttonPress 

void setup() {
  pinMode(buttonPin,INPUT);      //Arduino 從搖桿按鍵讀入電壓
  digitalWrite(buttonPin,HIGH);  //設定當按鍵沒按時，輸出為高電壓
 
}

void loop () {
  Xpos = analogRead(xposPin);            // 讀入搖桿 x 軸數值，0-1023
  Ypos = analogRead(yposPin);            //讀入搖桿 y 軸 數值，0-1023
}
```

:::

## BMP180

:::spoiler 

```arduino=
#include <Adafruit_BMP085.h>

/*************************************************** 
  This is an example for the BMP085 Barometric Pressure & Temp Sensor

  Designed specifically to work with the Adafruit BMP085 Breakout 
  ----> https://www.adafruit.com/products/391

  These pressure and temperature sensors use I2C to communicate, 2 pins
  are required to interface
  Adafruit invests time and resources providing this open source code, 
  please support Adafruit and open-source hardware by purchasing 
  products from Adafruit!

  Written by Limor Fried/Ladyada for Adafruit Industries.  
  BSD license, all text above must be included in any redistribution
 ****************************************************/

// Connect VCC of the BMP085 sensor to 3.3V (NOT 5.0V!)
// Connect GND to Ground
// Connect SCL to i2c clock - on '168/'328 Arduino Uno/Duemilanove/etc thats Analog 5
// Connect SDA to i2c data - on '168/'328 Arduino Uno/Duemilanove/etc thats Analog 4
// EOC is not used, it signifies an end of conversion
// XCLR is a reset pin, also not used here

Adafruit_BMP085 bmp;
  
void setup() {
  Serial.begin(9600);
  if (!bmp.begin()) {
	Serial.println("Could not find a valid BMP085 sensor, check wiring!");
	while (1) {}
  }
}
  
void loop() {
    Serial.print("Temperature = ");
    Serial.print(bmp.readTemperature());
    Serial.println(" *C");
    
    Serial.print("Pressure = ");
    Serial.print(bmp.readPressure());
    Serial.println(" Pa");
    
    // Calculate altitude assuming 'standard' barometric
    // pressure of 1013.25 millibar = 101325 Pascal
    Serial.print("Altitude = ");
    Serial.print(bmp.readAltitude());
    Serial.println(" meters");

    Serial.print("Pressure at sealevel (calculated) = ");
    Serial.print(bmp.readSealevelPressure());
    Serial.println(" Pa");

  // you can get a more precise measurement of altitude
  // if you know the current sea level pressure which will
  // vary with weather and such. If it is 1015 millibars
  // that is equal to 101500 Pascals.
    Serial.print("Real altitude = ");
    Serial.print(bmp.readAltitude(101500));
    Serial.println(" meters");
    
    Serial.println();
    delay(500);
}
```

:::

## I2C Scaner

:::spoiler 

```arduino=
#include "Wire.h"
 
void setup()
{
  Wire.begin();
  Serial.begin(9600);
  while (!Serial);             // Leonardo: wait for serial monitor
  Serial.println("nI2C Scanner");
}
 
void loop()
{
  byte error, address;
  int nDevices;
  Serial.println("Scanning...");
  nDevices = 0;
  for(address = 1; address < 127; address++ )
  {
    // The i2c_scanner uses the return value of
    // the Write.endTransmisstion to see if
    // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
 
    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
      nDevices++;
    }
    else if (error==4)
    {
      Serial.print("Unknow error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }    
  }
  if (nDevices == 0)
    Serial.println("No I2C devices foundn");
  else
    Serial.println("donen");
  delay(5000);           // wait 5 seconds for next scan
}
 

```

:::

## OLED-4(範例程式碼)

:::spoiler 

```arduino=
/**************************************************************************
 This is an example for our Monochrome OLEDs based on SSD1306 drivers
 Pick one up today in the adafruit shop!
 ------> http://www.adafruit.com/category/63_98
 This example is for a 128x64 pixel display using I2C to communicate
 3 pins are required to interface (two I2C and one reset).
 Adafruit invests time and resources providing this open
 source code, please support Adafruit and open-source
 hardware by purchasing products from Adafruit!
 Written by Limor Fried/Ladyada for Adafruit Industries,
 with contributions from the open source community.
 BSD license, check license.txt for more information
 All text above, and the splash screen below must be
 included in any redistribution.
 **************************************************************************/

#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels

// Declaration for an SSD1306 display connected to I2C (SDA, SCL pins)
// The pins for I2C are defined by the Wire-library. 
// On an arduino UNO:       A4(SDA), A5(SCL)
// On an arduino MEGA 2560: 20(SDA), 21(SCL)
// On an arduino LEONARDO:   2(SDA),  3(SCL), ...
#define OLED_RESET     -1 // Reset pin # (or -1 if sharing Arduino reset pin)
#define SCREEN_ADDRESS 0x3D ///< See datasheet for Address; 0x3D for 128x64, 0x3C for 128x32
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

#define NUMFLAKES     10 // Number of snowflakes in the animation example

#define LOGO_HEIGHT   16
#define LOGO_WIDTH    16
static const unsigned char PROGMEM logo_bmp[] =
{ 0b00000000, 0b11000000,
  0b00000001, 0b11000000,
  0b00000001, 0b11000000,
  0b00000011, 0b11100000,
  0b11110011, 0b11100000,
  0b11111110, 0b11111000,
  0b01111110, 0b11111111,
  0b00110011, 0b10011111,
  0b00011111, 0b11111100,
  0b00001101, 0b01110000,
  0b00011011, 0b10100000,
  0b00111111, 0b11100000,
  0b00111111, 0b11110000,
  0b01111100, 0b11110000,
  0b01110000, 0b01110000,
  0b00000000, 0b00110000 };

void setup() {
  Serial.begin(9600);

  // SSD1306_SWITCHCAPVCC = generate display voltage from 3.3V internally
  if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  }

  // Show initial display buffer contents on the screen --
  // the library initializes this with an Adafruit splash screen.
  display.display();
  delay(2000); // Pause for 2 seconds

  // Clear the buffer
  display.clearDisplay();

  // Draw a single pixel in white
  display.drawPixel(10, 10, SSD1306_WHITE);

  // Show the display buffer on the screen. You MUST call display() after
  // drawing commands to make them visible on screen!
  display.display();
  delay(2000);
  // display.display() is NOT necessary after every single drawing command,
  // unless that's what you want...rather, you can batch up a bunch of
  // drawing operations and then update the screen all at once by calling
  // display.display(). These examples demonstrate both approaches...

  testdrawline();      // Draw many lines

  testdrawrect();      // Draw rectangles (outlines)

  testfillrect();      // Draw rectangles (filled)

  testdrawcircle();    // Draw circles (outlines)

  testfillcircle();    // Draw circles (filled)

  testdrawroundrect(); // Draw rounded rectangles (outlines)

  testfillroundrect(); // Draw rounded rectangles (filled)

  testdrawtriangle();  // Draw triangles (outlines)

  testfilltriangle();  // Draw triangles (filled)

  testdrawchar();      // Draw characters of the default font

  testdrawstyles();    // Draw 'stylized' characters

  testscrolltext();    // Draw scrolling text

  testdrawbitmap();    // Draw a small bitmap image

  // Invert and restore display, pausing in-between
  display.invertDisplay(true);
  delay(1000);
  display.invertDisplay(false);
  delay(1000);

  testanimate(logo_bmp, LOGO_WIDTH, LOGO_HEIGHT); // Animate bitmaps
}

void loop() {
}

void testdrawline() {
  int16_t i;

  display.clearDisplay(); // Clear display buffer

  for(i=0; i<display.width(); i+=4) {
    display.drawLine(0, 0, i, display.height()-1, SSD1306_WHITE);
    display.display(); // Update screen with each newly-drawn line
    delay(1);
  }
  for(i=0; i<display.height(); i+=4) {
    display.drawLine(0, 0, display.width()-1, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);

  display.clearDisplay();

  for(i=0; i<display.width(); i+=4) {
    display.drawLine(0, display.height()-1, i, 0, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(i=display.height()-1; i>=0; i-=4) {
    display.drawLine(0, display.height()-1, display.width()-1, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);

  display.clearDisplay();

  for(i=display.width()-1; i>=0; i-=4) {
    display.drawLine(display.width()-1, display.height()-1, i, 0, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(i=display.height()-1; i>=0; i-=4) {
    display.drawLine(display.width()-1, display.height()-1, 0, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);

  display.clearDisplay();

  for(i=0; i<display.height(); i+=4) {
    display.drawLine(display.width()-1, 0, 0, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(i=0; i<display.width(); i+=4) {
    display.drawLine(display.width()-1, 0, i, display.height()-1, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000); // Pause for 2 seconds
}

void testdrawrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2; i+=2) {
    display.drawRect(i, i, display.width()-2*i, display.height()-2*i, SSD1306_WHITE);
    display.display(); // Update screen with each newly-drawn rectangle
    delay(1);
  }

  delay(2000);
}

void testfillrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2; i+=3) {
    // The INVERSE color is used so rectangles alternate white/black
    display.fillRect(i, i, display.width()-i*2, display.height()-i*2, SSD1306_INVERSE);
    display.display(); // Update screen with each newly-drawn rectangle
    delay(1);
  }

  delay(2000);
}

void testdrawcircle(void) {
  display.clearDisplay();

  for(int16_t i=0; i<max(display.width(),display.height())/2; i+=2) {
    display.drawCircle(display.width()/2, display.height()/2, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testfillcircle(void) {
  display.clearDisplay();

  for(int16_t i=max(display.width(),display.height())/2; i>0; i-=3) {
    // The INVERSE color is used so circles alternate white/black
    display.fillCircle(display.width() / 2, display.height() / 2, i, SSD1306_INVERSE);
    display.display(); // Update screen with each newly-drawn circle
    delay(1);
  }

  delay(2000);
}

void testdrawroundrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2-2; i+=2) {
    display.drawRoundRect(i, i, display.width()-2*i, display.height()-2*i,
      display.height()/4, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testfillroundrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2-2; i+=2) {
    // The INVERSE color is used so round-rects alternate white/black
    display.fillRoundRect(i, i, display.width()-2*i, display.height()-2*i,
      display.height()/4, SSD1306_INVERSE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testdrawtriangle(void) {
  display.clearDisplay();

  for(int16_t i=0; i<max(display.width(),display.height())/2; i+=5) {
    display.drawTriangle(
      display.width()/2  , display.height()/2-i,
      display.width()/2-i, display.height()/2+i,
      display.width()/2+i, display.height()/2+i, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testfilltriangle(void) {
  display.clearDisplay();

  for(int16_t i=max(display.width(),display.height())/2; i>0; i-=5) {
    // The INVERSE color is used so triangles alternate white/black
    display.fillTriangle(
      display.width()/2  , display.height()/2-i,
      display.width()/2-i, display.height()/2+i,
      display.width()/2+i, display.height()/2+i, SSD1306_INVERSE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testdrawchar(void) {
  display.clearDisplay();

  display.setTextSize(1);      // Normal 1:1 pixel scale
  display.setTextColor(SSD1306_WHITE); // Draw white text
  display.setCursor(0, 0);     // Start at top-left corner
  display.cp437(true);         // Use full 256 char 'Code Page 437' font

  // Not all the characters will fit on the display. This is normal.
  // Library will draw what it can and the rest will be clipped.
  for(int16_t i=0; i<256; i++) {
    if(i == '\n') display.write(' ');
    else          display.write(i);
  }

  display.display();
  delay(2000);
}

void testdrawstyles(void) {
  display.clearDisplay();

  display.setTextSize(1);             // Normal 1:1 pixel scale
  display.setTextColor(SSD1306_WHITE);        // Draw white text
  display.setCursor(0,0);             // Start at top-left corner
  display.println(F("Hello, world!"));

  display.setTextColor(SSD1306_BLACK, SSD1306_WHITE); // Draw 'inverse' text
  display.println(3.141592);

  display.setTextSize(2);             // Draw 2X-scale text
  display.setTextColor(SSD1306_WHITE);
  display.print(F("0x")); display.println(0xDEADBEEF, HEX);

  display.display();
  delay(2000);
}

void testscrolltext(void) {
  display.clearDisplay();

  display.setTextSize(2); // Draw 2X-scale text
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(10, 0);
  display.println(F("scroll"));
  display.display();      // Show initial text
  delay(100);

  // Scroll in various directions, pausing in-between:
  display.startscrollright(0x00, 0x0F);
  delay(2000);
  display.stopscroll();
  delay(1000);
  display.startscrollleft(0x00, 0x0F);
  delay(2000);
  display.stopscroll();
  delay(1000);
  display.startscrolldiagright(0x00, 0x07);
  delay(2000);
  display.startscrolldiagleft(0x00, 0x07);
  delay(2000);
  display.stopscroll();
  delay(1000);
}

void testdrawbitmap(void) {
  display.clearDisplay();

  display.drawBitmap(
    (display.width()  - LOGO_WIDTH ) / 2,
    (display.height() - LOGO_HEIGHT) / 2,
    logo_bmp, LOGO_WIDTH, LOGO_HEIGHT, 1);
  display.display();
  delay(1000);
}

#define XPOS   0 // Indexes into the 'icons' array in function below
#define YPOS   1
#define DELTAY 2

void testanimate(const uint8_t *bitmap, uint8_t w, uint8_t h) {
  int8_t f, icons[NUMFLAKES][3];

  // Initialize 'snowflake' positions
  for(f=0; f< NUMFLAKES; f++) {
    icons[f][XPOS]   = random(1 - LOGO_WIDTH, display.width());
    icons[f][YPOS]   = -LOGO_HEIGHT;
    icons[f][DELTAY] = random(1, 6);
    Serial.print(F("x: "));
    Serial.print(icons[f][XPOS], DEC);
    Serial.print(F(" y: "));
    Serial.print(icons[f][YPOS], DEC);
    Serial.print(F(" dy: "));
    Serial.println(icons[f][DELTAY], DEC);
  }

  for(;;) { // Loop forever...
    display.clearDisplay(); // Clear the display buffer

    // Draw each snowflake:
    for(f=0; f< NUMFLAKES; f++) {
      display.drawBitmap(icons[f][XPOS], icons[f][YPOS], bitmap, w, h, SSD1306_WHITE);
    }

    display.display(); // Show the display buffer on the screen
    delay(200);        // Pause for 1/10 second

    // Then update coordinates of each flake...
    for(f=0; f< NUMFLAKES; f++) {
      icons[f][YPOS] += icons[f][DELTAY];
      // If snowflake is off the bottom of the screen...
      if (icons[f][YPOS] >= display.height()) {
        // Reinitialize to a random position, just off the top
        icons[f][XPOS]   = random(1 - LOGO_WIDTH, display.width());
        icons[f][YPOS]   = -LOGO_HEIGHT;
        icons[f][DELTAY] = random(1, 6);
      }
    }
  }
}
```

:::

## OLED-7(範例程式碼)

:::spoiler 

```arduino=
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels

// Declaration for SSD1306 display connected using software SPI (default case):
#define OLED_MOSI   9
#define OLED_CLK   10
#define OLED_DC    11
#define OLED_CS    12
#define OLED_RESET 13
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
  OLED_MOSI, OLED_CLK, OLED_DC, OLED_RESET, OLED_CS);

/* Comment out above, uncomment this block to use hardware SPI
#define OLED_DC     6
#define OLED_CS     7
#define OLED_RESET  8
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
  &SPI, OLED_DC, OLED_RESET, OLED_CS);
*/

#define NUMFLAKES     10 // Number of snowflakes in the animation example

#define LOGO_HEIGHT   16
#define LOGO_WIDTH    16
static const unsigned char PROGMEM logo_bmp[] =
{ 0b00000000, 0b11000000,
  0b00000001, 0b11000000,
  0b00000001, 0b11000000,
  0b00000011, 0b11100000,
  0b11110011, 0b11100000,
  0b11111110, 0b11111000,
  0b01111110, 0b11111111,
  0b00110011, 0b10011111,
  0b00011111, 0b11111100,
  0b00001101, 0b01110000,
  0b00011011, 0b10100000,
  0b00111111, 0b11100000,
  0b00111111, 0b11110000,
  0b01111100, 0b11110000,
  0b01110000, 0b01110000,
  0b00000000, 0b00110000 };

void setup() {
  Serial.begin(9600);

  // SSD1306_SWITCHCAPVCC = generate display voltage from 3.3V internally
  if(!display.begin(SSD1306_SWITCHCAPVCC)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  }


  // Show initial display buffer contents on the screen --
  // the library initializes this with an Adafruit splash screen.
  display.display();
  delay(2000); // Pause for 2 seconds

  // Clear the buffer
  display.clearDisplay();

  // Draw a single pixel in white
  display.drawPixel(10, 10, SSD1306_WHITE);

  // Show the display buffer on the screen. You MUST call display() after
  // drawing commands to make them visible on screen!
  display.display();
  delay(2000);
  // display.display() is NOT necessary after every single drawing command,
  // unless that's what you want...rather, you can batch up a bunch of
  // drawing operations and then update the screen all at once by calling
  // display.display(). These examples demonstrate both approaches...

  testdrawline();      // Draw many lines

  testdrawrect();      // Draw rectangles (outlines)

  testfillrect();      // Draw rectangles (filled)

  testdrawcircle();    // Draw circles (outlines)

  testfillcircle();    // Draw circles (filled)

  testdrawroundrect(); // Draw rounded rectangles (outlines)

  testfillroundrect(); // Draw rounded rectangles (filled)

  testdrawtriangle();  // Draw triangles (outlines)

  testfilltriangle();  // Draw triangles (filled)

  testdrawchar();      // Draw characters of the default font

  testdrawstyles();    // Draw 'stylized' characters

  testscrolltext();    // Draw scrolling text

  testdrawbitmap();    // Draw a small bitmap image

  // Invert and restore display, pausing in-between
  display.invertDisplay(true);
  delay(1000);
  display.invertDisplay(false);
  delay(1000);

  testanimate(logo_bmp, LOGO_WIDTH, LOGO_HEIGHT); // Animate bitmaps
}

void loop() {
}

void testdrawline() {
  int16_t i;

  display.clearDisplay(); // Clear display buffer

  for(i=0; i<display.width(); i+=4) {
    display.drawLine(0, 0, i, display.height()-1, SSD1306_WHITE);
    display.display(); // Update screen with each newly-drawn line
    delay(1);
  }
  for(i=0; i<display.height(); i+=4) {
    display.drawLine(0, 0, display.width()-1, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);

  display.clearDisplay();

  for(i=0; i<display.width(); i+=4) {
    display.drawLine(0, display.height()-1, i, 0, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(i=display.height()-1; i>=0; i-=4) {
    display.drawLine(0, display.height()-1, display.width()-1, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);

  display.clearDisplay();

  for(i=display.width()-1; i>=0; i-=4) {
    display.drawLine(display.width()-1, display.height()-1, i, 0, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(i=display.height()-1; i>=0; i-=4) {
    display.drawLine(display.width()-1, display.height()-1, 0, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);

  display.clearDisplay();

  for(i=0; i<display.height(); i+=4) {
    display.drawLine(display.width()-1, 0, 0, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(i=0; i<display.width(); i+=4) {
    display.drawLine(display.width()-1, 0, i, display.height()-1, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000); // Pause for 2 seconds
}

void testdrawrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2; i+=2) {
    display.drawRect(i, i, display.width()-2*i, display.height()-2*i, SSD1306_WHITE);
    display.display(); // Update screen with each newly-drawn rectangle
    delay(1);
  }

  delay(2000);
}

void testfillrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2; i+=3) {
    // The INVERSE color is used so rectangles alternate white/black
    display.fillRect(i, i, display.width()-i*2, display.height()-i*2, SSD1306_INVERSE);
    display.display(); // Update screen with each newly-drawn rectangle
    delay(1);
  }

  delay(2000);
}

void testdrawcircle(void) {
  display.clearDisplay();

  for(int16_t i=0; i<max(display.width(),display.height())/2; i+=2) {
    display.drawCircle(display.width()/2, display.height()/2, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testfillcircle(void) {
  display.clearDisplay();

  for(int16_t i=max(display.width(),display.height())/2; i>0; i-=3) {
    // The INVERSE color is used so circles alternate white/black
    display.fillCircle(display.width() / 2, display.height() / 2, i, SSD1306_INVERSE);
    display.display(); // Update screen with each newly-drawn circle
    delay(1);
  }

  delay(2000);
}

void testdrawroundrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2-2; i+=2) {
    display.drawRoundRect(i, i, display.width()-2*i, display.height()-2*i,
      display.height()/4, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testfillroundrect(void) {
  display.clearDisplay();

  for(int16_t i=0; i<display.height()/2-2; i+=2) {
    // The INVERSE color is used so round-rects alternate white/black
    display.fillRoundRect(i, i, display.width()-2*i, display.height()-2*i,
      display.height()/4, SSD1306_INVERSE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testdrawtriangle(void) {
  display.clearDisplay();

  for(int16_t i=0; i<max(display.width(),display.height())/2; i+=5) {
    display.drawTriangle(
      display.width()/2  , display.height()/2-i,
      display.width()/2-i, display.height()/2+i,
      display.width()/2+i, display.height()/2+i, SSD1306_WHITE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testfilltriangle(void) {
  display.clearDisplay();

  for(int16_t i=max(display.width(),display.height())/2; i>0; i-=5) {
    // The INVERSE color is used so triangles alternate white/black
    display.fillTriangle(
      display.width()/2  , display.height()/2-i,
      display.width()/2-i, display.height()/2+i,
      display.width()/2+i, display.height()/2+i, SSD1306_INVERSE);
    display.display();
    delay(1);
  }

  delay(2000);
}

void testdrawchar(void) {
  display.clearDisplay();

  display.setTextSize(1);      // Normal 1:1 pixel scale
  display.setTextColor(SSD1306_WHITE); // Draw white text
  display.setCursor(0, 0);     // Start at top-left corner
  display.cp437(true);         // Use full 256 char 'Code Page 437' font

  // Not all the characters will fit on the display. This is normal.
  // Library will draw what it can and the rest will be clipped.
  for(int16_t i=0; i<256; i++) {
    if(i == '\n') display.write(' ');
    else          display.write(i);
  }

  display.display();
  delay(2000);
}

void testdrawstyles(void) {
  display.clearDisplay();

  display.setTextSize(1);             // Normal 1:1 pixel scale
  display.setTextColor(SSD1306_WHITE);        // Draw white text
  display.setCursor(0,0);             // Start at top-left corner
  display.println(F("Hello, world!"));

  display.setTextColor(SSD1306_BLACK, SSD1306_WHITE); // Draw 'inverse' text
  display.println(3.141592);

  display.setTextSize(2);             // Draw 2X-scale text
  display.setTextColor(SSD1306_WHITE);
  display.print(F("0x")); display.println(0xDEADBEEF, HEX);

  display.display();
  delay(2000);
}

void testscrolltext(void) {
  display.clearDisplay();

  display.setTextSize(2); // Draw 2X-scale text
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(10, 0);
  display.println(F("scroll"));
  display.display();      // Show initial text
  delay(100);

  // Scroll in various directions, pausing in-between:
  display.startscrollright(0x00, 0x0F);
  delay(2000);
  display.stopscroll();
  delay(1000);
  display.startscrollleft(0x00, 0x0F);
  delay(2000);
  display.stopscroll();
  delay(1000);
  display.startscrolldiagright(0x00, 0x07);
  delay(2000);
  display.startscrolldiagleft(0x00, 0x07);
  delay(2000);
  display.stopscroll();
  delay(1000);
}

void testdrawbitmap(void) {
  display.clearDisplay();

  display.drawBitmap(
    (display.width()  - LOGO_WIDTH ) / 2,
    (display.height() - LOGO_HEIGHT) / 2,
    logo_bmp, LOGO_WIDTH, LOGO_HEIGHT, 1);
  display.display();
  delay(1000);
}

#define XPOS   0 // Indexes into the 'icons' array in function below
#define YPOS   1
#define DELTAY 2

void testanimate(const uint8_t *bitmap, uint8_t w, uint8_t h) {
  int8_t f, icons[NUMFLAKES][3];

  // Initialize 'snowflake' positions
  for(f=0; f< NUMFLAKES; f++) {
    icons[f][XPOS]   = random(1 - LOGO_WIDTH, display.width());
    icons[f][YPOS]   = -LOGO_HEIGHT;
    icons[f][DELTAY] = random(1, 6);
    Serial.print(F("x: "));
    Serial.print(icons[f][XPOS], DEC);
    Serial.print(F(" y: "));
    Serial.print(icons[f][YPOS], DEC);
    Serial.print(F(" dy: "));
    Serial.println(icons[f][DELTAY], DEC);
  }

  for(;;) { // Loop forever...
    display.clearDisplay(); // Clear the display buffer

    // Draw each snowflake:
    for(f=0; f< NUMFLAKES; f++) {
      display.drawBitmap(icons[f][XPOS], icons[f][YPOS], bitmap, w, h, SSD1306_WHITE);
    }

    display.display(); // Show the display buffer on the screen
    delay(200);        // Pause for 1/10 second

    // Then update coordinates of each flake...
    for(f=0; f< NUMFLAKES; f++) {
      icons[f][YPOS] += icons[f][DELTAY];
      // If snowflake is off the bottom of the screen...
      if (icons[f][YPOS] >= display.height()) {
        // Reinitialize to a random position, just off the top
        icons[f][XPOS]   = random(1 - LOGO_WIDTH, display.width());
        icons[f][YPOS]   = -LOGO_HEIGHT;
        icons[f][DELTAY] = random(1, 6);
      }
    }
  }
}
```

:::

## AHT10

:::spoiler 

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

:::

## LCD

https://youtu.be/b_EbN71kp-Q

:::spoiler 

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

:::





## LED
https://youtu.be/zOiw8cNzU78

:::spoiler 

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
:::


## 可變電阻
https://youtu.be/iZT_zpbKRsk

:::spoiler 

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
:::



## RFID
https://youtu.be/l_DqxqUhu0I

:::spoiler 
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
:::

---

## Servo Motor 
https://youtu.be/kyXFV8ddGaM

:::spoiler 
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
:::

---



## DHT11
https://youtu.be/qpd6e-_cvW8

:::spoiler 
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

:::


---




## 超音波模組
https://youtu.be/MbmKsgL6y7w

:::spoiler 
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

:::



---





# Arduino 作品



---

## 俄羅斯方塊

![](https://i.imgur.com/Cc3pnXM.png)

:::spoiler 

```arduino=
#include <LedControl.h>
#define INI_speed 500 //初始速度 單位ms
//腳位定義
#define Rotate 2
#define Right 3
#define Left 4
#define Down 5

#define CIN 6
#define CS 7
#define CLK 8
//長寬
#define Width 8
#define Height 10

class Timer
{
  private:
    long Time;

  public:
    void TimerShutdown()
    {
      Time = -1;
    }

    void TimerSet()
    {
      Time = millis();
    }

    bool TimerTimeout(long Millis)
    {
      long C_Time = millis();
      if ((C_Time - Time) < Millis || Time == -1)
      {
        return false;
      }
      else
      {
        return true;
      }
    }
};

/*方塊種類定義
    |   |
  0 | 1 | 2
  ___|___|___
    |   |
  3 |4&9| 5
  ___|___|___
    |   |
  6 | 7 | 8
    |   |
*/

//Blocktype[方塊種類][選轉次數][補正碼]
int Blocktype[7][4][4] = {
  {{3, 4, 5, 4}, {1, 4, 7, 4}, {3, 4, 5, 4}, {1, 4, 7, 4}}, //0  I
  {{0, 1, 3, 4}, {0, 1, 3, 4}, {0, 1, 3, 4}, {0, 1, 3, 4}}, //1  O
  {{1, 3, 4, 5}, {1, 4, 5, 7}, {3, 4, 5, 7}, {1, 3, 4, 7}}, //2  T
  {{1, 2, 3, 4}, {1, 4, 5, 8}, {4, 5, 6, 7}, {0, 3, 4, 7}}, //3  S
  {{0, 1, 4, 5}, {2, 4, 5, 7}, {3, 4, 7, 8}, {1, 3, 4, 6}}, //4  Z
  {{0, 3, 4, 5}, {1, 2, 4, 7}, {3, 4, 5, 8}, {1, 4, 6, 7}}, //5  J
  {{2, 3, 4, 5}, {1, 4, 7, 8}, {3, 4, 5, 6}, {0, 1, 4, 7}}  //6  L
};
/*
   補正碼
   以4為中心x、y的相對位置
*/
int x_c[9] = { -1, 0, 1, -1, 0, 1, -1, 0, 1};
int y_c[9] = { -1, -1, -1, 0, 0, 0, 1, 1, 1};

int gravity = INI_speed;   //重力(掉落速度)
int state = -1;            //狀態
int T_map[Height][Width];  //地圖矩陣
int C_block = -1;          //目前方塊種類
int C_rotate = 0;          //目前旋轉狀態
int C_pos[2];              //目前方塊4號位置 0->y , 1->x;
int score = 0;             //分數


bool legit = true;         //檢測動作是否合法

Timer Timer1[10];
LedControl LC = LedControl(CIN, CLK, CS, 1); //LED矩陣物件
/*
   左上角為原點
   向右為+
   向下為+
*/
void Update(int Map[Height][Width])
{
  for (int i = 2; i < Height; i++)
  {
    for (int j = 0; j < Width; j++)
    {
      bool temp;
      if (Map[i][j] == 0)
      {
        temp = false;
      }
      else
      {
        temp = true;
      }
      LC.setLed(0, j, 9 - i, temp);
      //Serial.print(Map[i][j]);
    }
    //Serial.println();
  }
}

void C_Rotate(int nextstate)
{
  legit = true;
  int New_rotate = (C_rotate + 1) % 4;
  for (int i = 0; i < 4; i++)
  {
    int n = Blocktype[C_block][New_rotate][i];
    int new_x = C_pos[1] + x_c[n];
    int new_y = C_pos[0] + y_c[n];

    if ( T_map[new_y][new_x] == 1 || new_x > 7 || new_x < 0 || new_y >= Height )
    {
      legit = false;
      break;
    }
  }

  if (legit)
  {
    for ( int i = 0 ; i < 4 ; i++ )
    {
      int n = Blocktype[C_block][C_rotate][i];
      T_map[C_pos[0] + y_c[n]][C_pos[1] + x_c[n]] = 0;
    }
    C_rotate = New_rotate;
    for (int i = 0; i < 4; i++)
    {
      int n = Blocktype[C_block][C_rotate][i];
      int new_x = C_pos[1] + x_c[n];
      int new_y = C_pos[0] + y_c[n];
      T_map[new_y][new_x] = 2;
    }
  }
  state = nextstate;
  Update(T_map);
}

void C_Right(int nextstate)
{
  legit = true;
  for (int i = 0; i < 4; i++)
  {
    int n = Blocktype[C_block][C_rotate][i];
    int new_x = C_pos[1] + x_c[n] + 1;
    int new_y = C_pos[0] + y_c[n];
    if ( T_map[new_y][new_x] == 1 || new_x > 7 )
    {
      legit = false;
      break;
    }
  }

  if (legit)
  {
    for (int i = 0; i < 4; i++)
    {
      int n = Blocktype[C_block][C_rotate][i];
      int old_x = C_pos[1] + x_c[n];
      int old_y = C_pos[0] + y_c[n];
      T_map[old_y][old_x] = 0;
    }
    C_pos[1] += 1;
    for (int i = 0; i < 4; i++)
    {
      int n = Blocktype[C_block][C_rotate][i];
      int new_x = C_pos[1] + x_c[n];
      int new_y = C_pos[0] + y_c[n];
      T_map[new_y][new_x] = 2;
    }
  }
  state = nextstate;
  Update(T_map);
}

void C_Left(int nextstate)
{
  legit = true;
  for (int i = 0; i < 4; i++)
  {
    int n = Blocktype[C_block][C_rotate][i];
    int new_x = C_pos[1] + x_c[n] - 1;
    int new_y = C_pos[0] + y_c[n];
    if (T_map[new_y][new_x] == 1 || new_x < 0)
    {
      legit = false;
      break;
    }
  }

  if (legit)
  {
    for (int i = 0; i < 4; i++)
    {
      int n = Blocktype[C_block][C_rotate][i];
      int old_x = C_pos[1] + x_c[n];
      int old_y = C_pos[0] + y_c[n];
      T_map[old_y][old_x] = 0;
    }
    C_pos[1] -= 1;
    for (int i = 0; i < 4; i++)
    {
      int n = Blocktype[C_block][C_rotate][i];
      int new_x = C_pos[1] + x_c[n];
      int new_y = C_pos[0] + y_c[n];
      T_map[new_y][new_x] = 2;
    }
  }
  state = nextstate;
  Update(T_map);
}

void C_Down(int nextstate)
{
  legit = true;
  for (int i = 0; i < 4; i++)
  {
    int n = Blocktype[C_block][C_rotate][i];
    int new_x = C_pos[1] + x_c[n];
    int new_y = C_pos[0] + y_c[n] + 1;
    if (T_map[new_y][new_x] == 1 || new_y >= Height)
    {
      legit = false;
      break;
    }
  }
  for (int i = 0; i < 4; i++)
  {
    int n = Blocktype[C_block][C_rotate][i];
    int old_x = C_pos[1] + x_c[n];
    int old_y = C_pos[0] + y_c[n];

    if (legit)
    {
      T_map[old_y][old_x] = 0;
    }
    else
    {
      T_map[old_y][old_x] = 1;
    }

  }
  if (legit)
  {
    C_pos[0] += 1;
    for (int i = 0; i < 4; i++)
    {
      int n = Blocktype[C_block][C_rotate][i];
      int new_x = C_pos[1] + x_c[n];
      int new_y = C_pos[0] + y_c[n];
      T_map[new_y][new_x] = 2;
    }
    state = nextstate;
  }
  else
  {
    for (int i = 0; i < Height ; i++)
    {
      bool check = true;
      for (int j = 0; j < Width; j++)
      {
        if (T_map[i][j] != 1)
        {
          check = false;
          break;
        }
      }
      if (check == false)
      {
        continue;
      }
      else
      { 
        score += 10;
        gravity = INI_speed - score;
        Update(T_map);
        delay(10);
        for (int j = i ; j >= 1 ; j--)
        {
          for (int k = 0 ; k < Width ; k++)
          {
            T_map[j][k] = T_map[j - 1][k];
          }
        }
        for (int k = 0 ; k < Width ; k++)
        {
          T_map[0][k] = 0;
        }
        Update(T_map);
      }
    }
    bool over = false;
    for (int i = 0; i < Width ; i++)
    {
      if (T_map[1][i] == 1)
      {
        over = true;
        break;
      }
    }
    if (over)
    {
      for (int i = Height - 1; i >= 1; i--)
      {
        for (int j = 0; j < 8; j++)
        {
          T_map[i][j] = 0;
          LC.setLed(0, i - 1, j, false);
          Update(T_map);
        }
      }
      state = -1;
    }
    else
    {
      state = 15;
    }
  }
  Timer1[0].TimerSet();
  Update(T_map);
}

void setup()
{
  Serial.begin(9600);
  
  //定義接角模式
  pinMode(Rotate, INPUT);
  pinMode(Right, INPUT);
  pinMode(Left, INPUT);
  pinMode(Down, INPUT);

  pinMode(CIN, OUTPUT);
  pinMode(CS, OUTPUT);
  pinMode(CLK, OUTPUT);
  pinMode(A0, INPUT);
  
  randomSeed(analogRead(A0));   //產生隨機種子碼，A0空接會取得空氣中電壓，可視為亂碼
  C_block = random(7); 
  
  //LED矩陣開啟
  LC.shutdown(0, false);
  
  //LED矩陣挑整亮度(0~15)
  LC.setIntensity(0, 5);

  for (int i = 0 ; i < 10; i++) //重設所有計時器
  {
    Timer1[i].TimerShutdown();
  }

  //清空地圖
  for (int i = 0; i < Height; i++)
  {
    for (int j = 0; j < Width; j++)
    {
      T_map[i][j] = 0;
    }
  }
}
void loop()
{
  Serial.println(state);
  switch (state)
  {
    case -1://數值初始化
      score = 0;
      gravity = INI_speed;
      for (int i = 0 ; i < 10; i++) //重設所有計時器
      {
        Timer1[i].TimerShutdown();
      }
      for (int i = 0; i <= 8; i++)
      {
        for (int j = 0; j < 8; j++)
        {
          T_map[i][j] = 0;
        }
      }
      state = 0;
      break;


    case 0:
      if (digitalRead(Rotate) == HIGH)
      {
        state = 10;
      }
      break;

    case 10:
      if (digitalRead(Rotate) == LOW)
      {
        state = 14;
      }
      break;

    case 14:
      for (int i = 0; i < Height; i++)
      {
        for (int j = 0; j < 8; j++)
        {
          T_map[i][j] = 0;
        }
        state = 15;
      }
      break;

    case 15:                //產生隨機方塊
      C_block = random(7);
      C_pos[0] = 1;         //y
      C_pos[1] = 4;         //x
      C_rotate = 0;
      state = 20;
      break;

    case 20:
      Timer1[0].TimerSet();
      state = 25;
      break;

    case 25://偵測按鈕
      if (digitalRead(Rotate) == HIGH)
      {
        Serial.println("rotate");
        Timer1[1].TimerSet();
        state = 30;
      }
      else if (digitalRead(Right) == HIGH)
      {
        Serial.println("right");
        Timer1[1].TimerSet();
        state = 35;
      }
      else if (digitalRead(Left) == HIGH)
      {
        Serial.println("left");
        Timer1[1].TimerSet();
        state = 40;
      }
      else if (digitalRead(Down) == HIGH)
      {
        Serial.println("down");
        Timer1[1].TimerSet();
        state = 45;
      }
      break;

    case 30://旋轉
      if (digitalRead(Rotate) == LOW)
      {
        C_Rotate(25);
      }
      else if (Timer1[1].TimerTimeout(500))
      {
        Timer1[2].TimerSet();
        state = 31;
      }
      break;

    case 31:
      if (digitalRead(Rotate) == LOW)
      {
        C_Rotate(25);
      }
      else if (Timer1[2].TimerTimeout(100))
      {
        Timer1[2].TimerSet();
        C_Rotate(state);
      }
      break;


    case 35://右移
      if (digitalRead(Right) == LOW)
      {
        C_Right(25);
      }
      else if (Timer1[1].TimerTimeout(500))
      {
        Timer1[2].TimerSet();
        state = 36;
      }
      break;

    case 36:
      if (digitalRead(Right) == LOW)
      {
        C_Right(25);
      }
      else if (Timer1[2].TimerTimeout(100))
      {
        Timer1[2].TimerSet();
        C_Right(state);
      }
      break;

    case 40://左移
      if (digitalRead(Left) == LOW)
      {
        C_Left(25);
      }
      else if (Timer1[1].TimerTimeout(500))
      {
        Timer1[2].TimerSet();
        state = 41;
      }
      break;

    case 41:
      if (digitalRead(Left) == LOW)
      {
        C_Left(25);
      }
      else if (Timer1[2].TimerTimeout(100))
      {
        Timer1[2].TimerSet();
        C_Left(state);
      }
      break;

    case 45://下移
      if (digitalRead(Down) == LOW)
      {
        C_Down(25);
      }
      else if (Timer1[1].TimerTimeout(500))
      {
        Timer1[2].TimerSet();
        state = 46;
      }
      break;

    case 46:
      if (digitalRead(Down) == LOW)
      {
        C_Down(25);
      }
      else if (Timer1[2].TimerTimeout(100))
      {
        Timer1[2].TimerSet();
        C_Down(state);
      }
      break;
  }

  //Fall down
  if (Timer1[0].TimerTimeout(gravity))
  {
    C_Down(state);
  }
}
```
:::

## aht10 X OLED-7
```arduino=
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <AHT10.h>

double maxx=-100,minn=999;

uint8_t readStatus = 0;
AHT10 myAHT10(AHT10_ADDRESS_0X38);
const uint64_t pipe = 0x00FF; // 設定發送端 pipe 名稱
 

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels

// Declaration for SSD1306 display connected using software SPI (default case):
#define OLED_MOSI   9
#define OLED_CLK   10
#define OLED_DC    11
#define OLED_CS    12
#define OLED_RESET 13
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
  OLED_MOSI, OLED_CLK, OLED_DC, OLED_RESET, OLED_CS);

/*Comment out above, uncomment this block to use hardware SPI
#define OLED_DC     8
#define OLED_CS     10
#define OLED_RESET  9
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT,
  &SPI, OLED_DC, OLED_RESET, OLED_CS);
*/
void setup() {
  Serial.begin(9600);
  myAHT10.begin();
  if(!display.begin(SSD1306_SWITCHCAPVCC)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;); // Don't proceed, loop forever
  while (myAHT10.begin() != true)
  {
    Serial.println(F("AHT10 not connected...")); 
    delay(5000);
  }
  Serial.println(F("AHT10 initial OK"));
  }
}


void shower(){
 
    display.clearDisplay();
    double t = myAHT10.readTemperature();    //讀取 AHT10 溫度
    double h = myAHT10.readHumidity();   //讀取 AHT10 濕度
    maxx=max(maxx,t);
    minn=min(minn,t);
    Serial.print("Temperatura: ");
    Serial.print(t);
    Serial.print("   Humidity: ");
    Serial.println(h);

    //-------------------

    display.setTextSize(2); // Draw 2X-scale text
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0,0);
    display.print(F("Temp:"));
    display.println(t);
    display.print(F("Humi:"));
    display.println(h);
    display.print(F("maxt:"));
    display.println(maxx);
    display.print(F("mint:"));
    display.println(minn);
    
    display.display();      // Show initial text
  }

void loop(){
  shower();
  delay(500);
  }

```


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
