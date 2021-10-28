---
title: Déployer Azure Cloud Services (support étendu) – SDK
description: Déployer Azure Cloud Services (support étendu) à l’aide du SDK Azure
ms.topic: tutorial
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 774b118c1aa74c7de561e7b54843183ac4fc0afb
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2021
ms.locfileid: "130229864"
---
# <a name="deploy-cloud-services-extended-support-by-using-the-azure-sdk"></a>Déployer Azure Cloud Services (support étendu) à l’aide du SDK Azure

Cet article montre comment utiliser le [Kit de développement logiciel (SDK) Azure](https://azure.microsoft.com/downloads/) pour déployer une instance d’Azure Cloud Services (support étendu) qui dispose de plusieurs rôles (rôle Web et Worker) et de l’extension Bureau à distance. Azure Cloud Services (support étendu) est un modèle de déploiement d’Azure Cloud Services qui est basé sur Azure Resource Manager.

## <a name="before-you-begin"></a>Avant de commencer

Consultez les [prérequis du déploiement](deploy-prerequisite.md) de Cloud Services (support étendu) et créez les ressources associées.

## <a name="deploy-cloud-services-extended-support"></a>Déployer Azure Cloud Services (support étendu)
1. Installez le [package NuGet du Kit de développement logiciel (SDK) Azure Compute](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute/43.0.0-preview), puis initialisez le client à l’aide d’un mécanisme d’authentification standard.

    ```csharp
        public class CustomLoginCredentials : ServiceClientCredentials
        {
            private string AuthenticationToken { get; set; }
            public override void InitializeServiceClient<T>(ServiceClient<T> client)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenantID}");
                var credential = new ClientCredential(clientId: "{clientID}", clientSecret: "{clientSecret}");
                var result = authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);
                if (result == null) throw new InvalidOperationException("Failed to obtain the JWT token");
                AuthenticationToken = result.Result.AccessToken;
            }
            public override async Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                if (request == null) throw new ArgumentNullException("request");
                if (AuthenticationToken == null) throw new InvalidOperationException("Token Provider Cannot Be Null");
                request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", AuthenticationToken);
                request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
                //request.Version = new Version(apiVersion);
                await base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }
    
        var creds = new CustomLoginCredentials();
        m_subId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
        ResourceManagementClient m_ResourcesClient = new ResourceManagementClient(creds);
        NetworkManagementClient m_NrpClient = new NetworkManagementClient(creds);
        ComputeManagementClient m_CrpClient = new ComputeManagementClient(creds);
        StorageManagementClient m_SrpClient = new StorageManagementClient(creds);
        m_ResourcesClient.SubscriptionId = m_subId;
        m_NrpClient.SubscriptionId = m_subId;
        m_CrpClient.SubscriptionId = m_subId;
        m_SrpClient.SubscriptionId = m_subId;
    ```

2. Créez un groupe de ressources en installant le package NuGet Azure Resource Manager.

    ```csharp 
    var resourceGroups = m_ResourcesClient.ResourceGroups;
    var m_location = “East US”;
    var resourceGroupName = "ContosoRG";//provide existing resource group name, if created already
    var resourceGroup = new ResourceGroup(m_location); 
    resourceGroup = await resourceGroups.CreateOrUpdateAsync(resourceGroupName, resourceGroup);
    ```

3. Créez un compte de stockage et un conteneur où vous stockerez les fichiers du package de services (.cspkg) et de configuration de service (.cscfg). Installez le [package NuGet de Stockage Azure](https://www.nuget.org/packages/Azure.Storage.Common/). Cette étape est facultative si vous utilisez un compte de stockage existant. Le nom du compte de stockage doit être unique.

    ```csharp
    string storageAccountName = “ContosoSAS”
    var stoInput = new StorageAccountCreateParameters
       {
            Location = m_location,
            Kind = Microsoft.Azure.Management.Storage.Models.Kind.StorageV2,
            Sku = new Microsoft.Azure.Management.Storage.Models.Sku(SkuName.StandardRAGRS),
        };
            StorageAccount storageAccountOutput = m_SrpClient.StorageAccounts.Create(rgName,
            storageAccountName, stoInput);
            bool created = false;
            while (!created)
               {
                    Thread.Sleep(600);
                    var stos = m_SrpClient.StorageAccounts.ListByResourceGroup(rgName);
                    created =
                    stos.Any(
                        t =>
                        StringComparer.OrdinalIgnoreCase.Equals(t.Name, storageAccountName));
                }
        
    StorageAccount storageAccountOutput = m_SrpClient.StorageAccounts.GetProperties(rgName, storageAccountName);.
    var accountKeyResult = m_SrpClient.StorageAccounts.ListKeysWithHttpMessagesAsync(rgName, storageAccountName).Result;
    CloudStorageAccount storageAccount = new CloudStorageAccount(new StorageCredentials(storageAccountName, accountKeyResult.Body.Keys.FirstOrDefault(). Value), useHttps: true);
        
    var blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExistsAsync().Wait();
    sharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddDays(-1);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddDays(2);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;
    ```

4. Chargez le fichier du package de services (.cspkg) dans le compte de stockage. L’URL du package peut être un URI de signature d’accès partagé (SAP) de n’importe quel compte de stockage.

    ```csharp
    CloudBlockBlob cspkgblockBlob = container.GetBlockBlobReference(“ContosoApp.cspkg”);
    cspkgblockBlob.UploadFromFileAsync(“./ContosoApp/ContosoApp.cspkg”). Wait();
    
    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string cspkgsasContainerToken = cspkgblockBlob.GetSharedAccessSignature(sasConstraints);
    
    //Return the URI string for the container, including the SAS token.
    string cspkgSASUrl = cspkgblockBlob.Uri + cspkgsasContainerToken;
    ```

5. Chargez votre fichier de configuration de service (.cscfg) dans le compte de stockage. Spécifiez la configuration du service sous la forme d’une chaîne XML ou d’une URL.

    ```csharp
    CloudBlockBlob cscfgblockBlob = container.GetBlockBlobReference(“ContosoApp.cscfg”);
    cscfgblockBlob.UploadFromFileAsync(“./ContosoApp/ContosoApp.cscfg”). Wait();
    
    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string sasCscfgContainerToken = cscfgblockBlob.GetSharedAccessSignature(sasConstraints);
    
    //Return the URI string for the container, including the SAS token.
    string cscfgSASUrl = cscfgblockBlob.Uri + sasCscfgContainerToken;
    ```

6. Créez un réseau virtuel et un sous-réseau. Installez le [package NuGet de réseau Azure](https://www.nuget.org/packages/Azure.ResourceManager.Network/). Cette étape est facultative si vous utilisez un réseau et un sous-réseau existants.

    ```csharp
    VirtualNetwork vnet = new VirtualNetwork(name: vnetName) 
       { 
            AddressSpace = new AddressSpace 
               { 
                    AddressPrefixes = new List<string> { "10.0.0.0/16" } 
               }, 
                    Subnets = new List<Subnet> 
                    { 
                        new Subnet(name: subnetName) 
                        { 
                            AddressPrefix = "10.0.0.0/24" 
                        } 
                    }, 
                    Location = m_location 
               }; 
    m_NrpClient.VirtualNetworks.CreateOrUpdate(resourceGroupName, “ContosoVNet”, vnet);
    ```

7. Créez une adresse IP publique et définissez sa propriété d'étiquette DNS. Cloud Services (support étendu) prend uniquement en charge les adresses IP publiques de la référence SKU [De base](../virtual-network/ip-services/public-ip-addresses.md#basic). Les adresses IP publiques de référence SKU standard ne fonctionnent pas avec Azure Cloud Services.
Si vous utilisez une adresse IP statique, vous devez la référencer comme adresse IP réservée dans le fichier de configuration de service (.cscfg).

    ```csharp
    PublicIPAddress publicIPAddressParams = new PublicIPAddress(name: “ContosIp”) 
       { 
            Location = m_location, 
            PublicIPAllocationMethod = IPAllocationMethod.Dynamic, 
            DnsSettings = new PublicIPAddressDnsSettings() 
               { 
                    DomainNameLabel = “contosoappdns” 
               } 
       }; 
    PublicIPAddress publicIpAddress = m_NrpClient.PublicIPAddresses.CreateOrUpdate(resourceGroupName, publicIPAddressName, publicIPAddressParams);
    ```

8. Créez un objet de profil réseau et associez l’adresse IP publique au serveur front-end de l’équilibreur de charge. La plateforme Azure crée automatiquement une ressource d’équilibreur de charge de référence SKU « Classique » dans le même abonnement que la ressource de service cloud. La ressource d’équilibreur de charge est une ressource en lecture seule dans ARM. Les mises à jour de la ressource sont prises en charge uniquement par le biais des fichiers de déploiement de service cloud (.cscfg & .csdef).

    ```csharp
    LoadBalancerFrontendIPConfiguration feipConfiguration = new LoadBalancerFrontendIPConfiguration() 
        { 
            Name = “ContosoFe”, 
            Properties = new LoadBalancerFrontendIPConfigurationProperties() 
                { 
                PublicIPAddress = new CM.SubResource() 
                    { 
                        Id = $"/subscriptions/{m_subId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/publicIPAddresses/{publicIPAddressName}", 
                    } 
                } 
        }; 
    
    CloudServiceNetworkProfile cloudServiceNetworkProfile = new CloudServiceNetworkProfile() 
        { 
            LoadBalancerConfigurations = new List<LoadBalancerConfiguration>() 
                { 
                    new LoadBalancerConfiguration() 
                       { 
                        Name  = 'ContosoLB', 
                        Properties = new LoadBalancerConfigurationProperties() 
                            { 
                            FrontendIPConfigurations = new List<LoadBalancerFrontendIPConfiguration>() 
                                { 
                                feipConfig 
                                } 
                            } 
                        } 
                } 
        }; 
    
    ```

9. Création d’un coffre de clés Ce coffre de clés sera utilisé pour stocker les certificats associés aux rôles Azure Cloud Services (support étendu). Le coffre de clés doit se trouver dans la même région et le même abonnement que l’instance d’Azure Cloud Services et avoir un nom unique. Pour plus d’informations, consultez [Utiliser des certificats avec Azure Cloud Services (support étendu)](certificates-and-key-vault.md).

    ```powershell
    New-AzKeyVault -Name "ContosKeyVault” -ResourceGroupName “ContosoOrg” -Location “East US”
    ```

10. Mettez à jour la stratégie d’accès du coffre de clés et accordez des autorisations de certificat à votre compte d’utilisateur.

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosKeyVault' -ResourceGroupName 'ContosoOrg'      -UserPrincipalName 'user@domain.com' -PermissionsToCertificates create,get,list,delete
    ```

    Vous pouvez également définir la stratégie d’accès par le biais de l’ID d’objet (que vous pouvez récupérer en exécutant `Get-AzADUser`).

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosKeyVault' -ResourceGroupName 'ContosOrg' -     ObjectId 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' -PermissionsToCertificates          create,get,list,delete
    ```

11. Dans cet exemple, nous allons ajouter un certificat auto-signé à un coffre de clés. L’empreinte du certificat doit être ajoutée dans le fichier de configuration de service (.cscfg) pour le déploiement sur les rôles Azure Cloud Services (support étendu).

    ```powershell
    $Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -       SubjectName "CN=contoso.com" -IssuerName "Self" -ValidityInMonths 6 -ReuseKeyOnRenewal 
    Add-AzKeyVaultCertificate -VaultName "ContosKeyVault" -Name "ContosCert" -      CertificatePolicy $Policy
    ```

12. Créez un objet de profil de système d’exploitation. Le profil du système d’exploitation spécifie les certificats associés aux rôles Azure Cloud Services (support étendu). Ici, il s’agit du même certificat que celui créé à l’étape précédente.

    ```csharp
    CloudServiceOsProfile cloudServiceOsProfile = 
        new CloudServiceOsProfile 
           { 
                Secrets = new List<CloudServiceVaultSecretGroup> 
                   { 
                        New CloudServiceVaultSecretGroup { 
                        SourceVault = <sourceVault>, 
                        VaultCertificates = <vaultCertificates> 
                        } 
                   } 
           };
    ```

13. Créez un objet de profil de rôle. Le profil de rôle définit des propriétés spécifiques au rôle pour un SKU, comme le nom, la capacité et le niveau. 

    Dans cet exemple, nous définissons deux rôles : ContosoFrontend et ContosoBackend. Les informations de profil de rôle doivent correspondre à la configuration de rôle définie dans le fichier de configuration de service (.cscfg) et dans le fichier de définition de service (.csdef).

    ```csharp
    CloudServiceRoleProfile cloudServiceRoleProfile = new CloudServiceRoleProfile()
       {
            Roles = new List<CloudServiceRoleProfileProperties>();
            
                // foreach role in cloudService
                roles.Add(new CloudServiceRoleProfileProperties()
                    {
                        Name = 'ContosoFrontend',
                        Sku = new CloudServiceRoleSku
                        {
                            Name = 'Standard_D1_v2',
                            Capacity = 2,
                            Tier = 'Standard'
                        }
                    );
    
                roles.Add(new CloudServiceRoleProfileProperties()
                    {
                        Name = 'ContosoBackend',
                        Sku = new CloudServiceRoleSku
                        {
                            Name = 'Standard_D1_v2',
                            Capacity = 2,
                            Tier = 'Standard'
                        }
                    );
    
                    }
                    }
    ```

14. (Facultatif) Créez un objet de profil d’extension que vous devez ajouter à votre instance d’Azure Cloud Services (support étendu). Dans cet exemple, nous ajoutons l’extension RDP.

    ```csharp
    string rdpExtensionPublicConfig = "<PublicConfig>" +
        "<UserName>adminRdpTest</UserName>" +
        "<Expiration>2021-10-27T23:59:59</Expiration>" +
        "</PublicConfig>";
        string rdpExtensionPrivateConfig = "<PrivateConfig>" +
        "<Password>VsmrdpTest!</Password>" +
        "</PrivateConfig>";
    
        Extension rdpExtension = new Extension
                {
                    Name = name,
                    Properties = new CloudServiceExtensionProperties
                    {
                        Publisher = "Microsoft.Windows.Azure.Extensions",
                        Type = "RDP",
                        TypeHandlerVersion = "1.2.1",,
                        AutoUpgradeMinorVersion = true,
                        Settings = rdpExtensionPublicConfig,
                        ProtectedSettings = rdpExtensionPrivateConfig,
                        RolesAppliedTo = [“*”],
                    }
                };
                        
    CloudServiceExtensionProfile cloudServiceExtensionProfile = new CloudServiceExtensionProfile
        {
            Extensions = rdpExtension
        };
    ```

15. Créez le déploiement de l’instance d’Azure Cloud Services (support étendu).

    ```csharp
    CloudService cloudService = new CloudService
        {
            Properties = new CloudServiceProperties
                {
                    RoleProfile = cloudServiceRoleProfile
                    Configuration = < Add Cscfg xml content here>,
                    // ConfigurationUrl = <Add your configuration URL here>,
                    PackageUrl = <Add cspkg SAS url here>,
                    ExtensionProfile = cloudServiceExtensionProfile,
                    OsProfile= cloudServiceOsProfile,
                    NetworkProfile = cloudServiceNetworkProfile,
                    UpgradeMode = 'Auto'
                },
                    Location = m_location
                };
    
    CloudService createOrUpdateResponse = m_CrpClient.CloudServices.CreateOrUpdate(“ContosOrg”, “ContosoCS”, cloudService);
    ```

## <a name="next-steps"></a>Étapes suivantes
- Consultez les [questions fréquentes (FAQ)](faq.yml) concernant Cloud Services (support étendu).
- Déployez Azure Cloud Services (support étendu) à l’aide du [Portail Azure](deploy-portal.md), de [PowerShell](deploy-powershell.md), d’un [modèle](deploy-template.md) ou de [Visual Studio](deploy-visual-studio.md).
- Rendez-vous sur le [référentiel d’exemples d’Azure Cloud Services (support étendu)](https://github.com/Azure-Samples/cloud-services-extended-support).