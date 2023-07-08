// Include the necessary libraries
#include <AFMotor.h>

// Define the motion sensor pin
#define MOTION_SENSOR_PIN 2

// Define the motor driver pins
#define MOTOR_PIN_1 4
#define MOTOR_PIN_2 5

// Create an instance of the motor driver
AF_DCMotor motor(1);

// Variable to store motion state
volatile bool motionDetected = false;

void setup() {
  // Initialize the serial communication
  Serial.begin(9600);
  
  // Configure the motion sensor pin as input
  pinMode(MOTION_SENSOR_PIN, INPUT);
  
  // Attach an interrupt to the motion sensor pin
  attachInterrupt(digitalPinToInterrupt(MOTION_SENSOR_PIN), motionInterrupt, CHANGE);
  
  // Set up the motor pins
  motor.setSpeed(255); // Set the motor speed (0-255)
  pinMode(MOTOR_PIN_1, OUTPUT);
  pinMode(MOTOR_PIN_2, OUTPUT);
}

void loop() {
  if (motionDetected) {
    // Activate the motor for 5 seconds
    activateMotor(5000);
    
    // Reset the motion state
    motionDetected = false;
  }
}

void activateMotor(unsigned long duration) {
  // Set the motor direction (example: clockwise)
  digitalWrite(MOTOR_PIN_1, HIGH);
  digitalWrite(MOTOR_PIN_2, LOW);
  
  // Start the motor
  motor.run(FORWARD);
  
  // Wait for the specified duration
  delay(duration);
  
  // Stop the motor
  motor.run(RELEASE);
}

void motionInterrupt() {
  // Read the motion sensor state
  int motionState = digitalRead(MOTION_SENSOR_PIN);
  
  // Check if motion is detected
  if (motionState == HIGH) {
    Serial.println("Motion detected!");
    motionDetected = true;
  }
}
