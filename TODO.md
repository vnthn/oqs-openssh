# TODO

Follow Arch Package Guidelines

- [ ] Get rid of newly introduced variables
- [ ] Check dependencies (of OpenSSH) to get them complete
- [ ] Copy optional dependencies from openssh
- [ ] Reduce line length in PKGBUILD to under 100 chars
- [ ] Remove empty lines
- [ ] Quote variables that may contain spaces

Complete OpenSSH support

- [ ] Copy additional files into the pkg (like openssh does)

General

- [ ] Remove check for darwin OS
- [ ] Remove OpenSSL-if-construct, if possible
- [ ] Remove Travis and CircleCI checks
- [ ] Add checksums to the files not coming from Git
