# wireshark-network-sec
# Introduction :

![image](https://user-images.githubusercontent.com/121964432/227807918-5460b208-7533-4d1f-82a1-73f6bd1a75ec.png)

Hello! Based on the image, it appears that we will be setting up a network where we are going to use three Ubuntu virtual machines. One of these virtual machines will serve as the server while the other two will be clients. To set up the network, we will need to create two subnets: 200.4.1.0/24 and 200.4.2.0/24.

The primary objective of this simulation is to analyze the ICMP packets that are being sent from one client to another, so that we can better understand how the IP datagrams are being transmitted over the network. To accomplish this, we will use a tool called Wireshark, which is a popular and widely used packet sniffing software.

![Capture d’écran 2023-03-26 162844](https://user-images.githubusercontent.com/121964432/227809806-0744c5bc-9ee0-4193-9120-e82049216bc6.png)

# Configuration of the machines :

## server :

for our server machine we are going to add a network adapter so we can connect our machine to both networks
 
![Capture d’écran 2023-03-26 162926](https://user-images.githubusercontent.com/121964432/227809858-4abb0bd2-7ae5-4982-b555-776ddc0dafdf.png)
 
 now lets give Ip addresses for both interfaces :
 
 - ens33 :
 
 ![image](https://user-images.githubusercontent.com/121964432/227810031-7b3bbab8-9c55-4adf-b445-ca98c6de446b.png)

 - ens37 :
 
 ![image](https://user-images.githubusercontent.com/121964432/227810067-eeecfb6f-d48e-4c04-a171-a46399e6c5b4.png)

## Client 1 :

we are going to give this machine the following IP 200.4.1.2

![image](https://user-images.githubusercontent.com/121964432/227810484-253ced1b-86ab-4979-8780-8ad9d6370eba.png)


## client 2 :

we are going to give this machine the following IP 200.4.2.2


![image](https://user-images.githubusercontent.com/121964432/227810569-962dfd15-f1dd-4b95-9c92-5dd0a4d923bd.png)


> all these machine are connected to lans

> ![Capture d’écran 2023-03-26 163322](https://user-images.githubusercontent.com/121964432/227810674-8e2bdf25-1b59-4ebe-ae2d-995afe50393b.png)

# Test :

Now lets try and ping these three ip addresses from client 1 : 200.4.1.1 200.4.2.1 200.4.2.2

![Capture d’écran 2023-03-26 164334](https://user-images.githubusercontent.com/121964432/227810799-b2fe2b5a-2f5e-4836-bd00-2f45be3f4877.png)

as you can see we couldnt ping the last ip add wich is the client 2 add

so to be able communicate with client 2 we need to do ip forwarding

# IP forwarding :

IP forwarding is a network function that allows data to be sent from one network to another. It is a feature in network routers that allows them to forward packets of data from one network to another, which enables traffic to be routed between different IP subnets.

## Check IP Forwarding status:

we use the folloeing command :

```cmd
cat /proc/sys/net/ipv4/ip_forward
```

the result is 0 do the ip forwarding is disabeld

## Enable Ip forwarding :

we use this command to change the ip forwarding status :

```cmd
echo 1 > /proc/sys/net/ipv4/ip_forward
```

![Capture d’écran 2023-03-26 165405](https://user-images.githubusercontent.com/121964432/227811251-6180ab03-48bd-491b-8c66-a3525585265d.png)

Now  we specify the getways of both networks

![Capture d’écran 2023-03-26 165306](https://user-images.githubusercontent.com/121964432/227811301-ed5df791-f27a-43f3-96cf-bab1066b01c5.png)

lets check the ip route table

![Capture d’écran 2023-03-26 165338](https://user-images.githubusercontent.com/121964432/227811325-7251061a-6dc5-4673-955b-44c67f996e26.png)

# test :

lets try again and ping these three ip addresses from client 1 : 200.4.1.1 200.4.2.1 200.4.2.2

![Capture d’écran 2023-03-26 165504](https://user-images.githubusercontent.com/121964432/227811360-c524fe75-e0a6-4061-bd40-b5bd117f68ff.png)

# wireshark :

Wireshark is a popular open-source network protocol analyzer that allows users to capture, analyze, and display network traffic. It can be used for a wide range of tasks, including network troubleshooting, security analysis, and protocol development.

![Capture d’écran 2023-03-26 165558](https://user-images.githubusercontent.com/121964432/227811655-9de52392-4d9e-47c9-a5a6-f5ee6337b49a.png)

When you first open Wireshark, the initial interface is the "Capture" interface. This interface allows us to select the network interface we want to use to capture network traffic and choose the protocol we want to capture.

- Interface selection : a list of available network interfaces. This includes both wired and wireless interfaces on our computer. we can select the interface we want to use to capture network traffic by clicking on it.
- Filter selection : a filter bar where we can specify the protocol you want to capture. By default, all protocols are selected
- capture: Once we've selected our interface and protocol, we click on the "Capture" button to begin capturing network traffic.

# test :

we sent an icmp packet from client 2 to client 1 

here is the result on wireshark :

![image](https://user-images.githubusercontent.com/121964432/227812511-9037ce83-a36f-408e-b996-110ee5e96552.png)

The Wireshark user interface is divided into three main sections: the packet list pane, the packet details pane, and the packet bytes pane

- The packet list pane shows a list of all captured network packets. Each packet is displayed as a separate row in the list and includes information such as the source and destination IP addresses, protocol type, and time of arrival.
- The packet details pane shows a detailed view of the currently selected packet in the packet list pane. This pane displays a hierarchical view of the packet data, with each layer of the protocol stack shown in a separate expandable section. Users can drill down into each section to see detailed information about the packet data.
- The packet bytes pane shows the raw packet data in hexadecimal and ASCII format. This pane is useful for analyzing packet payloads and identifying patterns in the data
