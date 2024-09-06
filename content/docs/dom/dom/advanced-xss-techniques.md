---
title: "Advanced XSS Techniques: Bypassing Filters and Crafting Evasive Payloads"
description: "This tutorial delves into advanced XSS techniques, including filter bypass methods, polyglot payloads, and mutation XSS, empowering you to craft highly evasive attacks."
image: "https://armur-ai.github.io/armur-blog-pentest/images/security-fundamentals.png"
icon: "code"
draft: false
---
## Introduction to Advanced XSS Techniques

As websites implement more robust security measures, such as input validation and output encoding, attackers are constantly developing new and sophisticated techniques to bypass these defenses and successfully execute XSS attacks. This tutorial delves into advanced XSS techniques that go beyond basic payload injections, focusing on filter evasion, encoding tricks, and exploiting browser parsing quirks.

## Bypassing XSS Filters

**1. Multiple Encoding:**

* **Double Encoding:** Encode the payload multiple times using different encoding schemes. This can confuse or bypass filters that only decode once. For example, you could URL encode a payload and then HTML encode the result.
* **Mixed Encoding:** Combine different encoding schemes within the payload. For example, you could URL encode part of the payload and HTML encode another part.

**2. Exploiting Filter Limitations:**

* **Incomplete Blacklists:** Some filters rely on blacklists of specific keywords or characters. Attackers can bypass these filters by using variations of keywords, such as using `<sCrIpT>` instead of `<script>`.
* **Case Sensitivity:** Some filters are case-sensitive. Attackers can exploit this by using mixed-case variations of keywords, such as `<ScRiPt>`.
* **Filter Evasion with HTML5 Features:** Explore new HTML5 features, like event handlers on custom tags or data attributes, that might not be properly filtered.

**3. Obfuscation Techniques:**

* **String Concatenation:** Break the payload into smaller strings and concatenate them using JavaScript. This can make it harder for filters to detect the complete payload.
* **Unicode Escaping:**  Represent characters using their Unicode escape sequences (e.g., `\u003c` for `<`). This can bypass filters that are looking for specific ASCII characters.
* **JavaScript Obfuscation:** Use JavaScript obfuscation tools to make the payload's code difficult to understand and analyze, hindering filter effectiveness.


## Polyglots and Mutation XSS

**1. Polyglot Payloads:**

Polyglot payloads are designed to be valid in multiple contexts, such as HTML, JavaScript, and even CSS. This allows them to bypass filters that are designed to handle only one specific context.

**Example: Polyglot Payload**

```
javascript:/*<script>*/alert(1)/*</script>*/
```

This payload can be interpreted as a JavaScript comment, an HTML comment, and executable JavaScript code.

**2. Mutation XSS:**

Mutation XSS exploits quirks in the way browsers parse and render HTML and JavaScript. By carefully crafting payloads that trigger these quirks, attackers can bypass filters and inject code that wasn't present in the original input.

**Example: Mutation XSS with InnerHTML**

```html
<div id="target"></div>
<script>
let userInput = "<img src=x onerror='alert(1)' </img>"; 
document.getElementById("target").innerHTML = userInput; 
</script>
```

Some older browsers might incorrectly parse the `</img>` tag and create a new script context, allowing the `onerror` event handler to execute even though it wasn't explicitly injected.

## Best Practices for Mitigating Advanced XSS Attacks

* **Layered Defenses:** Implement multiple layers of security, including input validation, output encoding, and Content Security Policy (CSP).
* **Regular Expression Caution:** Be cautious when using regular expressions for input validation or filtering, as they can be complex and prone to errors that attackers can exploit.
* **Stay Updated:** Keep your web application frameworks and libraries up-to-date to patch known vulnerabilities that attackers could leverage for advanced XSS attacks.
* **Security Testing:** Conduct regular security testing, including penetration testing and vulnerability scanning, to identify and address potential weaknesses before they can be exploited.
* **Browser Security Features:** Leverage browser security features, such as XSS Auditor (now deprecated in Chrome) and Content Security Policy (CSP), to mitigate the impact of XSS attacks.