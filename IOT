#include <Wire.h>
#include <LiquidCrystal.h>
#include <Servo.h>
#include <Arduino.h>

// water level 
const int sensorPin = A0;  // select the input pin for the water level sensor
const int threshold = 35;// to turn on light to alert the user that the water level is almost high 
int led1 = 6; // LED pin to indicate the water level is high 

// turbidity sensor pin 
const int turbidityPin = A1;    // Pin connected to turbidity sensor

// water pump 
int relay1Pin = 8;       // Pin connected to relay1
int button1Pin = 10;       // Pin connected to button1 ( water pump)
bool buttonPressed1 = false; // State of the button

//water extract (waste)
int relay2Pin = 13;       // Pin connected to relay1
int button2Pin = 7;       // Pin connected to button1 ( water ext)
bool buttonPressed2 = false; // State of the button

//feeding system (food despenser)
const int servoPin = 9;        // Pin connected to servo
const int openDegree = 90;     // Degree to open the servo
const int closeDegree = 0;     // Degree to close the servo

Servo myservo;

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);


void setup(){
  pinMode(led1, OUTPUT);

  pinMode(servoPin, OUTPUT);
  myservo.attach(servoPin);
  
  pinMode(relay1Pin, OUTPUT);
  pinMode(button1Pin, INPUT);
  digitalWrite(relay1Pin, LOW); // Initially turn off the water pump

  pinMode(relay2Pin, OUTPUT);
  pinMode(button2Pin, INPUT);
  digitalWrite(relay2Pin, LOW); // Initially turn off the water ext

lcd.begin(16, 2); // Set up the number of columns and rows on the LCD.
Serial.begin(9600);



}

void readTurbidity() {
  int sensorValue = analogRead(sensorPin);
  float voltage = sensorValue * (5.0 / 1023.0); // convert the analog reading to voltage
  float turbidity = voltage / 0.01; // convert the voltage to NTU, assuming 0.01 V per NTU
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Turbidity:"); // move cursor to the 11th column, 0th row
  lcd.print(turbidity, 2); // print the turbidity reading with 2 decimal places
   delay(1000); 
}
void readWaterLevel() {
  int level = analogRead(sensorPin);
  int percent = map(level, sensorMin, sensorMax, 0, 100);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Water Level: ");
  lcd.print(percent);
  lcd.print("%");
  delay(1000);
  if (percent > threshold){
    digitalWrite(led1, HIGH); // Turn on the indicator LED
  }
  else {
    digitalWrite(led1, LOW);
  }
}
void controlServo() {
  myservo.write(openDegree);
  lcd.clear();
  lcd.setCursor(0, 1 );
  lcd.print("Food:Open");
  Serial.println("food: OPEN");
  delay(1000);
  lcd.clear();
  lcd.setCursor(0, 1);
  lcd.print("Food:Closed");
  Serial.println("Servo: CLOSE");
  myservo.write(closeDegree);
    delay(1000);
}
void pump (){
  
  if(buttonPressed1){    
    digitalWrite(relay1Pin, HIGH); // Turn on the water pump
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Water Pump:ON");
    Serial.println("Pump: ON");
     delay(1000);
  }else{    
    digitalWrite(relay1Pin, LOW); // Turn off the water pump
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Water Pump:OFF");
    Serial.println("Pump: OFF");
     delay(1000);
  }
  if(digitalRead(button1Pin)){
    buttonPressed1 = !buttonPressed1; // Change the state of the button
    while(digitalRead(button1Pin)){
    }
  }
}
void air(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Air Pump: ON");
  delay(1000);
}
void ext(){
  
  if(buttonPressed2){    
    digitalWrite(relay2Pin, HIGH); // Turn on the water pump
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Water Waste:ON");
    Serial.println("EXT: ON");
    delay(1000);
  }else{    
    digitalWrite(relay2Pin, LOW); // Turn off the water pump
   lcd.clear();
   lcd.setCursor(0, 0);
    lcd.print("Water Waste:OFF");
    Serial.println(" EXT: off");
    delay(1000);
  }
  
  if(digitalRead(button2Pin)){
    buttonPressed2 = !buttonPressed2; // Change the state of the button
    while(digitalRead(button2Pin)){
    }
  }
}

void loop(){
  readWaterLevel();
  readTurbidity();
  air();
  pump();
  ext();
  controlServo();
}