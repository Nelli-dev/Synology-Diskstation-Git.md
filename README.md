# Git-Server Synology-Diskstation
Einrichten eines eigenen Git-Servers auf einer Synology Diskstation

Konfiguration des Synology NAS Servers
--------------------------------------

Anleitung wie man einen Git-Server auf einer Synology Diskstation installiert.
Ich nutze dabei eine DS214play und/oder eine DS218+ mit DSM 6.2.3-25426 Update 2


### Als erstes muss man einen User und ein Ordner auf der Diskstation bereitstellen

- Du kannst dein normales Benutzerprofil benutzen oder legst dir einen `gituser` an
- Lege dazu über DMS einen neuen Benutzer `gituser` an. Der Benutzer benötigt Zugriffsrecht auf die File Station und WebDAV!
- Lege nun einen neuen Order unter `/volume1/git` mit dem namen `git` an. Dieser Ordner soll nun unsere Reposetories halten.
- Falls noch nicht geschehen installiere das Git-Server Paket aus dem Paketzentrum.
- Öffne den Git-Server und gewähre `gituser`die nötigen Rechte.
- Damit du ein Repository erstellen kannst musst du SSH über `Systemsteuerung`-> `Terminal & SNMP`aktivieren.


### Konfiguration SSH Zugang

- erstelle `~/.ssh`für den Benutzer gituser

```
ssh root@diskstation (oder IP)
mkdir /volume1/homes/gituser/.ssh
```
- in dieses Verzeichnis kopieren wir nun den RSA Schlüssel von unserem lokalen Rechner.

```
ssh-copy-id -i ~/.ssh/id_rsa.pub root@diskstation
```

- Teste den key

```
ssh -i ~/.ssh/id_rsa.pub root@diskstation
```

### Nun können wir ein neues Repository auf dem NAS anlegen

- erstelle eine *bare* repo als root Benutzer
 
```
ssh root@diskstation
cd /volume1/git/
git --bare init <repo-name>.git
```

### Optional fall

```
ssh root@diskstation
cd /volume1/git/
git --bare init <repo-name>.git
chown -R gituser:users <repo-name>.git
cd <repo-name>.git
git update-server-info
```

### Auf deinen PC kannst du nun das Repository clonen

- clone Dein gerade angelegtes Repository

```
git clone ssh://gituser@diskstation/volume1/git/<repo-name>.git
```


## Quellen

http://stackoverflow.com/questions/20074692/set-up-git-on-a-nas-with-synologys-official-package
