![[Pasted image 20220925083149.png]]

1. Microcontroller: The brain of the arduino
2. USB port: Connect the arduino to the computer
3. USB to serial chip: Helps translate data from the computer to the microcontroller
4. Digital pins: Pins that use digital logic (0,1 or HIGH, LOW), commonly used for switches
5. Analog pins: pins that can read analog values, 10 bit resolution (0-1023)
6. 5V/3.3V: These pins are used to power components
7. GND: ground/negative it's used to complete a circuit, where the electrical level is at 0 volt
8. VIN: Voltage in, connect external power supplies.

## Circuit basics
![[Pasted image 20220925083748.png]]

A simple example of a circuit, is an **LED circuit**. A wire is connected from a pin on the Arduino, to an LED via a resistor (to protect the LED from high current), and finally to the ground pin (GND). When the pin is set to a **HIGH state**, the microcontroller on the Arduino board will allow an electric current to flow through the circuit, which turns on the LED. When the pin is set to a **LOW state**, the LED will turn off, as an electric current is not flowing through the circuit.

![[Pasted image 20220925083826.png]]

## Electronic signals

### Analog signal
![[Pasted image 20220925083921.png]]
It is bound to a range, in Arduino: 0-5 V or 0-3,3 V
Read more about this: [analog inputs](https://docs.arduino.cc/learn/microcontrollers/analog-input), [analog outputs](https://docs.arduino.cc/learn/microcontrollers/analog-output)


### Digital signal
![[Pasted image 20220925084043.png]]

It only represents 2 binary states, 1 and 0.  This is the most common signal type.
It can be very easily read.
You can make a sequence of them to transmit data. For example
```binary
101101
101110001110011
```
becomes:
```decimal
45
23667
```

For this, we use [Serial communication protocols](https://docs.arduino.cc/learn/starting-guide/getting-started-arduino#serial-communication-protocols)

## Sensors and actuators

### What is a sensor?
It is used to sense it's environment and record a physical parameter like temperature, and send it as a digital signal.

It can also just be a simple button.

The easiest way to use them is to use an analog pin.

There are also digital sensors, that are a little more advanced, requiring Serial Communication protocols, and it requires more effort to translate the data. A lot of sensors have software libraries for easier use of them.
In many cases it's just a line of code.
```arduino
sensorValue = sensor.read();
```

### What is an actuator?

An actuator, in simple terms, is used to _actuate_ or _change_ a physical state. For example: 
- A light
- A motor
- A switch

They convert electrical signals into light or mechanical energy.

To control them, we can use `analogWrite()` or `digitalWrite()` in this way:

```arduino
digitalWrite(LED, HIGH);
digitalWrite(LED, LOW);

analogWrite(motor, 255); //set a motor to max capacity
analogWrite(motor, 25); // set a motor to 10% of it's capacity

```

## Input and Output
Sensors and Actuators, are usually refered to as input and output.
If we wanted to make a program checking if a led is turned on or off:
```arduino
int buttonState = digitalRead(buttonPin); // read and store state

if (buttonState == HIGH) {
	digitalWrite(LED, HIGH);
}
else {
	digitalWrite(LED, LOW);
}
```

## Example program: 
```arduino
/* 
This is a comment at the top of a program, 
it will not be recognized as code. Very good 
to add an explanation of what your code does 
here.

This sketch shows how to read a value from a
sensor connected to pin A1, print it out in 
the Serial Monitor, and turn on an LED connected
to pin number 2 if a conditional is met.
*/

int sensorPin = A1; //define pin A1 (analog pin)
int ledPin = 2; //define pin 2 (digital pin)
int sensorValue; //create variable for storing readings

//void setup is for configurations on start up
void setup() { 
    Serial.begin(9600); //initialize serial communication
    pinMode(ledPin, OUTPUT); //define ledPin as an output
}

void loop() {
    sensorValue = analogRead(sensorPin); // do a sensor reading
    
    Serial.print("Sensor value is: "); //print a message to the serial monitor
    Serial.println(sensorValue); //print the value to the serial monitor
    
    //check if sensorValue is below 200
    if(sensorValue < 200) { 
        digitalWrite(ledPin, HIGH); //if it is, turn on the LED on pin 2.
    }
    //if sensorValue is above 200, turn off the LED
    else{ 
        digitalWrite(ledPin, LOW);
    }
}
```

