Video of the Tach Working 
https://youtu.be/ANwjYZHo20g


To use this you will need 
- 9g servo (common rc plane ones usually blue) https://www.amazon.com/s?k=9g+servos&ref=nb_sb_noss
- a 10k pot(for now this isnt actually used its more of a pivot point, was planning on using it but didnt need to. 
- the 3d printed files https://www.thingiverse.com/thing:4899148 ( or zip file here) 
- 5mm screw and 2x 4 mm screw ( this will change to all the same jsut made a mistake at first)

Assembly:
pretty easy, 
- The servo goes thru the slot then you attach the big gear to the servo shoudl be a nice press fit. 
- the pot goes thru the hole inbetween the two closer screws, then the smaller gear on the end of the pot. 
- screw that assembly to the face and pop the needle on, make sure that your pot is turned allthe way anti clockwise when installing(the pins of the pot should be faceing the 5oclock on the face). 
- as far as wiring its just the servo, you shoudl only need 5v, gnd and the signal wire from the servo goes to pin 9(I am using nano, if you want to use another pin just change it below  in the ``` tach.attach(9); ``` to whatever pin you are using. 
- done. 

Getting it working:
- open simhub, connect your arduino, go to the arduino setup tool, click file then open in arduino ide. 
- this will open the ide with all the files simhub uses, you need to look at the name of the files on the top and find SHCustomProtocol.h (should be the tenth file) 
- after this line ```#include <Arduino.h>``` add 
``` #include <Servo.h>```

- now scroll down till you see ```void setup()``` above this you need to paste the following
 ```
 Servo tach;
 int pos = 0;  

long map3( long x, long in_min, long in_max, long out_min, long out_max )
{
  long out_range;
  long in_range;

  out_range = out_max - out_min;
  if ( out_range > 0 )
    ++out_range;
  else if ( out_range < 0 )
    --out_range;
  else
    return( 0 );

  in_range = in_max - in_min;
  if ( in_range > 0 )
    ++in_range;
  else if ( in_range < 0 )
    --in_range;
  else
    // Is actually infinity but long has no such thing.  The least negative long is another choice.
    return( 0 );

  return (x - in_min) * (out_range) / (in_range) + out_min;
}
```
- within the ```void setup()``` function we need to add 
``` tach.attach(9); ```

- and then the next function down shoudl be ```void read()``` in this function paste 
   ``` int rpm = FlowSerialReadStringUntil('\n').toInt();
    pos = map3(rpm,0,10000,0,107);
    FlowSerialDebugPrintLn("RPM : " + String(rpm));```
    
- and lastly in the ```void loop()``` function we add  ```tach.write(pos);``` 
- now you can click file->save. 
- You can either click upload here just make sure you correct board and com port are choosen, or close and upload using the simhub tool. 
- It should be autopicked up by simhub and start revving to the moon. 
   
