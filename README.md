# ziorpm

Inside the `rpm -i mypkg` cmd


### Running the cmd

'rpm -i andromeda-dummy-2.0-1.1.noarch.rpm -vvvv'

First the rpm is looking and checking for the rpm signed key.

This is needed for checking the integrity of the pkg, e.g distros use trusted gpg-keys for signing the pkg on repos,
 ensuring that the pkg comes from controlled source and not a 3rd party malvelliant.

```console
rpm -i andromeda-dummy-2.0-1.1.noarch.rpm -vvvv
D: ============== andromeda-dummy-2.0-1.1.noarch.rpm
D: loading keyring from pubkeys in /var/lib/rpm/pubkeys/*.key
D: couldn't find any keys in /var/lib/rpm/pubkeys/*.key
D: loading keyring from rpmdb
D: opening  db environment /var/lib/rpm cdb:private:0x201
D: opening  db index       /var/lib/rpm/Packages 0x400 mode=0x0
D: locked   db index       /var/lib/rpm/Packages
D: opening  db index       /var/lib/rpm/Name nofsync:0x400 mode=0x0
D:  read h#     252 Header SHA1 digest: OK (2fc2ad5c3630b1b8e172e4978ced43ff1ab38077)
D: added key gpg-pubkey-8a7c64f9-536756cf to keyring
D:  read h#     253 Header SHA1 digest: OK (b62eb951e4ac8e967987254186481ef7df0988e5)
D: added key gpg-pubkey-39db7c82-5847eb1f to keyring
D: Using legacy gpg-pubkey(s) from rpmdb
D: Expected size:         9174 = lead(96)+sigs(344)+pad(0)+data(8734)
D:   Actual size:         9174
D: andromeda-dummy-2.0-1.1.noarch.rpm: Header V3 DSA/SHA1 Signature, key ID 8a7c64f9: OK
```


```console
D: 	added binary package [0]
D: found 0 source and 1 binary packages
```

After the pkg is the right and expected one,  rpm check for 2 diff types of rpms

- the Source RPM (SRPM) and the binary RPM. The binary RPM is the mostly used, and srpm are for devs that need the src code of pkg and want to build the pkg.

Installing vim for example from a repo, you will always install the binary rpm, without the SRPM.

- The SRPM contain the SPEC file (which describes how to install/remove and build an pkg for RPM) and the actually source code that the resulting binary RPM.


Here rpm is checking  prerequisites for the pkg version given the arch.

```
D: opening  db index       /var/lib/rpm/Conflictname nofsync:0x400 mode=0x0
D: ========== +++ andromeda-dummy-2.0-1.1 noarch/linux 0x0
D: opening  db index       /var/lib/rpm/Basenames nofsync:0x400 mode=0x0
D:  read h#      74 Header V3 RSA/SHA256 Signature, key ID 39db7c82: OK
D:  Requires: /bin/sh                                       YES (db files)
D:  Requires: /bin/sh                                       YES (cached)
D: opening  db index       /var/lib/rpm/Providename nofsync:0x400 mode=0x0
D:  read h#     120 Header V3 RSA/SHA256 Signature, key ID 39db7c82: OK
D:  Requires: pam                                           YES (db provides)
D:  Requires: rpmlib(CompressedFileNames) <= 3.0.4-1        YES (rpmlib provides)
D:  Requires: rpmlib(PayloadFilesHavePrefix) <= 4.0-1       YES (rpmlib provides)
D:  Requires: rpmlib(PayloadIsLzma) <= 4.4.6-1              YES (rpmlib provides)
D: opening  db index       /var/lib/rpm/Obsoletename nofsync:0x400 mode=0x0
D: ========== recording tsort relations
D: ========== tsorting packages (order, #predecessors, #succesors, depth)
```

### Installing pkgs and execute spec basic


Once installing rpm, rpm will basically execute what is in specfiles.(execpt from the build option, which is only when building from spec files)

Basically, perform task before the install(changing file permisions etc), during install, and after install. (like execute ldconfig after shared library installed.)
```console
%prep
cp %{S:0} .

%build

%install

%post

%postun

%files
%defattr(-,root,root)
%doc COPYING

%changelog
```



```console
D:     0    0    0    1   +andromeda-dummy-2.0-1.1.noarch
D: installing binary packages
D: Selinux disabled.
D: closed   db index       /var/lib/rpm/Obsoletename
D: closed   db index       /var/lib/rpm/Conflictname
D: closed   db index       /var/lib/rpm/Providename
D: closed   db index       /var/lib/rpm/Basenames
D: closed   db index       /var/lib/rpm/Name
D: closed   db index       /var/lib/rpm/Packages
D: closed   db environment /var/lib/rpm
D: opening  db environment /var/lib/rpm cdb:private:0x201
D: opening  db index       /var/lib/rpm/Packages (none) mode=0x42
D: locked   db index       /var/lib/rpm/Packages
D: sanity checking 1 elements
D: opening  db index       /var/lib/rpm/Name nofsync mode=0x42
D: running pre-transaction scripts
D: computing 2 file fingerprints
D: opening  db index       /var/lib/rpm/Basenames nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Group nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Requirename nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Providename nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Conflictname nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Obsoletename nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Triggername nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Dirnames nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Installtid nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Sigmd5 nofsync mode=0x42
D: opening  db index       /var/lib/rpm/Sha1header nofsync mode=0x42
Preparing packages...
D: computing file dispositions
D: 0x0000fe01     4096     48601357     13062245 /
D: ========== +++ andromeda-dummy-2.0-1.1 noarch-linux 0x0
D: Expected size:         9174 = lead(96)+sigs(344)+pad(0)+data(8734)
D:   Actual size:         9174
D: andromeda-dummy-2.0-1.1.noarch: Header V3 DSA/SHA1 Signature, key ID 8a7c64f9: OK
D:   install: andromeda-dummy-2.0-1.1 has 2 files
andromeda-dummy-2.0-1.1.noarch
D: ========== Directories not explicitly included in package:
D:          0 /usr/share/doc/packages/
D: ==========
D: create     040755  2 (   0,   0)     0 /usr/share/doc/packages/andromeda-dummy
D: create     100644  1 (   0,   0) 17992 /usr/share/doc/packages/andromeda-dummy/COPYING;59941e50
XZDIO:      10 reads,    18428 total bytes in 0.000920 secs

```

For keeping track of pkgs, rpm update his database
```console
D: adding "andromeda-dummy" to Name index.
D: adding 2 entries to Basenames index.
D: adding "Applications/System" to Group index.
D: adding 6 entries to Requirename index.
D: adding 2 entries to Providename index.
D: adding 2 entries to Dirnames index.
D: adding 1 entries to Installtid index.
D: adding 1 entries to Sigmd5 index.
D: adding "9d9dea0c68caac62f3e8a7ed7e403e493b146a8d" to Sha1header index.
D: %post(andromeda-dummy-2.0-1.1.noarch): scriptlet start
D: %post(andromeda-dummy-2.0-1.1.noarch): execv(/bin/sh) pid 12843
D: %post(andromeda-dummy-2.0-1.1.noarch): waitpid(12843) rc 12843 status 0
D: running post-transaction scripts
D: closed   db index       /var/lib/rpm/Sha1header
D: closed   db index       /var/lib/rpm/Sigmd5
D: closed   db index       /var/lib/rpm/Installtid
D: closed   db index       /var/lib/rpm/Dirnames
D: closed   db index       /var/lib/rpm/Triggername
D: closed   db index       /var/lib/rpm/Obsoletename
D: closed   db index       /var/lib/rpm/Conflictname
D: closed   db index       /var/lib/rpm/Providename
D: closed   db index       /var/lib/rpm/Requirename
D: closed   db index       /var/lib/rpm/Group
D: closed   db index       /var/lib/rpm/Basenames
D: closed   db index       /var/lib/rpm/Name
D: closed   db index       /var/lib/rpm/Packages
D: closed   db environment /var/lib/rpm
```

The biggest limitation of rpm -i is that it will not install the dependencies of a pkg, like yum/dnf/zypper pkg manager can do.
