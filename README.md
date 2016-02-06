#include <AFMotor.h> //import your motor shield library
#include <Servo.h>
#define trigPin 13 // define the pins of your sensor
#define echoPin 11 
AF_DCMotor motor2(2,MOTOR12_8KHZ); // set up motors.
AF_DCMotor motor3(3, MOTOR34_8KHZ);
Servo myservo;

void setup() {
  Serial.begin (9600);
    myservo.attach(7);

  Serial.begin(9600); // begin serial communitication  
  Serial.println("Motor test!");
   pinMode(trigPin, OUTPUT);// set the trig pin to output (Send sound waves)
  pinMode(echoPin, INPUT);// set the echo pin to input (recieve sound waves)
  motor2.setSpeed(255); //set the speed of the motors, between 0-255
motor3.setSpeed (225);  
}

void sweepServo(){
        myservo.write(180);
	delay(200);
	myservo.write(90);
	delay(100);
        myservo.write(90);
	delay(100);
	myservo.write(1);
	delay(100);
        myservo.write(1);
	delay(100);
	myservo.write(90);
	delay(100);
        myservo.write(90);
	delay(100);
	myservo.write(180);
        delay(100);
        myservo.write(180);
	delay(100);
	myservo.write(90);
	delay(1000);

}

 
void loop() {
  sweepServo();
   long duration, distance; // start the scan
  digitalWrite(trigPin, LOW);  
  delayMicroseconds(2); // delays are required for a succesful sensor operation.
  digitalWrite(trigPin, HIGH);

  delayMicroseconds(10); //this delay is required as well!
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;// convert the distance to centimeters.
  if (distance < 25)/*if there's an obstacle 25 centimers, ahead, do the following: */ {   
   Serial.println ("Close Obstacle detected!" );
Serial.println ("Obstacle Details:");
Serial.print ("Distance From Robot is " );
Serial.print ( distance);
Serial.print ( " CM!");// print out the distance in centimeters.

Serial.println (" The obstacle is declared a threat due to close distance. ");
Serial.println (" Turning !");
    motor2.run(FORWARD);  // Turn as long as there's an obstacle ahead.
    motor3.run (BACKWARD);

}
  else {
   Serial.println ("No obstacle detected. going forward");
   delay (15);
   motor2.run(FORWARD); //if there's no obstacle ahead, Go Forward! 
    motor3.run(FORWARD);  
  }  
  
  

  
  
  
 
}
