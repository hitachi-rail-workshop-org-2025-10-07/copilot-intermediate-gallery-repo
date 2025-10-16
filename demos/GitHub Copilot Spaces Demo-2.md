# GitHub Copilot Spaces Demo

Welcome to the GitHub Copilot Spaces demo! In this exercise, you'll learn how to create and utilize GitHub Copilot Spaces to collaborate on development tasks within the Photo Gallery & Portfolio application.

> **Demo Reference:**  
> This walkthrough is based on [@ps-copilot-sandbox/copilot-intermediate-gallery-repo/files/demos/copilot-spaces.md](https://github.com/ps-copilot-sandbox/copilot-intermediate-gallery-repo/blob/main/demos/copilot-spaces.md).

## What You'll Learn
By the end of this demo, you will:
- [ ] Understand what GitHub Copilot Spaces are and their benefits
- [ ] Know how to create a new Copilot Space
- [ ] Be able to set up a Space with specific goals and context
- [ ] Complete development tasks using collaborative AI assistance
- [ ] Share and manage Spaces with team members

**Estimated Time:** 20-25 minutes

## 🎯 Step 1: Create Your First Copilot Space

**Goal:** Set up a dedicated Copilot Space for working on gallery features.

### Setup
1. Go to `https://github.com/copilot/spaces`
2. Select `Create Space`

### Space Creation Option

1. Type in name `Photo Gallery - Security Assessment`
2. Select the owner `Username` OR `OrgName`
3. Add in description `Implement security best practices for the photo gallery application`
4. Select `Save`

**Adding instructions**

5. Select `Instructions` and add the following prompt:
```markdown
You are a security expert helping to analyze and improve the security posture of a Next.js 15 photo gallery application. Focus on:

- File upload security vulnerabilities and mitigations
- Input validation and sanitization
- Authentication and authorization patterns
- XSS prevention in user-generated content
- Secure image processing and storage
- OWASP Top 10 web application security risks
- Next.js specific security best practices

Provide specific code examples and security recommendations that follow industry standards and OWASP guidelines. Consider both client-side and server-side security measures.
```
6. Select save

**Adding sources**

7. Select `Add sources` and select `Add files and repositories`
8. Add the following files and press `save`
```markdown
src/components/upload/UploadZone.tsx
src/lib/mock-photo-data.ts
src/app/layout.tsx
next.config.ts
```

9. Select `Add sources` and select `Link files, pull requests, and issues`
10. Add issue link relevant to your org or personal account.

> [!NOTE]
> If you use a personal account as the Copilot Space owner, you can link issues directly (e.g., `https://github.com/ps-copilot-sandbox/copilot-intermediate-gallery-repo/issues/3`).  
> When the Space owner is set to an enterprise organization, you may see an error ("This doesn't belong to the intermediate-demos organization.") if you try to link a repo issue from outside your org.  
> To demo this successfully, copy the issue into your org's repo and link it there.

11. Select `Add sources` and select `Add text content`
12. Add the following content and press `save`
```markdown
## OWASP Top 10 2021 - Key Security Risks for Web Applications

1. **A01 Broken Access Control** - Users can act outside of their intended permissions
2. **A02 Cryptographic Failures** - Failures related to cryptography which often leads to sensitive data exposure
3. **A03 Injection** - User-supplied data is not validated, filtered, or sanitized by the application
4. **A04 Insecure Design** - Risks related to design and architectural flaws
5. **A05 Security Misconfiguration** - Missing appropriate security hardening across any part of the application stack
6. **A06 Vulnerable and Outdated Components** - Using components with known vulnerabilities
7. **A07 Identification and Authentication Failures** - Confirmation of the user's identity, authentication, and session management
8. **A08 Software and Data Integrity Failures** - Code and infrastructure that does not protect against integrity violations
9. **A09 Security Logging and Monitoring Failures** - Failures in logging and monitoring coupled with missing or ineffective integration with incident response
10. **A10 Server-Side Request Forgery** - SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL

## Next.js Security Headers
- Content Security Policy (CSP)
- X-Frame-Options
- X-Content-Type-Options
- Referrer-Policy
- Permissions-Policy

## File Upload Security Considerations
- File type validation
- File size limits
- Malware scanning
- Secure file storage
- Image processing vulnerabilities
```

---

## 🤝 Step 2: Collaborate and Share

**Goal:** Use your Copilot Space to complete the analysis and tasks listed below.

### Analysis Task

1. Go to your Copilot Space
2. Type in the following prompt to analyze security vulnerabilities:

```markdown
I need help identifying and fixing security vulnerabilities in our photo gallery application. Please analyze our file upload component and suggest:

1. How to validate file types securely (not just by extension)
2. Protection against malicious file uploads and XSS attacks
3. Proper input sanitization for photo titles and tags
4. Content Security Policy (CSP) headers for Next.js
5. Rate limiting strategies for upload endpoints

Based on the OWASP Top 10 guidelines, what are the most critical security issues I should address first in this photo gallery application?
```

3. Ask another question! What else do you want to learn about Copilot Spaces?

### Final discussion

- How were you able to collaborate with your team using Copilot Spaces?
- How did Copilot’s suggestions help (or hinder) your collaboration?
- What would you do differently next time to improve teamwork and productivity?

Share your thoughts and any tips you discovered for making the most of Copilot Spaces in a team setting.

**Expected Result:** You will have successfully used AI assistance with industry-standard external sources to conduct an analysis for the Photo Gallery & Portfolio application.

## ✅ Completion Checklist

Mark off each item as you complete it:

- [ ] Created a new GitHub Copilot Space with a clear focus
- [ ] Set detailed instructions incorporating industry standards
- [ ] Added relevant project files to the Space context
- [ ] Used the Space to analyze existing code structure
- [ ] Implemented or proposed improvements with AI assistance
- [ ] Tested the implemented improvements and addressed issues
- [ ] Documented progress and decisions within the Space
- [ ] Shared or saved the Space for future collaboration
