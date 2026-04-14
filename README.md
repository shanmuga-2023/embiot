# embiot
PROGRAM – Square of a Number

MOV R0, #02H

MOV A, R0

MOV B, A

MUL AB

MOV r1, A

MOV A, B

MOV r2, A

L1: SJMP L1

END

PROGRAM – Cube of a Number

MOV A, #0FFH

MOV B, A

MOV R1, A

MUL AB

MOV R2, B

MOV B, R1

MUL AB

MOV R4, A

MOV R3, B

MOV B, R2

MOV A, R1

MUL AB

ADD A, R3

MOV R5, A

MOV A, #00H

ADDC A, B

MOV R6, A //Answer is in R6, R5, and R4 (FD02FF)

L1: SJMP L1

END

PROGRAM – 1’S Compliment & 2’S Compliment

MOV R0, #03H

MOV A, R0

CPL A

MOV R1, A

ADD A, #01H

MOV R2, A

L1: SJMP L1

END



ex2



ORG 0000H



MOV R1, #05H        ; 

MOV DPTR, #2500H    ; 

MOV R0, #75H        ; 



HERE: MOV A, R0     ; 

      MOVX @DPTR, A ; 

      INC DPTR      ; 

      INC R0        ; 

      DJNZ R1, HERE ; 



; Read back values

MOV DPTR, #2500H



MOVX A, @DPTR

MOV R2, A



INC DPTR

MOVX A, @DPTR

MOV R3, A



INC DPTR

MOVX A, @DPTR

MOV R4, A



INC DPTR

MOVX A, @DPTR

MOV R5, A



INC DPTR

MOVX A, @DPTR

MOV R6, A



END



x3

ADDITION

CLR C

MOV R7, #00H

MOV R1, #0FFH

MOV A, R1

MOV R2, #01H

ADD A, R2

JNC L2

INC R7

L2: MOV R3, A

L1: SJMP L1

END

SUBTRACTION

CLR C

MOV R7, #00H

MOV R1, #0FFH

MOV A, R1

MOV R2, #01H

SUBB A, R2

JNC L2

INC R7

L2: MOV R3, A

L1: SJMP L1

END

MULTIPLICATION

MOV A, #02H

MOV B, #04H

MUL AB

MOV R0, A

MOV A, B

MOV R1, A

L1: SJMP L1

ENDDIVISION

MOV A, #0AH

MOV B, #02H

DIV AB

MOV R0, A

MOV A, B

MOV R1, A

L1: SJMP L1

END

LOGICAL AND

MOV R7, #0FH

MOV A, R7

MOV R1, #0AH

ANL A, R1

MOV R0, A

L1: SJMP L1

END

LOGICAL OR

MOV R7, #0FH

MOV A, R7

MOV R1, #0AH

ORL A, R1

MOV R0, A

L1: SJMP L1

END

LOGICAL EX-OR

MOV R7, #0FH

MOV A, R7

MOV R1, #0AH

XRL A, R1

MOV R0, A

L1: SJMP L1

END

ex4

Program to toggle all the bits of Port

Specification:

time delay-250ms

port used:Port1

#include <reg51.h> // include 8051 register file

void MSDelay(unsigned int); //delay routine definition

void main( )

{

while (1) //repeat forever

{

P1= 0x55;

MSDelay(250);

P1= 0xAA;

MSDelay(250);

}

}

// delay routine implementation

void MSDelay(unsigned int itime)

{

unsigned int i, j;

for (i = 0; i < itime; i++)

for (j = 0; j< 1275; j++);

}

2. Program to generate a square wave using Timer

Specification:

Frequency: 10Hz

Port used: P1.0

Timer- T0, mode-1

#include <reg51.h> // include 8051 register file

sbit pin = P1^0; // declare a variable type sbit for P1.0

main()

{

P1 = 0x00; // clear port

TMOD = 0x09; // initialize timer 0 as 16 bit timer

loop: TL0 = 0xAF; // load value 15535 = 3CAFh so after

TH0 = 0x3C; // 50000 counts timer 0 will be overflow

pin = 1; // send high logic to P1.0

TR0 = 1; // start timer

while(TF0 == 0)

{ } // wait for first overflow for 50 ms

TL0 = 0xAF; // again reload count

TH0 = 0x3C;

pin = 0; // now send 0 to P1.0

while(TF0 == 0)

{} // wait for 50 ms again

goto loop; // continue with the loop

}

3. Program to transfer the letter “ECE” serially.

Specifications:

Baud rate: 9600 baud.

Mode: Use 8-bit data and 1 stop bit- mode-1

# include <reg51.h>

void sendChar(unsigned char);

void main(void)

{

TMOD=0x20; //use Timer 1, mode 2

TH1=0xFD; //9600 baud rate

SCON=0x50;

TR1=1; //start timer

while (1)

{

sendChar(‘E’);

sendChar(‘C’);

sendChar(‘E’);

}

}

void sendChar(unsigned char x)

{

SBUF = x; //place value in buffer

while (TI = = 0); //wait until transmitted

TI = 0;

}



ex5

PROGRAM 1: FLASHING OF LED

const int LED1 = 2;

const int LED2 = 4;

void setup()

{

pinMode(LED1, OUTPUT);

pinMode(LED2, OUTPUT);

}

void loop()

{

digitalWrite(LED1, HIGH);

digitalWrite(LED2, LOW);

delay(500);

digitalWrite(LED1, LOW);

digitalWrite(LED2, HIGH);

delay(500);

}



const int sensorPin = A0;



void setup() {

  Serial.begin(9600);

}



void loop() {

  int sensorValue = analogRead(sensorPin);

  float voltage = sensorValue * (5.0 / 1023.0);

float temperature = voltage * 100;



  Serial.print("Temperature: ");

  Serial.print(temperature);

  Serial.println(" °C");



  delay(1000);

}



ex 6 blueetooth

#include "BluetoothSerial.h"



BluetoothSerial SerialBT;



const int LED = 2;   // LED pin



void setup() {

  Serial.begin(115200);

  SerialBT.begin("ESP32test");  // Bluetooth name



  pinMode(LED, OUTPUT);



  Serial.println("Bluetooth started");

  Serial.println("Send 'a' to turn ON LED");

  Serial.println("Send 'b' to turn OFF LED");

}



void loop() {

  if (SerialBT.available()) {

    char data = SerialBT.read();



    if (data == 'a') {

      digitalWrite(LED, HIGH);

      Serial.println("LED ON");

      SerialBT.println("LED ON");

    }

    else if (data == 'b') {

      digitalWrite(LED, LOW);

      Serial.println("LED OFF");

      SerialBT.println("LED OFF");

    }

  }

}

x7

import RPi.GPIO as GPIO

import time

GPIO.setwarnings(False)

GPIO.setmode(GPIO.BOARD)

GPIO.setup(11, GPIO.IN)

while True:

i=GPIO.input(11)

if i==0:

print("No intruders",i)

time.sleep(0.1)

elif i==1:

print("Intruder detected",i)

time.sleep(0.1)

ex8a

DHT 11 Sensor (Temperature and Humidity Sensor)

import Adafruit_DHT

sensor = Adafruit_DHT.DHT11

pin = 4

while True:

humidity, temperature = Adafruit_DHT.read_retry(sensor, pin)

print('Temp={0:0.1f}*C Humidity={1:0.1f}%'.format(temperature, humidity))

ex8b

importsmbus

import time

address = 0x48

bus = smbus.SMBus(1)

address = 0x48

ldr_add = 0x42

while True:

bus.write_byte(address,ldr_add)

value = bus.read_byte(address)

print(value)

time.sleep(0.1)

ex8b2

mport smbus

import time

address = 0x48

bus = smbus.SMBus(1)

address = 0x48

lm35_add = 0x40

while True:

bus.write_byte(address,lm35_add)

value = bus.read_byte(address)

v=value*(3.3/255.0)

v=round(v,2)



ex9

import socket

import time



message = "Hi ESP32"

server_address = ("192.168.4.1", 2000)



client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)



while True:

    client.sendto(message.encode(), server_address)

    print("Message Sent")

    time.sleep(1)



#include <WiFi.h>

#include <WiFiUdp.h>



const char* ssid = "Test";

const char* password = "Password123";



WiFiUDP udp;

char packetBuffer[255];

unsigned int localPort = 2000;



void setup() {

  Serial.begin(115200);



  WiFi.softAP(ssid, password);

  Serial.println("WiFi AP Started");



  udp.begin(localPort);

  Serial.println("UDP Server Started");

}



void loop() {

  int packetSize = udp.parsePacket();



  if (packetSize) {

    int len = udp.read(packetBuffer, 255);

    if (len > 0) {

      packetBuffer[len] = '\0';

    }



    Serial.print("Received Data: ");

    Serial.println(packetBuffer);

  }

}



ex10

#include "UbidotsEsp32Mqtt.h"



const char* TOKEN = "YOUR_TOKEN";

const char* WIFI_SSID = "Home";

const char* WIFI_PASS = "Password123";



Ubidots ubidots(TOKEN);



void setup() {

  Serial.begin(115200);

  pinMode(2, OUTPUT);



  ubidots.connectToWifi(WIFI_SSID, WIFI_PASS);

  ubidots.setup();

}



void loop() {

  float ldr = analogRead(39);

  float temp = analogRead(36);



  ubidots.add("ldr", ldr);

  ubidots.add("temperature", temp);



  ubidots.publish("esp32");



  delay(2000);

}

ex11

import Adafruit_DHT

import requests

import time



TOKEN = "YOUR_TOKEN"

DEVICE = "RPI4"



sensor = Adafruit_DHT.DHT11

pin = 4



while True:

    humidity, temperature = Adafruit_DHT.read_retry(sensor, pin)



    data = {

        "temperature": temperature,

        "humidity": humidity

    }



    url = f"http://industrial.api.ubidots.com/api/v1.6/devices/{DEVICE}"

    headers = {"X-Auth-Token": TOKEN}



    requests.post(url, headers=headers, json=data)



    print("Data Sent:", data)

    time.sleep(2)



ex12

#include "UbidotsEsp32Mqtt.h"



const char* TOKEN = "YOUR_TOKEN";

const char* WIFI_SSID = "Home";

const char* WIFI_PASS = "Password123";



Ubidots ubidots(TOKEN);



void setup() {

  Serial.begin(115200);

  pinMode(2, OUTPUT);

  pinMode(4, OUTPUT);

  pinMode(12, OUTPUT);



  ubidots.connectToWifi(WIFI_SSID, WIFI_PASS);

  ubidots.setup();

}



void loop() {

  float ldr = analogRead(39);

  float temp = analogRead(36);



  // LDR control

  if (ldr < 2000) digitalWrite(4, HIGH);

  else digitalWrite(4, LOW);



  // Temperature control

  if (temp > 2000) digitalWrite(12, HIGH);

  else digitalWrite(12, LOW);



  ubidots.add("ldr", ldr);

  ubidots.add("temperature", temp);



  ubidots.publish("esp32");



  delay(2000);

}



#include <WiFi.h>

#include <WiFiUdp.h>



const char* ssid = "Test";

const char* pass = "12345678";



WiFiUDP udp;

char buffer[255];



void setup() {

  Serial.begin(115200);

  WiFi.softAP(ssid, pass);



  udp.begin(2000);

  Serial.println("UDP Started");

}



void loop() {

  int size = udp.parsePacket();



  if (size) {

    int len = udp.read(buffer, 255);

    buffer[len] = 0;



    Serial.print("Received: ");

    Serial.println(buffer);

  }

}



