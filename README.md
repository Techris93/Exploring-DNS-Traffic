# Exploring-DNS-Traffic

## Objectives:
- Capture DNS Traffic
- Explore DNS Query Traffic
- Explore DNS Response Traffic

## Scenario:
- I used Wireshark to filter for DNS packets and view the details of both DNS query and response packets.

## Required Resources:
- A PC with Internet access.
- Wireshark.

## Step 1: Capture DNS Traffic.
a. Start Wireshark. Select an active interface with traffic for packet capture.
<br>
b. Clear the DNS cache. For macOS, enter 
```
sudo killall -HUP mDNSResponder
```
to clear the DNS cache in the Terminal. 
<br>
c. At a command prompt or terminal, type 
```
nslookup
```
to enter the interactive mode. Enter the domain name of a website.
<br>
d. Exit when finished and close the command prompt.
<br>
e. Click Stop capturing packets to stop the Wireshark capture

## Step 2: Explore DNS Query Traffic.
a. Observe the traffic captured in the Wireshark Packet List pane. Enter <b>udp.port == 53</b> in the filter box and press enter. Select the DNS packet that contains <b>Standard query<b> and <b>the website<b> in the Info column
<br>
<img src="https://i.imgur.com/G0FDol3.png" height="80%" width="80%" alt="DNS capture"/>
<br>
<br>
<img src="https://i.imgur.com/X0gwKRx.png" height="80%" width="80%" alt="UDP pane"/>
<br>

## findings:
- source MAC addresses: x:x:x:x:x:x
- destination MAC addresses: x:x:x:x:x:x
- source port: 58734
- destination port: 53, which is the default DNS port number.
- source ip address: 192.168.1.196
- destination ip: 191.168.1.1
- the RD flag is set to 1, it indicates that the client requesting the query wants the DNS server to perform recursive resolution.
- the source MAC address is associated with the NIC on the PC and the destination MAC address is associated with the default gateway. If there is a local DNS server, the destination MAC address would be the MAC address of the local DNS server.

## Step 3: Explore DNS Response Traffic.
a. Select the corresponding response DNS packet has <b>Standard query response<b> and the <b>website<b> in the Info column.
<br>
<img src="https://i.imgur.com/jiScAh6.png?1" height="80%" width="80%" alt="reverse ethernet pane"/>
<br>
<br>
<img src="https://i.imgur.com/ilQkFor.png" height="80%" width="80%" alt="reverse udp pane"/>
<br>
<br>
<img src="https://i.imgur.com/3xUvm8q.png?1" height="80%" width="80%" alt="reverse pane"/>
<br>
## findings:
- The source IP, MAC address, and port number in the query packet are now destination addresses. The destination IP, MAC address, and port number in the query packet are now source addresses.
- the DNS can handle recursive queries because the RD flag set to 1, it suggests that the DNS server is capable of performing recursive queries.

## Conclusion:
An attacker on the LAN can use Wireshark to observe the network traffic and can get sensitive information in the packet details if the traffic is not encrypted.
