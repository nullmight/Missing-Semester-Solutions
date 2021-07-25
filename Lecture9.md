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



1. **Symmetric cryptography.** Encrypt a file with AES encryption, using
   [OpenSSL](https://www.openssl.org/): `openssl aes-256-cbc -salt -in {input
   filename} -out {output filename}`. Look at the contents using `cat` or
   `hexdump`. Decrypt it with `openssl aes-256-cbc -d -in {input filename} -out
   {output filename}` and confirm that the contents match the original using
   `cmp`.
1. **Asymmetric cryptography.**
    1. Set up [SSH
       keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
       on a computer you have access to (not Athena, because Kerberos interacts
       weirdly with SSH keys). Rather than using RSA keys as in the linked
       tutorial, use more secure [ED25519
       keys](https://wiki.archlinux.org/index.php/SSH_keys#Ed25519). Make sure
       your private key is encrypted with a passphrase, so it is protected at
       rest.
    1. [Set up GPG](https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages)
    1. Send Anish an encrypted email ([public key](https://keybase.io/anish)).
    1. Sign a Git commit with `git commit -S` or create a signed Git tag with
       `git tag -s`. Verify the signature on the commit with `git show