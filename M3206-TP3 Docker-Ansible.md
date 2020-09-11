---
title :  M3206-TP3 Docker/Ansible
---

<h1 text-align= "center"> M3206-TP3 Docker/Ansible </h1>

## Docker sous Linux
2. 1. La version de Docker installé sur la VM est la suivante:
    
    ```bash=1
    $ docker info
         Containers: 1
         Running: 0
         Paused: 0
         Stopped: 1
         Images: 2
         Server Version: 18.09.7
       [...]
        Init Binary: docker-init
        containerd version: 894b81a4b802e4eb2a91d1ce216b8817763c29fb
        runc version: 425e105d5a03fabd737a126ad93d62a9eeede87f
        init version: fec3683
        [...]
        Registry: https://index.docker.io/v1/
        Labels:
        Experimental: false
        Insecure Registries:
         127.0.0.0/8
        Live Restore Enabled: false
        Product License: Community Engine

        WARNING: No swap limit support
    ```
    2. L'installation de Docker fonctionne bien car la commande **docker run hello-world** renvoie Hello from Docker.
    ```bash=1
    $ docker run hello-world

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    [...]

    For more examples and ideas, visit:
     https://docs.docker.com/get-started/

    ```
    2. 1.  Le retour de la commande suivante nous explique que le client a contacté le démon Docker, le démon a ensuite **pull** l'image du Docker Hub. Docker a grâce à ce **pull** créé un nouveau conteneur, et **run** celui-ci.
    ```bash=1
        To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    ```
    2. 2. L'image hello-world se trouve https://hub.docker.com/_/hello-world
         3. Le fichier sur DockerHub qui permet de créer le container est https://hub.docker.com/_/scratch/


## commandes création imageping:solen
```bash=1
    git clone https://registry.iutbeziers.fr:5443/pouchou/debianiut.git
    ls
    cd debianiut/
    ls
    vim Dockerfile
    docker build -t monimage:solen
    docker build -t monimage:solen .
    ls
    vim Dockerfile
    vim dockerfile.solen
    vim Dockerfile.solen
    cd essais 
    vim dockerfile.ssh
    vim dockerfile.ssh.stretch 
    cp Dockerfile.stretch 
    vim  Dockerfile.stretch 
    cp dockerfile.ssh dockerfile.solen
    ls
    docker build -t imageping:solen -f ./dockerfile.solen .
    vim dockerfile.solen 
    docker build -t imageping:solen -f ./dockerfile.solen .
    vim dockerfile.solen 
    docker build -t imageping:solen -f ./dockerfile.solen .
    vim dockerfile.solen 
    docker run dockerfile.solen 
    docker run imageping:solen 
    vim dockerfile.solen 
    docker run imageping:solen 
    vim dockerfile.solen
```

## Orchestration avec Ansible
4. 4. 1.
    
    
