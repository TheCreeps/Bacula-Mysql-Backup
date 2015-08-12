# Bacula-Mysql-Backup
Script in shell for prepare incremental MySql backup from Bacula.

- This script must be declared in BeforeJob

##Example of declaration

- Copy bacula-mysql-backup.conf into /etc/bacula/ and verify the owner for bacula access.

```sh
$ chown bacula:bacula /etc/bacula/bacula-mysql-backup.conf
```

- Copy bacula-mysql-backup.sh into /usr/share/ and add +x and verify the owner for bacula access.

```sh
$ chmod +x /usr/share/bacula-mysql-backup.sh
$ chown bacula:bacula /usr/share/bacula-director/bacula-mysql-backup.sh
```

- Open the configuration file on bacula directory.

```sh
$ nano /etc/bacula/bacula-dir.conf
```

- Add this lines at the end.

```
Pool {
  Name = Mysql
  Pool Type = Backup
  Volume Retention = 60 days
  Recycle = yes
  AutoPrune = yes
  LabelFormat = Mysql-
  Maximum Volume Bytes = 1G
}
FileSet {
  Name = Mysql
  Include {
    File = /srv/bacula/mysql
    Options {
      signature = MD5
    }
  }
}
Job {
  Name = "Sauvegarde Mysql"
  Type = Backup
  Level = Incremental
  Client = debian-8.1-involved-gaming-fd
  FileSet = Mysql
  Schedule = WeeklyCycle
  Storage = File  # Change for your storage
  Pool = Mysql
  Messages = Standard
  Priority = 2
  Run Before Job = "/usr/share/bacula-director/./bacula-mysql-backup.sh"
}
```
