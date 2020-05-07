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

------------------------- ROM --------------------------

MSB                                                  LSB
↓                                                      ↓
A0, A1, A2, A3, A4, A5, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4


--- OUT ---

  MSB LSB
   3   2

## License

MIT
