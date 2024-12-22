# raspi5kiosk
How a kiosk worked for me

1. install raspberry pi os (headless)
2. raspi-config
   1. select System Options (option 1)
      1. select Boot/Auto Login
         1. select Console Autologin
   1. select Display Options (option 2)
      1. Select Underscan (D1)
      2. When prompted for overscan compensation, select NO
   2. select Advanced Options (option 6)
      1. select Wayland (A6)
      2. select X11
4. install some apps
   1. xinit
   2. chromium-browser
   3. openbox
   4. vim
5. edit ~.profile
   1. add `[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor` to the very end
6. edit /etc/xdg/openbox/autostart
   1. add this to the end of the file
   ```
   xset s off
   xset s noblank
   xset -dpms
   xrandr --output HDMI-1 --rotate left
   #setxbmap -option terminate ctrl_alt_bksp
   #/home/eos-usr/kioskwordclock/wordclock
   cd /home/eos-usr/kioskwordclock && ./wordclock --host 0.0.0.0 --port 8080 &
   sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/'Local State'
   sed -i 's/"exited_cleanly":false/"exited_cleanly":true/; s/"exit_type":"[^"]\+"/"exit_type":"Normal"/' ~./config/chromium/Default/Preferences
   chromium-browser --disable-infobars --force-dark-mode --kiosk 'http://localhost:8080' 
   ```
