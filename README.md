# V1-Nerf-Turret-Proof-of-concept
### Code 
<table>
  <tr>
    <th width="50%">Hardware Demonstration</th>
    <th width="50%">Trigger and Flywheel logic

</th>
  </tr>
  <tr>
    <td valign="top">




https://github.com/user-attachments/assets/ae349f70-2eb1-47dc-9ca1-c463f80f88df




  </td>
  <td valign="top">

```cpp
#include <Servo.h>

const int TRIG_PIN = 2;
const int ECHO_PIN = 3;
const int GUN_PIN = 4;
Servo myServo;

void setup() {
  Serial.begin(9600);
  
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(GUN_PIN, OUTPUT);
  
  digitalWrite(GUN_PIN, LOW);
  myServo.attach(5);
  myServo.write(0); 
}

void loop() {
  
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  long duration = pulseIn(ECHO_PIN, HIGH, 30000);

  float distanceCm = (duration * 0.0343) / 2.0;

  if (duration == 0) {
    Serial.println("Warning: No echo detected (Out of range)");
    digitalWrite(GUN_PIN, LOW);
  } 
  else {
    Serial.print("Distance: ");
    Serial.print(distanceCm);
    Serial.println(" cm");

    if (distanceCm >= 2.0 && distanceCm <= 100.0) {
      digitalWrite(GUN_PIN, HIGH);
      
      delay(1000);
      
      myServo.write(0);
      delay(300);
      myServo.write(90);
      delay(300);
      myServo.write(0);
      delay(300);

    } else {
      digitalWrite(GUN_PIN, LOW);
      myServo.write(0);
    }
  }

  delay(100);
}
