# PID Box

## Planning 

### Description 
The purpose of this project is for us to attempt to make a wheel spin to a desired speed using a Potentiometer. 

### Criteria & Constraints 

#### Criteria
* Needs to be able to spin and be controlled by the Potentiometer
* Needs to be able to be controlled by a switch connected to a LED
* Needs to be able to display its speed on an LCD Screen with "Speed: " + Speed
* Needs to use a Potentiometer to control PID function to set the speed

#### Constraints
* Finish before the end of school
* Code and Wire for the Motor

### Materials

* LED
* Motor
* Arduino
* LCD Screen
* Potentiometer
* Photo Interrupter
* Breadboard
* Laser Cut Box & Wheel
* Wires

### Scope

* This project will be especially challenging because I have about 3 weeks and some change to finish this project
* The SolidWorks will take a very long time because of the amount of parts and improvements to it
* I feel like the code will be challenging especially because of the PID part
* Assembling the project will also take a lot of time so we need to finish quickly

### Schedule 

* 5/10 - Start Code and Solidworks
* 5/17 - Have wheel and wheel holder finished in Solidworks, have the motor spinning
* 5/24 - Have the Box Done and test PID Code
* 5/31 - Assemble Box and get the LCD Screen and Potentiometer Working
* 6/6 - Have all code working and troubleshoot


### Measure of Success 

* Have the Potentiometer control the Wheel's Speed
* Display the speed of the PID
* Have a box printed out with all parts inside of it
* Hopefully get the PID portion working

### Protoype 
[[File:IMG-0545.JPG|300px]]


## Milestones 

### 5/10 

* This week we finished the Motor Control Project on Monday
* Finished the Solidworks Project on Wednesday
* Stayed after Wednesday to start the PID Proposal
* Got the Proposal confirmed on Thursday
* Had to remake the Battery Holder to solder because I soldered wrong and it broke
* Harry Finished the Wheel

### 5/17 

* Harry Made first sides of box
* Harry Started Motor Holder
* Jude started Wiring The Potentiometer and Motor
* Jude wired the LCD Screen
* Harry made adjustments to the preexisting wheel that didn't fit and cut another one out cut the wheel

* This is our finished box.

[[File:Pid5.2.PNG|200px]]

### 5/24 
* Jude finished basic code on Monday.
* Had to rewire the breadboard because the code wasn't working on Tuesday.
* Spent Tuesday afternoon troubleshooting wiring and had to get new Motor because it died.
* Harry finished the photo interrupter and Motor holder on Thursday and is now putting it into and assembly.
* Jude Spent Thursday wiring up the photo interrupter and trying to get it working.
* Jude Spent Friday working on RPM Code and needs help figuring out the code for it.
* Harry started putting the walls, motor holder, and wheel into and assembly to start making cuts and holes required for the screws and other things.

### 5/31 
* This week we started cutting out the Box.
* We also started Assembling our box.
* we ran into an issue with the wheel and had to change it to two spokes.
* While setting up the wiring our potentiometer started smoking and we figured out that we have a short.
* The LCD stopped working and we don't know why.
*There were multiple shorts when we tried to transfer our hardware inside our box.

### 6/6 
* This week we have been working on assembling the box
* Seth soldered the switch and got it working
* Harry finished assembling the box after Jude rewired everything.
* We are now working on our wiki
* We have finished working on RPM and the only thing left is the PID code.

## Jude's Notes 
* Basic code is not very hard and it relates to the assignments we have already done. 
* I don't fully understand RPM code and you can not start it if you don't have the photo interrupter wired.
* Since we started the PID Box with 4 weeks left of school it is very unlikely that we will finish the PID Code, that being said we are finished with our third week and the RPM code looks as if it can be finished
* RPM can either be calculated by the amount of revolutions in a certain amount of revolutions in a set time or how long it takes to do a revolution.
* I got the RPM working and I am trying to figure out the Max RPM

## Harry's Notes 
* The only challenging part of the Solidworks is the Motor holder so far.
* The Box has proven harder that first though.
* I ran into a problem with the assembly because I tried to change the size of the walls.
* I tried to make the entire box out of one wall and it ended very poorly

## Seth's Notes
*I was very behind in our project and after I learned my partner wasn't coming anymore, Jude and Harry offered to let me join their group.
*The box is all ready half way done so Harry and Jude asked me to make this notes section and then upload pictures on the wiki.
*We were having trouble wiring the switch with where ground went but figured we just needed another battery
*June 3 - we finished everything except the PID and just adding anything we can to our wiki.

## Code Advancements ##

### Basic Code

```ruby


//http://pasted.co/a664c08a
#include <Wire.h>
#include <Arduino.h>
#include <LCD.h>
#include <LiquidCrystal_I2C.h>
#include <LiquidCrystal.h>

LiquidCrystal_I2C lcd(0x27, 2, 1, 0, 4, 5, 6, 7);
const byte interrupterPin1 = 2;
int motorPin = 9;
int potPin = 3;
int potVal = 0;
int motorVal = 0;
int interruptPin = 2;
long counter;
volatile byte revolutions;
float dt;
int oldTime;
int actualSpeed;
long int currentTime = 0;
int previousTime;
int numberInterrupts = 0;

void setup() {
  pinMode(motorPin, OUTPUT);
  pinMode(potPin, INPUT);
  lcd.begin(16, 2);
  lcd.setBacklightPin(3 , POSITIVE);
  lcd.setBacklight(HIGH);
  lcd.setCursor(0, 0);
  delay(500);
  attachInterrupt(0, rev_counter, FALLING);
  attachInterrupt(1, rev_counter, RISING);

  lcd.begin (16, 2);
  lcd.setCursor(0, 0);
  lcd.print("Actual Speed: ");

}

void loop() {
  potVal = analogRead(potPin);

  int num = map(potVal, 0, 1023, 0, 100);
  motorVal = map(potVal, 0, 1023, 0, 255);
  analogWrite(motorPin, motorVal);
 /*
 lcd.setCursor(0, 1);
  lcd.print(num);
  delay(20);
  lcd.print("  ");

*/
}

void rev_counter() {

  revolutions++;
}


```

## Code with RPM


```ruby
// PID Box
// 5/31/19
// The purpose of a PID Box is to have a motor spin to a desired speed and calculate RPMs
#include <Wire.h>
#include <Arduino.h>
#include <LCD.h>
#include <LiquidCrystal_I2C.h>
#include <LiquidCrystal.h>

#define seconds() (millis()/1000)

LiquidCrystal_I2C lcd(0x27, 2, 1, 0, 4, 5, 6, 7);
unsigned long rot;
const byte interrupterPin1 = 2;
int motorPin = 9;
int potPin = 2;
int potVal = 0;
int motorVal = 0;
int switchPin = 7;
int switchState = 0;
unsigned long maxRPM = 15000;
unsigned long counter = 0;
unsigned long rpm;
unsigned long oldtime = 0;
volatile byte revolutions;
const byte interruptPin  = 2;
volatile byte state = LOW;

void setup() {
  pinMode(motorPin, OUTPUT);
  pinMode(potPin, INPUT);
  lcd.begin(16, 2);
  lcd.setBacklightPin(3 , POSITIVE);
  lcd.setBacklight(HIGH);
  lcd.setCursor(0, 0);
  delay(500);
  attachInterrupt(0, blink, RISING);
  attachInterrupt(1, off, FALLING);


  Serial.begin(9600);

  lcd.begin (16, 2);
  lcd.setCursor(0, 0);
  lcd.print("RPM: ");

}

void loop() {
  potVal = analogRead(potPin);
  //long time1 = millis();
  //float seconds = time1 / 1000;
  //if (seconds == int(seconds))
  //  oldtime = int(seconds);
  int num = map(potVal, 0, 1023, 0, 100);
  motorVal = map(potVal, 0, 1023, 0, 255);
  analogWrite(motorPin, motorVal);
  if ( millis() - oldtime > 1000) {
    rpm = counter * 15;
    lcd.setCursor(0, 1);
    lcd.print(rpm);
    lcd.print("  ");
    Serial.println(seconds());
    counter = 0;
    oldtime = millis();
    if(rpm > maxRPM){
  rpm = 15000;
}
  }
  delay(100);
  //Serial.println(counter);

}


void blink() {
  state = !state;
  counter++;
}
void off() {
  state = !state;
}

```

## Wiring and Solidworks Advancements 
### These are the wiring advancements I take a picture whenever I add something new to the wiring. ###

1. This picture is from our first week on the project where I was just trying to get the wiring working with the LCD Screen.

2. This picture is when I added the motor and it was taken after I got the LCD to print the speed and control the motor speed.

3. This picture shows our progress after we implemented the wiring into the box, it works completely except we are wiring the switch.

4. This picture show our top wall and the most important one, it also shows where we put out switch and potentiometer.

5. This is a picture of our wheel, we had to change it to 2 spokes because the photo interrupter was not reading the wheel fully.

1. [[File:IMG-0569.JPG|200px]]
2. [[File:IMG-0568.JPG|200px]]
3. [[File:IMG-0618.JPG|200px]]

4. [[File:Top wall on pid.PNG|200px]]
5. [[File:Pid_wheel_2.PNG|200px]]

## Final Solidworks Renderings 


[[File:PreviewJude.JPG|600px]]  [[File:Preview2.JPG|600px]]

## Miscellaneous ##

1. This is the box before we put on the last wall.

[[File:IMG_2362.jpg|150px]]

2. This is the motor wired up and in the holder spinning.

[[File:IMG_2360.jpg|150px]]

3. This is the finished wiring before putting the switch on and assembling the box fully.

[[File:IMG_2364.jpg|150px]]

4. This is the video of the project working

{{#ev:youtube|PNhbEk7_qNE}}
