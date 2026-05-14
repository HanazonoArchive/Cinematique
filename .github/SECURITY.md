# Security Policy

> **📦 ARCHIVED PROJECT:** This is a completed educational/school project. For security concerns or questions, please understand this is provided for learning purposes only.

## Important Note

**This project is ARCHIVED and NOT PRODUCTION-READY.** It is maintained solely for portfolio and educational reference. Do not use this code in production environments without substantial security improvements.

## Reporting Security Vulnerabilities

This is a school/educational project, but if you discover a security issue, please responsibly disclose it by:

1. **Email notification** - Contact the project maintainers directly
2. **Include details:**
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if applicable)

## Known Security Considerations

This project is created for **educational purposes** and is not intended for production use. The following security practices should be improved before any production deployment:

### Current Limitations:

1. **Hardcoded Credentials**
   - Database credentials are stored in source code
   - Default admin/user credentials included
   - **Never use this code with real user data**

2. **Password Security**
   - Passwords may not be properly hashed
   - Missing salt in password storage
   - No encryption for sensitive data

3. **Database Security**
   - Limited input validation/SQL injection protection
   - No prepared statements in some areas
   - Default MySQL permissions

4. **Authentication**
   - Basic authentication without modern security measures
   - No session timeout
   - Missing multi-factor authentication

5. **Data Privacy**
   - No encryption for data in transit or at rest
   - User data stored in plain text fields
   - No GDPR compliance measures

## For Production Use

If you fork this project for production purposes, you **must**:

- [ ] Implement proper password hashing (bcrypt, argon2)
- [ ] Use environment variables for credentials
- [ ] Add input validation and sanitization
- [ ] Implement SQL prepared statements
- [ ] Add HTTPS/TLS encryption
- [ ] Implement proper session management
- [ ] Add rate limiting and DDoS protection
- [ ] Audit and penetration test
- [ ] Implement proper logging and monitoring
- [ ] Follow OWASP security guidelines

## Educational Context

This project demonstrates:
- Basic database connectivity
- User authentication concepts
- Role-based access control fundamentals
- MVC architectural patterns

It is **not** a production-ready security example.

## Responsible Disclosure

If security issues are found, please give the maintainer reasonable time to respond and address concerns before public disclosure.
