A bash script for converting a passwordstore directory (ZX2C4's pass, the standard linux password manager) to a csv format supported by bitwarden.

# Usage

if you are paranoid, don't just do `pass2bw ~/passwords > passwords.csv`, the following is more secure:
```sh
# Enter a subshell and make it so created files are only readable by you
bash
umask $((2#000))$((2#111))$((2#111))

# Convert the passwords to a csv file store in RAM
pass2bw ~/passwords > /dev/shm/passwords.csv

# After importing the csv, delete it and leave the
# subshell to restore the umask
rm /dev/shm/passwords.csv
exit # or CTRL+D
```
# Features

- Short, Simple, well documented script, easily modifiable for your use case.
- Proper escaping of weird characters.
- Content is never discarded; it either consumed, or left in the notes section.


# Missing features

- No automatic handling of TOTP or URIs; these aren't lost however, they are left as notes

# Why

I made this in order to migrate from passwordstore to bitwarden. I did so as a reaction to "android password store" being archived (see https://github.com/android-password-store/Android-Password-Store/discussions/3260). I thought people would benifit from a simple to edit, good quality converter. I decided on bash because it's readily available and is easy to edit and hack on
