# LAB 2:
by Cao Khải Minh

# Task 1: Transfer files between computers  
**Question 1**: 
Conduct transfering a single plaintext file between 2 computers, 
Using openssl to implementing measures manually to ensure file integerity and authenticity at sending side, 
then veryfing at receiving side. 

**Answer 1**:
using: `ifconfig` on the ubuntu virtual machine to get the ip:
![image](https://github.com/user-attachments/assets/4e3072e3-6430-4aee-9e02-5a14e2ef8cb2)</br>

1. Prepare a text file, in this case i call it file.text: </br>
![image](https://github.com/user-attachments/assets/d6fa50be-19e0-4471-baa1-ad856e19c5fe)</br>
2. Generate a SHA-256 hash of the file:
using the cmd: ```openssl dgst -sha256 -binary file.txt > file.txt.hash```
![image](https://github.com/user-attachments/assets/a3f5f902-d4cc-42f3-8a55-725cf7f6e8f4)</br>
the content of the file.hash: </br>
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
6.Verify the file's integrity using the provided signature and public key:
using the cmd: `openssl dgst -sha256 -verify public_key.pem -signature file.signature file.txt`
![image](https://github.com/user-attachments/assets/bd886d14-ed3e-49a5-a237-92d3823130c4)</br>
if `Verify OK` meaning it was successful, else openssl will indicate an error.

# Task 2: Transfering encrypted file and decrypt it with hybrid encryption. 
**Question 1**:
Conduct transfering a file (deliberately choosen by you) between 2 computers. 
The file is symmetrically encrypted/decrypted by exchanging secret key which is encrypted using RSA. 
All steps are made manually with openssl at the terminal of each computer.

**Answer 1**:
Using the `file.txt`, `private_key.pem` & `public_key.pem` from task 1.
1. Generating a Secret Key and Symmetric Encryption (AES):
  1.1 using cmd: `openssl rand -out secret.key 32` to generate a Random Secret Key for symmetric encryption (AES-256):
  the content of the `secret.key` file: </br>
  ![image](https://github.com/user-attachments/assets/dc75572d-a222-409b-af9c-f362763702cb)</br>
  1.2 Encrypt the File using AES-256-CBC with the generated secret key, by via cmd: </br>
  `openssl enc -aes-256-cbc -salt -in file.txt -out file.txt.enc -pass file:./secret.key -pbkdf2`
2. Encrypt the Secret Key with RSA:
Using cmd: `openssl pkeyutl -encrypt -inkey public_key.pem -pubin -in secret.key -out secret.key.enc`
3. Transferring the Encrypted File and Key:
Vi using cmd: `scp file.txt.enc secret.key.enc user@vm_ip:/path/to/destination`
![image](https://github.com/user-attachments/assets/c7493381-d554-4dfb-91d8-b7da09deba05)</br>
4. Decrypting on the Receiver Side:
![image](https://github.com/user-attachments/assets/713d7de3-1b1c-4d4a-a82b-0dcb13de8def) </br>
first, we decript the `secret.key.enc` into the  `secret.key` by using the `private_key.pem` which both machine should have.
To continue, we decript the `file.txt.enc` using the newly decripted `secret.key` on the ubuntu client.
5. Check the content of the file:
using `nano` to inter the file.txt to check if the the content is the same.
![image](https://github.com/user-attachments/assets/14299762-510f-4b8c-9bc1-562402ccffc9)</br>

#### Ubuntu's file.txt:</br>
![image](https://github.com/user-attachments/assets/ae67ca98-1440-42d9-a627-b3c95ac11bef)</br>

#### Original file.txt:</br>
![image](https://github.com/user-attachments/assets/a5093e4e-ee6e-43a8-a62d-a3a3caa1729e)</br>

# Task 3: Firewall configuration
**Question 1**:
From VMs of previous tasks, install iptables and configure one of the 2 VMs as a web and ssh server. Demonstrate your ability to block/unblock http, icmp, ssh requests on the other host.

1. Check for iptables default POLICY:
![image](https://github.com/user-attachments/assets/7a1c8e44-b866-48fa-a7e2-7f852317c165)</br>
All 3 POLICY is set as "ACCEPT" which mean all services should be available.
2. Check for Access on the Client (Window):
first use `ipconfig` to check for the client's ip:</br>
![image](https://github.com/user-attachments/assets/59e23c7a-920b-4bdb-a8e8-cb20e71c613e)</br>
second, on Ubuntu VM using `ifconfig`:</br>
![image](https://github.com/user-attachments/assets/369fad06-de8f-4f1c-be43-28899c0fe5b5)</br>
  2.1 Check for icmp services via the `ping` cmd:</br>
  ![image](https://github.com/user-attachments/assets/f11a0a88-a81f-4f32-b771-04abd7143567)</br>
  2.2 Check for http services:</br>
  
  
















 
