# LAB 4: ACTIVE ACTIVITY – INDIVIDUAL

## Student Information
- **Name:** ANIS SOFIA BINTI ADNAN
- **Student ID:** 52215226027
- **Class:** L01B01

# 1. Target Setup (Victim)

- **Operating System:** Ubuntu Linux
- **Application:** OWASP Juice Shop
- **Target IP:** 10.0.2.15
- **Target Port:** 3000
- **URL:** http://10.0.2.15:3000

**Evidence:**
<img width="852" height="120" alt="image" src="https://github.com/user-attachments/assets/56f64e73-d008-4794-a834-f0e490235edc" />

<img width="975" height="557" alt="image" src="https://github.com/user-attachments/assets/6b3e1724-8be9-41d0-9eed-99f9d6a42da4" />

# 2. Own Host (Attacker)

- **Operating System:** Kali Linux
- **Attacker IP:** 10.0.2.3
- **Network Mode:** NAT Network
- **Network Name:** Lab4-Network
- **Assigned CIDR:** 10.0.2.0/24

**Commands Used:**
```bash
ip addr
ip route
```

**Interpretation:**

Kali Linux was used as the attacker machine. It was connected to the same NAT Network as the Ubuntu target, allowing communication between both machines.

**Evidence:**
 <img width="736" height="569" alt="image" src="https://github.com/user-attachments/assets/98549ae0-3462-4d64-aa36-9bb35acc96fe" />

# 3. Active Reconnaissance

## Part 1: Lab Setup

Both virtual machines were configured using VirtualBox and connected to the same NAT Network.

- Attacker: Kali Linux
- Target: Ubuntu Linux
- Adapter Mode: NAT Network
- Network CIDR: 10.0.2.0/24

**Evidence:** 
<img width="581" height="393" alt="image" src="https://github.com/user-attachments/assets/eebe4ac2-07d8-49ec-a3f9-5880b166ee93" />

<img width="587" height="392" alt="image" src="https://github.com/user-attachments/assets/9f1bfa61-c9ed-4b32-b43d-cc4bb09cfde9" />


## Part 2: Own Host and Discovered Hosts

**Commands Used:**
```bash
ip addr
ip route
nmap -sn 10.0.2.0/24
```

**Discovered Hosts:**

<img width="560" height="173" alt="image" src="https://github.com/user-attachments/assets/e6ce1687-1741-4c01-a862-ebc456a88de1" />


**Interpretation:**

The Nmap scan discovered four active hosts. Ubuntu Linux (10.0.2.15) was identified as the target machine.

**Evidence:**
<img width="690" height="320" alt="image" src="https://github.com/user-attachments/assets/77003059-a9a5-4cec-915f-c7fe571fca97" />


## Part 3: Services and Interpretation

**Command Used:**
```bash
nmap -sV 10.0.2.15
```

**Scan Result:**

<img width="560" height="77" alt="image" src="https://github.com/user-attachments/assets/e758667e-9902-4388-bdae-df2e34c223e9" />


**Interpretation:**

Port 3000 was found open on the target machine. The HTTP response and OWASP Juice Shop page title confirmed that the port was hosting the web application.

The application was accessible from Kali Linux through:

`http://10.0.2.15:3000`

**Evidence:** 

<img width="702" height="511" alt="image" src="https://github.com/user-attachments/assets/ac08ca6a-c7ab-4255-8575-dc0fb7459f73" />


## Part 4: Limitations and Risks

- Some hosts or services may not be detected (false negatives).
- Filtered ports may prevent accurate identification of services.
- Nmap may not always identify the exact service or version.
- The scan was limited to the assigned virtual network.
- Results were based on the network configuration and scan performed.

# 4. Findings and Evidence

## 4.1 SQL Injection

**Objective:** Test whether the login function is vulnerable to SQL Injection.

**Method:** Entered `' OR 1=1--` into the email field with a random password.

**Observation:** The application allowed access to the administrator account without valid credentials.

**Impact:** Unauthorised users may bypass authentication.

**Evidence:** 

<img width="975" height="440" alt="image" src="https://github.com/user-attachments/assets/855988b6-054c-45e5-b538-a5c41d305364" />

<img width="975" height="494" alt="image" src="https://github.com/user-attachments/assets/b2871df3-adb4-4204-8d77-512e73a057eb" />


## 4.2 Broken Access Control

**Objective:** Test whether one customer can access another customer's basket.

**Method:** Created two customer accounts and used Burp Suite Repeater to change the basket ID in the request.

**Observation:** Customer A was able to access Customer B's basket using Customer A's session.

**Impact:** Unauthorised users may access another user's basket information.

**Evidence:** 

/rest/basket/8 belongs to testcustomer1@example.com

<img width="975" height="551" alt="image" src="https://github.com/user-attachments/assets/2261926b-ba70-4a12-998e-3aafed747078" />
<img width="975" height="475" alt="image" src="https://github.com/user-attachments/assets/6c55ad21-1210-4eb1-a57a-ab8502e06b08" />

/rest/basket/7 belongs to testcustomer2@example.com

<img width="975" height="542" alt="image" src="https://github.com/user-attachments/assets/b00d4efd-fae4-40b2-90fc-ec26ced9ce2a" />
<img width="975" height="470" alt="image" src="https://github.com/user-attachments/assets/4a678406-ede8-497c-a61d-0aa231d5739c" />

Here as we login as testcustomer1 (/rest/basket/8), we can still view the ordering details of testcustomer2 (/rest/basket/7):

<img width="975" height="547" alt="image" src="https://github.com/user-attachments/assets/debd4de4-b9e7-4aa5-be57-eef4838746e9" />

## 4.3 DOM-Based Cross-Site Scripting (DOM XSS)

**Objective:** Investigate whether user input can cause unsafe JavaScript execution in the browser.

**Method:**  Entered a test payload into the Juice Shop search field to check whether JavaScript could be executed through user input.

**Observation:** A JavaScript alert appeared in the browser after submitting the test payload.

**Impact:** Attackers may inject malicious JavaScript that executes in another user's browser.

**Evidence:** 
<img width="975" height="444" alt="image" src="https://github.com/user-attachments/assets/f4eb06dd-7d02-411b-9cc8-eead39e54a0a" />


# 5. Overall Analysis and Conclusion

The assessment covered active reconnaissance, host discovery, service identification and vulnerability testing using OWASP Juice Shop.

The findings demonstrated security weaknesses involving authentication and access control. The reconnaissance process also helped identify the target machine and its available web service.

# 6. References

1. OWASP Juice Shop: https://owasp.org/www-project-juice-shop/
2. Nmap: https://nmap.org/
3. PortSwigger Web Security Academy: https://portswigger.net/web-security
