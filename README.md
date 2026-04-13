#include <Servo.h>

// Настройки пинов
const int moisturePin = A0;   // Пин датчика влажности
const int servoPin = 9;       // Пин сервопривода

// Настройки порогов
const int wetThreshold = 500; // Порог влажности (подбирается экспериментально)
const int posDry = 0;         // Угол серво для сухого мусора
const int posWet = 90;        // Угол серво для мокрого мусора

Servo separatorServo;

void setup() {
  Serial.begin(9600);
  separatorServo.attach(servoPin);
  
  // Устанавливаем в исходное положение
  separatorServo.write(posDry);
  delay(1000);
}

void loop() {
  // Читаем значение с датчика (от 0 до 1023)
  int moistureValue = analogRead(moisturePin);
  
  Serial.print("Влажность: ");
  Serial.println(moistureValue);

  // Логика распределения
  // Внимание: большинство датчиков выдают МЕНЬШЕЕ значение, когда мокро.
  // Если у тебя стандартный датчик влажности почвы:
  if (moistureValue < wetThreshold) {
    Serial.println("Состояние: МОКРО");
    separatorServo.write(posWet);
  } else {
    Serial.println("Состояние: СУХО");
    separatorServo.write(posDry);
  }

  delay(2000); // Задержка 2 секунды перед следующим измерением
}
