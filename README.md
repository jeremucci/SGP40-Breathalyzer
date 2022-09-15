# SGP40-Breathalyzer
 Breathalyzer prototype, using a SGP40 air quality sensor (V1.0) and an ESP-32 Board with built in wifi &amp; bluetooth. 

Code:

 #include <Wire.h>
#include "Adafruit_SGP40.h"

Adafruit_SGP40 sgp;

#include <Adafruit_NeoPixel.h>
#ifdef AVR
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif

// Which pin on the Arduino is connected to the NeoPixels?
#define PIN        19 // On Trinket or Gemma, suggest changing this to 1

// How many NeoPixels are attached to the Arduino?
#define NUMPIXELS 9 // Popular NeoPixel ring size

#define DELAYVAL 500 // Time (in milliseconds) to pause between pixels

// When setting up the NeoPixel library, we tell it how many pixels,
// and which pin to use to send signals. Note that for older NeoPixel
// strips you might need to change the third parameter -- see the
// strandtest example for more information on possible values.
Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

int color;

void setup() {
  Serial.begin(115200);
  while (!Serial) { delay(10); } // Wait for serial console to open!

  Serial.println("SGP40 test");

  if (! sgp.begin()){
    //Serial.println("Sensor not found :(");
    //while (1);
  }
  /*
  Serial.print("Found SGP40 serial #");
  Serial.print(sgp.serialnumber[0], HEX);
  Serial.print(sgp.serialnumber[1], HEX);
  Serial.println(sgp.serialnumber[2], HEX);
*/
  pixels.begin(); // INITIALIZE NeoPixel strip object (REQUIRED)

  pixels.setPixelColor(8, pixels.Color(0, 150, 0));

  pixels.show();   // Send the updated pixel colors to the hardware.
  
}

int counter = 0;

void loop() {
  uint16_t raw;
  
  raw = sgp.measureRaw();

  Serial.print("Measurement: ");
  Serial.println(raw);

  pixels.clear(); // Set all pixel colors to 'off'

  if (raw>24000) pixels.setPixelColor(8, pixels.Color(0, 150, 0));
  if (raw<=24000) pixels.setPixelColor(8, pixels.Color(255, 0, 0));
  
  pixels.show();   // Send the updated pixel colors to the hardware.
  
  delay(1000);
}
