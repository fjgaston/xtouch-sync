# xtouch-sync
sync xtouch with mixing station using only ethernet 

## What I tried to achieve
I use a X-touch controller with a XR18 mixer. I like the way the X-touch interact with the XR18 in Xctl mode via ethernet and it seems pretty quick and reliable but it lack of visibility. I wanted to have my actions shown on a large display. X-air-edit and mixing-station are great but they don't follow the selected channel or the selected layer. I tried using mixing station with the X-tcouh controller via USB but it was not ideal.
What I did is, I used my computer to intercept udp packets between the X-touch and the XR18.

## What is needed
 - A commputer with Mac OS (sorry I don't know how to do it with windows or linux)
 - A X-touch controler
 - A XR18 Mixer
 - Mixing-station https://mixingstation.app/
 - homebrew
   
## usage
### Install Homebrew if you don't have it already
https://brew.sh/

### Install Ettercap :
Ettercap allows to intercept packet between two devices using a Mitm attack.

`brew install ettercap`

### Install sendmidi
[https://github.com/gbevin/SendMIDI]([url](https://github.com/gbevin/SendMIDI)https://github.com/gbevin/SendMIDI)


### Enable IAC Driver
Go to Audio MIDI setup in Mac OS >> double click IAC Driver (a renamed mine 'virtual')
toggle 'device is online'
![iacdriver](https://github.com/fjgaston/xtouch-sync/assets/47718427/0d14ae19-1065-49d9-9a33-08da7988a034)

### add a new device in mixing station
The tricky part is that if you have your virtual driver as an inpiut and output it will mess everything up however mixing station doesn't allow you to add a midi device with only input.
The trick is to have at least one midi device plugged to your computer (It can be your X-touch) when configuring the device.
Create new midi device with your virtual driver (IAC driver) as an input and any other device as the output. (make sure your virtual driver is not in the output)
In device Type, choose X-touch (MCU)

### start the script
`sudo ettercap -T -S -V hex -i en5 -M arp:remote /192.168.1.46// /192.168.1.100// | grep --line-buffered "0000: 90" | while IFS= read -r line; do echo "$line" | ./x-touch-sync >out.txt | sendmidi file out.txt; done`


