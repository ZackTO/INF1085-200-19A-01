# 2 Keys

:one: Authoriser l'acces à sa machine au professeur

* Connectez vous à votre machine avec `git bash` ou `putty`

```
$ ssh monID@monAdresseIP
```

* Ouvrir le fichier d'authorisation de clés publiques suivant avec Nano 

```
$ nano ~/.ssh/authorized_keys
```

* copier la clé publique ci-dessous dans une nouvelle ligne 

(attention copier la clé tel quel sans espace ou autre charactere, i.e. la clé est longue)

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD2pLhMqFGKffSdYvNCMAyM7598oBY+m/3q5AMXmb7IE6vq42+yGzqEUzZu9WrFckFD4Hq52rIU5DeOvi83DCF3uroXjNTEtCKdi+tY7cV18bHmsDsBHMqTnpuvroofgFWA0Pi++b2kGW2I5eyy1Qjv5rOp7y11Xe6XeZFEz7qQO1/xNiBMJEruG9Xldgooe4hkaOF39qnbqD4ui3LxYaTUTEulstw4wN70dSB8Zu9YQP7A7KU2zIEwJ1aw8whfO1CAM/AVvoDyqMtV8VXoaZSHOBgluMtinQfyyt473S2ZZeJlnmhK0F1gdOhO4SVZNRMj96m30ryYkYBFWvvLRP5N b300098957@ramena
```

## Identifiant

:two: Renseigner son utilisateur dans le tableau ci-dessous (i.e. MONUSER@10.13.237.126 => brice@10.13.237.126)

|:hash:| :id:      | Utilisateur à remplacer      | ssh              | Docker Engine    | 
|------|-----------|------------------------------|------------------|------------------|
| 01   | 300104524 | echnaideurg@10.13.237.19     |:heavy_check_mark:|:heavy_check_mark:|
| 02   | 300104541 | romeo@10.13.237.41           |:heavy_check_mark:|:x:               |
| 03   | 300105201 | fabrice@10.13.237.78         |:heavy_check_mark:|:x:               |
| 04   | 300106918 | amir@10.13.237.18            |:heavy_check_mark:|:heavy_check_mark:|
| 05   | 300107361 | toch90@10.13.237.99          |:heavy_check_mark:|:heavy_check_mark:|
| 06   | 300108234 | haliuser@10.13.237.55        |:heavy_check_mark:|:heavy_check_mark:|
| 07   | 300110500 | MONUSER@10.13.237.75         |:x:               |:x:               |
| 08   | 300110529 | david@10.13.237.80           |:x:               |:x:               |
| 09   | 300111671 | sadissou@10.13.237.63        |:x:               |:x:               |
| 10   | 300111766 | thiam@10.13.237.66         |:x:               |:x:               |
| 11   | 300112017 | orden@10.13.237.60           |:heavy_check_mark:|:x:               |
| 12   | 300112687 | nsomwe@10.13.237.87          |:heavy_check_mark:|:x:               |
| 13   | 300112917 | djumaster@10.13.237.79       |:heavy_check_mark:|:x:               |
| 14   | 300113775 | widby@10.13.237.77           |:heavy_check_mark:|:x:               |

:three: Installer Docker Engine sur sa machine Linux

Suivre le tutoriel suivant

https://github.com/CollegeBoreal/Tutoriels/tree/master/2.Cloud-Native/2.Docker/Linux

### Tester si Dcoker Engine est fonctionel

```
$ systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-10-08 17:23:47 UTC; 1 day 23h ago
     Docs: https://docs.docker.com
 Main PID: 24174 (dockerd)
    Tasks: 26
   CGroup: /system.slice/docker.service
           └─24174 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Oct 08 17:23:44 echnaideurg dockerd[24174]: time="2019-10-08T17:23:44.382151023Z" level=warning msg="Your kernel does not support swap memory limit"
Oct 08 17:23:44 echnaideurg dockerd[24174]: time="2019-10-08T17:23:44.382179791Z" level=warning msg="Your kernel does not support cgroup rt period"
Oct 08 17:23:44 echnaideurg dockerd[24174]: time="2019-10-08T17:23:44.382190586Z" level=warning msg="Your kernel does not support cgroup rt runtime"
Oct 08 17:23:44 echnaideurg dockerd[24174]: time="2019-10-08T17:23:44.382361631Z" level=info msg="Loading containers: start."
Oct 08 17:23:45 echnaideurg dockerd[24174]: time="2019-10-08T17:23:45.084064004Z" level=info msg="Default bridge (docker0) is assigned with an IP address 172.17.0.0/16. Daemon 
Oct 08 17:23:45 echnaideurg dockerd[24174]: time="2019-10-08T17:23:45.412643470Z" level=info msg="Loading containers: done."
Oct 08 17:23:46 echnaideurg dockerd[24174]: time="2019-10-08T17:23:46.830608514Z" level=info msg="Docker daemon" commit=6a30dfc graphdriver(s)=overlay2 version=19.03.2
Oct 08 17:23:46 echnaideurg dockerd[24174]: time="2019-10-08T17:23:46.830955345Z" level=info msg="Daemon has completed initialization"
Oct 08 17:23:47 echnaideurg dockerd[24174]: time="2019-10-08T17:23:47.022365319Z" level=info msg="API listen on /var/run/docker.sock"
Oct 08 17:23:47 echnaideurg systemd[1]: Started Docker Application Container Engine.
```

