## Overview
- Using TLS/SSL is one of the most common way to add a security layer into the application
- SSL is an old protocol used in the security in the 7-layer network architecture, TLS is the new one with better security feature. (essentially whenever we refer to TLS/SSL it basically just means security layer)

## Public-private Key pair
- TLS used **public-private key pair** for client-server authentication
	- Client-server authentication is to prevent man-in-the-middle attach, which means having a malicious person sitting in the middle of the request and read/modify all the communication between client and server.
	- We need a way for the server to verify itself, guarantee that it is what it says it is => The solution is **public-private key pair**. 
	- This solution just means that the server will have a pair of private key and public key, of which the private key is hidden by the server and the public key will be distributed freely. When a client and a server form a connection (via TLS handshake), this key pair will be used. The server will encrypt the data using its hidden private key, and the client will decrypt using the public key and encrypt using the public key and send back. The data can only be decrypt using the correct key pair. 
	- **NOTE**: This is only to verify the server, so if we really think about it, if we have the public key, which is easy to get cause it is public, there is no security happening here => That's why we need **mutual authentication** or **two-way authentication**, which is both the client and server verifying each other.
- We need a way to securely distribute public key and sign the key cause since anyone can generate a key pair then how do we know which one is the correct one to verify.
=> **Certificate Authority (CA)**
## Certificate Authority (CA)
- **CA** can be understand as a Department of Homeland Security: people go in, fill out a form and submit necessary document to verify themself, and if everything is valid, DHS will provide them with a passport/id so that they can bring everywhere that require identity verification. The conversation will be like this: "This is my id to verify everything that I said about myself is true => This id looks legit, DHS probably did issue this and already trust this person so I probably should too."
	- With that being said, we can understand **CA** as: someone will submit a form to an organization that issues **CA**, the form will include many information and also a public key that will be distributed. After that, the organization will issue us a **certificate** and a **signed key pair**.
	- Next time whenever anyone wants to connect to our server, it will ask for a **certificate**, after that it will use the public key to communicate with the server (check the below picture)
![](https://i.imgur.com/DIB5vas.png)
### Mutual Authentication
- To achieve mutual authentication, we can just the client also register with the CA so both client and server will be trusted by CA.