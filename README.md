# Lift-and-tilt-a-coffee-cup
Arduino + Adafruit + IFTTT


#include "config.h"
AdafruitIO_Feed *coffeecups = io.feed("coffeecups");

int i = 0;
int x = 1;

void setup(){
  io.connect();
  Serial.begin(9600);
  pinMode(1, INPUT);
  pinMode(2, INPUT);
}

void loop(){
  
  io.run();
  int button_Switch = digitalRead(2);
  int buttonOnTheCup_Switch = digitalRead(1);

  //If the button is pressed and the numnber of consumed cups didn't achieve 5
  if (buttonOnTheCup_Switch == HIGH && i>=0 && i<5){
    if(x==1){
      
  //...then increase the number of cups
    i++;    
    Serial.println(i);
    coffeecups->save(i);
    x = 2;
  }}
    //...else if the number of cups equal to/ greater than 5...
  else if(buttonOnTheCup_Switch == HIGH && i >=5){
       //...then reset the number of cups to 0...
    i=0;
    coffeecups->save(i);
    x = 2;
  }
  
  if(buttonOnTheCup_Switch == LOW && i >=0 && i<5){
        if(x==2){
         x = 1;    
  }}

    //...if the user presses an extra button...
if (button_Switch == HIGH){
      //...then the number of consumed coffee cups will be reduced to 0.
    i=0;
    Serial.println(i);
    coffeecups->save(i);
}
}


