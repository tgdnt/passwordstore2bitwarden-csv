A bash script that takes a directory of passwordstore passwords as an argument, then outputs the passwords in a csv format supported by bitwarden.

I made this in order to migrate from passwordstore to bitwarden. I did so as a reaction to "android password store" being archived (see https://github.com/android-password-store/Android-Password-Store/discussions/3260)

# Features

- Short, Simple, well documented script, easily modifiable for your use case.
- Proper escaping of weird characters
- No content is lost; if it isn't consumed, the content is left in the notes section

# Missing features

- No automatic handling of TOTP or URIs; these aren't lost however, they are left as notes

