Arduino-2bit-CPU-emulate
====

## Overview

This program is a pseudo emulation of a 2bit CPU with Arduino. ROM is reproduced with 16 digital input pins. Data is output on two output pins.

## Program

````cpp:example.ino
byte romPin[16] = {A0, A1, A2, A3, A4, A5, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4};

byte n = 0;
unsigned int rgst;
unsigned int pgm = 0;
byte calc;

void setup() {
  for(int i = 0; i < 16; i++){
    pgm = (pgm << 1) | dalRead(romPin[i]);
  }
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
}

void loop() {
  calc = (pgm >> ((3 - n) * 4)) & 0b1111;
  if(calc >> 2 == 0){
    rgst = calc & 0b11;
  }else if(calc >> 2 == 1){
    digitalWrite(3, rgst >> 1);
    digitalWrite(2, rgst & 0b01);
  }else if(calc >> 2 == 2){
    rgst = (rgst + calc & 0b11) % 4;
  }else if(calc >> 2 == 3){
    n = calc & 0b11;
  }
  if(calc >> 2 != 3){
    n = (n + 1) % 4;
  }
  delay(500);
}
````

## Usage

### Command

00: Write immediate data to register  
01: Output register data to output port  
10: Add immediate data to register data  
11: Jump to the address of immediate data

### ROM

MSB <--&nbsp;&nbsp;&nbsp;--> LSB  
A0, A1, A2, A3, A4, A5, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4


### OUT

MSB, LSB
3, 2

### Example

program01 count
MSB <--&nbsp;&nbsp;&nbsp;--> LSB
0000 0100 1001 1101

program02 blink
MSB <--&nbsp;&nbsp;&nbsp;--> LSB
0001 0100 0010 0100

## License

MIT
