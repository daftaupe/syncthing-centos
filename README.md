# Syncthing rpm

## RPM Build

#### Install rpmbuild requirements

```
yum install -y spectool git mock
```

### Setup build environment

```
cd ~
git clone https://gitlab.com/daftaupe/syncthing-rpm.git
mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
ln -s ~/syncthing-rpm/SPECS/syncthing.spec ~/rpmbuild/SPECS/syncthing.spec
echo '%_topdir %(echo $HOME)/rpmbuild' > ~/.rpmmacros
cd ~/rpmbuild/SOURCES/
spectool -g ../SPECS/syncthing.spec
cd ~/rpmbuild/SPECS/
```
### Build the SRPM
```
rpmbuild -bs ~/rpmbuild/syncthing.spec
# Get the SRPM in ~/rpmbuild/SRPMS
```

### Build the RPM from the SRPM
You build it as indicated before (in that case be careful about the name of the SRPM you will get).
```
#RPM file will be found in ~/rpmbuild/RPMS
# Centos7-64bits
mock --cleanup-after --resultdir ~/rpmbuild/RPMS -r epel-7-x86_64 ~/rpmbuild/SRPMS/syncthing-0.14.46-0.el7.src.rpm
```

### Start  syncthing systemd service

```
useradd syncthing
sudo systemctl start syncthing@syncthing
```

You can now access the GUI through this URL: 
http://127.0.0.1:8384
