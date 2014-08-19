# How NOT to use Encryption
---
# Encryption is cool
---
# But... DDIY + DRITW
---
# Example

- ## 'Share' link to a website
- ## Access is normally password protected
- ## We want to give (limited) access to non-users
- ## non-user = no password (or user...)

---
# Website Access

- ## Normal access: http://bla.com/report/xyz/
- ## Shared access via: <br/>http://bla.com/<font color="red">share</font>/report/xyz/<font color="red">kutfuauLw61xx3dTFXjw</font>
- ## is this secure?

---
# It depends...
---
# More info...

 - ## Requirement 1: Do NOT store extra stuff in the database
 - ## Requirement 2: Only allow existing users to generate a shared link
 - ## Requirement 3: Shared link expires after 24 hours 

---
# Solution Option 0
 - ## Use SSL
 - ## http://bla.com/report/xyz/ => <font color="red">https</font>://bla.com/...
 - ## is this secure?

---
# Solution option 1

## share link
 - Generate a timestamp
 - Encrypt timestamp => token
 - append token to url
## verify link
 - decrypt token
 - verify timestamp
 - if (now - timestamp) < 24 hours, allow access
---
# Solution option 1

## http://bla.com/share/report/xyz/<font color="red">kutfuauLw61xx3dTFXjw</font>
## (Decrypt) => http://bla.com/share/report/xyz/<font color="red">1323864246</font>

<br/>
<br/>
<br/>
<br/>

verify_link.py
---------
	!python
	from time import time

	timestamp = int(decrypt(token)) # 1323875734
	now = int(time())

	if now - timestamp < (24 * 60 * 60):
		return True
	return False
---
# What's wrong with this picture?
![](whats_wrong.jpg)

---
# What's wrong with this picture?
 - ## http://bla.com/share/report/<font color="red">xyz</font>/kutfuauLw61xx3dTFXjw
 - ## http://bla.com/share/report/<font color="red">abc</font>/kutfuauLw61xx3dTFXjw

---
# What's wrong with this picture?
 - ## http://bla.com/share/report/xyz/<font color="red">kutfuauLw61xx3dTFXjw</font>
 - #### http://bla.com/share/report/xyz/<font color="red">kutfuauLw61xx3dTFXjwkutfuauLw61xx3dTFXjw</font>

---
# How can we fix this?
 - ## encrypt the entire path? (share/report/xyz/{timestamp})
 - ## check the length of the encrypted string?
 - ## Hide the timestamp between lots of noise?
 - ## Add a salt / initialization-vector (IV)?
 - ## Use RSA/AES/Blowfish?
---
# Maybe we're using the wrong approach?
---
# Encryption does not prevent or detect modification
---
# DON'T USE ENCRYPTION
---
# How can you tell? 

 - ## Only Encrypt when there is a secret
 - ## If there is no secret, there is no need for encryption
 - ## timestamp is NOT a secret
 - ## URL path is NOT a secret

---
# Solution Option 2

 - ## We need to validate/authenticate the request, not hide it
 - ## We need to detect or prevent modification
 - ## We need a secure 'signature' (e.g. HMAC, Oauth)
 - ## Don't re-invent the wheel
 - ## Try to use a protocol rather than an algorithm or a cryptographic primitive (SHA1 < HMAC-SHA1 < OAUTH)

---
# Coming next
 - ## What is a cryptographic hash function and what can you do with it?
 - ## What makes crypto hashes secure?
 - ## What are collisions and do they really hurt?
 - ## How to turn your <font color="red">hash</font> into <font color="red">cash</font>? or how to become rich in 21 days?
---
# That's it.
