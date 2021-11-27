<pre>
            __    __ _         ___ _               
           / / /\ \ (_)       / __(_)              
           \ \/  \/ / |_____ / _\ | |              
            \  /\  /| |_____/ /   | |              
             \/  \/ |_|     \/    |_|              
                                                   
   ___ _                _   __ _               _   
  / __\ |__   ___  __ _| |_/ _\ |__   ___  ___| |_ 
 / /  | '_ \ / _ \/ _` | __\ \| '_ \ / _ \/ _ \ __|
/ /___| | | |  __/ (_| | |__\ \ | | |  __/  __/ |_ 
\____/|_| |_|\___|\__,_|\__\__/_| |_|\___|\___|\__|
                                                   
</pre>

### A compilation of tools and techniques targeting Wi-Fi networks.


Verify Wireless card

`iwconfig`

`iw list`

Change card's channel

`iwconfig <interface> channel <channel_number>`

`iw dev <interface> set channel <channel_number>`

Change adapter's transmit power
##### Please note that using a high transmission power may be illegal in your country, so be careful.

`iw reg set B0`

`iw dev <interface> set txpower fixed 30dbm`

Set/delete monitor interface

`airmon-ng start <interface>`

`airmong-ng stop <mon_interface>`

Get rid of blocked device conditions

`airmon-ng check kill`

Run test mode

`<aireplay-ng -9 <mon_interface>`

