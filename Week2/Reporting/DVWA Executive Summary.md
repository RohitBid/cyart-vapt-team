

Rohit Kumar Bid conducted a comprehensive Vulnerability Assessment and Penetration Test (VAPT) against the DVWA (Damn Vulnerable Web Application) hosted at 10.236.92.45:8080. The assessment was performed using a black-box approach between March 23, 2026, and March 27, 2026, without prior knowledge of the target environment.

The objective of this assessment was to identify security weaknesses, evaluate their potential impact, and demonstrate real-world exploitation scenarios.

During the assessment, a total of **5 vulnerabilities** were identified:
- **3 Critical**
- **2 High**
- **0 Medium / Low / Informational**

Key critical vulnerabilities included:
- SQL Injection enabling full database compromise
- Unrestricted File Upload leading to Remote Code Execution (RCE)
- Brute Force vulnerability allowing credential compromise

High-risk vulnerabilities included:
- Cross-Site Request Forgery (CSRF)
- Cross-Site Scripting (XSS)

Successful exploitation of these vulnerabilities allowed the tester to gain unauthorized access, execute system commands, retrieve sensitive data (e.g., `/etc/passwd`), and demonstrate full system compromise.

Overall, the security posture of the application is **weak and highly vulnerable to attack**. Immediate remediation is required for critical and high-risk findings to prevent exploitation by malicious actors.

It is recommended that DVWA implements secure coding practices, input validation, authentication controls, and continuous security testing to strengthen its security posture.
