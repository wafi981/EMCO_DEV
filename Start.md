# SETUP

mkdir work

cd work

git clone https://gitlab.com/project-emco/core/emco-base.git

cd emco-base

# Docker Registry



docker run -d -p 5000:5000 --name registry registry:2.7

curl localhost:5000

# Set EMCODOCKERREPO 

export EMCODOCKERREPO=localhost:5000/

# Make-deploy

make build-base

export BUILD_CAUSE=DEV_TEST

make deploy


# Run installation script

cd bin/helm

./emco-base-helm-install.sh  -k /home/ubuntu/.kube/config -p disable  install

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

DNS=false
HOST_IP=localhost
KUBE_PATH=/home/ubuntu/.kube/config
LOGICAL_CLOUD_LEVEL=admin
CLM_SERVICE_PORT=30461
DCM_SERVICE_PORT=30477
DCM_STATUS_PORT=30478
DTC_CONTROL_PORT=9048
DTC_SERVICE_PORT=30418
GAC_CONTROL_PORT=9033
GAC_SERVICE_PORT=30420
NCM_SERVICE_PORT=30481
NCM_STATUS_PORT=30482
NPS_CONTROL_PORT=9038
OVN_CONTROL_PORT=9032
OVN_SERVICE_PORT=30451
ORCH_SERVICE_PORT=30415
ORCH_STATUS_PORT=30416
RSYNC_CONTROL_PORT=9031


# Run the script 

./setup.sh create

Output files of this command are:

values.yaml: specifies useful variables for the creation of EMCO resources

emco_cfg.yaml: defines the deployment details of EMCO (IP addresses and ports of each service)

prerequisites.yaml: defines all non usecase-specific EMCO resources to create

Helm charts and profile tarballs for all the usecases.

# Next step
emcoctl --config emco-cfg.yaml apply -f prerequisites.yaml -v values.yaml


