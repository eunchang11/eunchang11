#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <swRTC.h>

swRTC rtc; // 새로운 객체 선언

// LCD 설정 (I2C 주소: 0x27)
LiquidCrystal_I2C lcd(0x27, 16, 2);

// 센서 및 LED 핀 설정
const int rainSensorPin = A0;
const int ledPin = 12;
const int light = A1;

// 메시지 전환을 위한 변수
unsigned long previousMillis = 0;
const long interval = 10000; // 5초 간격
bool showUptime = true; // true일 때 Uptime을 표시, false일 때 Smart Farm Ready를 표시

void setup() {
  lcd.init(); 
  lcd.begin(16, 2);
  lcd.backlight();
  Serial.begin(9600);
  
  // 센서 및 LED 핀 모드 설정
  pinMode(rainSensorPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(light, INPUT);

  // 초기 화면 표시
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Starting...Ready");
  delay(3000);
  lcd.setCursor(0, 0);
  lcd.print("                ");
  
  rtc.stopRTC(); 
  rtc.setTime(0, 0, 0); 
  rtc.startRTC(); 
  //Serial.begin(115200); 
  delay(1000);
}

void loop() {
  unsigned long currentMillis = millis();

  // 5초마다 메시지 전환
  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;
    showUptime = !showUptime; // 메시지 전환
    lcd.clear();
  }

  // 빗물 감지 센서 상태 확인
  int rainSensorValue = analogRead(rainSensorPin);
  int lightSensor = analogRead(light);
  if (rainSensorValue <= 1000) {
    lcd.setCursor(0, 1);
    lcd.print("Water Detected");
  } else {
    lcd.setCursor(0, 1);
    lcd.print("              "); // 이전 메시지 지우기
  }

  if (lightSensor >= 800) {
    digitalWrite(ledPin, HIGH);
  } else {
    digitalWrite(ledPin, LOW);
  }

  // 디버그용 센서 값 출력
  Serial.println(lightSensor);

  // 메시지 전환에 따른 LCD 출력
  if (showUptime) {
    lcd.setCursor(0, 0);
    lcd.print("Uptime");
    lcd.setCursor(8, 0);
    lcd.print(rtc.getHours());
    lcd.print("  ");
    lcd.setCursor(10, 0);
    lcd.print(":");
    lcd.setCursor(11, 0);
    lcd.print(rtc.getMinutes());
    lcd.print("  ");
    lcd.setCursor(13, 0);
    lcd.print(":");
    lcd.setCursor(14, 0);
    lcd.print(rtc.getSeconds());
    lcd.print("  ");
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Smart Farm Ready");
  }

  delay(1000); // 1초 대기 후 루프 반복
}
