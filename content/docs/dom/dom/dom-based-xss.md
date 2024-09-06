---
title: "DOM Based XSS: Unveiling Client-Side Vulnerabilities and Exploitation Techniques"
description: "This tutorial provides an in-depth exploration of DOM Based XSS, focusing on identifying client-side vulnerabilities and demonstrating practical exploitation techniques."
image: "https://armur-ai.github.io/armur-blog-pentest/images/security-fundamentals.png"
icon: "code"
draft: false
---
## Introduction to DOM Based Cross-Site Scripting (XSS)

DOM Based XSS, also known as Type-0 XSS, is a unique type of cross-site scripting vulnerability that arises from client-side code manipulating the Document Object Model (DOM) insecurely. Unlike reflected and stored XSS, which involve the server reflecting or storing malicious scripts, DOM Based XSS exploits vulnerabilities within the client-side JavaScript code itself. 

**Understanding the DOM:**

The DOM is a programming interface for HTML and XML documents. It represents the page as a tree-like structure, where each node represents an element, attribute, or text content. JavaScript can interact with the DOM to dynamically modify the page's structure, content, and style.

**How DOM Based XSS Works:**

1. **User Input:** The user provides input through various means, such as URL parameters, form fields, or even browser features like `document.location`.
2. **Client-Side Scripting:** Client-side JavaScript code retrieves the user input and utilizes it to modify the DOM.
3. **Vulnerable DOM Manipulation:** If the JavaScript code doesn't properly sanitize the user input before using it to update the DOM, an attacker can inject malicious scripts.
4. **Script Execution:** The browser, while rendering the DOM, encounters the injected script and executes it, leading to potential security breaches.

**Key Characteristics of DOM Based XSS:**

* **Client-Side Nature:** The vulnerability resides entirely within the client-side code, without direct server-side interaction.
* **Source of Data:** User-supplied data often comes from sources like URL parameters, cookies, or user interactions.
* **Dynamic Content Modification:** The vulnerability arises when JavaScript dynamically modifies the DOM based on unsanitized user input.

**Impact of DOM Based XSS:**

* **Data Theft:** Attackers can steal sensitive information, including cookies, session IDs, and user input.
* **Session Hijacking:** By stealing session cookies, attackers can impersonate legitimate users.
* **Website Defacement:** Malicious scripts can alter the website's appearance and content.
* **Browser Redirection:** Users can be redirected to malicious websites.


## Identifying and Exploiting DOM Based XSS Vulnerabilities

**1. Analyzing JavaScript Code:**

* **Reviewing Event Handlers:** Examine event handlers like `onclick`, `onmouseover`, and `onload` for instances where user input is used without proper sanitization.
* **Identifying Sources of User Input:**  Trace how user input flows through the JavaScript code, pinpointing areas where it's used to manipulate the DOM.
* **Inspecting `innerHTML`, `document.write`, and `eval`:** Pay close attention to functions like `innerHTML`, `document.write`, and `eval`, as they can be vulnerable if used with unsanitized data.

**Example: Vulnerable JavaScript Code**

```javascript
// Vulnerable code using innerHTML
let userInput = document.location.hash.substring(1); // Getting data from URL hash
document.getElementById("myDiv").innerHTML = userInput; 
```

**2. Using Browser Developer Tools:**

* **Inspecting the DOM:** Use the browser's developer tools (e.g., Chrome DevTools, Firefox Developer Tools) to inspect the DOM structure and identify how JavaScript is modifying it.
* **Debugging JavaScript Execution:** Set breakpoints in the debugger to step through the JavaScript code execution, observing how user input is handled and if it's properly sanitized.
* **Monitoring Network Requests:** While DOM Based XSS doesn't directly involve server-side interaction, monitoring network requests can help identify potential sources of user input or reveal if the client-side code is making unexpected requests based on user input.

**3. Crafting Payloads for DOM Manipulation:**

* **Targeting Vulnerable Functions:** Craft payloads that specifically target the vulnerable functions identified in the JavaScript code. For example, if `innerHTML` is used insecurely, inject a payload like `</script><img src=x onerror=alert(1)>`.
* **Exploiting Event Handlers:** Design payloads that trigger when specific events occur. For example, you could inject a payload into a link's `href` attribute that executes when the link is clicked: `<a href="javascript:alert(1)">Click me</a>`.
* **Encoding and Obfuscation:** Use encoding techniques (URL encoding, HTML encoding) to bypass filters or obfuscate your payloads to make them less detectable.

**Example: DOM Based XSS Payload**

```
http://example.com/#<img src=x onerror=alert(1)> 
```

In this example, the JavaScript code retrieves the value after the `#` in the URL (the hash) and directly inserts it into the DOM using `innerHTML`, leading to the execution of the injected `img` tag with the `onerror` event handler.


## Best Practices for Preventing DOM Based XSS

* **Context-Aware Escaping:** Encode user input based on the context where it's being used. Use HTML encoding when inserting data into HTML, JavaScript encoding when inserting data into JavaScript strings, and URL encoding when constructing URLs.
* **Avoid `eval`, `innerHTML`, and `document.write`:**  Minimize or avoid using functions like `eval`, `innerHTML`, and `document.write`, as they can be prone to DOM Based XSS if not used carefully.
* **Use Safe APIs:** Opt for safer APIs for DOM manipulation, such as `textContent` or `setAttribute`, which automatically handle encoding.
* **Input Validation:**  Validate user input on the client-side to ensure it conforms to expected formats and data types.
* **Content Security Policy (CSP):** Implement a Content Security Policy (CSP) to restrict the sources from which the browser can load scripts. This can help mitigate DOM Based XSS by preventing the execution of scripts from unexpected sources.