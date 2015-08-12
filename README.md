# Bacula-Mysql-Backup
Script in shell for prepare incremental MySql backup from Bacula.

- This script must be declared in BeforeJob

##Example of declaration

- Copy the script into /usr/sbin/ and add +x and verify the owner for bacula access.

```sh
$ chmod +x /usr/sbin/bacula-mysql-backup.sh
$ chown bacula:bacula /usr/sbin/bacula-mysql-backup.sh
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
  Storage = File
  Pool = Mysql
  Messages = Standard
  Priority = 2
  Run Before Job = "/usr/sbin/./bacula-mysql-backup.sh"
}
```
