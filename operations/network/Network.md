# Network

There are several different networks supported by the HPE Cray EX system. The following are the available internal and external networks, as well as the devices that connect to each network:

-   Networks external to the system:
    -   Customer Network \(Data Center\)
        -   `ncn-m001` BMC is connected by the customer network switch to the customer **management** network
        -   All NCNs \(worker, master, and storage\) are connected
        -   ClusterStor System Management Unit \(SMU\) interfaces
        -   User Access Nodes \(UANs\)
-   System networks:
    -   Customer Network \(Data Center\)
        -   `ncn-m001` BMC is connected by the customer network switch to the customer **management** network
        -   ClusterStor SMU interfaces
        -   User Access Nodes \(UANs\)
    -   Hardware Management Network \(HMN\)
        -   BMCs for Admin tasks
        -   Power distribution units \(PDU\)
        -   Keyboard/video/mouse \(KVM\)
    -   Node Management Network \(NMN\)
        -   All NCNs and compute nodes
        -   User Access Nodes \(UANs\)
    -   ClusterStor Management Network
        -   ClusterStor controller management interfaces of all ClusterStor components \(SMU, Metadata Management Unit \(MMU\), and Scalable Storage Unit \(SSU\)\)
    -   High-Speed Network \(HSN\), which connects the following devices:
        -   Kubernetes worker nodes
        -   UANs
        -   ClusterStor controller data interfaces of all ClusterStor components \(SMU, MMU, and SSU\)
        -   There must be at least two NCNs whose BMCs are on the HMN. If these are not present, there cannot be multiple DVS servers that function correctly, which will have an effect on compute node root file system and PE scaling/performance/reliability.

During initial installation, several of those networks are created with default IP address ranges. See [Default IP Address Ranges](Default_IP_Address_Ranges.md).

A default configuration of Access Control Lists \(ACL\) is also set when the system is installed. The default configuration of ACLs between the NMN and HMN are described below:

The network management system \(NMS\) data model and REST API enable customer sites to construct their own "networks" of nodes within the high-speed fabric, where a "network" is a collection of nodes that share a VLAN and an IP subnet.

The low-level network management components \(switch, DHCP service, ARP service\) of the NCNs and ClusterStor interfaces are configured to serve one particular network \(the "supported network"\) on the high-speed fabric. The supported network includes all of the compute nodes, thereby enabling those compute nodes to access the gateway, user access services, and ClusterStor devices. A site may create other networks as well, but it is only the supported network that is served by those devices.

![Management Network Connections - Liquid Cooled](../../img/Management_Network_Connections_Liquid_Cooled.png "Management Network Connections - Liquid Cooled")

