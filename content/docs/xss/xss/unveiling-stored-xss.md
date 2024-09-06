---
title: "Unveiling Stored XSS: Understanding Persistence and Exploiting Vulnerabilities"
description: "This tutorial delves into the world of stored XSS, exploring its impact, identifying vulnerabilities, and demonstrating exploitation techniques."
image: "https://armur-ai.github.io/armur-blog-pentest/images/security-fundamentals.png"
icon: "code"
draft: false
---
## Introduction to Stored Cross-Site Scripting (XSS)

Stored XSS, also known as persistent or Type II XSS, is a more dangerous type of XSS vulnerability that occurs when malicious scripts are permanently stored on the server. This means that every time a user accesses the affected page, the malicious script is executed in their browser.

**How Stored XSS Works:**

1. **Malicious Script Injection:** An attacker injects malicious JavaScript code into a website through a vulnerable input field, such as a comment section, forum post, or user profile.
2. **Server Storage:** The server stores the injected script along with the legitimate data.
3. **Script Execution:** When a user visits the page containing the stored script, the server sends the script to the user's browser along with the rest of the page content.
4. **Browser Execution:** The user's browser executes the malicious script, potentially compromising their session, stealing their data, or modifying the website's content.

**Impact of Stored XSS:**

* **Wider Impact:** Stored XSS affects all users who access the affected page, making it a more widespread threat than reflected XSS.
* **Increased Severity:**  Attackers can achieve greater control over the victim's browser and potentially gain access to more sensitive data.
* **Website Defacement:** Stored XSS can be used to deface websites by modifying the content displayed to all users.


## Understanding Stored XSS Persistence

The key characteristic of stored XSS is the persistence of the malicious script on the server. This means that the attack is not limited to a single user or session, and it can continue to affect users for a long time until the vulnerability is patched.

**Persistence Mechanisms:**

* **Databases:**  Malicious scripts can be stored in database fields, such as comment text, user profiles, or forum posts.
* **Server-Side Files:** Attackers can upload malicious files to the server if the website has a file upload functionality and does not properly validate the uploaded files.
* **Server-Side Caching:**  If the website uses server-side caching, the malicious script can be cached and served to multiple users even after the original vulnerable input is removed.

## Finding and Exploiting Stored XSS Vulnerabilities

**1. Testing Input Fields:**

* **Focus on Data Storage:** Identify input fields that store data on the server, such as comment sections, forum posts, user profiles, and contact forms.
* **Injecting Test Payloads:**  Insert simple payloads, such as `<script>alert(1)</script>` or `'"><img src=x onerror=alert(1)>`, into these fields and submit the form. Then, visit the page where the data is displayed and check if the payload is executed.

**2. Examining Server-Side Code:**

* **Code Review:**  If you have access to the server-side code, review it for vulnerabilities related to user input handling. Look for instances where user input is directly inserted into the HTML output without proper sanitization.
* **Vulnerability Scanners:**  Use automated vulnerability scanners, such as Burp Suite or OWASP ZAP, to identify potential stored XSS vulnerabilities in the server-side code.

**3. Injecting Persistent Payloads:**

* **Crafting Malicious Scripts:**  Once a vulnerability is identified, craft a malicious script that will achieve the attacker's goal. This could involve stealing cookies, redirecting users, or modifying the website's content.
* **Storing the Payload:**  Inject the payload into the vulnerable input field and submit the form. The payload will be stored on the server and executed whenever a user accesses the affected page.

**Tools for Finding and Exploiting Stored XSS:**

* **Burp Suite:** Burp Suite's web application scanner can automatically detect stored XSS vulnerabilities. It can also be used to manually inject payloads and analyze the server's response.
* **OWASP ZAP:** OWASP ZAP's active and passive scanners can identify stored XSS vulnerabilities. It also provides tools for crafting and delivering payloads.

## Best Practices for Preventing Stored XSS

* **Input Validation:**  Validate all user input on the server-side before storing it in the database or any other persistent storage.
* **Output Encoding:** Encode all user-supplied data before displaying it on the website, even if it has been previously sanitized. 
* **Content Security Policy (CSP):** Implement a Content Security Policy (CSP) to restrict the sources from which the browser can load scripts, styles, and other resources. 
* **Database Security:**  Use parameterized queries or prepared statements when interacting with the database to prevent SQL injection attacks that can lead to stored XSS.
* **Regular Security Audits:** Regularly conduct security audits and penetration testing to identify and address potential vulnerabilities before they can be exploited.