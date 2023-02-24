# ITI Final Project

Deploy a fully automated node js application on GKE cluster using CI/CD with jenkins and configuration management with Ansible and IaC with terraform.


## Usage
This repo contains bode js application and the Jenkinsfile,the Dockerfile and the deployment files for the this application. 

Steps:

1) We need a jenkins server and jenkins slave up and running. You mau use this [repo](https://github.com/amr-elzahar/Final-Project-Infra-Code) to create them


2) We have to exec into the slave container:

```
kubectl exec -it <pod> -- bash
```

to start SSH service:

```
service ssh start
```


to change pemissions of the docker binaries directory (/var/run/docker.sock):

```
chmod 666 /var/run/docker.sock
```


to install gcloud and gcloud SDK and authenticate to the GKE cluster:

```
# 1)
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" |  tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

# 2)
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg |  apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

# 3)
apt-get update &&  apt-get install google-cloud-cli

# 4)
gcloud init

# 5)
apt-get install google-cloud-sdk-gke-gcloud-auth-plugin

```

to create a new passwd for the jenkins user (we will later use this passwd when we configure a new node)

```
passwd jenkins
```

3) In the kenkins server, we need to create a multibranch item and connect it to this repo by adding source and choose git and provide the link of this repo.

4) Configure a new node for the slave and add credentials form the username and password type and use the same passowrd that you set before for jenkins user

5) We are gonna need some other credentials, we will need docker hub credentials from username and paaword type and cluster config credentials from secret file type. The most easiest way for the cluster config credentials is to copy the config file in this path ~/.kube/config. It will be something like the config file in this repo.

6) We are done!. You can build now.


The final outpust is gonna be something like that:

![Final Output](https://github.com/amr-elzahar/Final-Project-Application-Code/blob/main/final-output/final-output.png?raw=true)