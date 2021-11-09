#SIMPLE PETIT GUIDE POUR INSTALLER ET UTILISER DOCKER DANS PROXMOX AUX COTE DE LXC

0: CREER UN CONTAINER LXC AVEC LE TEMPLATE "turnkey-core" :

General : donner un nom au conteneur lxc exemple (docker_lab) 
Decocher : Unprivileged conatiner
Renseigner un mdp

Template : choisir turnkey-core

1: AVANT DE LANCER LE CONTAINER : Cliquer sur le container LXC nouvellement cree et se rendre dans options > Double cliquer sur Features 
et cliquer sur Nesting (ce qui permettra de lancer docker dans le container LXC) puis cliquer sur OK.

2: Se conencter en console proxmox donner login et mdp creer lors ded la creation du container lxc , skip tous les ecrans jusqu'a l'ecran de "INSTALL".
   Une fois l'installation terminée faire "CTRL+C" pour retourner au prompt.
   
3: Lancer une maj des repos "apt update" et "apt dist-upgrade -y"

4 : Installation de DOCKER :D  :

5 :Supprimer ancienne install docker si il y'en a eu une avant : apt-get remove docker docker-engine docker.io containerd runc

6: Installer certificats : apt-get install     ca-certificates     curl     gnupg     lsb-release

7: Ajout des clefs : curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

#Ajout dans sourcelist
8 : echo   "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
  
9: apt-get update

10 : Si pas d'erreurs a l'etape 9 : apt-get install docker-ce docker-ce-cli containerd.io

11: Tester docker 
root@dockertest ~# docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
12 :
root@dockertest ~# docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
e8330c1c06eb   hello-world   "/hello"   25 minutes ago   Exited (0) 25 minutes ago             exciting_easley


#*************************************************************
#SOurces Videos : https://www.youtube.com/watch?v=gXuLiglJceY
#Sources Docker : https://docs.docker.com/engine/install/debian/
