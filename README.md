# Proxmox Talos HCI Cluster
## Introduction
A fully working 03 nodes HCI (Hyper Converged Infrastructure) which can host both VM (Virtual Machine) and Kubernetes workload based on:
- Promox (Compute)
- Ceph (Storage - Included with Proxmox)
- Talos Linux (Kubernetes Cluster)
## Proxmox Installation
* Hardware Nodes
  The test environment consists of 03 HPE servers
  - Proxmox is installed on a boot device `/dev/sda`
  - The rest of the disks used for Ceph is configured in HBA mode (pass-through mode)
  - Network overview  
    ![image](https://github.com/user-attachments/assets/99f8a3ef-9c04-4f60-b352-bdeb30cf6e6b)  
* Setup Proxmox Network  
  - **Management Network**: 2x 1Gbps connections in active-passive mode
  - **Ceph Public**: 2 x 10Gbps connections in LACP mode. This network is where the clients (k8s cluster in this case) will connect for persistence volume.  
  - **VM Traffic***: 2 x 10Gbps connections in LACP mode.  
  ![image](https://github.com/user-attachments/assets/95a3b3a0-f92d-4272-84b3-69905d780944)  
* Add 03 nodes to cluster
  Configure Proxmox cluster is a very straight forward process. Link to officical document: https://pve.proxmox.com/wiki/Cluster_Manager
  ![image](https://github.com/user-attachments/assets/83dadb0a-eb40-4932-863a-24cc07849179)
* Configure HA  
  ![image](https://github.com/user-attachments/assets/74f7c090-a64f-4397-8fcf-e042229a963b)
## Ceph Installation  
Configure Ceph cluster is pretty easy to follow: https://pve.proxmox.com/wiki/Deploy_Hyper-Converged_Ceph_Cluster  
* Storage Pools:  
  `vm-storage`: for storing VM disks
  `k8s-storage`: providing persistent volume for Kubernetes cluster
* OSDs:
  ![image](https://github.com/user-attachments/assets/018eb10f-2e84-46bb-a4f2-c68d5133e182)

* Monitor:
  ![image](https://github.com/user-attachments/assets/532f8841-1ff0-4016-b50d-b5495398eb59)  

At this stage, all nodes should see ceph storage pools  
![image](https://github.com/user-attachments/assets/7d615686-6b11-4a44-ac92-f2de24b3a0de)

## Kubernetes Cluster Bootstrap
Following VMs are used for K8s cluster:
````
# Control Planes
talos-cp-01
talos-cp-02
talos-cp-03
# Worker Nodes
talos-wk-01
talos-wk-02
talos-wk-03
````
Talos Linux is used due to its declarative (config via .yaml file), security (no direct shell access)  



  

    

