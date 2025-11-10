# First Contact Protocol — Secure Initial Communication without Pre-Shared Keys

This project implements a working “First Contact Protocol” that allows two users
(**Pilot-Alpha** and **Control-Bravo**) to securely communicate on an untrusted network **without any pre-shared secrets**.

## Authentication Model Used
**Trusted Introducer Model (Model A)**  
A Registry Server is trusted by all clients.  
Clients publish their long-term public keys to this server.  
Before communication, each client requests the peer’s public key from the trusted registry.

## Security Goals Achieved
| Requirement | Achieved? | How |
|------------|-----------|-----|
| Confidentiality | ✅ | AES-256-GCM encrypted channel |
| Authentication | ✅ | Ed25519 signatures (LT identity keys) |
| Forward Secrecy | ✅ | X25519 ephemeral keys per session |
| Integrity | ✅ | AEAD tags — MITM tampering detected |

**Forward Secrecy**: even if an attacker steals the main private key later, older messages cannot be decrypted.

## How the Protocol Flow Works
1. Both clients register their identity + public key into the Registry.
2. Pilot requests Control’s public key from Registry.
3. Pilot sends signed *ClientHello* + ephemeral key.
4. Control verifies signature and sends signed *ServerHello* + ephemeral key.
5. Both compute shared secret using X25519.
6. HKDF derives AES-GCM session keys.
7. Secure encrypted messages exchange.
8. MITM tampering test is shown (AES-GCM detects the modification).

## Run the Demo
Just run the main Python / notebook file.

You will see logs like:

- Registry started
- Both users registered
- Mutual signature verification success
- Secure message exchange
- MITM attack simulation → integrity failure detected


## Result
This implementation fulfills the assignment requirements by demonstrating:

- discovery  
- mutual authentication  
- secure key agreement  
- encrypted messaging  
- integrity violation detection  

And all happens **without any pre-shared keys**.

