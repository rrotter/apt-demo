---
tile: README
---
# README
---
## Setup
To use this repository:
```
# import keyring
mkdir -p /etc/apt/keyrings
curl -fsSL https://this.repo.url.invalid/keyring.asc -o /etc/apt/keyrings/reponame-keyring.asc

# add repo
echo "\
Types: deb
URIs: https://this.repo.url.invalid/
Suites: $(grep -m1 '^VERSION_CODENAME=' /etc/os-release | cut -d= -f2)
Components: main
Signed-By: /etc/apt/keyrings/reponame-keyring.asc\
" > /etc/apt/sources.list.d/reponame.sources

# apt update
apt update
```
