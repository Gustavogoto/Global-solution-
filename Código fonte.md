# Código fonte

#include <LiquidCrystal.h>

LiquidCrystal lcd(13, 12, 5, 4, 3, 2);

int tempValue = 0;

int sensorValue = 0;

int gas = 0;

int presenca = A1;

int SensorTempPino=0;

int piezo = 11;

void setup(){

  lcd.begin(16,2);
  
  pinMode(piezo, OUTPUT);
  
  pinMode(presenca, INPUT);
  Serial.begin(9600);
}


void loop()
{
  byte movimento = digitalRead(presenca);
  Serial.println(movimento);
  sensorValue = analogRead(A2);
  int SensorTempTensao = analogRead(SensorTempPino);
  float Tensao = SensorTempTensao*5;
  Tensao/=1024;
  float TemperaturaC = (Tensao-0.5)*100;

  if(sensorValue>20){
    lcd.setCursor(1,0);
    lcd.print("gas detectado");
    lcd.setCursor(1,2);
      lcd.print("gas: ");
      lcd.print(sensorValue);
      delay(1000);
      lcd.clear();
  }
  else
  {
    lcd.setCursor(1, 2);
    lcd.print("Gas nao detectado");
    delay(1000);
      lcd.clear();
 }
  delay(2000);
  if (20<TemperaturaC){
    lcd.setCursor(1,0);
    lcd.print("temperatura alta");
       lcd.setCursor(1,2);
      lcd.print("temp: ");
      lcd.print(TemperaturaC);
      delay(1000);
      lcd.clear();
  }
  else if(TemperaturaC<10)
  {
    lcd.setCursor(1, 0);
    lcd.print("Tempetatura baixa");
    lcd.setCursor(1,2);
      lcd.print("temp: ");
      lcd.print(TemperaturaC);
    delay(1000);
      lcd.clear();
  }
  else{
  lcd.setCursor(1,0);
  lcd.print("temperatura ideal");
  lcd.setCursor(1,2);
  lcd.print("temp: ");
  lcd.print(TemperaturaC);
  delay(1000);
  lcd.clear();
  }
  delay(1000);
   if(movimento){
        digitalWrite(piezo, HIGH);
         lcd.setCursor(1,2);
         lcd.print("Esta movimentando");
         delay(1500);
         digitalWrite(piezo, LOW);
         lcd.clear();
         delay(1000);


  }else{
         digitalWrite(piezo, LOW);
      lcd.setCursor(1,2);
      lcd.print("Esta parado");
      delay(1500);
      lcd.clear();

  }
  delay(1500);
}
