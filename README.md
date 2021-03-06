Firmware Site Config for Freifunk Cuxhaven
------------------------------------------

## Firmware Kompilieren

### Voraussetzungen (Stand Gluon v2018.2.x):

Muss auf dem Rechner installiert sein. Hier Beispiel Debian:

    apt-get install git subversion python build-essential gawk unzip libncurses-dev libz-dev libssl-dev

### Gluon kompilieren

Auf dieser Seite wird beschrieben, wie man die Gluon Firmware für das Freifunk Nordwest Netzwerk kompiliert. Um die Firmware zu kompilieren wird ein Rechner mit einem Linux Betriebssystem und ca. 70-100GB freier Speicher benötigt. Der make Befehl passt sich automatisch an die Anzahl von cores an.

*Wichtig* Je nach Entwicklungsstand muss die Branch Version angepasst werden.

    git clone https://github.com/freifunk-gluon/gluon.git ./freifunk_build -b v2018.2.x && cd ./freifunk_build
    git clone https://github.com/Freifunk-Cuxhaven/site-ffcux site

    cd ..
    make update
    # GLUON_BRANCH: gibt den zu verwendenden Gluon-Branch an
    # GLUON_TARGET: gibt die Gruppe der zu bauenden Images an (siehe Gluon Doku)
    # V: wenn V=s dann wird debug Output beim Kompilieren eingeschaltet
    make -j $(($(grep -c processor /proc/cpuinfo)*2)) GLUON_BRANCH=stable GLUON_TARGET=ar71xx-generic V=s

### Manifest und initiale Signatur erstellen

    make manifest GLUON_BRANCH=stable

    ./contrib/sign.sh ../firmware/release_keys/ecdsa-privatekey ./output/images/sysupgrade/stable.manifest

Weitere Informationen z.B. zu automatischen Builds auch unter https://gluon.readthedocs.org/en/latest/features/autoupdater.html

### Prüfsummen erstellen

Die Prüfsummen werden auf dem Server automatisiert generiert.

### Referenzen

* https://wiki.openwrt.org/doc/howto/build
* https://buildroot.org/downloads/manual/manual.html
