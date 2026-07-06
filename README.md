# Web Security — Cryptography Session
### S4118 Unit I: "Cryptography and the Web" — 1 hour, interactive, implementation-based

This folder has everything you need:
- `crypto-demo.html` — open this in any browser (Chrome/Edge/Firefox). It's a self-contained live demo console, no install, no internet needed. Every button runs real cryptography using the browser's built-in Web Crypto API.
- `README.md` (this file) — your run-of-show script, timing, and talking points.

Test the HTML file once *before* class, on the actual laptop/projector you'll use, so you're not debugging live.

---

## Timing at a glance (60 min)

| # | Segment | Time | Type |
|---|---------|------|------|
| 1 | Hook — Caesar Cipher | 5 min | Interactive (board) |
| 2 | Symmetric vs Asymmetric — AES & RSA | 10 min | Live demo |
| 3 | Working Protocols — HTTPS/TLS | 15 min | Interactive + terminal demo |
| 4 | Password Hashing | 10 min | Interactive + live demo |
| 5 | Digital Signatures / Digital ID | 10 min | Live demo |
| 6 | Legal Restrictions on Cryptography | 5 min | Discussion |
| 7 | Wrap-up — Class Exchange | 5 min | Interactive closer |

Syllabus keywords covered: **Cryptography and Web Security · Working Cryptographic Systems and Protocols · Legal Restrictions on Cryptography · Digital Identification.**

---

## 1. Hook — Caesar Cipher (5 min)

- Call a volunteer to the board. Give them a shift of `+3` and the word `HELLO WORLD`. Have them encode it by hand.
- Give the encoded word to a second volunteer with no key. Watch them struggle or brute-force it.
- Open `crypto-demo.html` → **Section 1**. Type the same message, hit **Brute-force all 26 shifts** — show that a "secret" cipher like this is broken instantly by a computer.
- Line to say: *"This is why real cryptography needs math that can't be brute-forced in a fraction of a second."*

## 2. Symmetric vs Asymmetric Cryptography (10 min)

- Explain with two analogies:
  - **Symmetric (AES):** one key locks and unlocks the same box. Fast. Problem: both people need the same key — how do you send the key safely in the first place?
  - **Asymmetric (RSA):** a mailbox with a public slot anyone can drop mail into, but only your private key opens it. Solves the key-sharing problem.
- Open **Section 2 (AES)**: click *Generate key* → *Encrypt* → *Decrypt*. Show the same key does both.
- Open **Section 3 (RSA)**: click *Generate keypair* → *Encrypt with PUBLIC key* → *Decrypt with PRIVATE key*. Point out the public key was shown on screen but the private key decrypts fine — because it's a different key.
- Line to say: *"HTTPS actually uses both together — asymmetric to safely exchange a key, then symmetric because it's much faster for the actual data."*

## 3. Working Cryptographic Systems & Protocols — HTTPS/TLS (15 min)

**Interactive part:**
- Have everyone open any HTTPS site on their phone/laptop and click the padlock icon next to the URL.
- Ask: *"What's the name on the certificate? Who issued it?"* — let a few classmates shout out answers (Let's Encrypt, DigiCert, GTS, etc.)

**Live terminal demo (real implementation, not slides):**
```
openssl s_client -connect google.com:443
```
This prints the actual live certificate chain and the real cipher suite in use. If `openssl` isn't available on the demo laptop, `curl -v https://google.com` also shows the TLS handshake steps in the verbose output.

**Explain the handshake in plain steps:**
1. Client says hello, lists what ciphers it supports.
2. Server replies with its certificate (its "ID card") and picks a cipher.
3. Client and server use asymmetric crypto (RSA/ECDHE) to safely agree on a temporary symmetric session key.
4. From here on, everything is encrypted with the fast symmetric key (AES) — this is the padlock you see.

## 4. Password Hashing (10 min)

- Ask 2–3 classmates to shout out a **fake** password (not a real one!).
- Type each into `crypto-demo.html` → **Section 4**, click **Hash it**.
- Click **Hash the SAME password again** — show the hash is completely different each time.
- Explain: that's the **salt** — a random value added before hashing, so two people with the same password don't get the same hash, and pre-computed "rainbow table" attacks don't work.
- Key point: *"This is not encryption. You can't reverse a hash back into the password. That's the whole point — even if the database leaks, the real passwords are never exposed."*

## 5. Digital Signatures / Digital Identification (10 min)

- Open **Section 5**. Click **Sign with PRIVATE key**, then **Verify (unmodified)** — shows valid ✔.
- Click **Tamper 1 character, then verify** — shows invalid ✘ instantly.
- Tie it to real life: *"This is exactly how software updates, app stores, and even UPI/bank transactions verify that a message wasn't altered and really came from who it claims."*

## 6. Legal Restrictions on Cryptography (5 min — pure discussion, no demo needed)

Talking points to raise, then open the floor for 2 minutes of debate:
- Historically, the US classified strong encryption as a munition and restricted its export (loosened significantly in the late 1990s).
- Some countries today restrict or require licensing for strong encryption / VPN use.
- The ongoing "backdoor" debate: law enforcement wants guaranteed access to encrypted apps (e.g. the Apple vs FBI case); security experts argue any backdoor weakens security for everyone, not just criminals.
- **Discussion prompt for the class:** *"Should governments be allowed to force a backdoor into apps like WhatsApp?"*

## 7. Wrap-up — Class Exchange (5 min)

- Pick two classmates, A and B.
- A clicks **Generate keypair** in Section 6 and reads their public key out loud (or just says "it's public, anyone can have it").
- B types a message in the box and clicks **Encrypt with A's public key**.
- Project the ciphertext on screen — ask the class: *"Can anyone here read this?"*
- A clicks **Decrypt with private key** — message appears. End on that "wow, it actually works" beat.

---

## Quick answers for likely questions

- **"Is AES or RSA 'better'?"** Different jobs. AES is fast and used for bulk data; RSA is slower but solves key exchange and identity. Real systems use both together.
- **"Can hashes be cracked?"** Not reversed, but weak/common passwords can be guessed by hashing common passwords and comparing — which is exactly why salting and using slow hash functions (bcrypt, Argon2) matters. The demo uses SHA-256 for simplicity; production systems use slower, purpose-built password hashes.
- **"What if someone steals the private key?"** Then the whole scheme breaks — this is why private keys are never transmitted, and why key management (HSMs, secure enclaves) is its own huge field in real-world security.

---

*Built as a self-contained implementation demo for a 1-hour Web Security session — no installs, no internet dependency, runs entirely client-side in the browser.*
