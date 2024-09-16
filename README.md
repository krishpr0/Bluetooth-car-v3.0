# Bluetooth-car-v3.0
```
//This program is used to control a robot using a app that communicates with Arduino through a bluetooth module.

#define in1 3 //L298n Motor Driver pins.
#define in2 4
#define in3 5
#define in4 6
int command; //Int to store app command state.
int Speed = 204; // 0 - 255.
int Speedsec;
int buttonState = 0;
int lastButtonState = 0;
int Turnradius = 0; //Set the radius of a turn, 0 - 255 Note:the robot will malfunction if this is higher than int Speed.
int brakeTime = 45;
int brkonoff = 1; //1 for the electronic braking system, 0 for normal.
int ledPin1 = 2;
int ledPin2 = 3;
int ledPin3 = 4;
int ledPin4 = 5;
int buzzer_Pin = 10;// Pin for buzzer
void setup() {
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
  pinMode(ledPin1, OUTPUT);
  pinMode(ledPin2, OUTPUT); 
  pinMode(ledPin3, OUTPUT);
  pinMode(ledPin4, OUTPUT);
  pinMode(buzzer_Pin, OUTPUT);//Set the LED pin.
  Serial.begin(9600);  //Set the baud rate to your Bluetooth module.
}

void loop() {
  if (Serial.available() > 0) {
    command = Serial.read();
    Stop(); //Initialize with motors stoped.
    switch (command) {
      case 'F':
        forward();
        break;
      case 'B':
        back();
        break;
      case 'L':
        left();
        break;
      case 'R':
        right();
        break;
      case 'G':
        forwardleft();
        break;
      case 'I':
        forwardright();
        break;
      case 'H':
        backleft();
        break;
      case 'J':
        backright();
        break;
      case '0':
        Speed = 100;
        break;
      case '1':
        Speed = 140;
        break;
      case '2':
        Speed = 153;
        break;
      case '3':
        Speed = 165;
        break;
      case '4':
        Speed = 178;
        break;
      case '5':
        Speed = 191;
        break;
      case '6':
        Speed = 204;
        break;
      case '7':
        Speed = 216;
        break;
      case '8':
        Speed = 229;
        break;
      case '9':
        Speed = 242;
        break;
      case 'q':
        Speed = 255;
        break;
    }
    Speedsec = Turnradius;
    if (brkonoff == 1) {
      brakeOn();
    } else {
      brakeOff();
    }
    if (command == 'W'){// trun on the front lights//
      digitalWrite(ledPin1, HIGH);
      digitalWrite(ledPin2, HIGH);
    }
    if (command == 'w'){// trun off the front lights//
      digitalWrite(ledPin1, LOW);
      digitalWrite(ledPin2, LOW);
    }
    if (command == 'U'){// trun on the backlights//
      digitalWrite(ledPin3, HIGH);
      digitalWrite(ledPin4, HIGH);
    }
    if (command == 'u'){//trun off the backlights//
      digitalWrite(ledPin3, LOW);
      digitalWrite(ledPin4, LOW);
    }
    if (command == 'X'){
      digitalWrite(ledPin1, HIGH);
      digitalWrite(ledPin2, HIGH);
      digitalWrite(ledPin3, HIGH);
      digitalWrite(ledPin4, HIGH);
      delay(1000);
      digitalWrite(ledPin1, LOW);
      digitalWrite(ledPin2, LOW);     
      digitalWrite(ledPin3, LOW);
      digitalWrite(ledPin4, LOW);
      delay(1000);
    }
    if (command == 'x'){
      digitalWrite(ledPin1, LOW);
      digitalWrite(ledPin2, LOW);
      digitalWrite(ledPin3, LOW);
      digitalWrite(ledPin4, LOW);
    }
    if (command == 'V'){
      digitalWrite(buzzer_Pin, HIGH);
    }
    if (command == 'v'){
      digitalWrite(buzzer_Pin, LOW);
    }
  }
}

void forward() {
  analogWrite(in1, Speed);
  analogWrite(in3, Speed);
}

void back() {
  analogWrite(in2, Speed);
  analogWrite(in4, Speed);
}

void left() {
  analogWrite(in3, Speed);
  analogWrite(in2, Speed);
}

void right() {
  analogWrite(in4, Speed);
  analogWrite(in1, Speed);
}
void forwardleft() {
  analogWrite(in1, Speedsec);
  analogWrite(in3, Speed);
}
void forwardright() {
  analogWrite(in1, Speed);
  analogWrite(in3, Speedsec);
}
void backright() {
  analogWrite(in2, Speed);
  analogWrite(in4, Speedsec);
}
void backleft() {
  analogWrite(in2, Speedsec);
  analogWrite(in4, Speed);
}

void Stop() {
  analogWrite(in1, 0);
  analogWrite(in2, 0);
  analogWrite(in3, 0);
  analogWrite(in4, 0);
}

void brakeOn() {
  //Here's the future use: an electronic braking system!
  // read the pushbutton input pin:
  buttonState = command;
  // compare the buttonState to its previous state
  if (buttonState != lastButtonState) {
    // if the state has changed, increment the counter
    if (buttonState == 'S') {
      if (lastButtonState != buttonState) {
        digitalWrite(in1, HIGH);
        digitalWrite(in2, HIGH);
        digitalWrite(in3, HIGH);
        digitalWrite(in4, HIGH);
        delay(brakeTime);
        Stop();
      }
    }
    // save the current state as the last state,
    //for next time through the loop
    lastButtonState = buttonState;
  }
}
void brakeOff() {

}
