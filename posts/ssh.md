---
title: Overview of SSH
date: 2023-08-26
---



# **Hello!** 
So I’ve used Secure Socket Shell(SSH) in the past for school and work/intern projects. I have an understanding of how it works on a basic level, you connect to a machine over a network, and it should be secure but I got curious as to how it worked a bit on a deeper level. Today I want to give an overview on how SSH works, and some things I found interesting while reading about SSH. 

## **What is SSH**
SSH is a network protocol that serves as a way to connect to a computer/server over a network. It generally provides strong password/public key authentication for encrypted data over a network.  SSH follows a client-server model, where a client tries to connect to the application on an SSH server. This is where the session takes place and runs. 

SSH mostly uses AES and Blowfish encryption standards. SSH is also built on top of TCP/IP. IP is used as an underlying protocol for routing packets between devices but not directly. SSH instead uses TCP for creating an established communication channel. 

### **SSH-1 vs SSH-2 **
It’s important to note Two protocol versions of SSH exist, SSH-1 and SSH-2. Modern servers use SSH-2. SSH1 is now not very commonly used because of its monolithic architecture and weak security. SSH-2 is more commonly used and It’s generally a better version of SSH that has better encryption, key exchange methods and better auth. SSH-1 had a flawed key exchange where it was vulnerable to man in the middle attacks, SSH-2 uses a different key exchange (diffie-hellman-group1-sha256) method that provides more security against this. SSH-1 also uses DES, while SSH-2 uses the stronger AES encryption. SSH-2 tends to be the default in modern systems and is more widely supported. 

### **SSH use cases: **
Network admins tend to use SSH to manage applications or systems. SSH is also embedded into Linux servers and in data centers. It’s used to secure communications that happen between machines like remote access to resources, command execution, software updates, and more. SSH is also used whenever you need secure file transfer sessions, automated file transfers, and secure network infrastructure management.  

# **SSH process/handshake **

+ **Client Connection:**

    -Initate through terminal or GUI tools like **PuTTY, KiTTY, WinSCP**. The syntax would look like the following

    ```ssh -p port_num username@hostname_or_ip ```

    `-p port_number` connects to a specific port, leaving it out by default connects to **port 22**  

+ **Server Identification:**

    -Server responds with identification and public key/ID

+ **Client Key Exchange**

    -Client sends a key encrypted with the server's public ID

+ **Session Encryption and Authentication:**

    -Both server/client create keys for two-way communication

    -Server decrypts client's encrypted value using its private key

    -Derived value key used for session key creation


+ **Client Authentication:**

    -Client proves identity using password or key-based authentication

    -Based on the public key exchanged earlier


+ **Data Exchange:**

    -Encrypted channel established for secure data transfer

    -Enables tasks like file transfers, remote commands, etc


+ **Session Termination:**

    -Either client or server can end the session

    -Resources released, encrypted connection closed


# **SSH tunneling/forwarding**

SSH has a cool technique called SSH tunneling, basically it lets you transmit data over two devices.  3 types of tunneling techniques exist, local port forwarding, remote port forwarding, and dynamic port forwarding

###### **Local port forwarding **
Local port forwarding creates a tunnel from your client machine to a remote server, and data then goes through your SSH connection to that server, then from there gets sent elsewhere. A possible use case is like trying to access services behind a firewall or remote network.  


###### **Remote port forwarding**
Remote port forwarding means you create a tunnel from a remote server to your local machine.
The data gets sent from a port on the server to the SSH connection to your machine, then from your machine to another location. This usually is used for making services on the client accessible from a remote server. 

###### **Dynamic port forwarding**
Dynamic port forwarding will make your SSH connection into a SOCKS(Socket Secure) proxy server. With this you can route your traffic through the connection, creating a tunnel for all your online activity. This has a lot of use cases like adding extra layers of security, say if you're on a public WIFI, it also allows for bypassing network restrictions that might block content. It can also be used for masking your IP, and Geo-Spoofing, since you can route your traffic through an SSH connection through a server in a different location with a different IP. 


# **Structure of SSH**
SSH is built in layers, there’s three layers for how SSH is built, the Transport Layer, User Auth protocol layer, and the Connection protocol Layer. 

###### **Transport layer**
The transport layer is responsible for establishing a secure connection between the client and the server. It manages the exchange of data between these two endpoints. This layer employs encryption to safeguard the integrity and confidentiality of the transmitted data. Additionally, it employs cryptographic techniques to ensure that the data remains untampered during transmission. The transport layer also facilitates tunneling and port forwarding capabilities. Its primary role involves overseeing connection establishment and management.


###### **Authentication Layer **
The user authentication layer plays a crucial role in verifying the identity of the client and establishing a secure connection to the server. It ensures that only authorized users or applications can gain access to the server. This layer is intricately involved in the SSH handshake process, as outlined previously. It supports various authentication methods, including password-based authentication and public key authentication.


###### **Connection Layer**
After successful user authentication, the connection layer takes charge of managing multiple communication channels through which data can flow between the client and the server. These channels serve diverse purposes, including enabling shell access, facilitating file transfers, and enabling port forwarding. 

 
# **SSH resources**
The Request for Comments (RFC) 4250-4254 by the Engineering Task Force (IETF) contains a detailed amount of information on how SSH works, and where I got a majority of this information. 

+ RFC 4250 - SSH Protocol Assigned Numbers
+ RFC 4251 - SSH Protocol Architecture
+ RFC 4252 - SSH Authentication Protocol
+ RFC 4253 - SSH Transport Layer Protocol
+ RFC 4254 - SSH Connection Protocol
+ RFC 4419 - Diffie-Hellman Group Exchange for SSH
 
Goteleport also has a pretty detailed blog on the SSH handshake.
https://goteleport.com/blog/ssh-handshake-explained/

I also like some O’Reilly resources like 
 
+ 3.3. The Architecture of an SSH System 
    https://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch03_03.htm

+ 3.4. Inside SSH-1 

    https://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch03_04.htm

+ 3.5. Inside SSH-2

    https://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch03_05.htm




