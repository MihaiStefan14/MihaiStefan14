- 👋 Hi, I’m @MihaiStefan14
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
 #include <dht.h>
#include <SoftwareSerial.h>
dht DHT;
#define dht_apin A2

int Buzzer = 8;
int LED = 13;
int note = 1950;
const int BTRX = 10;
const int BTTX = 11;
const int DO = 12; // gaz

float sensor_volt;
float Gaz;
float ratio;
float R0 = 0.91;

int sensorValue;

int vibr_Pin = 2;

int soundPin = 7;

SoftwareSerial SerialBT(BTRX, BTTX);
void setup ()
{
pinMode(DO, INPUT);
pinMode(vibr_Pin, INPUT);
Serial.begin(9600);
}

void flash(int duration)
{
  digitalWrite(LED, HIGH);
  delay(duration);
  digitalWrite(LED, LOW);
  delay(duration);

}
void loop()
{
  DHT.read11(dht_apin);

  SerialBT.print("Current humidity = ");
  SerialBT.print(DHT.humidity);
  SerialBT.println("%  ");
  SerialBT.print("temperature = ");
  SerialBT.print(DHT.temperature);
  SerialBT.println("C  ");

  int sensorValue = analogRead(A0);
  sensor_volt = ((float)sensorValue / 1024) * 5.0;
  Gaz = (5.0 - sensor_volt) / sensor_volt;
  ratio = Gaz / R0; //ratio = RS/R0

  SerialBT.println("S O S");
  SerialBT.print("voltaj=");
  SerialBT.println(sensor_volt);
  SerialBT.print("ratie=");
  SerialBT.println(Gaz);
  SerialBT.print("Rs/R0=");
  SerialBT.println(ratio);

  long masura = pulseIn (vibr_Pin, HIGH);
  if (masura > 46000)
    SerialBT.print("Miscare violenta "), SerialBT.print(masura);
  else if (masura < 500)
    SerialBT.print("Nu este miscare "), SerialBT.print(masura);
  else
    SerialBT.print("Miscare "), SerialBT.print(masura);
    
  long sum = 0;
    sum = analogRead(soundPin);
 
  if (sum > 140)
    SerialBT.println("Sunet");
  else
    SerialBT.println("nu este sunet");

  SerialBT.println(sum);
  SerialBT.println("media sunetului");


  unsigned int AnalogValue;
  AnalogValue = analogRead(5); // Senzor foc
  if (AnalogValue < 87)
    SerialBT.println("FOC");
  else if (AnalogValue < 92 && AnalogValue > 87)
    SerialBT.println("Probabilitate FOC");
  else
    SerialBT.println("Nu este FOC");


  sensorValue = analogRead(A1);       // read analog input pin 1
  SerialBT.print("AirQua=");
  SerialBT.print(sensorValue, DEC);               // prints the value read
  SerialBT.println(" PPM");
  SerialBT.print("\n");

  for (int i = 0; i < 3; i++)
  {
    flash(200);
    tone(Buzzer, note, 200);
    delay(100);
  }
  delay(1000);

  for (int i = 0; i < 3; i++)
  {
    flash(600);
    tone(Buzzer, note, 600);
    delay(100);
  }
  delay(1000);

  for (int i = 0; i < 3; i++)
  { flash(200);
    tone(Buzzer, note, 200);
    delay(100);
  }
  delay(1000);
}
<!---
MihaiStefan14/MihaiStefan14 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
