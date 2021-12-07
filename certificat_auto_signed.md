# Comment générer un certificat SSL auto-signé sur Linux

## Étape 1 : créer une paire de clés RSA

#### Installation de openssl

`sudo yum install openssl`

#### générez la paire de clés :

`openssl genrsa -des3 -passout pass:x -out keypair.key 2048`

## Étape 2 : extrayez la clé privée dans le dossier « httpd »

### créer le dossier "httpscertificat" (vous pouvez donner le nom que vous voulez) dans le repertoire '/etc/httpd/'

`sudo mkdir /etc/httpd/httpscertificat`

### Pour extraire la clé privée du fichier de paire de clés que nous venons de créer, saisissez ce qui suit :

`openssl rsa -passin pass:x -in keypair.key -out /etc/httpd/httpscertificate/ <server_ip_address>.key`

## Étape 3 : Création d'un fichier « Demande de signature de certificat » (CSR)

Avec la clé, nous pouvons créer un .csrfichier spécial que nous pouvons soit signer nous-mêmes, soit soumettre à une « autorité de certification ». C'est dans un format standardisé, et peut être facilement généré avec notre clé de l'étape précédente. Pour le créer, tapez la commande suivante :

`openssl req -new -key /etc/httpd/httpscertificate/ <server_ip_address>.key -out /etc/httpd/httpscertificate/ <server_ip_address>.csr`

Lorsque vous exécutez cette commande, l'outil vous demandera certaines de vos informations personnelles, telles que votre emplacement et le nom de votre organisation .

## Étape 4 : Création du fichier de certificat « .crt »

Avec le CSR, nous pouvons créer le fichier de certificat final comme suit. Nous allons maintenant utiliser nos fichiers .csret .keypour créer notre .crtfichier :

`openssl x509 -req -days 365 -in /etc/httpd/httpscertificate/ <server_ip_address>.csr -signkey /etc/httpd/httpscertificate/ <server_ip_address>.key -out /etc/httpd/httpscertificate/ <server_ip_address>.crt`

Cela crée un .crtfichier à l'emplacement avec tous nos autres fichiers. Nous savons maintenant comment générer notre certificat SSL auto-signé.

Maintenant, nous devons dire à Apache où se trouvent ces fichiers.

## Étape 5 : Configuration d'Apache pour utiliser les fichiers

Tout ce que nous devons faire maintenant est de montrer à Apache où se trouvent nos certificats auto-signés générés. Tout d'abord, nous devons installer le mod_sslpackage avec la commande :

`sudo yum install mod_ssl`

Une fois cela fait, cela placera un ssl.conf fichier dans le /etc/httpd/conf.d/dossier. Nous devons modifier ce fichier par défaut. Utilisez votre éditeur de texte préféré :

`sudo vi /etc/httpd/conf.d/ssl.conf`

Sauvegarder vos modifications .Maintenant redemarer apache

`sudo apachectl restart`
