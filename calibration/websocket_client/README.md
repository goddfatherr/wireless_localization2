# Websocket server
Command-line based Websocket client for interacting with the websocket server

## How to Use

### Dependencies
- python 3(Development used 3.12)
- websocket python library

### Set the websocket server address

```
ws = websocket.WebSocketApp('ws://ws_server_ip_addr/ws',
                                on_open=on_open,
                                on_message=on_message,
                                on_error=on_error,
                                on_close=on_close)
```

See the README in `websocket_server(ESP32)` on how to find out `ws_server_ip_addr`


## Scanning For WAPs

### Trigger Strings
Two trigger strings are defined:

* `FindApsAuto` : Upon receipt of this string, the server initiates an indefinite WAP scan in a loop. Results are sent to the the websocket client as soon as a scan completes. The client must make arrangements to persist this information in a database. 
```Note that the server cannot service any subsequent trigger string once it enters this loop. ```

* `FindApsManu` : 20 scans are carried out consecutively, and RSSIs averaged. Then sent back to the client. In this mode, you will not be prompted to enter the location tag until server returns the result. 

### Setting up the client for Auto Scan
```
PS wireless_localization2\calibration\websocket_client> py ws_client.py
Enter WSS addr: 1.1.1.1
Enter calibration type:(auto or manual): auto
Enter location tag: SB 305
```

### Setting up the client for Manual Scan
```
PS wireless_localization2\calibration\websocket_client> py ws_client.py
Enter WSS addr: 1.1.1.1
Enter calibration type:(auto or manual): manual
```

### Format of the Scan Output Sent to the client
| BSSID | SSID | ENCRYPTION| CHANNEL | RSSI | NO OF OCCURENCES |

* To write the data into database, the user must enter "save location" in the text prompt. Location is the tag of the fingerprint Upon reciept of this, the fingerprint of the location is constructed and saved


## Tools
`utils` is a collection of tools for manipulating the database
database: 
* devdb.db
tables: 
* fv_ordering: stores the ordering of the feature vectors
* fingerprints: stores the constructed fingerprints

`db_clean.py`: cleans the database

#### Usage
db_clean.py database_name
eg: db_clean.py devdb.db

`db_read.py`: reads table rows and prints them on the commandline

#### Usage
cmd: py db_read.py db_name tb_name n_rows
eg:  py db_read.py devdb.db fingerprints 10

## Example Usage Output

```
PS C:\Users\websocket_client> py .\ws_client.py
Enter WSS addr: 172.22.195.59
Opened connection
Enter message to send (or type 'exit' quit): findApsFast
Enter message to send (or type 'exit' quit): Received message: 88B1E1974120, -63, 1
88B1E1974121, -63, 1
88B1E19726C0, -72, 1
6C8D775D98A2, -76, 1
6C8D775D98A3, -76, 1
88B1E19726C1, -77, 1
6C8D775D98A4, -77, 1
88B1E1973441, -84, 1
88B1E1973440, -84, 1
88B1E14B78C0, -86, 1
F01D2DE77AE2, -87, 1
F01D2DE77AE3, -88, 1
88B1E14B78C1, -88, 1
88B1E14B6E81, -88, 1
88B1E14B6E80, -88, 1
F01D2DE780E2, -93, 1
88B1E19713A1, -94, 1
88B1E19713A0, -94, 1
F01D2DE780E3, -97, 1

Enter Location tAG:
Enter Location tAG: Location 1

Database Updated.
Enter message to send (or type 'exit' quit): findAps
Enter message to send (or type 'exit' quit): Received message: 88B1E1974120, -61, 20
88B1E1974121, -61, 20
88B1E19726C0, -73, 20
88B1E19726C1, -73, 20
6C8D775D98A2, -76, 20
6C8D775D98A3, -75, 20
6C8D775D98A4, -76, 20
F01D2DE77AE3, -85, 20
88B1E1973441, -87, 17
88B1E1973440, -87, 15
F01D2DE77AE2, -85, 20
88B1E14B78C0, -89, 14
88B1E14B78C1, -89, 12
88B1E14B6E80, -89, 12
88B1E14B6E81, -90, 14
F01D2DE74563, -90, 9
F01D2DE74562, -90, 11
F01D2DE780E2, -93, 2
000000000000, 0, 46

Enter Location tAG: Location 2

Database Updated.
```