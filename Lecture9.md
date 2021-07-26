# Lecture 9: Security and Cryptography

1. **Entropy.**
    1. Suppose a password is chosen as a concatenation of four lower-case
       dictionary words, where each word is selected uniformly at random from a
       dictionary of size 100,000. An example of such a password is
       `correcthorsebatterystaple`. How many bits of entropy does this have?
       #### **Solution:**
       Since there are 100,000 or 10<sup>5</sup> choices for each of the four words, total # of possibilities = (10<sup>5</sup>)<sup>4</sup>.  Therefore, entropy = log<sub>2</sub>(10<sup>20</sup>) ~ 66.4 bits.
    
    2. Consider an alternative scheme where a password is chosen as a sequence
       of 8 random alphanumeric characters (including both lower-case and
       upper-case letters). An example is `rg8Ql34g`. How many bits of entropy
       does this have?
       #### **Solution:**
       Since there are 62 choices for each of the 8 characters, total # of possibilities = 62<sup>8</sup>. Therefore, entropy = log<sub>2</sub>(62<sup>8</sup>) ~ 47.6 bits.

    3. Which is the stronger password?
        #### **Solution:**
        The first password is stronger as it has higher entropy.

    4. Suppose an attacker can try guessing 10,000 passwords per second. On
       average, how long will it take to break each of the passwords?
       #### **Solution:**
       Assuming a Geometric Distribution, the expected number of guesses for a password would be equal to its total # of possibilities. Total # of guesses in a year = 10<sup>4</sup> * 3600 * 24 * 365 = 3.1536 * 10<sup>11</sup>. So, average time to break the first password = (10<sup>20</sup> / 3.1536 * 10<sup>11</sup>) ~ 3.17 * 10<sup>8</sup> years and for the second password = (62<sup>8</sup> / 3.1536 * 10<sup>11</sup>) ~ 692.35 years.


2. **Cryptographic hash functions.** Download a Debian image from a
   [mirror](https://www.debian.org/CD/http-ftp/) (e.g. [from this Argentinean
   mirror](http://debian.xfree.com.ar/debian-cd/current/amd64/iso-cd/)).
   Cross-check the hash (e.g. using the `sha256sum` command) with the hash
   retrieved from the official Debian site (e.g. [this
   file](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/SHA256SUMS)
   hosted at `debian.org`, if you've downloaded the linked file from the
   Argentinean mirror).
   #### **Solution:**
   Download one of the iso files from the mirror(I chose `debian-10.10.0-amd64-netinst.iso`) and the hash file using `curl`:
   ```bash
   curl -O -L debian.xfree.com.ar/debian-cd/current/amd64/iso-cd/debian-10.10.0-amd64-netinst.iso
   curl -O -L https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/SHA256SUMS
   ```
   `-O` flag saves the output under the filename indicated by the URL and `-L` follows location redirects.

   Cross-check the hash using `sha256sum --check`:
   ```bash
   sed '/debian-10.10.0-amd64-netinst.iso/!d' SHA256SUMS | sha256sum --check
   ```
   `sed` is used to filter the other hashes stored in the file using `!d` and `sha256sum --check` reads a file of SHA256 sums and verifies all files have matching checksums.

3. **Symmetric cryptography.** Encrypt a file with AES encryption, using
   [OpenSSL](https://www.openssl.org/): `openssl aes-256-cbc -salt -in {input
   filename} -out {output filename}`. Look at the contents using `cat` or
   `hexdump`. Decrypt it with `openssl aes-256-cbc -d -in {input filename} -out
   {output filename}` and confirm that the contents match the original using
   `cmp`.
   #### **Solution:**
   Execute the commands on any file.
   

4. **Asymmetric cryptography.**
    1. Set up [SSH
       keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
       on a computer you have access to (not Athena, because Kerberos interacts
       weirdly with SSH keys). Rather than using RSA keys as in the linked
       tutorial, use more secure [ED25519
       keys](https://wiki.archlinux.org/index.php/SSH_keys#Ed25519). Make sure
       your private key is encrypted with a passphrase, so it is protected at
       rest.
       #### **Solution:**
       The tutorial was probably updated after the lecture notes were written, since it mentions ED25519 keys. Follow everything before Step-4 in the tutorial.
       
    2. [Set up GPG](https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages)
       #### **Solution:**
       Follow everything under "Set Up GPG Keys" section in the tutorial.

    3. Send Anish an encrypted email ([public key](https://keybase.io/anish)).
       #### **Solution:**
       Install `curl`(`sudo apt install curl`) if you don't have it.
       This will import Anish's public key:
       ```bash
       curl https://keybase.io/anish/pgp_keys.asc | gpg --import
       ```
       `gpg --list-keys` should display his key along with your own.
       Save your message in a file then run this:
       ```bash
       gpg --encrypt --sign --armor -r person@email.com name_of_file
       ```
       Replace `person@email.com` with any of Anish's emails from his public key and `name_of_file` with the file where you stored the message.

    4. Sign a Git commit with `git commit -S` or create a signed Git tag with
       `git tag -s`. Verify the signature on the commit with `git show --show-signature` or on the tag with `git tag -v`.
       #### **Solution:**
       Execute the commands on any git repository while making a commit. `git show --show-signature` should display an output similar to this if your signed the commit with `git commit -S`:
       ```bash
       gpg: Signature made Monday 26 July 2021 07:20:51 PM IST
       gpg:                using RSA key E9B81683E9CA467B19B32A93CDBDE4073CCE941E
       gpg:                issuer "saiabcxyz9@gmail.com"
       gpg: Good signature from "Sai Panda <saiabcxyz9@gmail.com>" [ultimate]
       ```