
  GPG Encryption Tutorial (for Ubuntu 18.04.2)
================================================

{ work in progress }

[for_windows]: ?
[guide]: https://help.ubuntu.com/community/GnuPrivacyGuardHowto


- Setup
- Usage


See the [other tutorial][for_windows] for an introduction of PGP, GPG, OpenPGP, or for a more comprehensible description of the overall process.

(Instructional commands starting with `$` mean that you should enter the part behind that in a terminal.)



Setup
-----

`gpg` already came pre-installed on Ubuntu. Using this [official guide][guide] as a base:

- Create key pair:

  `$ gpg --gen-key`

  Enter name, email, passphrase.


- Identify the keyid:

  This illustrative example shows many valid formats of the same keyid.

```
  fingerprint: 0D69 E11F 12BD BA07 7B37  26AB 4E1F 799A A4FF 2279
  long id:                                    4E1F 799A A4FF 2279
  short id:                                             A4FF 2279
  another valid format: 0D69E11F12BDBA077B3726AB4E1F799AA4FF2279
```

- Create an environment variable `$GPGKEY`:

  Copy the keyid that was reported during key creation (can be reprinted with `$ gpg -k`). Open your `.bashrc` file (or whatever your setup has) and append that keyid:

  `export GPGKEY=0D69E11F12BDBA077B3726AB4E1F799AA4FF2279`


- Backup keys:

```
  $ mkdir ~/.gpg-backup
  $ cd ~/.gpg-backup
  $ gpg -ao my-public-key.asc --comment "My comment" --export $GPGKEY
  $ gpg -o my-private-key.key --export-secret-keys $GPGKEY
```


- If you need to restore the key pair:

```
  $ cd ~/.gpg-backup
  $ gpg --import my-public-key.asc
  $ gpg --import my-private-key.key
```


- Backup trusted database:

  `$ gpg --export-ownertrust > ownertrust-database.txt`


- Never give away the private key!



Importing someone's public key
------------------------------

`$ gpg --import path/to/someones-public-key.asc`

`$ gpg -k` should list it with the `unknown` label. Let's change that.

```
$ gpg --edit-key user-name-or-id-or-email

…
trust: unknown       validity: unknown
…

gpg> trust
Your decision? { pick a trust level }

gpg> sign
gpg> yes

gpg> save
```

`$ gpg -k` now says `full` which is the validity.



Usage
-----

```
$ cd my/path/
$ gpg -e -r user-name-or-id-or-email my-important-file.txt
```

Which generates a `my-important-file.txt.gpg` in `my/path/`. Attach this file to your email and let the recipient decrypt it.

