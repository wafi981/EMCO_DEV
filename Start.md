# SETUP

``` 
   mkdir work

   cd work

   git clone https://gitlab.com/project-emco/core/emco-base.git

   cd emco-base 
```


# Docker Registry



``` 
    docker run -d -p 5000:5000 --name registry registry:2.7

    curl localhost:5000

```

# Set EMCODOCKERREPO 

export EMCODOCKERREPO=localhost:5000/

# Make-deploy

make build-base

export BUILD_CAUSE=DEV_TEST

make deploy


# Run installation script

cd bin/helm

 ./emco-base-helm-install.sh -k /home/ubuntu/.kube/config -p disable install  -s 'enableDbAuth=false'

# Install monitor by passing path to kubeconfig file
helm install monitor monitor-helm-root-latest.tgz --kubeconfig /home/ubuntu/.kube/config

cd ..

cd ..

# Run make to get emcoctl utility

cd /home/ubuntu/work/emco-base/src/tools/emcoctl

make

# Set PATH variable

cd /root

nano .bashrc

export PATH=$PATH:/usr/local/go/bin:/home/ubuntu/work/emco-base/bin/emcoctl

# Changing config files

/home/ubuntu/work/emco-base/examples/single-cluster

nano config

HOST_IP=localhost

KUBE_PATH=/home/ubuntu/.kube/config


# Run the script 

```
./setup.sh create

```
Output files of this command are:

- values.yaml: specifies useful variables for the creation of EMCO resources

- emco_cfg.yaml: defines the deployment details of EMCO (IP addresses and ports of each service)

- prerequisites.yaml: defines all non usecase-specific EMCO resources to create

Helm charts and profile tarballs for all the usecases.

# Next step
```
emcoctl --config emco-cfg.yaml apply -f prerequisites.yaml -v values.yaml

```
