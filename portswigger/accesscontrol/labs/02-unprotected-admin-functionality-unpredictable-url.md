# Lab 02 — Unprotected Admin Functionality with Unpredictable URL

## What We Learned

- **Security by Obscurity:** Making URLs unpredictable does not provide actual security.
- **JavaScript Disclosure:** Client-side JavaScript can expose hidden administrative URLs.
- **Source Code Inspection:** View Page Source, Developer Tools and Burp Suite can help discover hidden endpoints.
- **Direct URL Access:** Hidden links can be accessed directly if the server does not enforce authorization.
- **Server-Side Authorization:** Administrative functionality must be protected through proper server-side permission checks.

### Key Takeaway

A hidden or unpredictable URL is not an authorization mechanism. Anything delivered to the browser can be inspected, and every sensitive endpoint must enforce server-side access control.
