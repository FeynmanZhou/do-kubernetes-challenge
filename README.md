# DigitalOcean Kubernetes Challenge: Deploy KubeSphere on Kubernetes 

![KubeSphere+DOKS](/images/KubeSphere-DOKS.png)

## What is DigitalOcean Kubernetes

[DigitalOcean Kubernetes (DOKS)](https://www.digitalocean.com/products/kubernetes/) is a managed Kubernetes service that lets you deploy Kubernetes clusters without the complexities of handling the control plane and containerized infrastructure. Clusters are compatible with standard Kubernetes toolchains and integrate natively with DigitalOcean Load Balancers and block storage volumes.

## What is KubeSphere

[KubeSphere](https://kubesphere.io/) is a distributed operating system for cloud-native application management, using Kubernetes as its kernel. It provides a plug-and-play architecture, allowing third-party applications to be seamlessly integrated into its ecosystem.

KubeSphere is available on DigtalOcean Marketplace which allows you to deploy in a few clicks. However, the version of the built-in KubeSphere is not up-to-date, the KubeSphere team is working on upgrading it.

This guide walks you through the steps of deploying the latest version KubeSphere on [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes/).

## Prepare a DOKS Cluster

A Kubernetes cluster in DO is a prerequisite for installing KubeSphere. Go to your [DO account](https://cloud.digitalocean.com/) and refer to the image below to create a cluster from the navigation menu.

![create-cluster-do](/images/create-cluster-do.png)

You need to perform the steps below:

1. Kubernetes version (for example, *1.21.5-do.0*)
2. Datacenter region (for example, *San Francisco*)
3. VPC network (for example, *default-fra1*)
4. Cluster capacity (for example, 3 standard nodes with 2 vCPUs and 4GB of RAM each)
5. A name for the cluster (for example, *kubesphere-do*)

![config-cluster-do](/images/config-cluster-do.png)


> Notice
> - To install KubeSphere 3.2.1 on Kubernetes, your Kubernetes version must be v1.19.x, v1.20.x, v1.21.x, or v1.22.x (experimental).
> - 3 nodes are included in this example. You can add more nodes based on your own needs especially in a production environment.
> - The machine type Standard / 4 GB / 2 vCPUs is for minimal installation. If you plan to enable KubeSphere add-ons or use the cluster for production, you can upgrade your nodes to a more powerfull type.

When the cluster is ready, you can download the configuration file for using kubectl to connect DOKS cluster.

![download-config-file](/images/download-config-file.png)

## Install KubeSphere on DOKS

When the DOKS cluster is ready, you can install KubeSphere on DOKS by performing the steps below:

- Install KubeSphere using kubectl. The following commands are only for the default minimal installation.

  ```bash
  kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.2.1/kubesphere-installer.yaml
  
  kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.2.1/cluster-configuration.yaml
  ```

- Inspect the logs of installation:

  ```bash
  kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-install -o jsonpath='{.items[0].metadata.name}') -f
  ```

When the installation finishes, you can see the following message:

```bash
#####################################################
###              Welcome to KubeSphere!           ###
#####################################################
Console: http://10.XXX.XXX.XXX:30880
Account: admin
Password: P@88w0rd
NOTES：
  1. After logging into the console, please check the
     monitoring status of service components in
     the "Cluster Management". If any service is not
     ready, please wait patiently until all components
     are ready.
  2. Please modify the default password after login.
#####################################################
https://kubesphere.io             2020-xx-xx xx:xx:xx
```

## Access KubeSphere Console

After KubeSphere is installed, you can access the web console of KubeSphere by following the steps below.

- Go to the Kubernetes Dashboard provided by DigitalOcean.

  ![kubernetes-dashboard-access](/images/kubernetes-dashboard-access.png)

- Select the **kubesphere-system** namespace.

  ![kubernetes-dashboard-namespace](/images/kubernetes-dashboard-namespace.png)

- In **Services** under **Service**, edit the service **ks-console**.

  ![kubernetes-dashboard-edit](/images/kubernetes-dashboard-edit.png)

- Change the type from `NodePort` to `LoadBalancer`. Save the file when you finish.

  ![lb-change](/images/lb-change.png)

- Access the KubeSphere's web console using the endpoint generated by DO.

  ![access-console](/images/access-console.png)

 > Please note: Instead of changing the service type to `LoadBalancer`, you can also access KubeSphere console via `NodeIP:NodePort` (service type set to `NodePort`). You need to get the public IP of one of your nodes.


- Log in to the console with the default account and password (`admin/P@88w0rd`). In the cluster overview page, you can see the dashboard.


