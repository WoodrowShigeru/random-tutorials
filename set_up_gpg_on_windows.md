
  GPG Encryption Tutorial (for Windows)
=========================================

{ work in progress }

- What is it?
- Installation
- Importing someone's public key
- Usage


This tutorial covers the usage of GPG.

- *PGP* is an encryption software released in 1991.
- *GPG* is an encryption software by someone else released in 1999.
- *OpenPGP* is an encryption method.
- https://en.wikipedia.org/wiki/Pretty_Good_Privacy#OpenPGP
- https://en.wikipedia.org/wiki/GNU_Privacy_Guard



What is it?
-----------
OpenPGP is a file encryption method used for converting any file (txt, jpg, …) into a gpg file, which can then be sent to colleagues via mail.

gpg files are good for transferring sensitive data because that way only the sender and reader can open the files even if a third party intercepts them.

It works like this: I create a key pair (public and private, with a passphrase) and you create a key pair. Then we exchange the public keys and keep the private keys to ourselves.

Now I encrypt a file and say during the encryption that only you and I are allowed to open that resulting gpg file. Even if another person gets that encrypted file *and* your public key – they don't have your private key or passphrase which are needed as well.

Think of it like a treasure chest with one lock for each allowed person – except the locks are OR-connected instead of AND-connected.



Installation
------------

- Navigate to https://www.gpg4win.org/
- Download and execute the installer.
- Select "Donate no money" if you so wish.
- During that installation (there is a checkbox) also install Kleopatra which provides a nice GUI.

- Start Kleopatra and create a key pair.

  Provide a name (and/or optionally email address) so that the people you're communicating with can organize their many contacts' public keys. Choose a passphrase for this pair and store it securely.

- For good measure, also backup the keys as files, as suggested by the program. The button "Make backup" exports the private key, and `[ File › Export ]` exports the public key. When you look at them in a text editor, only one of them says "Begin public key block".

- Never give away the private key!



Importing someone's public key
------------------------------

- If you receive other people's public keys, you can manage them here in Kleopatra.



Usage
-----
- But do give away the public key unencrypted to whoever you want to communicate with. The public key usually needs to be sent only once for every new contact.
- Choose a sample file to test the encryption with.
- Right-click on file, select "Sign and verify" and then choose a contact from the list managed in Kleopatra.
- That gpg file can be sent – even with the public key in the same mail.

