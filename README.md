# Secure Coding Review - Vulnerable App Analysis

## Project Overview
Security audit of a vulnerable Python Flask application using Bandit static analyzer.

## What I Did
✅ Created vulnerable Python application
✅ Ran Bandit security scanner
✅ Identified 9 vulnerabilities (1 High, 4 Medium, 4 Low)
✅ Documented all findings with recommendations
✅ Provided remediation steps for each vulnerability

## Tools Used
- **Bandit 1.9.4** - Python security analyzer
- **Python 3.13.12**
- **Kali Linux**

## Vulnerabilities Found

### High Severity (1)
- Flask Debug Mode Enabled (B201)

### Medium Severity (4)
- SQL Injection (B608)
- Unsafe Pickle Deserialization (B301)
- Dangerous eval() Usage (B307)
- Binding to All Interfaces (B104)

### Low Severity (4)
- Hardcoded Passwords (B105 x3)
- Pickle Module Import (B403)
<img width="1326" height="1336" alt="Screenshot 2026-10-14 075216" src="https://github.com/user-attachments/assets/902050f9-1ee5-4031-acf4-502a62b73a9d" />

## Key Findings
- Total Issues: 9
- Critical issues that allow RCE and SQL injection
- All vulnerabilities documented with fixes

## Files
- `vulnerable_app.py` - The vulnerable application
- `secure_code_review_report.md` - Complete audit report with recommendations


## Executive Summary
Security audit of vulnerable_app.py identified 9 security vulnerabilities:
- 1 High Severity
- 4 Medium Severity
- 4 Low Severity

---

## Vulnerabilities Found

### 1. HIGH SEVERITY: Flask Debug Mode Enabled
**Issue:** B201 - Flask running with debug=True
**Location:** Line 47
**Risk:** Exposes Werkzeug debugger, allows arbitrary code execution
**CWE:** CWE-94

**Current Code:**
```python
app.run(debug=True, host='0.0.0.0')
```

**Recommendation:**
```python
app.run(debug=False, host='127.0.0.1')
```

**Why:** Disable debug mode in production. Use localhost instead of all interfaces.

---

### 2. MEDIUM SEVERITY: SQL Injection Vulnerability
**Issue:** B608 - String-based SQL query construction
**Location:** Line 19
**Risk:** Attackers can inject malicious SQL code
**CWE:** CWE-89

**Current Code:**
```python
cursor.execute(f"SELECT * FROM users WHERE name = '{query}'")
```

**Recommendation:**
```python
cursor.execute("SELECT * FROM users WHERE name = ?", (query,))
```

**Why:** Use parameterized queries to prevent SQL injection.

---

### 3. MEDIUM SEVERITY: Unsafe Pickle Deserialization
**Issue:** B301 - pickle.loads() on untrusted data
**Location:** Line 28
**Risk:** Attackers can execute arbitrary code through pickled objects
**CWE:** CWE-502

**Current Code:**
```python
obj = pickle.loads(data)
```

**Recommendation:**
```python
import json
obj = json.loads(data)  # Use JSON instead
```

**Why:** Never deserialize untrusted pickle data. Use JSON for safer serialization.

---

### 4. MEDIUM SEVERITY: Dangerous eval() Usage
**Issue:** B307 - Use of eval() function
**Location:** Line 42
**Risk:** Allows execution of arbitrary Python code
**CWE:** CWE-78

**Current Code:**
```python
result = eval(expression)
```

**Recommendation:**
```python
import ast
result = ast.literal_eval(expression)
```

**Why:** Use ast.literal_eval() for safer evaluation of literals.

---

### 5. MEDIUM SEVERITY: Binding to All Interfaces
**Issue:** B104 - host='0.0.0.0'
**Location:** Line 47
**Risk:** Application accessible from any network interface
**CWE:** CWE-605

**Current Code:**
```python
app.run(debug=True, host='0.0.0.0')
```

**Recommendation:**
```python
app.run(debug=False, host='127.0.0.1')
```

**Why:** Bind to localhost (127.0.0.1) instead of all interfaces in development.

---

### 6. LOW SEVERITY: Hardcoded Password - admin123
**Issue:** B105 - Hardcoded credentials
**Location:** Line 9
**Risk:** Credentials exposed in source code
**CWE:** CWE-259

**Recommendation:**
Use environment variables instead:
```python
import os
DATABASE_PASSWORD = os.getenv('DB_PASSWORD')
```

---

### 7. LOW SEVERITY: Hardcoded Secret Key
**Issue:** B105 - Hardcoded secret key
**Location:** Line 10
**Risk:** Secret exposed in code repository
**CWE:** CWE-259

**Recommendation:**
```python
import os
SECRET_KEY = os.getenv('SECRET_KEY')
```

---

### 8. LOW SEVERITY: Hardcoded Password - 123456
**Issue:** B105 - Weak hardcoded password
**Location:** Line 33
**Risk:** Weak password hardcoded in code
**CWE:** CWE-259

**Recommendation:**
Use secure password hashing:
```python
from werkzeug.security import check_password_hash
check_password_hash(stored_hash, password)
```

---

### 9. LOW SEVERITY: Pickle Module Import
**Issue:** B403 - Unsafe pickle import
**Location:** Line 3
**Risk:** Pickle module can deserialize arbitrary code
**CWE:** CWE-502

**Recommendation:**
Remove pickle import and use JSON:
```python
import json  # Instead of pickle
```

---

## Best Practices Applied

✅ **Parameterized Queries** - Prevent SQL injection
✅ **Environment Variables** - Secure credential management
✅ **Safe Serialization** - Use JSON instead of pickle
✅ **Restricted Bindings** - Localhost instead of 0.0.0.0
✅ **Debug Disabled** - No debug mode in production
✅ **Safe Evaluation** - Use ast.literal_eval instead of eval()

---

## Tools Used

**Static Analyzer:** Bandit 1.9.4
**Language:** Python 3.13.12
**Analysis Date:** 2026-10-13
**Total Lines Scanned:** 31

---

## Conclusion

The vulnerable application contained critical security issues that could allow:
- Remote Code Execution (RCE)
- SQL Injection attacks
- Unauthorized access

All identified vulnerabilities should be remediated before deployment.

---

## What I Learned
✅ How to use Bandit for code analysis
✅ Common Python security vulnerabilities
✅ SQL injection prevention techniques
✅ Secure coding best practices
✅ Credential management with environment variables


**CodeAlpha Cybersecurity Internship - Task 3**

Author: Dominic Joshua
