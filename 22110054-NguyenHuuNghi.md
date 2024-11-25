# Lab #2,22110054, Nguyen Huu Nghi, INSE330380E_01FIE
# Task 1: Transfer files between computers  
**Question 1**: 
Conduct transfering a single plaintext file between 2 computers, 
Using openssl to implementing measures manually to ensure file integerity and authenticity at sending side, 
then veryfing at receiving side. 

**Answer 1**: <br>
**At sending site** <br>
- Create the plaintext file
```
echo "This is the content of the file" > plaintext.txt
```
<br>
- Next we Generate a cryptographic hash for integrity by this command

```
openssl dgst -sha256 -out plaintext.txt.sha256 plaintext.txt

```
<br>
- Generate a private key for signing by this command

```
Generate a private key for signing
```
<br>
- Sign the hash for authenticity:

```
openssl dgst -sha256 -sign private_key.pem -out plaintext.txt.sig plaintext.txt.sha256
```
<br>
- Share the public key: Extract the public key from the private key to share with the receiver:

```
openssl rsa -in private_key.pem -pubout -out public_key.pem

```

Transfer files to the receiving side:
```
scp plaintext.txt plaintext.sig public_key.pem admin@10.0.2.15:/home
```
![image](https://github.com/user-attachments/assets/ab1fd74d-e080-4216-b973-89af3109678e)

**At receiving site** <br>
Verify the integrity of the file by this command:
```
openssl dgst -sha256 -out received_plaintext.txt.sha256 plaintext.txt

```
Compare the computed hash (received_plaintext.txt.sha256) with the signed hash using the public key

```
openssl dgst -sha256 -verify public_key.pem -signature plaintext.txt.sig plaintext.txt.sha256

```
Success
![image](https://github.com/user-attachments/assets/4ba321c7-4dc7-4b29-b82b-ddfce9d130ee)



# Task 2: Transfering encrypted file and decrypt it with hybrid encryption. 
**Question 1**:
Conduct transfering a file (deliberately choosen by you) between 2 computers. 
The file is symmetrically encrypted/decrypted by exchanging secret key which is encrypted using RSA. 
All steps are made manually with openssl at the terminal of each computer.

**Answer 1**:
**At sending site** <br>
Generate the RSA Key Pair
```
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
```
Extract the public key:
```
openssl rsa -in private_key.pem -pubout -out public_key.pem
```
Create a plaintext file to transfer
```
echo "This is a confidential message." > file.txt
```
Generate a random symmetric key
```
openssl rand -base64 32 > symmetric_key.bin
```
Encrypt the file with the symmetric key: Use AES-256-CBC for symmetric encryption:
```
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc -pass file:symmetric_key.bin
```
Encrypt the symmetric key with the RSA public key: Encrypt the symmetric key using the receiver's public key
```
openssl rsautl -encrypt -inkey public_key.pem -pubin -in symmetric_key.bin -out symmetric_key.enc
```
Transfer the encrypted file and encrypted symmetric key: Send the following files to the receiver
```
scp file.enc symmetric_key.enc admin@10.0.2.15:/home
```
![image](https://github.com/user-attachments/assets/76054d7d-8dce-45a6-a05a-9bc3f8183273)

**At receiving site** <br>
Decrypt the symmetric key with the RSA private key: Use the receiver's private key to decrypt the symmetric key
```
openssl rsautl -decrypt -inkey private_key.pem -in symmetric_key.enc -out symmetric_key.bin

```
Decrypt the file with the symmetric key
```
openssl enc -d -aes-256-cbc -in file.enc -out file_decrypted.txt -pass file:symmetric_key.bin

```
Verify the decrypted file
```
cat file_decrypted.txt

```


# Task 3: Firewall configuration
**Question 1**:
From VMs of previous tasks, install iptables and configure one of the 2 VMs as a web and ssh server. Demonstrate your ability to block/unblock http, icmp, ssh requests from the other host.

**Answer 1**:

To block HTTP, use this command
```
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```
![image](https://github.com/user-attachments/assets/6cf086dc-2b3e-4a5c-b3a2-65e4b81641cd)
<br>
To block ICMP:
```
sudo iptables -A INPUT -p icmp -j DROP
```
To block SSH:
```
sudo iptables -A INPUT -p tcp --dport 22 -j DROP

```
![image](https://github.com/user-attachments/assets/0159adb0-6791-4a1a-8104-9525868e13db)

