<h1> Packet Sniffer (Wireshark) </h1> 

![image](https://github.com/darias08/Analyze-network-with-Wireshark/assets/58616895/d3a7b756-239a-4709-a676-9f7613319cae)

<h2>Description: </h2>

In this project, I will be using a network packet sniffer tool called, "[Wireshark](https://www.wireshark.org)", which allows me to see all incoming and outgoing traffic in my network. I will also explain the process of how I was able to identify what type of an attack this was on a network, explain what may have caused this website to malfunction, and provide recommendations how this could be resolved.


<h2>Scenario: </h2>

You work as a security analyst for a travel agency that advertises sales and promotions on the company’s website. The employees of the company regularly access the company’s sales webpage to search for vacation packages their customers might like. 

One afternoon, you receive an automated alert from your monitoring system indicating a problem with the web server. You attempt to visit the company’s website, but you receive a connection timeout error message in your browser.

You use a packet sniffer to capture data packets in transit to and from the web server. You notice a large number of TCP SYN requests coming from an unfamiliar IP address. The web server appears to be overwhelmed by the volume of incoming traffic and is losing its ability to respond to the abnormally large number of SYN requests. You suspect the server is under attack by a malicious actor. 

You take the server offline temporarily so that the machine can recover and return to a normal operating status. You also configure the company’s firewall to block the IP address that was sending the abnormal number of SYN requests. You know that your IP blocking solution won’t last long, as an attacker can spoof other IP addresses to get around this block. You need to alert your manager about this problem quickly and discuss the next steps to stop this attacker and prevent this problem from happening again. You will need to be prepared to tell your boss about the type of attack you discovered and how it was affecting the web server and employees.

<h2>Walkthrough: </h2>

<h3> Identify the type of attack causing the network interruption</h3>

<p>
  
As I scan through the network with Wireshark, I am seeing three different highlighted colors that indicate the status of what the packets are doing. For example:
  - ${\color{lime}{Green}}$ represents that the connection is normal and is doing a complete three-way handshake.
  - ${\color{yellow}{Yellow}}$ represents that the connection is still a normal three-way handshake, however, the connection failed due to an attack. This is caused by a `[RST, ACK]` response.
  - ${\color{red}{Red}}$ represents that the attacker is sending a bunch of `[SYN]` responses to a specific device, which interrupts any traffic that is supposed to go to that network device.
</p>

<p>
  
  In this scenario, the attacker is doing a **DoS** attack because the attacker is using one machine of theirs to attack another machine. From  what is shown on this catalog, is that the attacker is attacking a web server since the IP address of `198.51.100.23` is the source of the web server. The attacker here is sending a flood of SYN responses which is considered a DoS attack.
  
</p>

![image](https://github.com/darias08/Analyze-network-with-Wireshark/assets/58616895/9cee67a8-d472-4851-b40d-c88675db426a)


<h3> Explain how the attack is causing the website to malfunction</h3>

<p>

Based on my analysis, I can see that the website visitors try to establish a connection with the web server, which is a three-way handshake that occurs using TCP protocol. I will explain a quick detail how the three-way handshake works:

</p>
  
  1. A `SYN` packet is sent from the source to the destination, requesting to connect.
  2. The destination replies back to the source with a `SYN/ACK` response. Telling the source that they did receive the response and are sending it back with the contents.
  3. Once the source IP address has received the `SYN/ACK` response it will finalize the response back to the server with an `ACK`. Meaning it has been acknowledged and is fully connected to the other device.


In an SYN flood attack, a malicious actor sends numerous SYN packets all at once, overwhelming the server's capacity to reserve resources for the connection. When this occurs, there are no resources left on the server for valid TCP connection requests.

The logs show that the web server is overloaded and unable to handle SYN requests from visitors. When a new visitor encounters a connection timeout message, the server is unable to establish a new connection.

<h3> Recommendations to resolve this problem</h3>

The steps for how I would resolve this issue is by contacting my boss and explaining what is causing the server to crash and how this can be fixed. There is a DoS attack occurring on the web server which is when an attacker sends a bunch of SYN requests to the web server. By doing this it will block any other incoming traffic and cause the server to crash.  

The best solution to fix this problem is to integrate a **Firewall** on the web server so that it can detect any malicious device that is trying to send out a flood of SYN responses and block them from the attack. This is so that other users can experience the web browser without any interruptions or lag as they browse through the web page.  
