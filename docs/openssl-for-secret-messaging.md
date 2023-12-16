---
title: Sharing Encrypted Files Using OpenSSL
---

I was curious how I would accomplish this and found (this)[https://opensource.com/article/21/4/encryption-decryption-openssl] handy source.

Here are the relevant commands from the blog post

```bash
openssl genrsa -aes128 -out alice_private.pem 1024
ls -l alice_private.pem
openssl genrsa -aes128 -out bob_private.pem 1024
ls -l bob_private.pem
openssl rsa -in alice_private.pem -noout -text
openssl rsa -in alice_private.pem -pubout > alice_public.pem
ls -l *.pem
openssl rsa -in alice_public.pem -pubin -text -noout
openssl rsa -in bob_private.pem -pubout > bob_public.pem
ls -l *.pem
scp alice_public.pem bob@bob-machine-or-ip:/path/
scp bob_public.pem alice@alice-machine-or-ip:/path/
ls -l bob_public.pem
ls -l alice_public.pem
echo "vim or emacs ?" > top_secret.txt
cat top_secret.txt
openssl rsautl -encrypt -inkey bob_public.pem -pubin -in top_secret.txt -out top_secret.enc
ls -l top_secret.*
cat top_secret.txt
cat top_secret.enc
hexdump -C ./top_secret.enc
file top_secret.enc
rm -f top_secret.txt
scp top_secret.enc bob@bob-machine-or-ip:/path/
ls -l top_secret.enc
cat top_secret.enc
hexdump -C top_secret.enc
openssl rsautl -decrypt -inkey bob_private.pem -in top_secret.enc > top_secret.txt
ls -l top_secret.txt
cat top_secret.txt
echo "nano for life" > reply_secret.txt
cat reply_secret.txt
openssl rsautl -encrypt -inkey alice_public.pem -pubin -in reply_secret.txt -out reply_secret.enc
ls -l reply_secret.enc
cat reply_secret.enc
hexdump -C ./reply_secret.enc
scp reply_secret.enc alice@alice-machine-or-ip:/path/
ls -l reply_secret.enc
cat reply_secret.enc
openssl rsautl -decrypt -inkey alice_private.pem -in reply_secret.enc > reply_secret.txt
ls -l reply_secret.txt
cat reply_secret.txt
```
