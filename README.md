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
 
**b. What song is playing?**


## Part E. Make your own timer

**a. Make a short video showing how your timer works, and what happens when time is up!**

**b. Post a link to the completed lab report your class hub GitHub repo.**
