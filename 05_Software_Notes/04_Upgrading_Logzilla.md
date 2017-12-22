<!-- @@@title:Upgrading LogZilla@@@ -->

# Release Version

Copy/paste the following code into the LogZilla server's console via ssh/shell:

```sh
source /etc/lsb-release
grep -q "stable main" /etc/apt/sources.list.d/logzilla.list 2>/dev/null || echo "deb http://apt.logzilla.net ${DISTRIB_CODENAME}-stable main" | sudo tee -a /etc/apt/sources.list.d/logzilla.list
wget -qO- http://apt.logzilla.net/logzilla.gpg.key | sudo apt-key add -
sudo apt update
sudo apt install logzilla/main
```

# Staging Branch

As noted in [Software Releases](/help/software_information/software_releases), our Staging branch is typically two weeks ahead of the Release version (master branch). Users wanting to test the staging branch code may do so by running the following commands:

```sh
source /etc/lsb-release
grep -q "staging main" /etc/apt/sources.list.d/logzilla.list 2>/dev/null || echo "deb http://apt.logzilla.net ${DISTRIB_CODENAME}-staging main" | sudo tee -a /etc/apt/sources.list.d/logzilla.list
wget -qO- http://apt.logzilla.net/logzilla.gpg.key | sudo apt-key add -
sudo apt update
sudo apt install logzilla/staging
```

# Development Branch

This branch should only be considered for `testing`. If you encounter any bugs, kindly [let us know](mailto:support@logzilla.net?Subject=I%20found%20a%20bug%20in%20LogZilla&body=&body=Steps%20To%20Reproduce%3A%0D%0A%0D%0AStep%201%3A%0D%0A%0D%0AStep%202%3A%20Etc...%0D%0A%0D%0A[screenshot])

```sh
source /etc/lsb-release
grep -q "devel main" /etc/apt/sources.list.d/logzilla.list 2>/dev/null || echo "deb http://apt.logzilla.net ${DISTRIB_CODENAME}-devel main" | sudo tee -a /etc/apt/sources.list.d/logzilla.list
wget -qO- http://apt.logzilla.net/logzilla.gpg.key | sudo apt-key add -
sudo apt update
sudo apt install logzilla/devel
```

# Warning
Neither the staging branch nor the Development branch should not be used in production as they have not been QA tested by our team.




