# Reciever


#include <SPI.h>
#include <nRF24L01.h>
#include <RF24.h>
RF24 radio(7, 8); // CE, CSN
const byte address[6] = "00001";
int I[5];

//Display
#include <TM1637Display.h>

// Module connection pins (Digital Pins)
#define CLK_1 48
#define DIO_1 49

#define CLK_2 44
#define DIO_2 45

#define CLK_3 4
#define DIO_3 5

#define CLK_4 46
#define DIO_4 47

#define Green1 35
#define Yellow1 34
#define Red1 36

#define Green2 37
#define Yellow2 39
#define Red2 38

#define Green3 41
#define Yellow3 40
#define Red3 42

#define Green4 23
#define Yellow4 24
#define Red4 25

#define buzzer A2
int i;

// The amount of time (in milliseconds) between tests
#define TEST_DELAY   2000

TM1637Display display_1(CLK_1, DIO_1);
TM1637Display display_2(CLK_2, DIO_2);
TM1637Display display_3(CLK_3, DIO_3);
TM1637Display display_4(CLK_4, DIO_4);


void setup() {
  Serial.begin(9600);
  radio.begin();
  radio.openReadingPipe(0, address);  
  radio.setPALevel(RF24_PA_MIN);
  radio.startListening();

  pinMode(Green1, OUTPUT);
  pinMode(Yellow1,OUTPUT);
  pinMode(Red1,OUTPUT);

  pinMode(Green2, OUTPUT);
  pinMode(Yellow2,OUTPUT);
  pinMode(Red2,OUTPUT);

  pinMode(Green3, OUTPUT);
  pinMode(Yellow3,OUTPUT);
  pinMode(Red3,OUTPUT);

  pinMode(Green4, OUTPUT);
  pinMode(Yellow4,OUTPUT);
  pinMode(Red4,OUTPUT);
}
void loop() {

  if (radio.available()) {
    char text[32] = "";  
    radio.read(&text, sizeof(text));
    Serial.println(text);
    int j=0;
    for(int i=0; i <= 30; i++){
      //Serial.print(text[i]);
      if(text[i]==44 || text[i]==36){
        I[j]=i;
        j++;
      }
    }

    int s1 = I[1]-I[0]-2;
    char r1[s1+1];
    for(int i=0; i <= s1+1; i++){
      r1[i]=char(text[i+1]);
    }    
    signed long reading1 = atol(r1);

    int s2 = I[2]-I[1]-2;
    char r2[s2+1];
    for(int i=0; i <= s2+1; i++){
      r2[i]=char(text[I[1]+i+1]);
    }    
    signed long reading2 = atol(r2);

    int s3 = I[3]-I[2]-2;
    char r3[s3+1];
    for(int i=0; i <= s3+1; i++){
      r3[i]=char(text[I[2]+i+1]);
    }    
    signed long reading3 = atol(r3);

    int s4 = I[4]-I[3]-2;
    char r4[s4+1];
    for(int i=0; i <= s4+1; i++){
      r4[i]=char(text[I[3]+i+1]);
    }    
    signed long reading4 = atol(r4);


  uint8_t data[] = { 0xff, 0xff, 0xff, 0xff };
  uint8_t blank[] = { 0x00, 0x00, 0x00, 0x00 };
  display_4.setBrightness(0x0f);


  delay(TEST_DELAY);

  /*
  for(k = 3; k >= 0; k--) {
  display.setSegments(data, 1, k);
  delay(TEST_DELAY);
  }
  */

   display_1.setBrightness(0x0f);
  //delay(TEST_DELAY);
  //display_1.clear();
  display_1.showNumberDec(reading1); 

  display_2.setBrightness(0x0f);
  //delay(TEST_DELAY);
  //display_2.clear();  
  display_2.showNumberDec(reading2); 

  display_3.setBrightness(0x0f);
  //delay(TEST_DELAY);
  //display_3.clear();
  display_3.showNumberDec(reading3);  

  display_4.setBrightness(0x0f);
  //delay(TEST_DELAY);
  //display_4.clear();
  display_4.showNumberDec(reading4);  


if(reading1>2000){
  digitalWrite(Green1, HIGH); digitalWrite(Yellow1, LOW); digitalWrite(Red1, LOW); 
}
else if(reading1<2000 && reading1>1000){
  digitalWrite(Green1, LOW); digitalWrite(Yellow1, HIGH); digitalWrite(Red1, LOW);
}
else if(reading1<1000){
  digitalWrite(Green1, LOW); digitalWrite(Yellow1, LOW); digitalWrite(Red1, HIGH);
}
else{
  digitalWrite(Green1, LOW); digitalWrite(Yellow1, LOW); digitalWrite(Red1, LOW);
}



if(reading2>2000){
  digitalWrite(Green2, HIGH); digitalWrite(Yellow2, LOW); digitalWrite(Red2, LOW); 
}
else if(reading2<2000 && reading2>1000){
  digitalWrite(Green2, LOW); digitalWrite(Yellow2, HIGH); digitalWrite(Red2, LOW);
}
else if(reading2<1000){
  digitalWrite(Green2, LOW); digitalWrite(Yellow2, LOW); digitalWrite(Red2, HIGH);
}




if(reading3>2000){
  digitalWrite(Green3, HIGH); digitalWrite(Yellow3, LOW); digitalWrite(Red3, LOW); 
}
else if(reading3<2000 && reading3>1000){
  digitalWrite(Green3, LOW); digitalWrite(Yellow3, HIGH); digitalWrite(Red3, LOW);
}
else if(reading3<1000){
  digitalWrite(Green3, LOW); digitalWrite(Yellow3, LOW); digitalWrite(Red3, HIGH);
}



if(reading4>2000){
  digitalWrite(Green4, HIGH); digitalWrite(Yellow4, LOW); digitalWrite(Red4, LOW); 
}
else if(reading4<2000 && reading4>1000){
  digitalWrite(Green4, LOW); digitalWrite(Yellow4, HIGH); digitalWrite(Red4, LOW);
}
else if(reading4<1000){
  digitalWrite(Green4, LOW); digitalWrite(Yellow4, LOW); digitalWrite(Red4, HIGH);
}





if(reading1<500 || reading2<500 || reading3<500 || reading4<500){
  for(i=0;i<3;i+1){
    digitalWrite(buzzer, HIGH);
    delay(1000);
    digitalWrite(buzzer,LOW);
  }}
  else{
    i=0;
    digitalWrite(buzzer,LOW);
  }
}
  }
//  display_1.setBrightness(0x0f);
//  //delay(TEST_DELAY);
//  //display_1.clear();
//  unsigned long reading1 = 100;
//  display_1.showNumberDec(reading1); 
//
//  display_2.setBrightness(0x0f);
//  //delay(TEST_DELAY);
//  //display_2.clear();  
//  unsigned long reading2 = 200;
//  display_2.showNumberDec(reading2); 
//
//  display_3.setBrightness(0x0f);
//  //delay(TEST_DELAY);
//  //display_3.clear();
//  unsigned long reading3 = 300;
//  display_3.showNumberDec(reading3);  
//
//  display_4.setBrightness(0x0f);
//  //delay(TEST_DELAY);
//  //display_4.clear();
//  unsigned long reading4 = 400;
//  display_4.showNumberDec(reading4);  
