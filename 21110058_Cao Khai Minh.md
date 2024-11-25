# LAB 2:
by Cao Khải Minh

## task 1:
**Question 1**: 
Conduct transfering a single plaintext file between 2 computers, 
Using openssl to implementing measures manually to ensure file integerity and authenticity at sending side, 
then veryfing at receiving side.

1. Prepare a text file, in this case i call it file.text:
![image](https://github.com/user-attachments/assets/d6fa50be-19e0-4471-baa1-ad856e19c5fe)
2. Generate a SHA-256 hash of the file:
using the cmd: sh```openssl dgst -sha256 -binary file.txt > file.txt.hash```
![image](https://github.com/user-attachments/assets/a3f5f902-d4cc-42f3-8a55-725cf7f6e8f4)
the content of the file.hash:
![image](https://github.com/user-attachments/assets/8835eb0b-b54e-4b44-b325-430e09f806f8)
3.
  3.1 Generate a private key:
using the cmd: sh```openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048```
![image](https://github.com/user-attachments/assets/702b95d7-0d0a-451d-8fa2-26fd693df22c)
  3.2 Extract the public key:
using the cmd: sh```openssl rsa -pubout -in private_key.pem -out public_key.pem```
![image](https://github.com/user-attachments/assets/ebf21530-4152-4069-a9ba-456462c363fa)



 
