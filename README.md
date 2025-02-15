# Autonomous-Pothole-Detection-and-Mapping-Vehicle
Autonomous Pothole Detection and Mapping Vehicle is an autonomous and efficient robotic system that uses sensors, GPS, and Arduino and can operate in urban environments with the primary goal of identifying and avoiding potholes.

#include <NewPing.h>
#include <AFMotor.h>
//hc-sr04 sensor
#define TRIGGER_PIN A4 //grey
#define ECHO_PIN A5 //blue
#define max_distance 50
//ir sensor
#define irLeft A2
#define irRight A0
//motor
#define MAX_SPEED 200
#define MAX_SPEED_OFFSET 20
NewPing sonar(TRIGGER_PIN, ECHO_PIN, max_distance);
AF_DCMotor motor_left(1, MOTOR12_1KHZ);
AF_DCMotor motor_right(4, MOTOR12_1KHZ);
int distance = 0;
void setup()
{
Serial.begin(9600);
pinMode(irLeft, INPUT);
pinMode(irRight, INPUT);
motor_left.setSpeed(200);
motor_right.setSpeed(200);

}
//line follow
void loop()
{
if(digitalRead(irLeft) ==1 && digitalRead(irRight) ==1)
{
objectAvoid();
//forword
}
else if(digitalRead(irLeft) ==1 && digitalRead(irRight) ==0)
{
objectAvoid();
Serial.println("TL");
//leftturn
moveLeft();
}
else if(digitalRead(irLeft) ==0 && digitalRead(irRight) ==1)
{
objectAvoid();
Serial.println("TR");
//rightturn
moveRight();
}
else if(digitalRead(irLeft) ==0 && digitalRead(irRight) ==0)
{
//Stop
Stop();
}
}
//object avoiding
void objectAvoid()
{
distance = getDistance();
if (distance >= 10)
{
//stop
Stop();
Serial.println("Stop");

turn();
Serial.println("moveLeft");
}
else
{
//forword
Serial.println("moveforword");
moveForward();
}
}
int getDistance()
{
delay(50);
int cm = sonar.ping_cm();
if (cm == 0)
{
cm = 100;
}
return cm;
}
void Stop() {
motor_left.run(RELEASE);
motor_right.run(RELEASE);
}
void moveForward() {
motor_left.run(FORWARD);
motor_right.run(FORWARD);
}
void moveBackward() {
motor_left.run(BACKWARD);
motor_right.run(BACKWARD);
}
void turn()
{
Serial.println("turn left");

moveLeft();
delay(700);
moveForward();
delay(800);
moveRight();
delay(800);
moveForward();
delay(800);
moveRight();
delay(800);
moveForward();
if (digitalRead(irRight) == 0)
{
loop();
}
else
{
moveForward();
}
}
void moveLeft()
{
motor_left.run(BACKWARD);
motor_right.run(FORWARD);
}
void moveRight()
{
motor_left.run(FORWARD);
motor_right.run(BACKWARD);
}

