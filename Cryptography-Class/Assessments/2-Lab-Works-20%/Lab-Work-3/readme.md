
# Labwork - 3 : Hands-on Exploration of Cryptographic Tools Hashing, Encryption, and Digital Signatures

- Student: Afa
- Tools: OpenSSL
- Platform: Kali Linux

> SELF NOTE : Don't put password together on openssl used some command with it, try looking it up, also ensure you use -base64, also using nosalt results in different stuff generated too


## Task 1: Symmetric Encryption and Decryption using AES-256-CBC

Tools used :
- OpenSSL

1. Generate a Strong Random Key and IV:

```sh
- openssl rand -hex 32 > key_haziq.bin 
- openssl rand -hex 16 > iv_haziq.bin
```
![alt text](Screenshot/generateAESkey.png)

2. Create a message file:

```sh
echo "Partner, this is a top secret message from Haziq!" > haziq_message.txt
```
![alt text](Screenshot/CreateMessageFile.png)

3. Encrypt using AES-256-CBC:

```sh
openssl enc -aes-256-cbc -in haziq_message.txt -out encrypted_message.bin -K $(cat key.bin) -iv $(cat iv.bin)
```
![alt text](Screenshot/EncryptingAES.png)

4. Zip the file to easily send it
```sh
zip task1-aes.zip encrypted_message.enc key_haziq.bin iv_haziq.bin
```
![alt text](Screenshot/ZIPfile.png)

5. Send the file and key via gmail:

![alt text](Screenshot/Sending.png)

5. Decrypt the file:

```sh
openssl enc -d -aes-256-cbc -in encrypted_message.bin -out decrypted_message.txt -K $(cat key_haziq.bin) -iv $(cat iv_haziq.bin)
```


6. Compare:

```sh
diff haziq_message.txt decrypted_message.txt
```
OR
```sh
cat haziq_message.txt
cat decrypted_message.txt
```

Analysis:

- The original and decrypted files are identical.
- CBC mode requires the same key and IV. Reusing IVs can lead to leakage.

## Task 2: Asymmetric Encryption and Decryption using RSA

Tools used:
- openSSL

1. Have the partner Generate an RSA Key Pair:

```sh 
- openssl genpkey -algorithm RSA -out afa_private.pem -pkeyopt rsa_keygen_bits:2048
- openssl rsa -pubout -in afa_private.pem -out afa_public.pem
```
![alt text](Screenshot/AfaPublics.jpg)

1. Get the partner's public key via gmail. 

![alt text](Screenshot/Gmail.png)

once you've downloaded the key, it'll be in download directory, so you'll have to move it to the folder you're doing work in 

```sh
cd Downloads
mv afa_public.pem /home/haziq
```

3. Create Secret Message

```sh
echo "Top secret: Hello from Haziq!" > rahsia.txt
```
![alt text](Screenshot/JomMamak.png)

4. Encrypt the Message using the partner's public key

```sh
openssl pkeyutl -encrypt -inkey afa_public.pem -pubin -in rahsia.txt -out rahsia_encrypted.bin
```
![alt text](Screenshot/EncWithPartner.png)

5. Partner uses their private key to Decrypt the Message 

```sh
openssl pkeyutl -decrypt -inkey afa_private.pem -in rahsia_encrypted.bin -out decrypted_rahsia.txt
```
![alt text](Screenshot/PartnerDecry.jpg)

6. Compare:

```sh
diff rahsia.txt decrypted_rahsia.txt
```
OR
```sh
cat rahsia.txt decrypted_rahsia.txt
```
![alt text](Screenshot/JomMamakDecry.jpg)

Analysis:

- RSA is slow for large files; only used to encrypt small data (e.g., symmetric keys).
- 2048 bits ensures strong encryption.

## Task 3: Hashing and Integrity using SHA-256

Tools used :
- openssl dgst -sha256

1. Key generation
Generate an RSA private key

```sh
openssl genpkey -algorithm RSA -out haziq_private.pem -pkeyopt rsa_keygen_bits:2048
```
and extract a public key from it
```sh
openssl rsa -pubout -in haziq_private.pem -out haziq_public.pem
```

![alt text](Screenshot/RSAkeyGeneration.png)

2. Create a message file:

```sh
echo "This message is digitally signed by Haziq" > message_haziq.txt
```
![alt text](Screenshot/NewMessage.png)

3. Generate Hash:

```sh
openssl dgst -sha256 -binary -out message_haziq.sha256 haziq_message.txt
```
![alt text](Screenshot/HashingGen.png)

signing with private key
```sh
openssl rsautl -sign -inkey haziq_private.pem -in haziq_message.sha256 -out haziq_signature.bin
```
![alt text](Screenshot/problemOccur.png)

This problem occur due to the reason that the command rsautl is an old and depreciated command because of lack of modern padding support. It is also has a poor flexibility

so now we have to use this command to sign it:

```sh
openssl pkeyutl -sign -inkey haziq_private.pem -in message_haziq.sha256 -out haziq_signature.bin
```
![alt text](Screenshot/Signing.png)

3. Modify file:

```sh
echo "!" > haziq_message.txt
```
![alt text](Screenshot/modify.png)

4. Generate hash again:

```sh
openssl dgst -sha256 -binary -out modified_message_haziq.sha256 haziq_message.txt
```
![alt text](Screenshot/GenerateNewHash.png)

5. Compare the hashes:

```sh
diff original_hash.txt modified_hash.txt
```
or
```sh
cat message_haziq.sha256
cat modified_message_haziq.sha256
```
![alt text](Screenshot/CompareingHash.png)

Analysis:

- Even a tiny change causes a completely different hash (avalanche effect).
- SHA-256 ensures integrity but not authenticity.


## Task 4:  Digital Signatures with RSA

Tools used: 
- OpenssL 
- Keys from Task 2


1. Create a File to Sign:

```sh
echo "This is a contract between Haziq and Afa." > agreement.txt
```
![alt text](Screenshot/FileToSign.png)

2. Create Digital Signature :

```sh
openssl dgst -sha256 -sign haziq_private.pem -out agreement.sig agreement.txt
```

![alt text](Screenshot/DigiSig.png)

3. have the partner verify the Signature:

```sh
openssl dgst -sha256 -verify haziq_public.pem -signature agreement.sig agreement.txt
```
![alt text](Screenshot/Verification.jpg)


4. Simulate a Modification and Verify Again:

```sh
echo "Tampered content" >> agreement.txt
openssl dgst -sha256 -verify haziq_public.pem -signature agreement.sig agreement.txt
```
![alt text](Screenshot/Failure.jpg)

Analysis:

- Digital signatures verify both integrity and authenticity.
- Any change to the file invalidates the signature.
