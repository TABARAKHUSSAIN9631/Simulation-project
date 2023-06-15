# TITTLE:
Password Based Door Lock Security System Using Arduino & Keypad with proteus</br>

## ABSTRACT:
In this project, we will learn how to make the Password-Based Security System Using Arduino & Keypad. As thefts are increasing day by day security is becoming a major concern nowadays. So a digital code lock can secure your home or locker easily. It will open your door only when the right password is entered.

## INTRODUCTION:
The circuit of this project is very simple which contains Arduino, keypad module, buzzer, Servo Motor, and LCD. Arduino controls the complete processes like taking a password from the keypad module, comparing passwords, driving buzzer, rotating servo motor, and sending status to the LCD display. The keypad is used for taking the password. The buzzer is used for indications. Servo motor is used for opening the gate while rotating and LCD is used for displaying status or messages on it.

## LITERATURE REVIEW:

## PROPOSED METHODLOGY:
The circuit or the schematic of Password Based Door Lock Security System is fairly simple. You can use the Fritzing Software to draw the schematic.
First of all, we will make a connection to the 4×3 Keypad. For connecting the keypad with the Arduino we are using digital pins D1 to D7. Connect all seven pins of the keypad to analog pins D1 ~ D7 of Arduino.
To connect the servo motor with the Arduino, use digital pin D9 of Arduino to output the PWM pin of the servo motor. Now connect the positive wire of the buzzer to the pin D10 of Arduino and the negative wire to the ground.

## CIRCUIT DIAGRAM:
![door lock circuit diagrma](https://github.com/AMANKUMAR2541/Simulation-project/assets/132323363/9dee9cf8-47b5-42d6-ae75-e270bb738ccf)

## PROGRAM:
#include <Keypad.h></br>
#include <LiquidCrystal_I2C.h></br>
#include <Servo.h></br>
Servo myservo;</br>
int pos=0; // LCD Connections</br>
LiquidCrystal_I2C lcd(0x3F,20,4);</br>
const byte rows=4;</br>
const byte cols=3;</br>
 char key[rows][cols]={</br>
{'1','2','3'},</br>
{'4','5','6'},</br>
{'7','8','9'},</br>
{'*','0','#'}</br>
};</br>
byte rowPins[rows]={6,7,8,9};</br>
byte colPins[cols]={3,4,5};</br>
Keypad keypad= Keypad(makeKeymap(key),rowPins,colPins,rows,cols);</br>
char* password="1327";</br>
int currentposition=0;</br>
int redled=13;</br>
int greenled=12;</br>
int buzz=11;</br>
int invalidcount=10;</br>
 void setup()</br>
{</br>
displayscreen();</br>
Serial.begin(9600);</br>
pinMode(redled, OUTPUT);</br>
pinMode(greenled, OUTPUT);</br>
pinMode(buzz, OUTPUT);</br>
myservo.attach(10); //SERVO ATTACHED//</br>
 lcd.begin(16,2);</br>
 }</br>
 void loop()</br>
{</br>
if( currentposition==0)</br>
{</br>
displayscreen();</br>
 }</br>
int l ;</br>
char code=keypad.getKey();</br>
if(code!=NO_KEY)</br>
{</br>
lcd.clear();</br>
lcd.setCursor(0,0);</br>
lcd.print("PASSWORD:");</br>
lcd.setCursor(7,1);</br>
lcd.print(" ");</br>
lcd.setCursor(7,1);</br>
for(l=0;l<=currentposition;++l)</br>
{</br>
 lcd.print("*");</br>
keypress();</br>
}</br>
 if (code==password[currentposition])</br>
{</br>
++currentposition;</br>
if(currentposition==4)</br>
{</br>
 unlockdoor();</br>
currentposition=0;</br>
 }</br>
 }</br>
 else</br>
{</br>
++invalidcount;</br>
incorrect();</br>
currentposition=0;</br>
 }</br>
if(invalidcount==5)</br>
{</br>
 ++invalidcount;</br>
torture1();</br>
 
}</br>
if(invalidcount==8)</br>
{</br>
torture2();</br>
}</br>
}</br>
// LOOP ENDS!!!//</br>
}</br>
 
//********OPEN THE DOOR FUNCTION!!!!***********//</br>
 
void unlockdoor()</br>
{</br>
delay(900);</br>
 
lcd.setCursor(0,0);</br>
lcd.println(" ");</br>
lcd.setCursor(1,0);</br>
lcd.print("Access Granted");</br>
lcd.setCursor(4,1);</br>
lcd.println("WELCOME!!");</br>
lcd.setCursor(15,1);</br>
lcd.println(" ");</br>
lcd.setCursor(16,1);</br>
lcd.println(" ");</br>
lcd.setCursor(14,1);</br>
lcd.println(" ");</br>
lcd.setCursor(13,1);</br>
lcd.println(" ");</br>
unlockbuzz();</br>
 
for(pos = 180; pos>=0; pos-=5) // goes from 180 degrees to 0 degrees</br>
{</br>
myservo.write(pos); // tell servo to go to position in variable 'pos'</br>
delay(5); // waits 15ms for the servo to reach the position</br>
}</br>
delay(2000);</br>
 delay(1000);</br>
counterbeep();</br>
delay(1000);</br>
 for(pos = 0; pos <= 180; pos +=5) // goes from 0 degrees to 180 degrees</br>
{ // in steps of 1 degree</br>
myservo.write(pos); // tell servo to go to position in variable 'pos'</br>
delay(15);</br>
 currentposition=0;</br>
 lcd.clear();</br>
displayscreen();</br>
 }</br>
}</br>
 //************WRONG CODE FUNCTION********//</br>
 
void incorrect()</br>
{</br>
delay(500);</br>
lcd.clear();</br>
lcd.setCursor(1,0);</br>
lcd.print("CODE");</br>
lcd.setCursor(6,0);</br>
lcd.print("INCORRECT");</br>
lcd.setCursor(15,1);</br>
lcd.println(" ");</br>
lcd.setCursor(4,1);</br>
lcd.println("GO AWAY!!!");</br>
 
lcd.setCursor(13,1);</br>
lcd.println(" ");</br>
Serial.println("CODE INCORRECT YOU ARE UNAUTHORIZED");</br>
digitalWrite(redled, HIGH);</br>
digitalWrite(buzz, HIGH);</br>
delay(3000);</br>
lcd.clear();</br>
digitalWrite(redled, LOW);</br>
digitalWrite(buzz,LOW);</br>
displayscreen();</br>
}</br>
//************** CLEAR THE SCREEN!!!*************//</br>
void clearscreen()</br>
{</br>
lcd.setCursor(0,0);</br>
lcd.println(" ");</br>
lcd.setCursor(0,1);</br>
lcd.println(" ");</br>
lcd.setCursor(0,2);</br>
lcd.println(" ");</br>
lcd.setCursor(0,3);</br>
lcd.println(" ");</br>
}</br>
//**************KEYPRESS********************//</br>
void keypress()</br>
{</br>
 digitalWrite(buzz, HIGH);</br>
delay(50);</br>
digitalWrite(buzz, LOW);</br>
}</br>
//********DISPALAY FUNCTION!!!*************//</br>
void displayscreen()</br>
{</br>
 lcd.setCursor(0,0);</br>
lcd.println("*ENTER THE CODE*");</br>
lcd.setCursor(1 ,1);</br>
 lcd.println("TO _/_ (OPEN)!!");</br>
}</br>
//*************** ARM SERVO***********//</br>
void armservo()</br>
{</br>
 
for (pos=180;pos<=180;pos+=50)</br>
{</br>
myservo.write(pos);</br>
delay(5);</br>
}</br>
delay(5000);</br>
 
for(pos=180;pos>=0;pos-=50)</br>
{</br>
myservo.write(pos);</br>
}</br>
 
}</br>
//**********UNLOCK BUZZ*************//</br>
void unlockbuzz()</br>
{</br>
 
digitalWrite(buzz, HIGH);</br>
delay(80);</br>
digitalWrite(buzz, LOW);</br>
delay(80);</br>
digitalWrite(buzz, HIGH);</br>
delay(80);</br>
digitalWrite(buzz, LOW);</br>
delay(200);</br>
digitalWrite(buzz, HIGH);</br>
delay(80);</br>
digitalWrite(buzz, LOW);</br>
delay(80);</br>
digitalWrite(buzz, HIGH);</br>
delay(80);</br>
digitalWrite(buzz, LOW);</br>
delay(80);</br>
}</br>
 
//**********COUNTER BEEP**********//</br>
void counterbeep()</br>
{</br>
delay(1200);</br>
 
 
lcd.clear();</br>
digitalWrite(buzz, HIGH);</br>
 
lcd.setCursor(2,15);</br>
lcd.println(" ");</br>
lcd.setCursor(2,14);</br>
lcd.println(" ");</br>
lcd.setCursor(2,0);</br>
delay(200);</br>
lcd.println("GET IN WITHIN:::");</br>
 
lcd.setCursor(4,1);</br>
lcd.print("5");</br>
delay(200);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
digitalWrite(buzz,LOW);</br>
delay(1000);</br>
//2</br>
digitalWrite(buzz, HIGH);</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
lcd.setCursor(4,1); //2</br>
lcd.print("4");</br>
delay(100);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
digitalWrite(buzz,LOW);</br>
delay(1000);</br>
//3</br>
digitalWrite(buzz, HIGH);</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
lcd.setCursor(4,1); //3</br>
lcd.print("3");</br>
delay(100);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
digitalWrite(buzz,LOW);</br>
delay(1000);</br>
//4</br>
digitalWrite(buzz, HIGH);</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
lcd.setCursor(4,1); //4</br>
lcd.print("2");</br>
delay(100);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN:");</br>
digitalWrite(buzz,LOW);</br>
delay(1000);</br>
//</br>
digitalWrite(buzz, HIGH);</br>
lcd.setCursor(4,1);</br>
lcd.print("1");</br>
delay(100);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.println("GET IN WITHIN::");</br>
digitalWrite(buzz,LOW);</br>
delay(1000);</br>
//5</br>
digitalWrite(buzz, HIGH);</br>
delay(40);</br>
digitalWrite(buzz,LOW);</br>
delay(40);</br>
digitalWrite(buzz, HIGH);</br>
delay(40);</br>
digitalWrite(buzz,LOW);</br>
delay(40);</br>
digitalWrite(buzz, HIGH);</br>
delay(40);</br>
digitalWrite(buzz,LOW);</br>
delay(40);</br>
digitalWrite(buzz, HIGH);</br>
delay(40);</br>
digitalWrite(buzz,LOW);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.print("RE-LOCKING");</br>
delay(500);</br>
lcd.setCursor(12,0);</br>
lcd.print(".");</br>
delay(500);</br>
lcd.setCursor(13,0);</br>
lcd.print(".");</br>
delay(500);</br>
lcd.setCursor(14,0);</br>
lcd.print(".");</br>
delay(400);</br>
lcd.clear();</br>
lcd.setCursor(4,0);</br>
lcd.print("LOCKED!");</br>
delay(440);</br>
}</br>
//*********TORTURE1***********//</br>
void torture1()</br>
{</br>
delay(1000);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.print("WAIT FOR ");</br>
lcd.setCursor(5,1);</br>
lcd.print("15 SECONDS");</br>
digitalWrite(buzz, HIGH);</br>
delay(15000);</br>
digitalWrite(buzz, LOW);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.print("LOL..");</br>
lcd.setCursor(1,1);</br>
lcd.print(" HOW WAS THAT??");</br>
delay(3500);</br>
lcd.clear();</br>
 
}</br>
//*****TORTURE2*****//</br>
void torture2()</br>
{</br>
delay(1000);</br>
lcd.setCursor(1,0);</br>
lcd.print(" ");</br>
lcd.setCursor(2,0);</br>
lcd.print("EAR DRUMS ARE");</br>
lcd.setCursor(0,1);</br>
lcd.print(" PRECIOUS!! ");</br>
delay(1500);</br>
lcd.clear();</br>
lcd.setCursor(1,0);</br>
lcd.print(" WAIT FOR");</br>
lcd.setCursor(4,1);</br>
lcd.print(" 1 MINUTE");</br>
digitalWrite(buzz, HIGH);</br>
delay(55000);</br>
counterbeep();</br>
lcd.clear();</br>
digitalWrite(buzz, LOW);</br>
lcd.setCursor(2,0);</br>
lcd.print("WANT ME TO");</br>
lcd.setCursor(1,1);</br>
lcd.print("REDICULE MORE??");</br>
delay(2500);</br>
lcd.clear();</br>
lcd.setCursor(2,0);</br>
lcd.print("Ha Ha Ha Ha");</br>
delay(1700);</br>
lcd.clear();</br>
}</br>

## RESULTS:

![door lock](https://github.com/AMANKUMAR2541/Simulation-project/assets/132323363/ed41b38c-a310-4258-8f50-ac8366fbb95b)

## CONCLUSION:

The conclusion of the project is that the password-based door lock security system using Arduino and a keypad is a viable and efficient solution for basic access control. It provides a layer of security by requiring a correct password to gain access. The use of Proteus for simulation and testing allows for validation and verification of the system's operation before actual deployment.

It is important to note that while this system offers enhanced security compared to traditional locks, it may not be suitable for high-security applications. Additional measures, such as encryption and advanced authentication methods, would be necessary for such scenarios.

Overall, the password-based door lock security system using Arduino and a keypad with Proteus integration is a valuable project that showcases the application of microcontrollers and simulation software in access control systems.

## REFERENCE:
https://www.youtube.com/watch?v=cVEBzXFESZk
