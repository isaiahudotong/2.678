
    // 2.678 - Lab 10: Motor Speed Control Template
    // Global declarations (accessible to all functions).
    // Based on the Arduino connections shown 1n the handout.
    // Note that pins 11 (PWMA) and 5 (PWMB) are both Arduino PWM pins.
    // Note:   We will only use motor A in this lab
const int AIN1  = 9;
const int AIN2  = 10;
const int PWMA  = 11;
const int BIN1  = 7;
const int BIN2  = 6;
const int PWMB  = 5;
const int STDBY = 8;
//const int A0 = 14;
//const int A1 = 15;
//const int A2 = 16;

void setup()  {
    pinMode(AIN1, OUTPUT);
    pinMode(AIN2, OUTPUT);
    pinMode(BIN1, OUTPUT);
    pinMode(BIN1, OUTPUT);
    pinMode(PWMA, OUTPUT);
    pinMode(PWMB, OUTPUT);
    pinMode(STDBY, OUTPUT);
    // whatever else you need to do...
    pinMode(A0, INPUT);
    pinMode(A1, INPUT);
    pinMode(A2, INPUT);
    digitalWrite(STDBY, HIGH);
    
    Serial.begin(9600);
    }

void loop(){
  int sensorLeft = analogRead(A0);
  int sensorMiddle = analogRead(A1);
  int sensorRight = analogRead(A2);
  //string string1 = "L = " + sensorLeft;
  // print out the value you read:
  Serial.print("L = ");Serial.print(sensorLeft);
  Serial.print(" M = ");Serial.print(sensorMiddle);
  Serial.print(" R = ");Serial.println(sensorRight);
  delay(10);
  forward(50);
  if (sensorLeft < 250 && sensorMiddle < 250){
    ctr_lt(50);
  } 
  if (sensorRight < 250 && sensorMiddle < 250){
    ctr_rt(50);
  }
  if (sensorLeft < 250){
    left(50);
  }
  if (sensorRight < 250){
    right(50);
  }
  // the following will test you function using motor A
  
  
  //motorWrite( 40, BIN1, BIN2, PWMB);     //  motor A forward at a slow speed
  //delay(1000);
  //motorWrite( -235, BIN1, BIN2, PWMB);   //  motor A reverse at a high speed
  //delay(1000);
  // keep on truckin'
  }

//----------------------------------------------
void motorWrite(int motorSpeed, int xIN1, int xIN2, int PWMx)
{
 
  if (motorSpeed > 0)          // it's forward
  {  digitalWrite(xIN1, LOW);
     digitalWrite(xIN2, HIGH);
  }
  else                         // it's reverse
  {  digitalWrite(xIN1, HIGH);
     digitalWrite(xIN2, LOW);
  } 

    motorSpeed = abs(motorSpeed);
    motorSpeed = constrain(motorSpeed, 0, 255);   // Just in case...
    analogWrite(PWMx, motorSpeed);
}

void forward(int spd){

  motorWrite(spd, AIN1, AIN2, PWMA);
  motorWrite(spd, BIN1, BIN2, PWMB);

}

void left(int spd){
  motorWrite(0, AIN1, AIN2, PWMA);
  motorWrite(spd, BIN1, BIN2,PWMB);
}

void right(int spd){
  motorWrite(spd, AIN1, AIN2, PWMA);
  motorWrite(0, BIN1, BIN2,PWMB);
}

void ctr_rt(int spd){
  motorWrite(spd, AIN1, AIN2, PWMA);
  motorWrite(-spd, BIN1, BIN2,PWMB);
}

void ctr_lt(int spd){
  motorWrite(spd, AIN1, AIN2, PWMA);
  motorWrite(-spd, BIN1, BIN2,PWMB);
}

