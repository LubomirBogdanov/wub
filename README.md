Description  
===================================================================   
The project aims at providing a bridge between the UART of a microcontroller and  
the Internet.  
  
The project is based on ESP8266_RTOS_SDK v3.1.0  
(2019.01.31).  
  
The ESP8266 firmware project (named wub) must be placed in:  
    
/path/to/espressif/ESP8266_RTOS_SDK/examples/wifi  
      
for the version 3.1.0 of the SDK (idf-style).  

Supported ESP8266 modules are:  

ESP8266-01, ESP8266-02, ESP8266-03, ESP8266-04, ESP8266-06,  
ESP8266-07, ESP8266-08, ESP8266-09, ESP8266-11, ESP8266-12 E/Q/D,  
ESP8266-13, ESP8266-14, Olimex MOD-WiFi-ESP8266-DEV  

Modules not supported:  

ESP8266-05, ESP8266-10, ESP8266-14  

The WUB supports SCPI commands over the network and over the UART.
The WUB can be made transparent which can be used for printf debugging 
over the Internet. Check the SCPI commands below. For a tcp-ip client you 
can use telnet, netcat, putty, tera term and so on.  


!!! NOTE: the default UART settings are 9600-8-E-1 !!!  


INSTALL  
===================================================================
Wifi-to-UART-Bridge (WUB).  
Example connections given for MSP430F5529:  
<pre>  

ESP8266 module                            MSP430F5529
----------------                          ----------------
               |                          |
               |                          | 
               |                          | 
    IO1 (U0TxD)|--------------------------| P1.2 (RxD)
    IO3 (U0RxD)|--------------------------| P1.1 (TxD)
               |                          |
               |                          |
----------------                          ----------------
</pre>  



Example usage  
===================================================================
Go to the wub directory and type:  

make menuconfig  

Then select "example config -> SSID" and "example config -> password"  
to enter your wi-fi router's network name and password (WPA2). The  
router must use the DHCP. Check your wi-fi router support web page   
and search for the newly connected device. Find your WUB device  
ip address. By default the WUB uses port 5556.   

Supported WUB SCPI commands  
===================================================================  
-------Wi-Fi commands:--------  
TRAN {ON} - Set transparent mode ON.  
BSLT {SHARED|DEDICATED} - select BSL entry sequence type - shared or dedicated.  
BOOT - initiate MSP430 boot sequence.  
RSTT - reset target. SBWTDIO (RST) must be tied to Vdd with pull-up.  
SETT <numeric value> - set timeout for the transparent mode. (not implemented yet)  
HALT - hold the target in reset.  
HELLo - display a warm greeting over TCP/IP to test connection.  
-------Uart commands:--------  
WIFT - stop the server and disconnect from wi-fi, this is UART command only.  
TRPI <numeric value> - init pin (0 - 16) as input that will toggle the TRAN ON/OFF state.  
TRPO - deinit pin transparency, use TRAN command only.  
PWRO - turn wifi-uart-bridge power off, wake-up only through its reset pin  
-------Common commands:--------  
*IDN? - request WUB identification number: <manufac>,<model>,<serno>,HW<1.0>,SW<1.0>.  
HELP - display this help over wifi.  
WUBR - restart wifi-uart-bridge without restarting the target.  
SSID <string> - change the SSID name over the current network.  
PASS <string> - change the network passord over the current network.  
PORT <numeric value> - change the listen port over the current network.  
WIFS - start a new server using the above parameters over the current network.  
SETB <numeric value> - set uart baudrate, see ESP8266 datasheet for valid values.  
SETW <numeric value> - set uart word length, valid values are 5, 6, 7 and 8.  
SETP {ODD|EVEN|NONE} - set uart parity, valid values are the strings ODD, EVEN or NONE.  
SETS {ONE|ONE_AND_HALF|TWO} - set uart stop bit.  
UARA - UART apply config with the new settings.  
-------Reply--------  
READy - WUB is ready for operation.  
DONE - the requested command has been executed. Some commands do not have a reply.  
NONE - the requested command does not exist or an error has occured.  
CONN - the wub has connected successfully to the wi-fi network.  
DISC - the wub has been disconnected from the wi-fi network.  
WERR - An unknown error has occured with the wi-fi adapter.  
UERR - An unknown error has occured on the UART interface.  
ERRU - An unknown command has been requested on the UART interface.  
CLIC - A client has connected to the WUB server.  
CLID - A client has disconnected from the WUB server. Transparency is also turned off.  

