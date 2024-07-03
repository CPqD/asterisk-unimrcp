# Asterisk + unimrcp Application

There are two possibilities to use the Asterisk + Unimrcp.
The first one is using docker containers as describe bellow.

## Using with docker containers

The folders build-os_name and exec-os_name contains the Dockerfiles to build and execute
Asterisk + unimrcp application with docker containers.

You can select the Asterisk Version with argument *--build-arg ASTERISK_VER=21.0.0*
in docker commands.

### Build

Enter in build directory for the selected SO and generate the container to build:

```bash
docker build -t asterisk-unimrcp-build .
```
In the directory install/resources of asterisk unimrcp sources there are
the asterisk configuration files used for the intallation. Set it according.
Execute built image and generate the installation file.

```bash
docker run --rm -v <path_to_asterisk_unimrcp_sources>:/src --name asterisk-build \
       asterisk-unimrcp-build /src/install/make_asterisk_unimrcp_install.sh <installation file name> <asterisk version>
```

The installation file is generated in install directory of asterisk unimrcp sources.

### Exec

Enter in exec directory for the selected SO and build container to exec

```bash
docker build -t asterisk-unimrcp-exec .
```

Exec asterisk

```bash
docker run -it --rm --name asterisk-exec \
           -v <path_to_asterisk_unimrcp_sources>:/src\
           -e LD_LIBRARY_PATH=/usr/local/unimrcp/lib:/usr/local/apr/lib:/usr/local/lib \
           <image name> \
           bash -c "/src/install/<instalation file name>.run &&  /usr/sbin/asterisk -vvvdddf -T -p"
```

## Using a Ubuntu 18.04/Centos 7.8 machine with Asterisk installed from Debian/RPM package

The second way to use the Asterisk + unimrcp application is used a machine with
asterisk installed from a debian or RPM pakages and install the aplication
using the installer built above.

### Generate debian/RPM packages

To use a recent asterisk version, specific packages should be build.
In the folder asterisk-deb or asterisk-centos run the below command to build the packages.

```bash
./build.sh
```

It generates pakages for Asterisk version 21.0.0 in debian format or Asteris version 16.8
in RPM format.
The debian packages are generated in 'dist' folder.
The RPM package is generated in RPMS folder.

### Install asterisk pre-requisites

In the target machine with Ubuntu 18.04, install the pre-requisites

```bash
apt-get update
apt-get install -y \
        libgsm1 \
        libasound2 \
        libgmime-3.0-0\
        libical3 \
        libiksemel3 \
        libjack-jackd2-0 \
        liblua5.1-0 \
        libneon27-gnutls \
        libodbc1 libogg0 \
        libportaudio2 \
        libpq5 \
        libradcli4 \
        libresample1 \
        libsnmp30 \
        libspandsp2 \
        libspeex1 \
        libspeexdsp1 \
        libsrtp2-1 \
        libsybdb5 \
        libunbound2 \
        libvorbis0a \
        libvorbisenc2 \
        libvorbisfile3 \
        libcap2 \
        liburiparser1 \
        libxslt1.1 \
        asterisk-core-sounds-en
```


Or in the target machine with Centos 7.8, install the pre-requisites

```bash
yum install -y \
    libedit \
    gsm \
    unixODBC \
    libogg \
    speex \
    libsrtp \
    unbound \
    uriparser \
    libvorbis \
    libxslt
```

### Install asterisk

#### In Ubuntu 22.04

```bash
dpkg -i asterisk-config_21.0.0-0cpqd1+ubu22.04_all.deb
dpkg -1 asterisk-extra-sounds_21.0.0-0cpqd1+ubu22.04_all.deb
dpkg -1 asterisk-modules_21.0.0-0cpqd1+ubu22.04_amd64.deb
dpkg -1 asterisk-modules-dbgsym_21.0.0-0cpqd1+ubu22.04_amd64.ddeb
dpkg -1 asterisk_21.0.0-0cpqd1+ubu22.04_amd64.deb
```

#### In Centos 7.8

```bash
rpm -iv asterisk-16.8-0.x86_64.rpm
```

### Install Asterisk + unimrcp Application

Using the instalation file produced in step "Build" of
"Using with docker containers" install the apllication.
The configration files could be adjust according to:

https://speechweb.cpqd.com.br/mrcp/docs/latest/ura/asterisk.html

```bash
./<instalation file name>.run
```
