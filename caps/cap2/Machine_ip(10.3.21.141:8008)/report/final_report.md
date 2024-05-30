# Executive Summary

## i. Approach
I as a Bug Bounty Hunter was hired by a RUB to check vulnerabilities in the web application that the university has developed. So I make sure to check all the common web vulnerabilities before deployed on the internet. I, as an ethical hacker, did this without causing any harm just to uncover as many vulnerabilities as possible.

## ii. Assessment Overview and Recommendations

During the bug bounty hunting, I have found 3 vulnerabilities for the http://10.3.21.141:8008(website), which were all vulnerable and easy to attack.

What I found is that, we can even  register into that website without even entering a password and we can also login without password. The website is vulnerable to cross-site scripting. Though inserting an alert message isn't terribly dangerous, if we can inject that, you can inject other scripts that are more malicious and steal the victim's private information associated with the website in question. 

The website is also vulnerable to privilege escalation. I can easily convert myself to administrator and gain all root access. The website is also vulnerable to path traversal where users can use a path traversal attack to read files from other folders that they shouldn't have access to. Likewise I looked for the common file that is secret.txt. 

# Website Penetration Test Assessment Summary
I began all activities from the perspective of a hacker on the given machine. Other than the machine ip address I was not provided with any additional  information.  Following are the 3 web applications.

1. http://10.3.21.141:8000
2. http://10.3.21.141:8008
3. https://www.hackthissite.org

## URL 2: 10.3.21.141:8008

### Vulnerability 1 Cross-Site Scripting (XSS)
Cross-site scripting (XSS) is a vulnerability that permits an attacker to inject code (typically HTML or JavaScript) into the contents of a website not under the attacker's control.

To carry out a cross-site scripting attack, an attacker injects a malicious script into user-provided input. Attackers can also carry out an attack by modifying a request. If the web app is vulnerable to XSS attacks, the user-supplied input executes as code. 

An attacker can use XSS to send a malicious script to an unsuspecting user. The end user’s browser has no way to know that the script should not be trusted, and will execute the script. Because it thinks the script came from a trusted source, the malicious script can access any cookies, session tokens, or other sensitive information retained by the browser and used with that site. These scripts can even rewrite the content of the HTML page. 


<b>Steps to reproduce cross-site scripting</b>

Once I logged into this website, in the nav bar there is an option to add a new snippet where users can add snippets.

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb1.png)

Typically, we also can get JavaScript to execute on a page when it's viewed by another user. A simple JavaScript function to use when hacking is the alert() function. 

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb2.png)

Then when other users hover on my snippets then their cookies will pop up. Likewise we can add any scripts which are harmful.

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb3.png)

So this shows that, this website has an XSS vulnerability.

### Vulnerability 2 Elevation of Privilege
Elevation of privilege is also known as privilege escalation attack is a security exploit or technique used by attackers, starting with compromised or stolen credentials to gain unauthorized access to higher-level permissions within a computer system.
 	
With this attack, hackers can get administrative control and they will steal data and manipulate the system database and many more.

Privilege escalation attacks can have severe consequences. Once hackers obtain elevated privileges, they can perform a range of unauthorized actions, such as deleting databases, installing malware, stealing sensitive files or disabling crucial services.

<b>Steps to reproduce elevation of privilege<b>

Right now I am just a user on that website but I can convert my user account to administrator account as this website is also vulnerable to privilege escalation.

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb4.png)

I can only edit my profile.
![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb5.png)

To get administrator access, I can set admin=True provided the username. The URL will look like this;

10.3.21.141:8008/471541268652354669237873339020654786900/saveprofile?action=update&is_admin=True&uid=Pulu

Though the account is now marked as an administrator but your cookie still says you're not. So sign out and back in to get a new cookie. After logging in, notice the 'Manage this server' link on the top right.

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb6.png)

Now I can edit the data of every user.

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb7.png)

This website is also vulnerable to privilege escalation.

### Vulnerability 3 Path Traversal
A path traversal vulnerability allows an attacker to access files on your web server to which they should not have access. Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application.

When a directory traversal attack is successful, one of the most serious outcomes is a data breach. This happens when confidential information which could include user data and many more.

<b>Steps to reproduce path traversal</b>

This attack is not even necessary in many cases, people often install applications and never change the defaults. So the first thing an attacker would try is the default value. So what I tried was the path to check the file containing the secret file like secret.txt.

http://10.3.21.141:8008/471541268652354669237873339020654786900/../secret.txt 

Some browsers, like Firefox and Chrome, block ../ in URLs. This doesn't provide any security protection because an attacker will use %2f to represent / in the URL.

http://10.3.21.141:8008/471541268652354669237873339020654786900/..%2fsecret.txt

![gruyere](/caps/cap2/Machine_ip(10.3.21.141:8008)/evidences/exploitation/bb8.png)

But in this website, though it contain the secret.txt file but the contain is nothing to deal with the data breach as the file is same as empty.

# Remediation Summary
As a result of this assessment there are several opportunities to strengthen this website security before deployed on the internet. Remediation efforts are as mentioned below;

## Cross site scripting
<b>Input Validation:</b> Ensure all user inputs are validated to conform to expected formats. Reject any inputs containing potentially malicious data.

<b>Output Encoding:</b> Encode outputs to prevent the execution of injected scripts in the browser.

<b>Content Security Policy (CSP):</b> Implement CSP to restrict the sources from which scripts can be loaded and executed.

<b>Sanitization Libraries:</b> Use well-established libraries for input sanitization, such as OWASP Java Encoder or DOMPurify for JavaScript.

## Privilege escalation
<b>Least Privilege Principle:</b> Ensure that users and processes operate with the least amount of privilege necessary.

<b>Access Controls:</b> Implement strict access controls and permissions for all users and processes.

<b>Authentication Mechanisms:</b> Strengthen authentication mechanisms, including multi-factor authentication (MFA) for sensitive accounts.

<b>Regular Audits:</b> Conduct regular audits of user privileges and access logs to detect and prevent unauthorized privilege escalation.


## Path traversal
<b>Input Validation:</b> Validate and sanitize all user inputs that interact with the file system to ensure they do not contain path traversal sequences such as ../.

<b>Canonicalization:</b> Use canonical paths to compare file locations and ensure they remain within the designated directory.

<b>Access Controls:</b> Implement strict access controls to prevent unauthorized file access.

<b>Error Handling:</b> Avoid exposing file system structure in error messages.

