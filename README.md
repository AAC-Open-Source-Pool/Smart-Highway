# Smart-Highway
#include <Servo.h>

#define TRIG_PIN_1  2 // Trigger pin for sensor 1
#define ECHO_PIN_1  3  // Echo pin for sensor 1

#define TRIG_PIN_2  5 // Trigger pin for sensor 2
#define ECHO_PIN_2  9   // Echo pin for sensor 2

#define TRIG_PIN_3  6 // Trigger pin for sensor 3
#define ECHO_PIN_3  10  // Echo pin for sensor 3


int s1_cm = 0;
int s2_cm = 0;
int s1CalmValue = 1000;
int s2CalmValue = 1000;	
long s1_time = 0;		
long s2_time = 0;
double speed = 0;		
int sensorDistance = 0;
int photosensor;
int photosensor1;
int photosensor2;
int sensorLDR = A0; 
int sensorLDR1 = A1; 
int sensorLDR2 = A2; 
Servo servo_8;
int led1 = 7;      // First bulb
int led2 = 4;
int led3= 12;  	  // Second bulb
int N1=0,N2=0;   
long duration1 = 0;
int inch1 = 0;
long duration2 = 0;
int inch2 = 0;
long duration3 = 0;
int inch3 = 0;
int slow=0;

void setup() {

    pinMode(TRIG_PIN_1, OUTPUT);
    pinMode(ECHO_PIN_1, INPUT);
    pinMode(TRIG_PIN_2, OUTPUT);
    pinMode(ECHO_PIN_2, INPUT);
     pinMode(TRIG_PIN_3, OUTPUT);
    pinMode(ECHO_PIN_3, INPUT);
    pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  pinMode(led3, OUTPUT);
  pinMode(sensorLDR, INPUT);
  pinMode(sensorLDR1, INPUT);
  pinMode(sensorLDR2, INPUT);
  	servo_8.attach(8);
  	servo_8.write(0);
  	Serial.begin(9600);
  	Reset();
}
long readUltrasonicDistance_1() {
    digitalWrite(TRIG_PIN_1, LOW);
    delayMicroseconds(1);
    digitalWrite(TRIG_PIN_1, HIGH);
    delayMicroseconds(1);
    digitalWrite(TRIG_PIN_1, LOW);
    return pulseIn(ECHO_PIN_1, HIGH);  // Convert pulse duration to distance in centimeters
}
long readUltrasonicDistance_2() {
    digitalWrite(TRIG_PIN_2, LOW);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN_2, HIGH);
    delayMicroseconds(5);
    digitalWrite(TRIG_PIN_2, LOW);
    return pulseIn(ECHO_PIN_2, HIGH);  // Convert pulse duration to distance in centimeters
}
long readUltrasonicDistance_3() {
    digitalWrite(TRIG_PIN_3, LOW);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN_3, HIGH);
    delayMicroseconds(5);
    digitalWrite(TRIG_PIN_3, LOW);
    return pulseIn(ECHO_PIN_3, HIGH);  // Convert pulse duration to distance in centimeters
}

void Reset()
{
  s1_time = 0;
  s2_time = 0;
  // s1CalmValue = 0.01723 * readUltrasonicDistance_1() - 10;
  //s2CalmValue = 0.01723 * readUltrasonicDistance_2() - 10;
  speed = 0;
  sensorDistance=10;
  servo_8.write(0);
  N1=0;
  N2=0;
}
int v1=0,v2=0,v3=0;
int val1,val2,val3;
int n1=0,n2=0,n3=0;
int V1=0,V2=0;
void loop(){
 
  photosensor = analogRead(sensorLDR);
  photosensor1 = analogRead(sensorLDR1);
  photosensor2 = analogRead(sensorLDR2);
  duration1 = readUltrasonicDistance_1();
  duration2 = readUltrasonicDistance_2(); 
  duration3 = readUltrasonicDistance_3();
  inch1 = duration1 * 0.0133 / 2;
  inch2 = duration2 * 0.0133 / 2;
  inch3 = duration3 * 0.0133 / 2;

  if(inch1<10 )
  {
    val1=1;
  }
  else
  {
    val1=0;
  }
  if(v1>v2)
  {
  if(inch2<10)
  {
    val2=1;
  }
  else
  {
    val2=0;
  }
  }
   if(inch3<10)
  {
    val3=1;
  }
  else
  {
    val3=0;
  }
 //Serial.println();
  //Serial.println(duration1);
 /* if(photosensor1<300)
  {
    Serial.println("set 1 fault");
  }
  if(photosensor2<300)
  {
    Serial.println("set 2 fault");
  }*/
  //Serial.println(photosensor);
 if(photosensor<20)
 {  

  if(val1==1 && n1==0)
  {
    digitalWrite(led1,HIGH);
    v1++;
    delay(1000);
    n1=1;
    //Serial.println(v2);
  }
   else if(val1==0)
   {
     n1=0;
   }
if(v2<=v1)
{
  if(val2==1 && n2==0)
  {
    digitalWrite(led2,HIGH); 
    v2++;
    n2=1;
    Serial.println(v2);
    if(v1==v2)
    {
     digitalWrite(led1,LOW); 
    }
  }
   else if(val2==0)
   {
     n2=0;
   }
}  
if(val3==1 && n3==0)
  {
    digitalWrite(led3,HIGH);
    v3++;
    n3=1;
    slow=0;
    //Serial.println(v1);
      if(v3==v2)
    {
     digitalWrite(led2,LOW); 
    }
  }
   else if(val3==0)
   {
     n3=0;
   }
  if(v2>v1)
  {
    v2=v1;
  }
  if(v3>v1)
   {
     v3=v1;
   }
 }
  else
  {
    digitalWrite(led1,LOW); 
    digitalWrite(led2,LOW);
    digitalWrite(led3,LOW); 
    v1=0;
    v2=0; 
    v3=0;
  }
 
//Serial.println(val1);
  if(speed==0)
  {
     if (s1_time == 0)
     {
       //s1_cm = 0.01723 * readUltrasonicDistance_1();
       //Serial.println(s1CalmValue);
     if (val1==1 && N1==0)
     {
      s1_time = millis(); 
     //Serial.println("ms: senzor 1");
   // Serial.println(s1_time);
    V1++;
     N1=1;
    }
    else
    {
      N1=0;
    }
  }
   if ((s2_time == 0) && (v1==v2))
  {
  	//s2_cm = 0.01723 * readUltrasonicDistance_2();
   
    if (val2==1 && N2==0)
    {
      s2_time = millis();
      //Serial.println("ms: senzor 2");
      //Serial.println(s2_time);
      V2++;
      N2=1;
    }
    else
    {
      N2=0;
    }
  }
   
  if ((s1_time != 0) && (s2_time != 0) && (speed == 0)&&(V1==V2))
  {
    int time = abs(s1_time - s2_time);
    //Serial.println("TIME");
    //Serial.println(time);
    speed = ((double)sensorDistance / ((double)time / 1000.0))* 3.6;
    //speed=((double)sensorDistance)/((double)time);
    //Serial.println("SPEED");
    //Serial.println(speed);
    //Serial.println("---------------------"); 
  } 
    if(speed>30 && slow==0)
    {
      int position = 0;
  		for (position = 1; position <= 180; position += 1) 
  		{
    		servo_8.write(position);
    		delay(0); 
 		 }
      delay(3000);
 		 for (position = 180; position >= 1; position -= 1) 
  		{
    		servo_8.write(position);
            delay(1);
  		}
    } 
    else
    {
      slow=1;
    } 
 }
  else
  {
    Reset();
  }
  }
