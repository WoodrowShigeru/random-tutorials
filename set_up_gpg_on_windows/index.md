
  GPG Encryption Tutorial (for Windows)
=========================================

[for_ubuntu]: https://github.com/WoodrowShigeru/random-tutorials/blob/master/set_up_gpg_on_ubuntu.md
[wiki_pgp]: https://en.wikipedia.org/wiki/Pretty_Good_Privacy#OpenPGP
[wiki_gpg]: https://en.wikipedia.org/wiki/GNU_Privacy_Guard
[gpg_official]: https://www.gpg4win.org/


- [What is it?](#user-content-what-is-it)
- [Installation](#user-content-installation)
- [Import someone's public key](#user-content-import-someones-public-key)
- [Encrypt a file](#user-content-encrypt-a-file)
- [Decrypt a file](#user-content-decrypt-a-file)
- [See also](#user-content-see-also)


------

This tutorial covers the usage of GPG.

- *PGP* is an encryption software released in 1991.
- *GPG* is an encryption software by someone else released in 1999.
- *OpenPGP* is an encryption method.
- [Wikipedia: Pretty Good Privacy][wiki_pgp]
- [Wikipedia: GNU Privacy Guard][wiki_gpg]



What is it?
-----------
OpenPGP is a file encryption method used for converting any file (txt, jpg, …) into a gpg file, which can then be sent to colleagues via mail.

gpg files are good for transferring sensitive data because that way only the sender and reader can open the files even if a third party intercepts them.

It works like this: I create a key pair (public and private, with a passphrase) and you create a key pair. Then we exchange the public keys and keep the private keys to ourselves.

Now I encrypt a file and say during the encryption that only you and I are allowed to open that resulting gpg file. Even if another person gets that encrypted file *and* your public key – they don't have your private key or passphrase which are needed as well.

Think of it like a treasure chest with one lock for each allowed person – except the locks are OR-connected instead of AND-connected.



Installation
------------
- [Download][gpg_official] and execute the installer.
- Select "Donate no money" if you so wish.
- During that installation (there is a checkbox) also install Kleopatra which provides a nice GUI.

- Start Kleopatra and create a key pair.

  Provide a name (and/or optionally email address) so that the people you're communicating with can organize their many contacts' public keys. Choose a passphrase for this pair and store it securely.

- For good measure, also backup the keys as files, as suggested by the program. The button "Make backup" exports the private key, and `[ File › Export ]` exports the public key. When you look at them in a text editor, only one of them says "Begin public key block".

- Never give away the private key!

- But do give away the public key to whoever you want to communicate with. That's why it's handy to have them already exported as files.



Import someone's public key
---------------------------
- In Kleopatra, select "Import" and select a given asc file (usually asc).
- Ask for and compare the fingerprint, if you feel especially insecure.
- Once finished importing, the recipient becomes available for encrypting files.



Encrypt a file
--------------
- Right-click on the file you wish to encrypt, select "Sign and verify" and then choose a contact from the list managed in Kleopatra.
- Possibly the passphrase needs to be entered as well.
- Send that gpg file, i.e. as email attachment. It can even be sent with the public key in the same mail.



Decrypt a file
--------------
- In Windows it is as comfortable as double-clicking the gpg file. The Kleopatra popup might appear somewhere in the background.
- Select a directory to save the content and decrypt.
- Possibly the passphrase needs to be entered as well.



See also
--------
- [Tutorial for Ubuntu][for_ubuntu]

