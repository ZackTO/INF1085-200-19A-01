## Présentation du VPN

Le réseau privé virtuel est un type de réseau privé qui utilise les télécommunications publiques, comme Internet, au lieu des lignes louées pour communiquer.
Devenu populaire au fur et à mesure que plus d'employés travaillaient dans les régions éloignées.

## Un bref aperçu de son fonctionnement

- Deux connexions - l'une à Internet et l'autre au VPN.
- Datagrammes - contient les données, la destination et l'adresse et les informations sur la source.
- Pare-feu - Les VPN permettent aux utilisateurs autorisés de passer à travers les pare-feu.
- Protocoles - les protocoles créent des tunnels VPN

## Comment installer !

## :pushpin: Première étape

:one: Avant de débuter l’installation premièrement il est recommandé de rechercher et d’installer sur le Raspberry les mises à jour disponibles :

```
sudo apt-get update

sudo apt-get upgrade
```

:two: Installer OpenVPN et mettre en place des fichiers easy-rsa

Commencez par utiliser la commande ci-dessous pour installer le programme OpenVPN et OpenSSL :

```
sudo apt-get install openvpn openssl
```

:three: Après avoir installé OpenVPN, copiez les scripts tout prêts « easy-rsa » dans le fichier de configuration OpenVPN.

```
sudo cp -r /usr/share/easy-rsa /etc/openvpn/easy-rsa
```

:four: Ouvrez ensuite le fichier suivant dans la console : « /etc/openvpn/easy-rsa/vars » et entrez-y la commande :

```
sudo nano /etc/openvpn/easy-rsa/vars

```

:five: Maintenant il s’agit d’ajuster le fichier. Vous pouvez modifier les paramètres en remplaçant l’intégralité de la ligne « export EASY_RSA="`pwd` » par la commande :

```
export EASY_RSA="/etc/openvpn/easy-rsa"
```

:six: Ensuite, retournez dans le dossier de configuration « easy-rsa », accordez les droits root et intégrez les paramètres réglés précédemment dans les variables de l’environnement en exécutant le script « vars » grâce à la commande « source ».

```
cd /etc/openvpn/easy-rsa
sudo su
source vars
ln -s openssl-1.0.0.cnf openssl.cnf
```

:seven: Certificats et clés pour installer OpenVPN (Réinitialiser les clés et créez les premiers fichiers de clés pour OpenVPN).

```
./clean-all
./build-ca OpenVPN
```

:eight: Rentrer les deux lettres correspondant à votre « nom de pays » : entrez CA pour le Canada. et générer les fichiers clés pour le serveur :

```
./build-key-server server
```

:nine: Entrez le code à deux lettres correspondant au pays et générez le certificat en appuyant deux fois sur la touche « Y ».

Compléter la génération du certificat grâce à la commande pour l’échange de clés :

```
./build-dh
```

## :pushpin: Deuxieme étape

:one: Une fois que le processus est complété, fermez votre session :

```
exit
```

:two: Générer les fichiers de configuration pour le serveur OpenVPN :

```
sudo nano /etc/openvpn/openvpn.conf
```

:three: Activez le routage par tunnel IP via « dev tun », sélectionnez le protocole de transport UDP, en sélectionnant « proto udp » Si vous souhaitez utiliser le protocole TCP, sélectionnez « proto tcp »). et déterminer si le serveur OpenVPN est accessible sur port 1194.

```
dev tun
proto udp
port 1194
```

:four: Apres, créez un certificat root (ca) SSL/TLS, un certificat digital (cert) et une clé digitale (key) grâce au fichier « easy-rsa ». le cryptage de bits (2048)

```
ca /etc/openvpn/easy-rsa/keys/ca.crt
cert /etc/openvpn/easy-rsa/keys/ca.crt
key /etc/openvpn/easy-rsa/keys/ta.key
dh /etc/openvpn/easy-rsa/keys/dh2048.pem
```

:five: Verifiez que le Raspberry est utilisé en tant que serveur. Nommer l’adresse IP et le masque réseau.

```
server 10.13.237.80 255.255.255.254
```

:six: La commande « redirect-gateway def1 bypass-dhcp » permet de diriger l’intégralité du trafic IP vers le tunnel IP. vous pourrez renommer les serveurs publics DNS avec lesquels fonctionnera votre serveur VPN. Dans la commande suivante, un serveur est désigné par « 10.13.237.1 », et un serveur de Google par « 8.8.8.8 ».

```
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS x.x.x.x"
push "dhcp-option DNS x.x.x.x"
log-append /var/log/openvpn
```

:seven: Créer un script pour un accès Internet avec un client

Pour accéder à votre réseau local grâce à un tunnel VPN, il faut créer une redirection :

```
Sudo nano /etc/init.d/rpivpn
```

:eight: Dans ce fichier, copiez les commentaires suivants :

```
#! /bin/sh
### BEGIN INIT INFO
# Provides:          rpivpn
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: VPN initialization script
### END INIT INFO
```

:nine: Activez « ip forward » en écrivant « 1 » dans ce dossier :

```
echo 'echo "1" > /proc/sys/net/ipv4/ip_forward' | sudo -s
```

## :pushpin: Troisième étape

:one: Créez une redirection pour les paquets VPN « iptables ».

```
iptables -A INPUT -i tun+ -j ACCEPT
iptables -A FORWARD -i tun+ -j ACCEPT
```

:two: Paramétrer les commandes qui autorisent vos clients VPN à accéder au LAN et à Internet :

```
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -t nat -F POSTROUTING
iptables -t nat -A POSTROUTING -s 10.8.0.1/24 -o eth0 -j MASQUERADE
```

:three: Enregistrez et fermez à le fichier 

Assigner les droits adaptés au script et l’installer en tant que script Init.

```
sudo chmod +x /etc/init.d/rpivpn
sudo update-rc.d rpivpn defaults
```

:four: Exécutez le script et redémarrez le serveur VPN.

```
sudo /etc/init.d/rpivpn
sudo /etc/init.d/openvpn restart
```

:five: Terminer l’installation des clients

Il faut leur accorder les droits root en ouvrant le dossier « /etc/openvpn/easy-rsa/keys/ », créer le fichier de configuration client, et entrer les commandes dans le fichier « Test ».

```
sudo su
cd /etc/openvpn/easy-rsa/keys
nano Test.ovpn
```

:six: Pour le fichier client « opvn », entrez les commandes :

```
client
dev tun
proto udp
remote 10.13.237.80 1194
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
tls-version-min 1.2
verify-x509-name raspberrypi_d959d7fb-b1cd-4f9a-a020-5d2c5468ba3b name
cipher AES-256-CBC
auth SHA256
auth-nocache
verb 3
<ca>
```

À la quatrième ligne, remplacez par l’adresse IP de votre fournisseur DDNS ( ou si vous utilisez une adresse publique statique, vous pouvez l’insérer), suivie par le port grâce auquel le serveur VPN doit être accessible.

## :pushpin: N.B: Vérifier let tunnel dans la configuration réseau tun0

ip addr

![image](addr.png)

## Voici la commande pour voir si le openvpn est actif :

```
$ systemctl status openvpn
```
![image]( active.png)

:seven: Enfin, copiez le fichier de configuration avec les certificats et les clés dans un fichier zip.

Installé de pack zip sur votre Raspberry :

```
apt-get install zip
```

:eight: Pour créer un fichier zip, utilisez les commandes ci-dessous en vous assurant que chaque ligne comprend le bon nom de client.

```
zip /home/pi/raspberry_Test.zip ca.crt ta.key Test.ovpn

```

:nine: Ensuite ajuster les droits des fichiers et terminer l’installation par « exit ».

```
chown pi:pi /home/pi/raspberry_Test.zip
exit
```
