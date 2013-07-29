---
layout: post
title:  "arduino evening"
date:   2013-07-28
categories: arduino 
---

I got to play around with Arduino for the evening, thanks to Daphne
taking a weekend class.

I'd been interested in Arduino for a while, but never got one myself.

Using the hour summary of her class and the code samples built into the
Arduino app, I cobbled together my first two projects.

Looking through the [reference part][arduino-reference] of the Arduino site, I quickly noticed the Fritzing images that show a layout of the Arduino and the breadboard.  The Fritzing app (download from [fritzing.org][fritzing-download]) is pretty simple, so after the fact, I made the sketches.  For Arduino, switching the breadboard layout is fast so I can't imagine really needing Fritzing for prototyping.  That said, I'm sure for boards where soldering is involved, the Fritzing app could be valuable.

#Blink Speed

*Idea*: A knob (technically known as a potentiameter) controls the speed
of a blinking led.

*Fritzing*:

<div><img src="/images/blinkspeed_board.jpg" alt="Blink Speed Fritzing layout" style="width: 300px;"/></div>

*Arudino Sketch*:

{% highlight c++ linenos %}
// start blink vars
int sensorPin = A3;    // input pin for the potentiometer
int ledPin = 7;      // pin for the LED
// end

void setup() {
 Serial.begin(9600);
 
 // declare the ledPin as an OUTPUT:
 pinMode(ledPin, OUTPUT); 
}

void blinkLoop(int delayTime){
  // turn the ledPin on
  digitalWrite(ledPin, HIGH);  
  // stop the program for <sensorValue> milliseconds:
  delay(delayTime);          
  // turn the ledPin off:        
  digitalWrite(ledPin, LOW);   
  // stop the program for for <sensorValue> milliseconds:
  delay(delayTime);
}

void loop(){
  // read the value from the sensor:
  int sensorValue = analogRead(sensorPin);
  Serial.print("Potentiometer value: ");
  Serial.println(sensorValue);
  blinkLoop(sensorValue);
}
{% endhighlight %}

#Odds or Evens

*Idea*: A game of Odds or Evens. The button starts the game off.  The
three leds flash to signal the start of the game.  Human decides odds or
evens.  The leds start a 3-2-1 countdown then throw a '1' (one led) or a
'2' (two leds).  At the same time, human throws their one or two
fingers. The odd- or even-ness of the added result determines the
winner.

*Fritzing*:

<div><img src="/images/odds_evens_board.jpg" alt="Odds or Evens Fritzing layout" style="width: 300px;"/></div>

*Arduino sketch*:

{% highlight c++ linenos %}
// hardware vars
int leds[] = { 5, 6, 7 };
int lengthOfLeds = 3;
int button = 3;

int odd = 1;
int even = 2;

int timer = 1000;

void setup() {
  // only needed for debugging
  // Serial.begin(9600);

  // declare the leds as OUTPUTs:
  for ( int index; index < lengthOfLeds; index++ ) {
    pinMode( leds[index], OUTPUT );
  }

  // declare the button as INPUT:
  pinMode( button, INPUT );

}

void playGame() {
  flashLights();
  delay(timer);
  countDownLights();
  delay( timer / 2 );
  int randy = generateRand( odd, even );
  switch (randy) {
  case 1:
    digitalWrite(leds[2], HIGH);
    // Serial.println("It was 1!");
    break;
  case 2:
    digitalWrite(leds[1], HIGH); 
    digitalWrite(leds[2], HIGH);
    // Serial.println("It was 2!");
    break;
  }
  delay(timer + 5000);
  resetLights();
  delay(timer);
}

void lightController( boolean ascending = true, boolean turnOn = HIGH,
int pause = 0 ) {
  switch (ascending) {
  case true:
    for ( int index = 0; index < lengthOfLeds; index++ ) {
      digitalWrite(leds[index], turnOn);
      delay(pause);
    }
  case false:
    for ( int index = lengthOfLeds - 1; index >=0; index-- ) {
      digitalWrite(leds[index], turnOn);
      delay(pause);
    }
  }
}

void flashLights() {
  lightController();
}

void countDownLights() {
  lightController( false, LOW, 500 );
}

void resetLights() {
  lightController( false, LOW );
}

int generateRand(int rmin, int rmax) {
  int randNumber;
  randomSeed(analogRead(0));
  randNumber = random( rmin, rmax + 1 );
  return randNumber;
}

void loop() {
  // read the input pin:
  int buttonState = digitalRead(button);
  // print out the state of the button:
  // Serial.println(buttonState);

  if ( buttonState == 1 ) {
    playGame();
  }
  delay(1);        // delay in between reads for stability
}
{% endhighlight %}

*Blog meta note*: For Arduino sketch code, I used Pygments' c++ lexar.
Seems more accurate than the c lexar.

[arduino-reference]: http://arduino.cc/en/Reference/HomePage
[fritzing-download]: http://fritzing.org/download/
