---

copyright:
  years: 2024
lastupdated: "2024-07-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with cluster networks
{: #cluster-network-getting-started}

TBD
{: shortdesc}


There are two clusters to consider:

Physical Cluster Implementation: This is a specific implementation of a cluster in the data center. A physical cluster has both a type and a name.
Cluster Network: This is virtual cluster that the user creates at the API. It is created based off a Cluster Network Profile, and internally (hidden from the user) will map to a compatible Physical Cluster Implementation.
The work flow is the following:

Physical Cluster Implementation:

A cluster definition is added to the mzone.yaml file.
Upon the next expansion, kube-define will be updated to create the appropriate objects to represent the cluster objects on the backend.
Release engineering determines when / where to define clusters.
Individual server nodes will be added to the cluster through additional modifications to the server spec in the mzone.yaml.
See m-zone yaml updates for more information on this flow.
Customer Flow:

Cluster Network
The customer will enumerate the cluster network profiles available for a given zone.
Once they choose a cluster network profile, they will create a cluster network.
Behind the scenes, the cluster network resource created by the user should then be mapped to a specific physical cluster implementation.
This is used to inform the VM scheduler candidate hypervisors when an Instance is provisioned which connects to the cluster network.
The user will then create subnets as child objects on the cluster network.
The user may pre-create interface objects on the cluster network, or they may do this as part of instance provision.
Instance Provision
Customer will identify a cluster network capable instance profile (specifically in this case the gx3d-152x1536-8h100 profile).
Customer will set the parameters on the instance creation that are standard (ex. image selection, volume definition, standard cloud networking, advanced configuration like metadata, etc...)
Customer will select a set of subnets or interfaces to attach to the cluster network. These are the cluster network attachments
The number of cluster network attachments are defined by the Instance Profile.
The order of the cluster network attachments defines the layout on the backend system
The instances will be created
If there is no capacity in the physical cluster implementation to support the instance, the instance will move to a failed state and the status_reason for the instance will indicate that there is a lack of capacity.
If there is capacity, the instance resource will be created and then available to the user.
See the API Definition for more details on the customer facing design.
