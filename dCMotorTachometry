// Purpose: To measure RPM of DC motor
// Reference: https://bit.ly/2xjDf4P
// Author: A. Goldman
// Date: April 11, 2020
// Status: Working

#include <TimerOne.h>     // Includes TimerOne library
#include <Wire.h>         // Includes I2C library
#include <Adafruit_GFX.h> // Includes display libraries
#include <Adafruit_SSD1306.h>
#define SCREEN_WIDTH 128 // Defines screen width, in pixels
#define SCREEN_HEIGHT 32 // Defines screen height, in pixels
// Configures display based on given information...
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);
#define TIMER_INTERVAL 1000000    // Defines interrupt period (1s)
#define TEETH_SHIFT 3             // Defines bit shift needed for RPM
#define SECONDSPERMIN 60          // 60 seconds in a minute...
#define POTPIN A0                 // Defines pin used for reading potentiometer
uint16_t RPM;                     // Stores final RPM value
volatile uint16_t interruptCount; // Stores interrupt count
uint16_t countCopy;               // Stores copy of interrupt count
volatile uint8_t periodUp = 0;    // Stores bool.time period state

void setup()
{
    display.begin(SSD1306_SWITCHCAPVCC, 0x3C); // Begins dislay at I2C address...
    display.setTextSize(1);                    // Sets text size
    display.setTextColor(WHITE);               // Sets text colour
    // ISR everytime falling edge on pin 2...
    attachInterrupt(digitalPinToInterrupt(2), ISR_Count, FALLING);
    Timer1.initialize(TIMER_INTERVAL);    // ISR when interval up
    Timer1.attachInterrupt(ISR_PeriodUp); // Interval period
}

void ISR_Count()
{                     // Triggered on falling edge...
    interruptCount++; // Adds one to count
}

void ISR_PeriodUp()
{                 // Triggered every period (1s)...
    periodUp = 1; // Time period up; triggers code in loop
}

void loop()
{
    if (periodUp == 1)
    {                               // If period is up...
        countCopy = interruptCount; // Makes copy of counts
        interruptCount = 0;         // Sets count back to 0
        periodUp = 0;               // Resets period variable
        // Converts 1s of counts to RPM; / by eight, * by 60...
        RPM = (countCopy >> TEETH_SHIFT) * SECONDSPERMIN;
        display.clearDisplay();  // Clears display
        display.setCursor(0, 0); // Sets cursor to home
        // Prints RPM and pot value...
        display.println("RPM: " + String(RPM));
        display.println("POT: " + String(analogRead(POTPIN)));
        display.display(); // Display info...
    }
}
