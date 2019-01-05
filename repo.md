# Background
Repo created using reprepro.

# Create Dummy Packages
```bash
apt install equivs
equivs-control $pkg-name
nano $pkg-name
equivs-build $pkg-name
```

# Adding repo to Debian

## 1. add my PGP key
`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv B6D32792566F0D55059947A90CDEA3BBE213DA29`

## 2. Add the repo

`echo "deb https://andrewharle.github.io/REPO/debian stretch experimental" | tee -a /etc/apt/sources.list`

# Dummy Packages Installation Example

Package *mongodb-server* shown for this example.

```bash
sudo echo "deb https://andrewharle.github.io/REPO/debian dummy misc" | tee -a /etc/apt/sources.list
sudo echo -e "Package: mongodb-server\nPin: release a=dummy\nPin-Priority: 500" | tee /etc/apt/preferences
sudo apt update
sudo apt install mongodb-server
````
