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

Port 21 is used to establish the connection between the 2 computers (or hosts) and port 20 to transfer data (via the Data channel) -  File Transfer Protocol (FTP).

Port 25: Simple Mail Transfer Protocol (SMTP). 

Port 53: Domain Name System (DNS).

Port 80: Hypertext Transfer Protocol (HTTP).

For other, check [this](https://docs.oracle.com/en/storage/tape-storage/sl4000/slklg/default-port-numbers.html#GUID-8B442CCE-F94D-4DFB-9F44-996DE72B2558) out.

-----
* [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser. 

1. IP stands for "Internet Protocol," which is the set of rules governing the format of data sent via the internet or local network. In essence, IP addresses are the identifier that allows information to be sent between devices on a network: they contain location information and make devices accessible for communication. There are four types of IP addresses: public, private, static, and dynamic.
2. A public IP address is an IP address that can be accessed directly over the internet and is assigned to your network router by your internet service provider (ISP). Your personal device also has a private IP that remains hidden when you connect to the internet through your router's public IP.
3. After you’ve typed the URL into your browser and pressed enter, the browser needs to figure out which server on the Internet to connect to. To do that, it needs to look up the IP address of the server hosting the website using the domain you typed in. It does this using a DNS lookup. Once the browser gets the DNS record with the IP address. Browser initiates Transmission Control Protocol (TCP) with the server. Browser sends the HTTP request to the server. Server processes request and sends back a response. Browser renders the content.

p.s An IP address is a string of numbers separated by periods. IP addresses are expressed as a set of four numbers — an example address might be 192.158.1.38. Each number in the set can range from 0 to 255. So, the full IP addressing range goes from 0.0.0.0 to 255.255.255.255.

-----
* [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.

[NGINX](https://www.nginx.com/resources/glossary/nginx/) is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.

NGINX performs with an asynchronous, event-driven architecture. It means that similar threads are managed under one worker process, and each worker process contains smaller units called worker connections. This whole unit is then responsible for handling concurrent requests. Worker connections deliver the requests to a worker process, which will also send it to the master process. Finally, the master process provides the result of those requests.

Nginx alternatives: **Apache**, is a web server notable for playing a key role in the initial growth of the World Wide Web. **Lighttpd** (pronounced "lighty") is a web server designed to be secure, fast, standards-compliant, and flexible while being optimized for speed-critical environments. **Caddy** is a web server like Apache, nginx, or lighttpd.


-----
* [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.

SSH is a protocol for computer-remote server interaction. SSH protocols specify standards for operating network services securely between untrusted hosts over unsecured networks. Communications between a client and server using SSH are encrypted, so it is ideal for use on unsecure networks. Originally, the word shell in SSH referred to a program that processed Unix commands. SSH is often used in conjunction with various other internet protocols.

The two to authenticate in an SSH server:
* Password authentication (using user name and passwords)

In password-based authentication, after establishing secure connection with remote servers, SSH users usually pass on their usernames and passwords to remote servers for client authentication. These credentials are shared through the secure tunnel established by symmetric encryption. The server checks for these credentials in the database and, if found, authenticates the client and allows it to communicate. 

* Public key-based authentication (using public and private key pairs)

In Public key-based authentication, after the client establishes a connection with the remote server, the client informs the server of the key pair it would like to authenticate itself with. The server verifies the existence of this key pair in its database and then sends an encrypted message to the client. The client decrypts the message with it’s private key and generates a hash value which is sent back to the server for verification. The server generates its own hash value and compares it with the one sent from the client. When both the hash values match, the server is convinced of the client’s authenticity and allows it to communicate with the server.

-----

## Problem [6.5]

A real-life situation that occurred to me several times over the years.

Imagine wrapping up a large bioinformatics project and wanting to share raw data with your colleagues in a friendly and straightforward format. The best option would be to use an online genome browser and host your data remotely, so it is easily accessible by anyone with a valid link. This is exactly what we will be doing here.

*Please consider doing this HW using Linux since setting up the SSH client on Windows is painful, and I won't be able to help you.*

-----

**Remote Server**:
* [2] Create a new virtual machine in the Yandex/Mail/etc cloud (order at least 10GB of free disk space). Generate SSH key pair and use it to connect to your server.

VM settings: 
1. Ubuntu 22.04
2. vSPU: 2
3. Доля vSPU: 20%
4. RAM: 4 GB
5. Disk size: 30GB

<img width="1423" alt="Screenshot 2022-12-22 at 12 19 01 AM" src="https://user-images.githubusercontent.com/82548512/209004564-e7762d1e-5e29-4927-b8bc-069179d628a3.png">


Now for the SSH:
```
ssh-keygen -t ed25519
```

Here's my SSH-key, made with keygen:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBr/VVkZTKCc92uN182gBXfhhg0B90ykxGtk5WJq15MG
```

Achieved connection to the server with:
```
ssh -i /Users/stepanletyagin/Desktop/key.txt saletyagin@51.250.64.138
```

* [1] Download the latest human genome assembly (GRCh38) from the Ensemble FTP server ([fasta](https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz), [GFF3](https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz)). Index the fasta using samtools (`samtools faidx`) and GFF3 using tabix. 

Since we have Ubuntu:
```
sudo apt-get install -y samtools
sudo apt-get install -y tabix
```
And for getting required human genome data:

```
wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa # indexing with samtools

wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz
gunzip Homo_sapiens.GRCh38.108.gff3.gz
```
Then, while using tabix for GFF3, I got:
```
tabix -p gff Homo_sapiens.GRCh38.108.gff3
# the compression of 'Homo_sapiens.GRCh38.108.gff3' is not BGZF
```
So I used:
```
bgzip Homo_sapiens.GRCh38.108.gff3 # to zip back
```
but then I got:
```
#Unsorted positions on sequence #1: 13221 followed by 12010 tbx_index_build failed: Homo_sapiens.GRCh38.108.gff3.gz
```
I found that [source](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-017-1930-3) and downloaded [tool](https://github.com/billzt/gff3sort) and used command again:
```
wget https://github.com/billzt/gff3sort/archive/refs/tags/v1.0.0.tar.gz
gunzip v1.0.0.tar.gz
tar xvf v1.0.0.tar
/home/saletyagin/gff3sort-1.0.0/gff3sort.pl --precise --chr_order natural Homo_sapiens.GRCh38.108.gff3 | bgzip > file.gff.gz
tabix -p gff file.gff.gz
```

* [1] Select and download BED files for three ChIP-seq and one ATAC-seq experiment from the ENCODE (use one tissue/cell line). Sort, bgzip, and index them using tabix.

Cell-line: GM12878 (ATAC-seq)

link: https://www.encodeproject.org/experiments/ENCSR637XSC/

RAD21 ChIP-seq on human GM12878.

link: https://www.encodeproject.org/experiments/ENCSR000EAC/

MAX ChIP-seq on human GM12878.

link: https://www.encodeproject.org/experiments/ENCSR000DZF/

POLR2AphosphoS5 ChIP-seq on human GM12878

link: https://www.encodeproject.org/experiments/ENCSR000BIF/

```
wget https://www.encodeproject.org/files/ENCFF620WFJ/@@download/ENCFF620WFJ.bed.gz -O atac.bed.gz
gunzip atac.bed.gz
wget https://www.encodeproject.org/files/ENCFF712UBB/@@download/ENCFF712UBB.bed.gz -O chip_1.bed.gz
gunzip chip_1.bed.gz
wget https://www.encodeproject.org/files/ENCFF432GNS/@@download/ENCFF432GNS.bed.gz -O chip_2.bed.gz
gunzip chip_2.bed.gz
wget https://www.encodeproject.org/files/ENCFF740DDH/@@download/ENCFF740DDH.bed.gz -O chip_3.bed.gz
gunzip chip_3.bed.gz
```

Sort, bgzip, and index:
```
sudo apt install bedtools

#sort
bedtools sort -i chip_1.bed.gz > sort_chip_1.bed
bedtools sort -i chip_2.bed.gz > sort_chip_2.bed
bedtools sort -i chip_3.bed.gz > sort_chip_3.bed
bedtools sort -i atac.bed.gz > sort_atac.bed

# tabix BEDs
bgzip sort_chip_1.bed
tabix sort_chip_1.bed.gz
bgzip sort_chip_2.bed
tabix sort_chip_2.bed.gz
bgzip sort_chip_3.bed
tabix sort_chip_3.bed.gz
bgzip sort_atac.bed
tabix sort_atac.bed.gz

# or simply like this:
# for el in sort_atac sort_chip_1 sort_chip_2 sort_chip_3
# do
#     bgzip el.bed
#     tabix el.bed.gz
# done
    
```

**JBrowse 2**
* [1] Download and install [JBrowse 2](https://jbrowse.org/jb2/). Create a new jbrowse [repository](https://jbrowse.org/jb2/docs/cli/#jbrowse-create-localpath) in `/mnt/JBrowse/` (or some other folder).

Instalment information got from [here](https://jbrowse.org/jb2/docs/quickstart_web/)

```
sudo apt install npm
sudo npm install -g @jbrowse/cli

# And for path from your source
jbrowse create JBrowse
```

* [0.25] Install nginx and amend its config(/etc/nginx/nginx.conf) to contain the following section:
```conf
http {
  # Don't touch other options!
  # ........
  # ........

  # Comment this line(!):
  # include /etc/nginx/sites-enabled/*;

  # Add this:
  server {
    listen 80 default_server;
    index index.html;
    server_name _;

    # Don't put JBrowse inside the home directory!
    # You will have problems with permissions
    location /jbrowse/ {
      alias /mnt/JBrowse/;	
    }
  }
}
```
For that we do:

```
sudo apt-get install -y nginx
```
and then go to folder and vim file:
```
cd /etc/nginx
sudo vim nginx.conf
```
And add that:

```
 # Add this:
  server {
    listen 80 default_server;
    index index.html;
    server_name _;

    # Don't put JBrowse inside the home directory!
    # You will have problems with permissions
    location /jbrowse/ {
      alias /saletyagin/JBrowse/;	
    }
  }
```

* [0.25] Restart the nginx (reload its config) and make sure that you can access the browser using a link like this: `http://64.129.58.13/jbrowse/`. Here `64.129.58.13` is your public IP address.

To restart and access again:
```
sudo nginx -s reload
http://51.250.64.138/jbrowse/
```

Works just fine.

* [1] Add your files (BED & FASTA & GFF3) to the genome browser and verify that everything works as intended. Don't forget to [index](https://jbrowse.org/jb2/docs/cli/#jbrowse-text-index) the genome annotation, so you could later search by gene names.

Adding genome assembly 
```
sudo jbrowse add-assembly Homo_sapiens.GRCh38.dna.primary_assembly.fa --load copy --out /mnt/JBrowse/
```

We should also do that before:

```
awk '{gsub(/^chr/,""); print}' $sort_chip_1.bed > $(echo $sort_chip_1.bed| cut -d '.' -f 1)'_renamed.bed'
awk '{gsub(/^chr/,""); print}' sort_chip_2.bed > $(echo sort_chip_2.bed| cut -d '.' -f 1)'_renamed.bed'
awk '{gsub(/^chr/,""); print}' sort_chip_3.bed > $(echo sort_chip_3.bed| cut -d '.' -f 1)'_renamed.bed'
awk '{gsub(/^chr/,""); print}' sort_atac.bed > $(echo sort_atac.bed| cut -d '.' -f 1)'_renamed.bed'
```
beacuse BED and GFF chromosomes names differ in files we have to rename them. 

Indexing and adding tracks:
```
tabix -f sort_atac_renamed.bed.gz
tabix -f sort_chip_1_renamed.bed.gz
tabix -f sort_chip_2_renamed.bed.gz
tabix -f sort_chip_3_renamed.bed.gz

sudo jbrowse add-track file.gff.gz --load copy --out /mnt/JBrowse/
sudo jbrowse add-track sort_atac_renamed.bed.gz --load copy --out /mnt/JBrowse/
sudo jbrowse add-track sort_chip_1_renamed.bed.gz --load copy --out /mnt/JBrowse/
sudo jbrowse add-track sort_chip_2_renamed.bed.gz --load copy --out /mnt/JBrowse/
sudo jbrowse add-track sort_chip_3_renamed.bed.gz --load copy --out /mnt/JBrowse/
```
***link***: http://51.250.64.138/jbrowse/?session=share-_knIlEIZwC&password=Ccp2z

**Remember to put a [persistent link](https://jbrowse.org/jb2/docs/user_guides/basic_usage/#sharing-sessions) to a JBrowse 2 session with all your BED files and the genome annotation in the report (like [this](https://jbrowse.org/code/jb2/v2.3.1/?session=share-HShsEcnq3i&password=nYzTU)). I must be able to access it without problems.**

-----

## Extra points [1.5]

* [1] Create a Docker container for running JBrowse 2. It should be a self-contained application, listening on the default HTTP port. Users must be able to mount directories with custom configs and access them later without any problems. 

Hint: to specify the config, use the config=PATH query parameter. E.g. `http://64.129.58.13/jbrowse/?config=my_folder%2Fconfig.json` where `my_folder%2Fconfig.json` is the [escaped](https://en.wikipedia.org/wiki/Percent-encoding) path to the config file.

* [0.5] Give an in-depth explanation of the OSI model and how the TCP/IP stack works. Don't copy-paste descriptions from the internet; paraphrase and shorten as much as possible (imagine writing a cheat sheet for yourself).

OSI (Open System Interconnection) is the model that characterizes the interaction between network equipment. In other words, with the help of OSI, PCs "communicate" with network cards, switches or routers. The process of data transmission over the network with OSI is organised from one level to another. Each of the layers uses information from the previous level and certain protocols:

1. Physical leayer. Information or bits are transmitted either by wires, cables, or via Bluetooth or Wi-Fi.
2. Channel layer. The layer takes bits and transforms them into frames (frames) to determine where to send the information.
3. Network layer. Traffic routing (determining the correspondence between the logical address of the network layer (IP) and the physical address of the device (MAC)).
4. Transport layer. Receiving packets and transmitting them over the network. Connection setup, reliability, and flow control.
5. Session layer. It coordinates communication between applications and it's responsible for establishing, maintaining and terminating communication and exchanging information itself.
6. View layer. Prepares information and converts (compresses, encodes, encrypts) it into an in interpreted language for the user or machine.
7. The application layer. With the help of its protocols, it displays data in a format understandable to the user.

The TCP/IP protocol stack (Transmission Control Protocol/Internet Protocol, Transmission Control Protocol/Internet Protocol) is a network model describing the process of digital data transmission. 

The basic principle of TCP/IP operation is a layer-by-layer grouping of solutions to communication problems, at that data packets are passed through all levels first in direct order before being sent to the target device, and then in reverse order to convert the message to the original format. This method of data transmission allows you to standardize the process and does not require hardware and software management.

The TCP/IP Stack works just like the OSI model by establishing a set of rules and standards for communication in and between the different layers. These rules ensure that different products can communicate with each other because they are developed around the same guidelines.

