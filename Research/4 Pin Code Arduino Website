/*
 * 4 Digit 7 Segment Display
 * darrencperry
 * 
 * Original source code:
 * _8x8 LED matrix
 *
 * (c) 2009 BlushingBoy.net
 */

#include "TimerOne.h"


//With this pin setup you can connect pin 1 through 12 on the display to pins 2-13 on Arduino
byte pins[16] = {2,3,4,5,6,7,8,9,10,11,12,13};
byte rows[4] = {6,8,9,12};
byte cols[8] = {11,7,4,2,1,10,5,3};

byte screen[8] = {0, 0, 0, 0, 0, 0, 0, 0};
volatile byte screenRow = 0;
volatile byte screenCol = 0;

byte numbers[11][7] = {
  {1,1,1,1,1,1,0}, //zero
  {0,0,0,0,1,1,0}, //one
  {1,1,0,1,1,0,1}, //two
  {1,0,0,1,1,1,1}, //three
  {0,0,1,0,1,1,1}, //four
  {1,0,1,1,0,1,1}, //five
  {1,1,1,1,0,1,1}, //six
  {0,0,0,1,1,1,0}, //seven
  {1,1,1,1,1,1,1}, //eight
  {1,0,1,1,1,1,1}, //nine
  {0,0,0,0,0,0,0}  //off
};

void setup() {

  Timer1.initialize(100);
  for (int i = 0; i < 16; i++)
    pinMode(pins[i], OUTPUT);
  Timer1.attachInterrupt(doubleBuffer);

}

// interrupt routine
void doubleBuffer() {
  // reset the previous iteration
  digitalWrite(translatePin(rows[screenRow]), HIGH); // set previous off
  digitalWrite(translatePin(cols[screenCol]), LOW);  // set previous off
  // go to the next iteration...
  // go to the next screenCol, wrap if necessary
  screenCol++;
  if (screenCol >= 8) {
    screenCol = 0;
    // when screenCol wraps, go to the next screenRow, wrap if necessary
    screenRow++;
    if (screenRow >= 4) {
      screenRow = 0;
    }
  }
  // set this iteration
  if((screen[screenRow]>>screenCol)&B1 == B1) {
    digitalWrite(translatePin(rows[screenRow]), LOW);  // set this on
    digitalWrite(translatePin(cols[screenCol]), HIGH); // set this on
  } else {
    digitalWrite(translatePin(rows[screenRow]), HIGH); // set this off
    digitalWrite(translatePin(cols[screenCol]), LOW);  // set this off
  }

}

byte translatePin(byte original) {
  return pins[original - 1];
}

void allOFF() {
  for (int i = 0; i < 8; i++)
    screen[i]=0;
}

void on(byte row, byte column) {
    screen[column-1] |= (B1<<(row-1)); 
}

void off(byte row, byte column) {
    screen[column-1] &= ~(B1<<(row-1)); 
}

void displayNumbers(byte time[4]) {
  for (int digit = 0; digit < 4; digit++) {
    for (int i = 0; i < 7; i++) {
      if (numbers[time[digit]][i] == 1) on(i+1,digit+1);
        else off(i+1,digit+1);
    }
  }
}

void decimals(byte bottom, byte top) {
  if(bottom) on(8,2);
  else off(8,2);
  if(top) on(8,3);
  else off(8,3);
}

void loop() {
  //just some randoms numbers for testing
  byte nums[4] = {random(12)-1,
                  random(12)-1,
                  random(12)-1,
                  random(12)-1};
  displayNumbers(nums);

  decimals(random(2)-1,random(2)-1);
  delay(1000);
}
