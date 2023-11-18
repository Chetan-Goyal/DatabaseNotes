---
title: Section 15 - Homomorphic Encryption
created: '2023-11-18T16:10:35.631Z'
modified: '2023-11-18T16:14:03.934Z'
---

# Section 15 - Homomorphic Encryption

It allows executing DB queries on encrypted data.

> Homomorphic encryption is the conversion of data into ciphertext that can be analyzed and worked with as if it were still in its original form. Homomorphic encryption enables complex mathematical operations to be performed on encrypted data without compromising the encryption.

### Note:
1. It is evolutionary since existing soution doesn't allow encrypting data in db and Level 7 Load Balancers (or Reverse Proxies) needs decryption keys to give more fine grained control like url pattern matching redirecting to different microservice. Currently, we have to give our private keys so they can decrypt and manage that.
2. It is extremely slow as of now (2-3 minutes time for very small query :') 
