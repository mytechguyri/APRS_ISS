# APRS_ISS
The code is pretty well commented, basically what this does is its called by Systemd on startup.  Make sure you're running chrony or ntpd as a reasonably
accurate clock is crucial to successful ISS tracking.

1.  You will create TWO direwolf config files, and place them in /etc  Name them direwolf.aprs for standard APRS operation and 
    name the other direwolf.iss for ISS operation.  
2.  On startup, this program will copy direwolf.aprs into the active profile for Direwolf
3.  Initializes the DRA-818V radio to the 144.390 MHz North American APRS frequency, and disables the filtering to ensure flat audio.
4.  Re-starts the Direwolf service so it will read the new config file, and begin normal operation.
5.  Program then, based on the latitude and logitude variables, determines when the ISS will next be passing overhead.
6.  Once the ISS is passing overhead, the program will copy the direwolf.iss profile to the active profile and restarts the Direwolf service to read the new config.
7.  If the pass is for 480 seconds or longer, it does a rudimentary doppler shift adjustment, splitting the pass into 5 segments of 19%, 19%, 24%, 19%, and 19%
    2.5 khz  for each segment, with the 24% segment being simplex at the ISS APRS frequency of 145.825 MHz. automatically adjusting the radio frequency in
    real time as the ISS passes overhead.
8.  At the end of the pass, the program will copy the direwolf.aprs profile back to the active profile, and restart the direwolf service
9.  Automatically change the radio back to the regular North American APRS frequence 144.390 MHz.
10  Calculates when the next pass of the ISS will be, and waits to begin the process over again.

Note, on passes of less than 480 seconds, there's little point to adjusting for doppler shift, so it just sets to the 145.825 MHz frequency for the 
entire pass.  Its actually said there's little point in adjusting for doppler shift at all on 2 meters, but I'm testing it and will see if it improves
contacts.

There may be some code that I've commented out, as this is a work in progress, and while works fine as is, I may be adding other capabilities in the future, such as
an ability to force power cycle the radio if it becomes non-responsive.
