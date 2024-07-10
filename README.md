# ESP32-with-sensors
# Description 
This project shows the connection of ESP32 with sensors, once an analog sensor, and the other with a digital one. The digital sensor used is PIR motion sensor; the analog sensor in the second circuit is LDR. The projects are done in Wokwi. Wokwi is an online simulator for electronics and microcontroller projects, allowing users to design, prototype, and test their circuits without the hardware.  

# What is ESP32? 
"A feature-rich MCU with integrated Wi-Fi and Bluetooth connectivity for a wide-range of applications." The ESP32 can function as a complete standalone system or as a slave device to a host MCU, lowering communication stack overhead on the primary application CPU. ESP32's SPI / SDIO and I2C / UART interfaces allow it to communicate with other systems and enable Wi-Fi and Bluetooth features. 


![image](https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/3001665b-d256-4537-8bec-e8b582de7769)

## Circuit 1: ESP32 with Digital Sensor (PIR) 
### Components
-	ESP32 
-	LCD (16x2 with I2C)
-	PIR sensor <br />

PIR is a digital sensor that gives an output of either high or low = motion or no motion. It detects motion by sensing the infrared radiation around it. The infrared radiation around the sensor changes whenever an object moves near it. 
  
### Circuit Diagram 
The figure below shows the connection of the components to make the circuit that will print "Motion Detected" whenever the PIR sensor detects motion (change in IR), and print "No Motion" when the PIR sensor detects nothing (constant IR). 
 
<img width="481" alt="image" src="https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/07150ef6-3291-4b7f-ad6a-bdcf70627a17">

### Programming the Circuit
The code used in this project is found below. The ```LiquidCrystal_I2C``` library has to be installed for the code to function with the lcd. The PIR sensor's initial state is set as **low** to start the simulation with no detection of motion and simulating it by the user. 
```
#include <LiquidCrystal_I2C.h>   //including the lcd library
LiquidCrystal_I2C lcd(0x27, 16, 2);   //choosing lcd 16x2
            
 int pirdata =15 ;     //PIR data pin is connected to ESP pin 15
 int pirstate =LOW ;    
 int value =0;            

void setup() {
  lcd.init();        //initializing the lcd
  lcd.backlight();   
   
  pinMode(pirdata, INPUT);    //setting data from PIR as input
}

void loop() {
  value = digitalRead(pirdata);  
  if (value == HIGH) {    //when there is motion
    lcd.setCursor(0, 1);      //positioning the statement 
    lcd.print("Motion Detected!");  //printing in the lcd
    if (pirstate == LOW) {
     
      Serial.println("Motion detected!");
      
      pirstate = HIGH;
    }
  } else {    //for no motion 
    lcd.setCursor(0, 1); 
    lcd.print("No Motion"); 
    if (pirstate == HIGH) {
      
      Serial.println("No Motion");
     
      pirstate = LOW;
    }
  }
}
```
### Results 
The figure below shows the initial state of the circuit. The motion is not simulated yet; thus, the PIR detects nothing.

<img width="481" alt="image" src="https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/13dabd21-e149-47a4-9813-1bc6b14f840d">

The figure below, in the other hand, is of the circuit after the motion being simulated. 

<img width="476" alt="image" src="https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/d0522ca5-4f01-4e6e-b964-5d1ca3b7c264">


## Circuit 2: ESP32 with Analog Sensor (LDR) 
### Components
- ESP32
- LDR Sensor (photoresistor)
- 10k Ohm Resistor
- LCD (16x2 with I2C) <br />

“Photoresistor is a variable resistor whose resistance varies inversely with the intensity of light.” What happens is when light is applied to the surface of the LDR, its material absorbs the photons. These photons have energy that is transmitted into the material's electrons, and it excites them. Excited electrons mean more charge carriers; more current. Since the voltage here is constant and based on ohms low, whenever the current increases, the resistance will decrease. This explains the reason behind the inversely proportional relationship between LUX and Rldr. 

### Circuit Diagram 
The figure below shows the connection of the components that will measure the LUX by the ldr, and display the light level and the voltage in the lcd.

<img width="476" alt="image" src="https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/1642d7fe-58eb-4967-97bf-017e71bb9e14">


### Programming the Circuit
The code used in this project is found below. The ```LiquidCrystal_I2C``` library has to be installed for the code to function with the lcd.  the ESP32 reads the analog value from an LDR connected to pin 34, converting this value to a voltage that reflects the ambient light intensity. The ```analogRead``` function captures the raw sensor data. This value is then converted to a voltage by scaling it according to the ESP32's 3.3V reference voltage. In the ```loop``` function, the code reads the ldr value, calculates the corresponding voltage, and prints both values to the serial monitor for debugging. Simultaneously, the lcd displays the light level (raw ldr value) and the voltage. 
```
#include <LiquidCrystal_I2C.h>

#define LDR_PIN 34

LiquidCrystal_I2C lcd(0x27, 16, 2);   //choosing lcd 16x2

void setup() {
  lcd.init();        //initializing the lcd
  lcd.backlight();   
}

void loop() {
  int ldrValue = analogRead(LDR_PIN);
  float voltage = ldrValue * (3.3 / 4095.0);
  
  Serial.print("LDR Value: ");
  Serial.println(ldrValue);
  Serial.print(" - Voltage: ");
  Serial.println(voltage);
  
 lcd.clear();
lcd.setCursor(0, 0);
lcd.print("Light level: ");
lcd.print(ldrValue); 
  
lcd.setCursor(0, 1);
lcd.print("Volt: ");
lcd.print(voltage, 2); 

  
  delay(500);
}
```
### Results 
Two values of LUX are shown below for comparison. When the LUX increases, bot the light level and the voltage increase. 

**With 3 LUX**:

<img width="473" alt="image" src="https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/c5ae90dc-5f34-45ed-a04f-26e3750eaf92">

**With 15 LUX**:

<img width="473" alt="image" src="https://github.com/HayaBinsalim/ESP32-with-sensors/assets/173661622/30a165f0-400b-4183-9f72-778d1b1b61a3">



