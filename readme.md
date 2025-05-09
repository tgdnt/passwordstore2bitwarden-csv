A bash script for converting a passwordstore directory to a csv format supported by bitwarden. password store is also known as pass, the standard linux password manager; it's built by ZX2C4

# Usage

`pass2bw ~/passwords > passwords.csv` would suffice.
if you are paranoid however, the following is more secure:
```sh
# Enter a subshell where created files are private
bash
umask $((2#000))$((2#111))$((2#111))

# Convert the passwords and store the results in RAM
./pass2bw ~/passwords > /dev/shm/passwords.csv

# After importing the csv, delete it and leave the subshell
rm /dev/shm/passwords.csv
exit # or CTRL+D
```
# Features

- Short, Simple, well documented script, easily modifiable for your use case.
- Proper escaping of weird characters.
- Content is never discarded; it's either consumed, or left in the notes section.


# Missing features

- No handling of TOTP
- No handling of URIs

Those fields aren't lost however, they are left as notes, like every other unhandled part of the content of the password file 

# Why

I made this in order to migrate from passwordstore to bitwarden. I did so as a reaction to "android password store" being archived (see https://github.com/android-password-store/Android-Password-Store/discussions/3260). I thought people would benifit from a simple to edit, good quality converter. I decided on bash because it's readily available and is easy to edit and hack on

# Thoughts

**Password Formats**
1. It would be nice to be able to easily convert from any password format to the other
2. For `n` password formats, `2^n` converters must be created; not a good solution for (1)
3. Introducing an intermediate format and a set of programs that convert back and fourth between the intermediate format and any other format reduces the number of conversion tools to `2*n`. much better solution compared to (2)
4. It maybe a good idea to take an already existing format as an intermediate, instead of introducing yet another bespoke format.
5. It would be nice if the intermediate format became a standard password format eliminating the need for converters altogether (1)

**Password store**

I much prefer password store's way of doing things.
- It relies directly on the filesystem for storing passwords, thereby bypassing the need for reimplementing a worse filesystem. one that doesn't integrate into the rest of the system. preventing it from benifiting from all the already existing filesystem management tools and preventing the user from using their favorite programs for managing files. 
- As a result of keeping the store simple, passwordstore doesn't have to concern itself with version management, syncing, or collaborative work. all of this work can be offloaded nicely to the already existing `git`
- Password store atomatically has more features than most password managers. no password manager is going to reimplement git and all of it's features. and even if they do. password store got there without wasting effort on reimplementation.
