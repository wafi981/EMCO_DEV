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

 ./emco-base-helm-install.sh -k /home/ubuntu/.kube/config -p disable -s 'enableDbAuth=false' install
 
 # NOTE: Installation scripts does not seem to be working as of now, go over to deployments/helm/emcoBase and run:
 
helm install --namespace emco --set global.disableDbAuth=true emco ./dist/packages/emco-1.0.0.tgz


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


# Next: [Single Cluster Example](https://github.com/wafi981/EMCO-Single_Cluster/blob/main/Start.md#changing-config-files)
