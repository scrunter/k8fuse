Enable the blob CSI on the cluster during creation with the correct flags, or update an existing cluster:

On creation 

Terraform:  blob_driver_enabled = true in the storage_profile block
Cli:  use the --enable-blob-driver flag

To add to an existing cluster:

az aks update --enable-blob-driver -n myAKSCluster -g myResourceGroup


Fuse can leverage an MSI account for Blob Auth, this is assigned to the system nodepool (NOT user nodes)

Details here: https://github.com/qxsch/Azure-Aks/tree/master/aks-blobfuse-mi

For example purposes, the user assigned managed identity created within
the nodepool resource group was utilised, and assigned Data Blob Owner on the resource
group containing the storage account (scope can probably be reduced to the storage
account itself).

The PV definition then leverages the MSI principal ID in its parameters:

    volumeAttributes:
      resourceGroup: rg-testing
      storageAccount: ihctestingaks
      containerName: ihctest
      protocol: fuse
      AzureStorageAuthType: MSI
      AzureStorageIdentityObjectID: "{YOUR MSI PRINCIPAL ID}"


Important:

When configuring against a storage account with Hierarchical namespace enabled, you must enable
the mount option: --use-adls=true in the pvc (see example)
