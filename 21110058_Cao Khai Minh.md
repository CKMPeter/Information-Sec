# LAB 2:
by Cao Khải Minh

## task 1:
**Question 1**: 
Conduct transfering a single plaintext file between 2 computers, 
Using openssl to implementing measures manually to ensure file integerity and authenticity at sending side, 
then veryfing at receiving side.

using: `ifconfig` on the ubuntu virtual machine to get the ip:
![image](https://github.com/user-attachments/assets/4e3072e3-6430-4aee-9e02-5a14e2ef8cb2)</br>

1. Prepare a text file, in this case i call it file.text: </br>
![image](https://github.com/user-attachments/assets/d6fa50be-19e0-4471-baa1-ad856e19c5fe)</br>
2. Generate a SHA-256 hash of the file:
using the cmd: ```openssl dgst -sha256 -binary file.txt > file.txt.hash```
![image](https://github.com/user-attachments/assets/a3f5f902-d4cc-42f3-8a55-725cf7f6e8f4)</br>
the content of the file.hash:
![image](https://github.com/user-attachments/assets/8835eb0b-b54e-4b44-b325-430e09f806f8)</br>
3. Create an Private and Public Key:
  3.1 Generate a private key:
using the cmd: ```openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048```
![image](https://github.com/user-attachments/assets/702b95d7-0d0a-451d-8fa2-26fd693df22c)</br>
  3.2 Extract the public key:
using the cmd: ```openssl rsa -pubout -in private_key.pem -out public_key.pem```
![image](https://github.com/user-attachments/assets/ebf21530-4152-4069-a9ba-456462c363fa)</br>
4. Sign the file:
using the cmd: `openssl dgst -sha256 -sign private_key.pem -out file.signature file.txt`
![image](https://github.com/user-attachments/assets/c65813d8-3da9-4725-8a20-521973ad61e3)</br>
5. Via "scp" send the file to the linux client:
using the cmd: `scp file.txt file.signature public_key.pem user@vm_ip:/destination/path`
with: `user` = client user (in this case is minh7903) and `vm_ip`: 172.18.181.152.
![image](https://github.com/user-attachments/assets/14794f64-475b-45e6-a696-b9a1824703ad)</br>
6. Go to the ubuntu client to check if file is send correctly:
using the cmd: `openssl dgst -sha256 -verify public_key.pem -signature file.signature file.txt`
![image](https://github.com/user-attachments/assets/bd886d14-ed3e-49a5-a237-92d3823130c4)</br>
if `Verify OK` meaning it was successful, else openssl will indicate an error.





 
