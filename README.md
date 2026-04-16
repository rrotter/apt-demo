# Demo apt repo

## Usage
* fork this repo
* edit the reprepro config to your liking
* put any info you want to share with users in /apt/README.md, this is the directory that will be published
* place all .deb files you want to serve, along with an appropirate .changes in /incoming
* generate a gpg key:
```
# use a throwaway container
podman run --rm -it debian:latest bash
# install gpg
apt update
apt install -y gpg
# configure defaults for new keys to disable SHA1 hashes, and other nonsense no
# one uses anymore (probably doesn't matter, since this is only for signing)
mkdir ~/.gnupg && chmod 700 ~/.gnupg
echo "default-preference-list SHA512 SHA384 SHA256 AES256 ZLIB BZIP2 ZIP Uncompressed" > ~/.gnupg/gpg.conf
# generate and dump key
gpg --batch --passphrase '' --quick-generate-key "Yourname Here (some comment) <email@example.com>" ed25519 sign never
gpg --export-secret-keys --armor
```
* save the resulting key as a secret called `GPG_PRIVATE_KEY` in your repo settings

* GH Action will:
  * install reprepro from source, since Debian/Ubuntu version is very old and missing critical features
  * generate apt repo using your config
  * add index.md directory listings
  * publish to GitHub Pages

## Scripts
* `changes`: use this to generate missing `.changes` files for an existing `.deb`
```
./bin/changes --help
# support specific distros
./bin/changes --distro "sarge woody sid" incoming/*.deb
./bin/changes --distro trixie incoming/*.deb
# support all distros supported by this repo
./bin/changes --all incoming/*.deb
```
* `index`: used by deploy workflow to generate directory listings
