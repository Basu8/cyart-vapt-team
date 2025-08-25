# Remediation Recommendations

## General Best Practices
- Enforce least privilege on all user accounts.
- Apply timely security patches for web servers and application frameworks.
- Maintain inventory of default credentials; disable or change them immediately.

## Task-Specific Remediations

### Task 1 & 2
- Harden exposed services: disable unnecessary ports.
- Use a Web Application Firewall (WAF) to block malicious payloads.
- Regularly update OpenVAS NVTs and perform authenticated scans.

### Task 3
- Remove default Tomcat manager accounts; enforce strong unique passwords.
- Limit manager application access via network ACLs.
- Apply Tomcat security patches to eliminate CVE-2009-3548.

### Task 4
- Enforce stricter UAC policies; avoid default “Prompt for credentials.”
- Enable Windows Defender Credential Guard.
- Implement endpoint detection to monitor suspicious process creation.
- Secure registry keys; audit and block unauthorized Run key modifications.

### Task 5
- Parameterize SQL queries; avoid dynamic concatenation of user input.
- Implement proper output encoding to neutralize XSS.
- Restrict file upload functionality: validate file type, scan for malware.
- Define Content Security Policy (CSP) headers.
- Conduct quarterly VAPT following PTES methodology.
