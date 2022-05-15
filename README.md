# dig333finalproject

My milestones 1 and 2 have kind of blended together and has been a more drawn out process than initially anticipated. I'm still in the process of figuring out how to use the GrovePi attachment with my Raspberry Pi, which will allow me to make use of the sensors that I need for my project. I've almost gotten both the light sensor and audio sensor set up, but I'm still troubleshooting some compatability issues between the Raspberry Pi and GrovePi, especially in the category of software. Hopefully in the next few days I'll have everything sorted out, which will allow me to start linking the data I receive from the sensors to manipulate an audio source. 

Milestone 3:
With the help of Dr. Mundy, I've successfully been able to get data from my GrovePi light sensor. I'm now doing research on capturing and playing back live audio from the WALT radio web stream. (https://embed.radio.co/player/d84812b.html?popout)

![Image 4-27-22 at 1 12 PM](https://user-images.githubusercontent.com/98902048/165582164-beb5b97b-be78-4b85-bbfe-bf42091e46f6.jpg)
 
I am currently in the process of downloading and installing RuneAudio, which should allow me to capture a radio stream, as well as SonicPi, which I believe allows for audio manipulation.



Final Deliverables:

Plugins:
 -MPG123 webstream audio player
 -Expect Shell Script Command

Filename: RANDOMEQ.py

```
import time
  
while(True):
    def random_numbers():
      import random
      i = 0
      holder = []
      while i < 64:
        i = i + 1
        holder.append(random.randrange(0, 2))
      return holder

    def list_split():
      split = list()
      numbers = random_numbers()
      final = ''
      
      for i in range(0, len(numbers), 2):
        split.append(numbers[i:i+2])
        
      for i in split:
        one = str(i)
        two = one.replace(',', '')
        three = two.replace('[', '')
        four = three.replace(']', '')
        final = final + four + '\n'

      with open('RANDEQ.txt', 'w') as f:
        f.write(final)
        f.close()

      return final

    list_split()
    time.sleep(0.25)
    
```
Filename: test.exp
```
#!/usr/bin/expect -f

set timeout -1

spawn mpg123 -R
expect "@R MPG123 (ThOr) v8\r"

send -- "SILENCE\r"
expect "@silence\r"

send -- "LOAD http://streamer.radio.co/s7de3f3534/listen\r"
expect "\r"
sleep 0.5

for {set x 0} {$x<10000000000} {incr x} {
        send -- "EQFILE ./RANDEQ.txt\r"
    expect "\r"
    sleep 0.25

    send -- "EQFILE ./RANDEQ.txt\r"
    expect "\r"
    sleep 0.25

    send -- "EQFILE ./RANDEQ.txt\r"
    expect "\r"
    sleep 0.5

    send -- "EQFILE ./RANDEQ.txt\r"
    expect "\r"
    sleep 0.25

    send -- "SEQ 0.6 0.5 0.6\r"
    expect "\r"
    sleep 0.5

}

exit
```

Filename: RANDEQ.txt
```
0 0
0 1
0 0
1 0
1 0
0 1
1 1
1 0
1 1
1 0
0 0
1 1
1 0
0 1
1 0
1 0
0 0
0 1
0 0
0 0
1 0
0 1
0 0
0 1
0 0
0 0
1 0
1 1
0 1
0 1
0 0
0 1
```
Controls:
To start the radio player, run the RANDOMEQ.py script perpectually in the background with
```
sudo python3 RANDOMEQ.py &
```
This is a looping program that repeatedly updates the RANDEQ.txt file. It is able to loop in the background due to the ampersand at the end.
The RANDEQ.txt file has two columns of 32 numbers. Each row represents a different equalizer band that is read by the MPG 123 audio player. A 0 means no sound is played at that frequency, and a 1 means that sound is played normally at that frequency. MPG 123 plays in stereo, so the left column controls the left audio, and the right column controls the right audio.
