# Pràctica 5

## Practica 5A: Escàner I2C

### Objectiu
Realitzar un programa que utilitzi un dispositiu I2C, com un display OLED SSD1306, per mostrar informació a la pantalla.

### Codi

```cpp
#include <Arduino.h>
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // Amplada del display OLED en píxels
#define SCREEN_HEIGHT 32 // Alçada del display OLED en píxels
#define OLED_RESET -1 // Pin de reset (o -1 si es comparteix amb el reset de l'Arduino)
#define SCREEN_ADDRESS 0x3C // Adreça I2C del display OLED

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
  Serial.begin(115200);
  
  // Inicialitza el display amb voltatge generat internament (3.3V)
  if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
    Serial.println(F("No s'ha pogut inicialitzar el display SSD1306"));
    for(;;); // No continuar, bucle infinit
  }
  
  // Mostra el contingut inicial del buffer a la pantalla
  display.display();
  delay(2000); // Pausa de 2 segons
  
  // Neteja el buffer
  display.clearDisplay();
  
  // Dibuixa un sol píxel en blanc
  display.drawPixel(10, 10, SSD1306_WHITE);
  
  // Mostra el buffer del display a la pantalla
  display.display();
  delay(2000);

  // Proves de dibuix
  testdrawline();
  testdrawrect();
  testfillrect();
  testdrawcircle();
  testfillcircle();
  testdrawroundrect();
  testfillroundrect();
  testdrawtriangle();
  testfilltriangle();
  testdrawchar();
  testdrawstyles();
  testscrolltext();
  testdrawbitmap();
  
  // Inverteix i restaura el display, fent una pausa entre les dues operacions
  display.invertDisplay(true);
  delay(1000);
  display.invertDisplay(false);
  delay(1000);
  
  // Anima bitmaps
  testanimate(logo_bmp, LOGO_WIDTH, LOGO_HEIGHT);
}

void loop() {}

void testdrawline() {
  display.clearDisplay();
  for(int16_t i = 0; i < display.width(); i += 4) {
    display.drawLine(0, 0, i, display.height() - 1, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(int16_t i = 0; i < display.height(); i += 4) {
    display.drawLine(0, 0, display.width() - 1, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);
  display.clearDisplay();
  for(int16_t i = 0; i < display.width(); i += 4) {
    display.drawLine(0, display.height() - 1, i, 0, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(int16_t i = display.height() - 1; i >= 0; i -= 4) {
    display.drawLine(0, display.height() - 1, display.width() - 1, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);
  display.clearDisplay();
  for(int16_t i = display.width() - 1; i >= 0; i -= 4) {
    display.drawLine(display.width() - 1, display.height() - 1, i, 0, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(int16_t i = display.height() - 1; i >= 0; i -= 4) {
    display.drawLine(display.width() - 1, display.height() - 1, 0, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(250);
  display.clearDisplay();
  for(int16_t i = 0; i < display.height(); i += 4) {
    display.drawLine(display.width() - 1, 0, 0, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  for(int16_t i = 0; i < display.width(); i += 4) {
    display.drawLine(display.width() - 1, 0, i, display.height() - 1, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testdrawrect(void) {
  display.clearDisplay();
  for(int16_t i = 0; i < display.height() / 2; i += 2) {
    display.drawRect(i, i, display.width() - 2 * i, display.height() - 2 * i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testfillrect(void) {
  display.clearDisplay();
  for(int16_t i = 0; i < display.height() / 2; i += 3) {
    display.fillRect(i, i, display.width() - i * 2, display.height() - i * 2, SSD1306_INVERSE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testdrawcircle(void) {
  display.clearDisplay();
  for(int16_t i = 0; i < max(display.width(), display.height()) / 2; i += 2) {
    display.drawCircle(display.width() / 2, display.height() / 2, i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testfillcircle(void) {
  display.clearDisplay();
  for(int16_t i = max(display.width(), display.height()) / 2; i > 0; i -= 3) {
    display.fillCircle(display.width() / 2, display.height() / 2, i, SSD1306_INVERSE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testdrawroundrect(void) {
  display.clearDisplay();
  for(int16_t i = 0; i < display.height() / 2 - 2; i += 2) {
    display.drawRoundRect(i, i, display.width() - 2 * i, display.height() - 2 * i, display.height() / 4, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testfillroundrect(void) {
  display.clearDisplay();
  for(int16_t i = 0; i < display.height() / 2 - 2; i += 2) {
    display.fillRoundRect(i, i, display.width() - 2 * i, display.height() - 2 * i, display.height() / 4, SSD1306_INVERSE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testdrawtriangle(void) {
  display.clearDisplay();
  for(int16_t i = 0; i < max(display.width(), display.height()) / 2; i += 5) {
    display.drawTriangle(display.width() / 2, display.height() / 2 - i, display.width() / 2 - i, display.height() / 2 + i, display.width() / 2 + i, display.height() / 2 + i, SSD1306_WHITE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testfilltriangle(void) {
  display.clearDisplay();
  for(int16_t i = max(display.width(), display.height()) / 2; i > 0; i -= 5) {
    display.fillTriangle(display.width() / 2, display.height() / 2 - i, display.width() / 2 - i, display.height() / 2 + i, display.width() / 2 + i, display.height() / 2 + i, SSD1306_INVERSE);
    display.display();
    delay(1);
  }
  delay(2000);
}

void testdrawchar(void) {
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  for(int16_t i = 0; i < 256; i++) {
    if(i == '\n') continue;
    display.write(i);
    if((i > 0) && (i % 21 == 0)) display.write('\n');
  }
  display.display();
  delay(2000);
}

void testdrawstyles(void) {
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println(F("Hello, world!"));
  display.setTextColor(SSD1306_BLACK, SSD1306_WHITE);
  display.println(3.141592);
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE);
  display.print(F("0x")); display.println(0xDEADBEEF, HEX);
  display.display();
  delay(2000);
}

void testscrolltext(void) {
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(10, 0);
  display.println(F("scroll"));
  display.display();
  delay(1);
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
    (display.width() - LOGO_WIDTH) / 2,
    (display.height() - LOGO_HEIGHT) / 2,
    logo_bmp, LOGO_WIDTH, LOGO_HEIGHT, 1);
  display.display();
  delay(1000);
}

void testanimate(const uint8_t *bitmap, uint8_t w, uint8_t h) {
  int8_t f, icons[10][3];

  for(f = 0; f < 10; f++) {
    icons[f][0] = random(1 - LOGO_WIDTH, display.width());
    icons[f][1] = -LOGO_HEIGHT;
    icons[f][2] = random(1, 6);
  }

  for(;;) {
    display.clearDisplay();
    for(f = 0; f < 10; f++) {
      display.drawBitmap(icons[f][0], icons[f][1], bitmap, w, h, SSD1306_WHITE);
      icons[f][1] += icons[f][2];
      if(icons[f][1] >= display.height()) icons[f][1] = -LOGO_HEIGHT;
    }
    display.display();
    delay(200);
  }
}
```

### Funcionament
Es configura el display OLED SSD1306 amb la seva amplada, alçada i adreça I2C per més tard inicialitzar el display i mostrar un píxel com a prova inicial.

Es defineixen diverses funcions per dibuixar línies, rectangles, cercles, i altres formes geomètriques.
També s'inclouen funcions per dibuixar text, bitmaps i per realitzar animacions.

En la funció setup(), es criden les funcions de dibuix per mostrar les diferents capacitats del display.
La funció loop() està buida perquè tota la demostració es realitza al setup().