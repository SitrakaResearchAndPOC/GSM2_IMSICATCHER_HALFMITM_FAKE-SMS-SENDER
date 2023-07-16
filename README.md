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
ctrl+shift+T
```
Configure config.json and test the ussd on it  
For automated fake sms sender in each connection on the BTS
Relaunch all setup but use this command instead of python3 main_uhd.py
```
python3 main.py -u -c config.json
```

