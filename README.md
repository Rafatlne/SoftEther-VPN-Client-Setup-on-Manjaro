# How to Install SoftEther VPN on Manjaro/Arch Based Linux
This tutorial only tested on Manjaro Linux.

## 1. Install SoftEther VPN

1. First download [SoftEther VPN Linux Client.](https://www.softether-download.com/en.aspx?product=softether)
2. Extract the tar.gz file.
3. `cd` to that extracted folder.
4. run this command `make` 
5. It will ask for license aggrement and you have to agree to that license prompt otherwise it will not install. 

## 2. How to Start SoftEther VPN

- To start you have give below command
```
sudo ./vpnclient start
```

## 3. How to Enter SoftEther Command Prompt

- After starting SoftEther VPN you can enter SoftEther Command Prompt. Run below command
``` 
./vpncmd 
```
- This will show three options and choose option 2 and press `Enter`. This will show below text
```
Specify the host name or IP address of the computer that the destination VPN Client is operating on. 
If nothing is input and Enter is pressed, connection will be made to localhost (this computer).
Hostname of IP Address of Destination: 
```
- If you don't give any input and pressed `Enter` again then you will enter into `localhost`. This will show you below text
```
Connected to VPN Client "localhost".
```
- Now we will check if SoftEther VPN successfully installed or not. Run this command `check` if output of text is like below then you have successfully installed SoftEther VPN
```
All checks passed. It is most likely that SoftEther VPN Server / Bridge can operate normally on this system.

The command completed successfully.
```

## 4. How to Create VPN Account

- Follow step 3.
- Upon Entering run the command to create VPN interface
```
NicCreate YourVpnName
```
> For my case I Entered Below Command
>
> `NicCreate bcc`
- Now we have to create account 
  - `AccountCreate YourAccountName` to create your account.
  >
  > For my case I Entered Below Command
  >
  > `AccountCreate bcc`
  - Destination VPN Server Host Name and Port Number
  ```
  Your destination VPN Server Host and Port Number
  Example: 123.45.67.789:443
  ```
  - Destination Virtual Hub Name
  ```
  Your Destination Virtual Hub Name
  Example: VPN
  ```
  - Connecting User Name
  ```
  Your User Name
  Example: test
  ```
  - Used Virtual Network Adapter Name
  ```
  Virtual Network Adapter Name (You created this earlier by NicCreate command)
  ```
  >
  > For my case
  >
  > `bcc`
  - After that you will get confirmation message like this
  ```
  AccountCreate command - Create New VPN Connection Setting
  Destination VPN Server Host Name and Port Number: 123.45.67.789:443
  Destination Virtual Hub Name: VPN
  Connecting User Name: test
  Used Virtual Network Adapter Name: bcc
  The command completed successfully.
  ```
## 5. How Setup VPN Account Password
- Enter below command and press `Enter`
```
AccountPassword YourAccountName
```
> For my case I Entered Below Command
>
> `AccountPassword bcc`
- You will have to enter password two times and after that you will get `Specify standard or radius:`, write `standard`.
- After that you will get confirmation message.
```
The command completed successfully.
```

## 6. How to connect VPN Account
- Enter below command and press `Enter` to connect to VPN account.
```
AccountConnect YourAccountName
```
> For my case I Entered Below Command
>
> `AccountConnect bcc`
- If account connected successfully, will get below message
```
AccountConnect command - Start Connection to VPN Server using VPN Connection Setting
The command completed successfully
```
- You can also check this by below command.
```
AccountList
```

## 7. How to Modify Route Table

You connected to SoftEther VPN but you ip route doesn't go through that VPN yet. You can see this by `ip route` which will show you current ip route. To route through the SoftEther VPN you have to connect VPN interface
- First run `cat /proc/sys/net/ipv4/ip_forward` to check if IP Forwarding is enabled. If '0' is returned then run the next command `echo 1 > /proc/sys/net/ipv4/ip_forward`<br>
(You may need to sudo su to perform some of the next commands)
- Run `dhclient vpn_YouVpnName` for my case `dhclient vpn_bcc` to obtain an IP address from the VPN DHCP server
- Run this command
```
sudo ip route add vpn_server_address/32 via 192.168.0.1
Example: sudo ip route add 123.45.67.789/32 via 192.168.0.1
```
- Delete old route 
```
sudo ip route del default via 192.168.0.1
```
- Now ping google's nameservers at `ping 8.8.8.8 -c4`

## 8. Disconnect from VPN and restore route table
- First run this command
```
sudo ./vpnclient stop 
```
- Then you have delete ip route that goes through vpn.
```
sudo ip route del vpn_server_address/32
Example: sudo ip route del 123.45.67.789/32
```
- Add default ip route
```
sudo ip route add default via 192.168.0.1
```
