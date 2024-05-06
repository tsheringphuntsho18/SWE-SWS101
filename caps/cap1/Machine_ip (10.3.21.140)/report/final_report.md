# Executive Summary

## Approach
I perform a penetration test on a new server deployed within our college network to check how secure the server is so it is safe when released on the internet.

I did some testing on that machine without any advance knowledge of the machine’s internally facing environment with the goal of identifying vulnerabilities in the machine setup. I, as an ethical hacker, did this without causing any harm just to uncover as many vulnerabilities as possible.

I looked carefully into every vulnerability that I found, to see if a hacker could break into it or not to gain root access.

## Scope
The scope of this penetration testing is to check how secure the new server that one of the lectures has hosted within our college network to make sure it is safe to release on the internet. 
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/scans/web/host.png)


## Assessment Overview and Recommendations
During the internal penetration test, I have found 23 ports are opened, which are all vulnerable and easily exploited using metasploit. I tried to gain root access to the machine ip address by metasploit where I successfully got into the machine.

What I found is that, by using metasploit only  I can gain root access through all open ports. For the ftp server I can easily  login with the anonymous username where I don’t need to enter a password. I recommend closing the ftp port.

One finding involves a ssh server login, where username and password can be easily brute forced. For my case I tried brute forcing using metasploit. Fortunately, these issues can be corrected by giving strong passwords and by not repeating the same passwords. 

The next issue was the http server that was found running on the machine. From the metasploit, I got the information that http server is powered by PHP/5.2.4 which is useful in exploitation. Here I also found shared files paths like /doc/ , /icons and more, which means any users in that network can access a considerable amount of data. This configuration should be changed to ensure that users can access only what is necessary.   

# Network Penetration Test Assessment Summary
## Summary of Finding
During the course testing, I uncovered a total of 3 findings that pose a material risk to the machine they are ftp, ssh and http. And I conclude that any open port can be exploited by metasploit.

In ftp, I found that anonymous username can be used to login without password. In ssh, username and password can be brute force. And in http, php info was leaked and can be exploited with the cgi_bin using metasploit.

# Network Compromise Walkthrough
The steps below demonstrate the steps taken from initial access to root access. The intent of this attack chain is to demonstrate how an attacker can gain root access to the machine if it was vulnerable.

Steps for attack are as followed: 
1. Upon connecting to the gcbs network, I started nmap to find the open port.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/scans/service-enumeration/nmap-all-port-scan.png)

2. I found many ports are open. Then I scan each port in particular.

This is for port 80
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/scans/service-enumeration/nmap-scan-p80.png)

This is for port 21
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/scans/service-enumeration/nmap-scan-p21.png)

3. From the scan I knew that there was a website, so I pasted the ip address on the browser.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/scans/vulnerability-scan/swscap1.1.png)
Since this was our assignment, it has detailed information about the assignment.

This was all about gathering information about the machine. Now from here I begin my pentest.

4. Started a msfconsole metasploit and I am going to look for the version and see if I can get more information and use the auxiliary inside of the metasploit. Auxiliary modules are a fascinating feature of the framework allowing it to extend for a variety of purposes other than exploitation. 
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step1.png)

5. Search for http_version
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step2.png)

6. I found the auxiliary for http_version, now let's use this auxiliary and see what are the options.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step3.png)

7. In the rhosts field we have to set a target ip address.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step4.png)
Now i see the target ip address in the rhosts field.

8. Let's run it
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step5.png)
We got some information here that Apache/2.2.8 (Ubuntu) is powered by PHP.

9. Search for dir_scanner for further information.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step6.png)

10. Let's use it and see options under dir_scanner.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step7.png)
Here also we have to set a target ip address.

11. Set the rhosts.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step8.png)

12. Now run it.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step9.png)
I am going to use php cgi for exploits.

13. Go back and search for php cgi.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step10.png)
Now I am going to use php_cgi_arg_injection argument for http exploit.

14. Again see what are the options in it.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step11.png)

15. We have set rhosts
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step12.png)
Now everything is ready to begin to exploit.

16. Lets exploit.
![nmap](/caps/cap1/Machine_ip%20(10.3.21.140)/evidences/exploitation/exploit_httpServer_using_metasploit/step13_exploit.png)
So boom, I have got into the machine. I can see it’s metasploitable and running in linux 2.6.24-16-server. Now I have full control of that machine. Technically we compromise the machine.

With this step, I can exploit every port that is open, I just need to use the correct auxiliary and the exploit module. As a sample I exploit ssh also and evidence is pushed in github.

# Remediation Summary
As a result of this assessment there are several opportunities to strengthen the machine security before resealing on the internet. Remediation efforts are as mentioned below;

1. Set strong passwords(24+ characters) for all accounts.
2. Disable directory listing on the http.
3. Enhance networking logging and monitoring.
4. Apply the latest security patches provided by the apache for the apache framework.
5. Implement network level control, such as firewall and intrusion detection.
6. Disable anonymous access to the FTP server and enforce user authentication for all file transfers.
7. Replace FTP with secure file transfer protocol(SFTP).
8. Conduct regular security assessments and penetration tests to identify and address vulnerabilities. 

 