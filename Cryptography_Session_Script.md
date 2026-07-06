# Web Security — Cryptography Session Script
### S4118 Unit I: "Cryptography and the Web" (1 Hour)

**Instructor/Presenter Note:** This is your detailed, minute-by-minute script. It includes exactly what to say, what to show on screen, and how to get your peers interacting.

---

## 0:00 - 0:05 | Segment 1: The Hook (Caesar Cipher Race)
**Setup:** Have `crypto-demo.html` open.
**Action:** Call two volunteers up to the front. Give Volunteer A a piece of paper with `HELLO WORLD` and a pen. Give Volunteer B nothing.

**Script:**
> "Welcome to Cryptography and the Web. We're going to start with a race. [Volunteer A], I want you to encrypt the phrase 'HELLO WORLD' by shifting every letter forward by 3. A becomes D, B becomes E, and so on. Ready? Go."

*(Let them struggle for about 15 seconds)*

> "While they are doing that, let's see how long it takes an attacker to break this exact encryption if they don't know the key."

**Action:** Type `KHOOR ZRUOG` into Section 1 of your demo. Click **Brute-force all 26 shifts**.
**Script:**
> "Done. A computer cracks this instantly. This is why real web security needs mathematical algorithms that can't be brute-forced in a fraction of a second. Today, we are looking at how these algorithms are actually implemented on the web."

---

## 0:05 - 0:15 | Segment 2: Symmetric vs Asymmetric (Live Demo)
**Concept:** Explain the difference between AES and RSA.

**Script:**
> "There are two main types of modern encryption. Symmetric is like a padlock with one physical key. It's fast, but if I want to send you a secure message, how do I get the key to you safely? Asymmetric encryption solves this. It gives you a public slot where anyone can drop a message, but only your private key can open it."

**Action (Interactive):** Open Section 2 (AES).
> "Here is symmetric encryption in action. We generate one key. We encrypt, we decrypt."

**Action:** Open Section 3 (RSA). Generate a keypair.
> "Now, look at asymmetric. I'm putting the Public Key on the screen for everyone to see. If you take a picture of this key, can you read my messages? No. You can only write them. The private key stays hidden."

---

## 0:15 - 0:25 | Segment 3: Implementation Deep Dive (The Lecturer's Request)
**Setup:** Open the source code of `crypto-demo.html` in your code editor (like VS Code) and project it.

**Script:**
> "Our lecturer emphasized understanding the implementation. We aren't just using external tools here; this is all happening natively in the browser using JavaScript and the Web Crypto API. No external libraries."

**Action:** Highlight the JavaScript code where `window.crypto.subtle.encrypt()` is called.
**Script:**
> "Look at this JavaScript snippet. When you build a web app, you don't write your own cryptography math—that's dangerous. You call `window.crypto.subtle`. It's built into every modern browser. We pass it an algorithm, like AES-GCM, the key, and an initialization vector. The browser handles the heavy math at the system level. If you are building web tools or games, this API is how you securely handle user data on the client side without needing a backend server."

---

## 0:25 - 0:40 | Segment 4: Working Protocols — HTTPS/TLS & Terminal
**Action:** Ask everyone to open any website on their phone and click the padlock icon.

**Script:**
> "Let's look at a working protocol: HTTPS. Who can tell me what certificate authority issued the certificate for the site you are looking at right now?" *(Field a few answers from peers)*

**Action:** Open your terminal and run `openssl s_client -connect google.com:443`.
**Script:**
> "When you visit a site, a handshake happens. Let's look at a real one in the terminal.
> 1. Your browser says hello.
> 2. The server sends its ID card (the certificate).
> 3. They use asymmetric crypto (RSA) to safely pass a secret session key.
> 4. Once they have it, they switch to symmetric crypto (AES) because it's much faster for streaming video or loading heavy web pages."

---

## 0:40 - 0:50 | Segment 5: Password Hashing & Digital Signatures
**Action:** Ask for a fake password from the audience.
**Script:**
> "Give me a terrible password. Don't use your real one." *(e.g., 'password123')*

**Action:** Hash it in Section 4. Then hash the SAME password again.
**Script:**
> "Notice how the output changed even though the password didn't? That's called a 'salt'. It's a random string added before hashing. If you're building a backend system with Firebase or a custom database, this is why two users with the same password won't have the same hash. It stops attackers from pre-computing dictionary attacks."

**Action:** Move to Section 5 (Signatures). Sign a message, then change one letter and verify.
**Script:**
> "This concept extends to digital signatures. When you download a software update or a game patch, your system hashes the file and checks the developer's signature. If even one byte is tampered with, the verification fails instantly, as you see here."

---

## 0:50 - 0:55 | Segment 6: Legal Restrictions (Class Debate)
**Script:**
> "Cryptography isn't just code; it's heavily regulated. In the 90s, the US classified strong encryption as a weapon. Today, the debate is about backdoors. Law enforcement argues they need access to encrypted apps to catch criminals. Security engineers argue that a backdoor for the 'good guys' is inevitably found by the 'bad guys'."

**Interactive Prompt:**
> "By a show of hands, who thinks messaging apps should be forced to have a backdoor for government investigations? Who thinks encryption should be mathematically unbreakable, no matter what?"
*(Let 2-3 students briefly voice their reasoning)*

---

## 0:55 - 1:00 | Segment 7: Wrap-up & Peer Exchange
**Action:** Pick two peers sitting on opposite sides of the room.
**Script:**
> "To close, let's do a live exchange. [Peer 1], generate a keypair in Section 6 and read the first 10 characters of your public key out loud. [Peer 2], type a message, encrypt it with [Peer 1's] key."

**Action:** Project the resulting ciphertext on the screen.
**Script:**
> "Can anyone read this? No. [Peer 1], use your private key to decrypt it. And there's the message. This math secures the entire web, from e-commerce checkouts to online gaming, and you can implement it directly in your browser. Thank you!"
