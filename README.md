# Make a Digital Timer!
 
## Overview
For this assignment, you are going to 

A) [Solder your LCD panel](#part-a-solder-your-lcd-panel)

B) [Write text to an LCD Panel](#part-b-writing-to-the-lcd) 

c) [Using a time-based digital sensor!](#part-c-using-a-time-based-digital-sensor)

D) [Make your Arduino sing!](#part-d-make-your-arduino-sing)

E) [Make your own timer](#part-e-make-your-own-timer) 
 


## Part A. Solder your LCD panel

**Take a picture of your soldered panel and add it here!**

![alt text](https://github.com/PGhzhang/IDD-Fa18-Lab2/blob/master/IMG_5973.jpg)

## Part B. Writing to the LCD
 
**a. What voltage level do you need to power your display?**

5V

**b. What voltage level do you need to power the display backlight?**

3.3V
   
**c. What was one mistake you made when wiring up the display? How did you fix it?**
I made the mistake of not connecting the two sides of breadboard. I placed LCD on one side of the breadboard and wires one the other side of breadboard. Thus, my LCD won't light board due to the incompletion of circut. I fixed the issue by arranding wires and LCD on the same side of the board. Another solution would be connectiong two sides of breadboard using wires.

**d. What line of code do you need to change to make it flash your name instead of "Hello World"?**
```
lcd.print("(whatever text to display)");
```

**e. Include a copy of your Lowly Multimeter code in your lab write-up.**
```
// include the library code:
#include <LiquidCrystal.h>

// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);


void setup() {
  // set up the LCD's number of columns and rows:
  lcd.begin(16, 2);
}

void loop() {
  // set the cursor position
  int sensorValue = analogRead(A0);
  // set the position 
  lcd.setCursor(2,0);
  // print value
  lcd.print(sensorValue);  
  delay(1000);
}
```

<a href="https://github.com/PGhzhang/IDD-Fa18-Lab2/blob/master/Lowly%20Multimeter.ino">Code Link</a>

## Part C. Using a time-based digital sensor

[![](http://img.youtube.com/vi/movEArC9qpo/0.jpg)](http://www.youtube.com/watch?v=movEArC9qpo "")


## Part D. Make your Arduino sing!

**a. How would you change the code to make the song play twice as fast?**

To make the song play twice as fast, I changed NoteDuration from 

```
int noteDuration = 1000 / noteDurations[thisNote]; 
```
to 

```
int noteDuration = 500 / noteDurations[thisNote];
```
 
**b. What song is playing?**

Star Wars Theme Song 


## Part E. Make your own timer

**a. Make a short video showing how your timer works, and what happens when time is up!**

[![](http://img.youtube.com/vi/83jmumjpODI/0.jpg)](http://www.youtube.com/watch?v=83jmumjpODI "")


code
```

#include <LiquidCrystal.h>
#include "pitches.h"

int melody[] = {
  NOTE_D3,NOTE_D3,NOTE_D3,NOTE_G3,NOTE_D4,NOTE_C4,NOTE_B3,NOTE_A3,NOTE_G4,NOTE_D4, \
  NOTE_C4,NOTE_B3,NOTE_A3,NOTE_G4,NOTE_D4,NOTE_C4,NOTE_B3,NOTE_C4,NOTE_A3,0};

int noteDurations[] = {
  10,10,10,2,2,10,10,10,2,4, \
  10,10,10,2,4,10,10,10,2,4};

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

#define ENC_A 8 //these need to be digital input pins
#define ENC_B 7

const int BTN=9;

int j = 0;

// variables will change:
int buttonstate = 0;         // variable for reading the pushbutton status
int prevstate = 0;

 
void setup()
{
  /* Setup encoder pins as inputs */
  lcd.begin(16, 2);

  pinMode(BTN, INPUT);
  
  pinMode(ENC_A, INPUT_PULLUP);
  pinMode(ENC_B, INPUT_PULLUP);
 
  Serial.begin (9600);
  Serial.println("Start");
   
  lcd.clear();
}
 
void loop() {
  lcd.setCursor(0, 0);
  static unsigned int counter4x = 0;      //the SparkFun encoders jump by 4 states from detent to detent
  static unsigned int counter = 0;
  static unsigned int prevCounter = 0;
  int tmpdata;
  tmpdata = read_encoder();
  if(tmpdata) {
    counter4x += tmpdata;
    counter = counter4x/4;
    if (prevCounter != counter){
      Serial.print("Counter value: ");
      Serial.println(counter);
      lcd.print(counter);
    }
    prevCounter = counter;
  }
  
  buttonstate = digitalRead(BTN);
    
  if (buttonstate == HIGH && prevstate==LOW){ 
    timer(counter);
  }
}

void timer(int x){
  int a = x;
 
  for (int i=a; i >= 0; i--){
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print(i);
    delay(1000);
    j = i;
  }
  if(j==0){
    lcd.clear();
    lcd.print("Times up");
    play();
   }
 }


void play(){
   for (int thisNote = 0; thisNote < 20; thisNote++) {

    // to calculate the note duration, take one second divided by the note type.
    //e.g. quarter note = 1000 / 4, eighth note = 1000/8, etc.
    int noteDuration = 1000 / noteDurations[thisNote];
    tone(6, melody[thisNote], noteDuration);

    // to distinguish the notes, set a minimum time between them.
    // the note's duration + 30% seems to work well:
    int pauseBetweenNotes = noteDuration * 1.30;
    delay(pauseBetweenNotes);
    
    // stop the tone playing:
    noTone(6);
  }
}
 
/* returns change in encoder state (-1,0,1) */
int read_encoder()
{
  static int enc_states[] = {
    0,-1,1,0,1,0,0,-1,-1,0,0,1,0,1,-1,0  };
  static byte ABab = 0;
  ABab *= 4;                   //shift the old values over 2 bits
  ABab = ABab%16;      //keeps only bits 0-3
  ABab += 2*digitalRead(ENC_A)+digitalRead(ENC_B); //adds enc_a and enc_b values to bits 1 and 0
  return ( enc_states[ABab]);
}
```

**b. Post a link to the completed lab report your class hub GitHub repo.**
