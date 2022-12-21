# 2 - Working with remote servers

**git branch name:** jbrowser

## Theory [2]

* [0.4] What are [computer ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/) on a high level? How many ports are there on a typical computer?

A port is a virtual point where network connections start and end. Ports are software-based and managed by a computer's operating system. Each port is associated with a specific process or service. Ports allow computers to easily differentiate between different kinds of traffic: emails go to a different port than webpages, for instance, even though both reach a computer over the same Internet connection. Between the protocols User Datagram Protocol (UDP) and Transmission Control Protocol (TCP), there are **65,535** ports available for communication between devices.

-----
* [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.

The Hypertext Transfer Protocol (HTTP) is the foundation of the World Wide Web, and is used to load webpages using hypertext links. HTTP is an application layer protocol designed to transfer information between networked devices and runs on top of other layers of the network protocol stack. A typical flow over HTTP involves a client machine making a request to a server, which then sends a response message.

HTTPS is HTTP with encryption and verification. The only difference between the two protocols is that HTTPS uses TLS (SSL) to encrypt normal HTTP requests and responses, and to digitally sign those requests and responses. As a result, HTTPS is far more secure than HTTP and that is the main difference between HTTP and HTTPS.

SSH or Secure Shell is a network communication protocol that enables two computers to communicate (c.f http or hypertext transfer protocol, which is the protocol used to transfer hypertext such as web pages) and share data.

HTTPS and SSH they are all have the same functionality, which is Encryption. 

Port 21 is used to establish the connection between the 2 computers (or hosts) and port 20 to transfer data (via the Data channel).



-----
* [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser. 
* [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.
* [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.

SSH is a protocol for computer-remote server interaction. SSH protocols specify standards for operating network services securely between untrusted hosts over unsecured networks. Communications between a client and server using SSH are encrypted, so it is ideal for use on unsecure networks. Originally, the word shell in SSH referred to a program that processed Unix commands. SSH is often used in conjunction with various other internet protocols.

The two to authenticate in an SSH server:
* Password authentication (using user name and passwords)

In password-based authentication, after establishing secure connection with remote servers, SSH users usually pass on their usernames and passwords to remote servers for client authentication. These credentials are shared through the secure tunnel established by symmetric encryption. The server checks for these credentials in the database and, if found, authenticates the client and allows it to communicate. 

* Public key-based authentication (using public and private key pairs)

In Public key-based authentication, after the client establishes a connection with the remote server, the client informs the server of the key pair it would like to authenticate itself with. The server verifies the existence of this key pair in its database and then sends an encrypted message to the client. The client decrypts the message with it’s private key and generates a hash value which is sent back to the server for verification. The server generates its own hash value and compares it with the one sent from the client. When both the hash values match, the server is convinced of the client’s authenticity and allows it to communicate with the server.

-----
