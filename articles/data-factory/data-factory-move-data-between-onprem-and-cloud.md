---
title: Déplacer des données – Passerelle de gestion des données | Microsoft Docs
description: Mettez en place une passerelle de données pour déplacer vos données entre un emplacement local et le cloud. Utilisez la passerelle de gestion des données dans Azure Data Factory pour déplacer vos données.
keywords: passerelle de données, intégration de données, déplacer des données, informations d’identification de passerelle
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: jingwang

---
# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Déplacement de données entre des sources locales et le cloud à l’aide de la passerelle de gestion des données
Cet article présente l’intégration des données entre les magasins de données locaux et les magasins de données cloud à l’aide de Data Factory. Il s’appuie sur l’article [Activités de déplacement des données](data-factory-data-movement-activities.md) et d’autres articles sur les principaux concepts Data Factory : les [jeux de données](data-factory-create-datasets.md) et les [pipelines](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Passerelle de gestion de données
Vous devez installer la passerelle de gestion des données sur votre ordinateur local pour activer le déplacement des données vers/depuis un magasin de données local. La passerelle peut être installée sur le même ordinateur que le magasin de données ou sur un autre ordinateur, pourvu qu’elle puisse se connecter au magasin de données. Consultez l’article [Data Management Gateway](data-factory-data-management-gateway.md) (Passerelle de gestion des données) pour obtenir des informations détaillées sur la passerelle de gestion des données.   

La procédure pas à pas suivante vous explique comment créer une fabrique de données avec un pipeline qui déplace les données d’une base de données SQL Server locale vers un objet blob Azure. Dans le cadre de la procédure pas à pas, vous installez et configurez la passerelle de gestion des données sur votre ordinateur. 

## <a name="walkthrough:-copy-on-prem-data-to-cloud"></a>Procédure pas à pas : copier des données locales vers le cloud
## <a name="create-data-factory"></a>Créer une fabrique de données
Dans cette étape, vous allez utiliser le portail Azure pour créer une instance Azure Data Factory nommée **ADFTutorialOnPremDF**. Vous pouvez également créer une fabrique de données à l’aide des applets de commande Azure Data Factory. 

1. Une fois connecté au [portail Azure](https://portal.azure.com), cliquez dans le coin inférieur gauche sur **NOUVEAU**, sélectionnez **Analyse de données** dans le panneau **Créer**, puis cliquez sur **Fabrique de données** dans le panneau **Analyse de données**.
   
   ![Nouveau -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png) 
2. Dans le panneau **Nouvelle fabrique de données** :
   
   1. Saisissez **ADFTutorialOnPremDF** for the **Nom**.
   2. Cliquez sur **RESOURCE GROUP NAME** et sélectionnez **ADFTutorialResourceGroup**. Vous pouvez sélectionner un groupe de ressources existant ou en créer un. Pour créer un groupe de ressources :
      1. Cliquez sur **Créer un groupe de ressources**.
      2. Dans le panneau **Créer un groupe de ressources**, saisissez un **nom** pour le groupe de ressources et cliquez sur **OK**.
3. Remarquez que l’option **Ajouter au tableau d’accueil** est activée sur le panneau **Nouvelle fabrique de données**.
   
    ![Ajouter au Tableau d’accueil](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)
4. Dans le panneau **Nouvelle fabrique de données**, cliquez sur **Créer**.
   
    Le nom de la fabrique de données Azure doit être un nom global unique. Si l'erreur suivante s'affiche : **Le nom de la fabrique de données « ADFTutorialOnPremDF » n'est pas disponible**, changez le nom de la fabrique de données (par exemple, votrenomADFTutorialOnPremDF), puis essayez de la recréer. Utilisez ce nom à la place d'ADFTutorialOnPremDF quand vous effectuez les étapes restantes de ce didacticiel.  
5. Recherchez des notifications à partir du processus de création en cliquant sur le bouton **Notifications** sur la barre de titre, comme indiqué dans l’image suivante. Cliquez dessus de nouveau pour fermer la fenêtre de notifications. 
   
    ![Hub NOTIFICATIONS](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNotificationsHub.png)
6. Une fois la création terminée, le panneau **Fabrique de données** s’affiche comme sur l’image suivante :
   
   ![Page d’accueil Data Factory](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Créer une passerelle
1. Dans le panneau **DATA FACTORY**, cliquez sur la vignette **Créer et déployer** pour lancer **l’éditeur** de la fabrique de données.
   
    ![Vignette Créer et déployer](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
2. Dans Data Factory Editor, cliquez sur **... (points de suspension)** sur la barre d’outils, puis cliquez sur **Nouvelle passerelle de données**. 
   
   ![Nouvelle passerelle de données sur la barre d’outils](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. Dans le panneau **Créer**, saisissez **adftutorialgateway** dans le champ **Nom**, puis cliquez sur **OK**.    
   
    ![Panneau Créer une passerelle](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
4. Dans le panneau **Configurer**, cliquez sur **Installer directement sur cet ordinateur**. Cette action télécharge le package d’installation de la passerelle, installe, configure et inscrit la passerelle sur l’ordinateur.  
   
   > [!NOTE]
   > Utilisez Internet Explorer ou un navigateur web compatible Microsoft ClickOnce.
   > 
   > Si vous utilisez Chrome, accédez au [Chrome Web Store](https://chrome.google.com/webstore/), faites une recherche sur le mot-clé « ClickOnce », choisissez l’une des extensions ClickOnce, puis installez-la. 
   > 
   > Faites de même pour Firefox (installez un complément). Cliquez sur le bouton **Ouvrir le menu** dans la barre d’outils (**trois lignes horizontales** en haut à droite), cliquez sur **Modules complémentaires**, effectuez une recherche avec le mot-clé « ClickOnce », choisissez l’une des extensions de ClickOnce et installez le programme.    
   > 
   > 
   
    ![Passerelle - Panneau Configurer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)
   
    Il s’agit de la méthode la plus simple (un clic) pour télécharger, installer, configurer et inscrire la passerelle en une seule étape. Vous pouvez voir que l’application **Gestionnaire de configuration de la passerelle de gestion de données Microsoft** est installée sur votre ordinateur. Vous pouvez également trouver l’exécutable **ConfigManager.exe** dans le dossier suivant : **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.
   
    Vous pouvez également télécharger et installer manuellement la passerelle en utilisant les liens de ce panneau et l’enregistrer à l’aide de la clé indiquée dans la zone de texte **NOUVELLE CLÉ** .
   
    Consultez l’article [Data Management Gateway](data-factory-data-management-gateway.md) (Passerelle de gestion des données) pour obtenir des informations détaillées sur la passerelle.
   
   > [!NOTE]
   > Vous devez être administrateur sur l’ordinateur local pour pouvoir installer et configurer la passerelle de gestion des données avec succès. Vous pouvez ajouter des utilisateurs supplémentaires au groupe Windows local d’utilisateurs de la passerelle de gestion des données. Les membres de ce groupe peuvent utiliser l’outil Gestionnaire de configuration de la passerelle de gestion des données pour configurer la passerelle. 
   > 
   > 
5. Patientez deux minutes, puis lancez l’application **Gestionnaire de configuration de la passerelle de gestion de données** sur votre ordinateur. Dans la fenêtre **Rechercher**, saisissez **passerelle de gestion de données** pour accéder à cet utilitaire. Vous pouvez également trouver l’exécutable **ConfigManager.exe** dans le dossier suivant : **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 
   
    ![Gestionnaire de configuration de la passerelle](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Patientez jusqu’à ce que :
   
   1. Le **nom de la passerelle** soit défini sur **adftutorialgateway**.
   2. La barre d’état située au bas de l’écran affiche le message **Connecté au service de cloud** accompagné d’une **coche verte**.
      
      Sur l’onglet **Accueil**, vous pouvez également effectuer les opérations suivantes :      - **Inscrire** une passerelle avec une clé du portail Azure en utilisant le bouton Inscrire. 
      
      * **Arrêter** le service d’hébergement de la passerelle de gestion des données en cours d’exécution sur l’ordinateur de passerelle. 
      * **Planifier les mises à jour** à installer à une heure spécifique de la journée. 
      * Afficher l’heure de **dernière mise à jour**de la passerelle. 
7. Basculez vers l’onglet **Paramètres** . Le certificat spécifié dans la section **Certificat** est utilisé pour chiffrer/déchiffrer les informations d’identification du magasin de données local que vous fournissez dans le portail (facultatif). Cliquez sur **Modifier** pour utiliser votre propre certificat à la place. Par défaut, la passerelle utilise le certificat généré automatiquement par le service Data Factory.
   
    ![Configuration de certificat de la passerelle](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)
   
    Vous pouvez également effectuer les actions suivantes dans l’onglet Paramètres : - Afficher ou exporter le certificat utilisé par la passerelle.
   
        - Modifier le point de terminaison HTTPS utilisé par la passerelle - 
8. (facultatif) Basculez sur l’onglet **Diagnostics**, et cochez l’option **Activer la journalisation détaillée** si vous souhaitez activer la journalisation détaillée à utiliser pour résoudre les problèmes de passerelle. Vous trouverez les informations de journalisation dans **l’Observateur d’événements** sous le nœud **Journaux des applications et des services** -> **Passerelle de gestion des données**. 
   
    ![Onglet Diagnostic](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)
   
    Vous pouvez également effectuer les actions suivantes dans l’onglet **Diagnostics** : 
   
   * Utilisez la section **Tester la connexion** à une source de données locale à l’aide de la passerelle.
   * Cliquez sur **Afficher les journaux** pour consulter le journal de la passerelle de gestion des données dans une fenêtre de l’Observateur d’événements. 
   * Cliquez sur **Envoyer des journaux** pour charger un fichier zip contenant les journaux des sept derniers jours et l’envoyer à Microsoft pour faciliter le dépannage en cas de problèmes. 
9. Dans le portail Azure, cliquez sur **OK** dans le panneau **Configurer**, puis dans le panneau **Nouvelle passerelle de données**.
10. Vous devez voir **adftutorialgateway** sous **Passerelles de données** dans l’arborescence de gauche.  Si vous cliquez dessus, vous devez voir le code JSON associé. 

## <a name="create-linked-services"></a>Créez des services liés
Au cours de cette étape, vous créez deux services liés : **AzureStorageLinkedService** et **SqlServerLinkedService**. Le service lié **SqlServerLinkedService** associe une base de données SQL Server locale, et le service lié **AzureStorageLinkedService** associe un magasin d’objets blob Azure à la fabrique de données. Plus loin dans cette procédure pas à pas, vous allez créer un pipeline qui copie les données de la base de données SQL Server locale vers le magasin d’objets blob Azure. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Ajout d’un service lié à une base de données SQL Server locale
1. Dans **Data Factory Editor**, cliquez sur **Nouvelle banque de données** sur la barre d’outils, puis sélectionnez **SQL Server**. 
   
   ![Nouveau service lié SQL Server](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
2. Dans l’ **éditeur JSON**, procédez comme suit : 
   
   1. Pour **gatewayName**, spécifiez **adftutorialgateway**. 
   2. Si vous utilisez l’authentification Windows, procédez comme suit :
      
      1. Dans **connectionString**, définissez la **sécurité intégrée** sur **true**, spécifiez le **nom du serveur** de la base de données, le **nom de la base de données**, puis supprimez **l’ID utilisateur** et le **mot de passe**. 
      2. Spécifiez le nom d’utilisateur et le mot de passe pour les propriétés **userName** et **password**.  
         
              "typeProperties": {
                  "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;",
                  "gatewayName": "adftutorialgateway",
                  "userName": "<Specify user name if you are using Windows Authentication. Example: <domain>\\<user>",
                  "password": "<Specify password for the user account>"
              }
   3. Si vous utilisez l’authentification SQL, procédez comme suit :
      
      1. Spécifiez le **nom du serveur**, le **nom de la base de données**, ainsi que les paramètres **User ID** et **Password** de la base de données dans **connectionString**.       
      2. Supprimez les deux dernières propriétés JSON (**userName** et **password**) du code JSON.
      3. Supprimez la `,` (virgule)** à la fin de la ligne spécifiant la valeur de la **propriété gatewayName**. 
         
              "typeProperties": {
                  "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
                  "gatewayName": "<Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database>"
              }
         
         Les informations d’identification sont **chiffrées** à l’aide d’un certificat détenu par le service Data Factory. Si vous préférez utiliser le certificat qui est associé à la passerelle de gestion des données, consultez [Set credentials securely](#set-credentials-and-security)(Configuration des informations d’identification de manière sécurisée). 
3. Cliquez sur l’option **Déployer** de la barre de commandes pour déployer le service lié SQL Server. 

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Ajout d’un service lié pour un compte de stockage Azure
1. Dans **Data Factory Editor**, dans la barre de commandes, cliquez sur **Nouvelle banque de données**, puis sur **Stockage Azure**.
2. Entrez le nom de votre compte de stockage Azure dans le champ **Nom du compte**.
3. Entrez la clé de votre compte de stockage Azure dans le champ **Clé du compte**.
4. Cliquez sur **Déployer** pour déployer le service lié **AzureStorageLinkedService**.

## <a name="create-datasets"></a>Créez les jeux de données
Dans cette étape, vous allez créer des jeux de données d’entrée et de sortie qui représentent les données d’entrée et de sortie pour l’opération de copie (base de données SQL Server locale = > stockage d’objets blob Azure). Avant de créer des jeux de données ou des tables (jeux de données rectangulaires), vous devez effectuer les opérations suivantes (les étapes sont détaillées après la liste) :

* Créez une table nommée **emp** dans la base de données SQL Server que vous avez ajoutée en tant que service lié à la fabrique de données et insérez quelques exemples d’entrées dans la table.
* Créez un conteneur d’objets blobs nommé **adftutorial** dans le compte de stockage d’objets blobs Azure que vous avez ajouté en tant que service associé à la fabrique de données.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Préparation du serveur SQL Server local pour le didacticiel
1. Dans la base de données que vous avez spécifiée pour le service lié SQL Server local (**SqlServerLinkedService**), utilisez le script SQL suivant pour créer la table **emp** dans la base de données.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO


1. Insérer des exemples dans la table : 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')



### <a name="create-input-table"></a>Créer une table d’entrée
1. Dans **Data Factory Editor**, dans la barre de commande, cliquez sur **Nouveau jeu de données**, puis sur **Table SQL Server**. 
2. Remplacez le code JSON du volet droit par le texte suivant :    
   
       {
         "name": "EmpOnPremSQLTable",
         "properties": {
           "type": "SqlServerTable",
           "linkedServiceName": "SqlServerLinkedService",
           "typeProperties": {
             "tableName": "emp"
           },
           "external": true,
           "availability": {
             "frequency": "Hour",
             "interval": 1
           },
           "policy": {
             "externalData": {
               "retryInterval": "00:01:00",
               "retryTimeout": "00:10:00",
               "maximumRetry": 3
             }
           }
         }
       }
   
   Notez les points suivants : 
   
   * Le **type** est défini sur **SqlServerTable**.
   * **tableName** est défini sur **emp**.
   * Le paramètre **linkedServiceName** est défini sur **SqlServerLinkedService** (vous avez créé ce service lié à l’étape 2).
   * Dans le cas d’une table d’entrée qui n’est pas générée par un autre pipeline dans Azure Data Factory, vous devez définir **external** sur **true**. Cela signifie que les données d’entrée sont produites à l’extérieur du service Azure Data Factory. Vous pouvez éventuellement spécifier des stratégies de données externes à l’aide de l’élément **externalData** dans la section **Policy**.    
   
   Pour plus d'informations sur les propriétés JSON, consultez la rubrique [Référence de script JSON][json-script-reference].
3. Cliquez sur l’option **Déployer** de la barre de commandes pour déployer le jeu de données (la table est un jeu de données rectangulaire). Vérifiez que le message **TABLE DÉPLOYÉE AVEC SUCCÈS**s’affiche dans la barre de titre. 

### <a name="create-output-table"></a>Créer la table de sortie
1. Dans **Data Factory Editor**, cliquez sur **Nouveau jeu de données** dans la barre de commandes, puis sur **Stockage Blob Azure**.
2. Remplacez le code JSON du volet droit par le texte suivant : 
   
       {
         "name": "OutputBlobTable",
         "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
             "folderPath": "adftutorial/outfromonpremdf",
             "format": {
               "type": "TextFormat",
               "columnDelimiter": ","
             }
           },
           "availability": {
             "frequency": "Hour",
             "interval": 1
           }
         }
       }
   
   Notez les points suivants : 
   
   * **type** est défini sur **AzureBlob**.
   * Le paramètre **linkedServiceName** est défini sur **AzureStorageLinkedService** (que vous avez créé à l’étape 2).
   * Le paramètre **folderPath** est défini sur **adftutorial/outfromonpremdf**, où « outfromonpremdf » est le dossier dans le conteneur adftutorial. S’il n’existe pas déjà, créez le conteneur **adftutorial** . 
   * **availability** est défini sur **hourly** (**frequency** a la valeur **hour** et **interval** est défini sur **1**).  Le service Data Factory génère une tranche de données de sortie toutes les heures dans la table **emp** de la base de données SQL Azure. 
   
   Si vous ne spécifiez pas de nom **fileName** pour une table **d’entrée**, tous les fichiers/objets blobs du dossier d’entrée (**folderPath**) sont considérés comme des entrées. Si vous spécifiez un fileName dans le JSON, seul le fichier/objet blob spécifié est considéré comme une entrée. Pour en savoir plus, consultez les fichiers d'exemple du [didacticiel][adf-tutorial].
   
   Si vous ne spécifiez pas de **fileName** pour une **table de sortie**, les fichiers générés dans le **folderPath** sont nommés selon le format suivant : Data.<Guid>.txt (par exemple : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).
   
   Pour affecter une valeur à **folderPath** et **fileName** de manière dynamique en fonction de l’heure de **SliceStart**, utilisez la propriété partitionedBy. Dans l’exemple suivant, folderPath utilise les valeurs Year, Month et Day à partir de SliceStart (heure de début de la partie en cours de traitement), alors que fileName utilise la valeur Hour à partir de SliceStart. Par exemple, si une partie est produite pour 2014-10-20T08:00:00, la valeur folderName est wikidatagateway/wikisampledataout/2014/10/20, alors que la valeur de fileName est 08.csv. 
   
       "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
       "fileName": "{Hour}.csv",
       "partitionedBy": 
       [
           { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
           { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
           { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
           { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
       ],

    Pour plus d'informations sur les propriétés JSON, consultez la rubrique [Référence de script JSON][json-script-reference].

1. Cliquez sur l’option **Déployer** de la barre de commandes pour déployer le jeu de données (la table est un jeu de données rectangulaire). Vérifiez que le message **TABLE DÉPLOYÉE AVEC SUCCÈS**s’affiche dans la barre de titre.

## <a name="create-pipeline"></a>Création d’un pipeline
Dans cette étape, vous créez un **pipeline** avec une **activité Copier l’activité** qui utilise **EmpOnPremSQLTable** en tant qu’entrée et **OutputBlobTable** en tant que sortie.

1. Dans le panneau **DATA FACTORY**, cliquez sur la vignette **Créer et déployer** pour lancer **l’éditeur** de la fabrique de données.
   
   ![Vignette Créer et déployer](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
2. Cliquez sur **Nouveau pipeline** dans la barre de commandes. Si le bouton ne s’affiche pas, cliquez sur **... (points de suspension)** pour développer la barre de commandes.
3. Remplacez le code JSON du volet droit par le texte suivant : 
   
           {
               "name": "ADFTutorialPipelineOnPrem",
               "properties": {
               "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
               "activities": [
               {
                   "name": "CopyFromSQLtoBlob",
                   "description": "Copy data from on-prem SQL server to blob",
                   "type": "Copy",
                   "inputs": [
                   {
                       "name": "EmpOnPremSQLTable"
                   }
                   ],
                   "outputs": [
                   {
                       "name": "OutputBlobTable"
                     }
                   ],
                   "typeProperties": {
                     "source": {
                       "type": "SqlSource",
                       "sqlReaderQuery": "select * from emp"
                     },
                     "sink": {
                       "type": "BlobSink"
                     }
                   },
                   "Policy": {
                     "concurrency": 1,
                     "executionPriorityOrder": "NewestFirst",
                     "style": "StartOfInterval",
                     "retry": 0,
                     "timeout": "01:00:00"
                   }
                 }
               ],
               "start": "2016-07-05T00:00:00Z",
               "end": "2016-07-06T00:00:00Z",
               "isPaused": false
             }
           }
   
   Notez les points suivants :
   
   * Dans la section des activités, toutes les activités ont le **type** **Copy**.
   * **L’entrée** de l’activité est définie sur **EmpOnPremSQLTable** et la **sortie** de l’activité, sur **OutputBlobTable**.
   * Dans la section **Transformation**, le paramètre **SqlSource** est spécifié en tant que **type de source** et **BlobSink** en tant que **type de récepteur**.
   * La requête SQL `select * from emp` est spécifiée pour la propriété **sqlReaderQuery** de **SqlSource**.
   
   Remplacez la valeur de la propriété **start** par le jour actuel et la valeur **end** par le jour suivant. Les dates/heures de début et de fin doivent toutes deux être au [format ISO](http://en.wikipedia.org/wiki/ISO_8601). Par exemple : 2014-10-14T16:32:41Z. L’heure de fin ( **end** ) est facultative, mais nous allons l’utiliser dans ce didacticiel. 
   
   Si vous ne spécifiez aucune valeur pour la propriété **end**, cette dernière est calculée comme suit : « **start + 48 heures** ». Pour exécuter le pipeline indéfiniment, spécifiez **9/9/9999** comme valeur pour la propriété **end**. 
   
   En spécifiant la période active pour un pipeline, vous définissez la durée pendant laquelle les tranches de données seront traitées, selon les propriétés de **disponibilité** qui ont été définies pour chaque table Azure Data Factory.
   
   Dans l’exemple ci-dessus, il existe 24 tranches de données, car une tranche est générée par heure.  
4. Cliquez sur l’option **Déployer** de la barre de commandes pour déployer le jeu de données (la table est un jeu de données rectangulaire). Vérifiez que le message **PIPELINE DÉPLOYÉ AVEC SUCCÈS**s’affiche dans la barre de titre.  
5. Maintenant, fermez le panneau **Éditeur** en cliquant sur **X**. Cliquez de nouveau sur **X** pour fermer le panneau ADFTutorialDataFactory avec la vue d’arborescence et la barre d’outils. Si le message **Vos modifications non enregistrées seront ignorées** s’affiche, cliquez sur **OK**.
6. Vous devez être revenu au panneau **DATA FACTORY** pour **ADFTutorialOnPremDF**.

**Félicitations !**  Vous avez correctement créé une fabrique de données Azure, les services liés, les tables et un pipeline et avez planifié le pipeline.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Afficher une vue schématique d'une fabrique de données
1. Dans le **portail Azure**, cliquez sur la vignette **Diagramme** sur la page d’accueil de la fabrique de données **ADFTutorialOnPremDF**. :
   
    ![Lien Diagramme](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Le diagramme devrait ressembler à l’image suivante :
   
    ![Vue de diagramme](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)
   
    Vous pouvez faire un zoom avant, un zoom arrière, un zoom à 100 %, un zoom pour ajuster, positionner automatiquement les pipelines et les tables, et afficher les informations de lignage (mise en surbrillance des éléments en amont et en aval des éléments sélectionnés).  Vous pouvez double-cliquer sur un objet (table ou pipeline d’entrée/de sortie) pour afficher ses propriétés. 

## <a name="monitor-pipeline"></a>Surveillance d’un pipeline
Dans cette étape, vous utilisez le portail Azure pour surveiller ce qui se passe dans une fabrique de données Azure. Vous pouvez également utiliser les applets de commande PowerShell pour surveiller les jeux de données et les pipelines. Pour plus de détails sur la surveillance, consultez [Surveillance et gestion des pipelines](data-factory-monitor-manage-pipelines.md).

1. Ouvrez le **portail Azure** (si vous l’avez fermé)
2. Si le panneau **ADFTutorialOnPremDF** n’est pas ouvert, ouvrez-le en cliquant sur **ADFTutorialOnPremDF** dans le **tableau d’accueil**.
3. Vous devez voir le **nombre** et les **noms** des tables et du pipeline que vous avez créés sur ce panneau.
   
    ![Page d’accueil Data Factory](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)
4. Cliquez maintenant sur la vignette **Jeux de données** .
5. Dans le panneau **Jeux de données**, cliquez sur **EmpOnPremSQLTable**.
   
    ![Tranches EmpOnPremSQLTable](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
6. Notez que les tranches de données jusqu’à l’heure actuelle ont déjà été produites et ont l’état **Prêt**. Cela est dû au fait que vous avez inséré les données dans la base de données SQL Server et qu’elles y sont tout le temps. Vérifiez qu’aucune tranche n’apparaît dans la section **Tranches problématiques** , sur la partie inférieure de la fenêtre.
   
    Les listes **Tranches récemment mises à jour** et **Tranches ayant échoué récemment** sont triées en fonction de **l’HEURE DE LA DERNIÈRE MISE À JOUR**. L’heure de mise à jour d’une tranche est modifiée dans les situations suivantes.
   
    Vous mettez à jour l’état de la tranche manuellement, par exemple à l’aide de **Set-AzureRmDataFactorySliceStatus** (ou) en cliquant sur **EXÉCUTER** dans le panneau **TRANCHE** de la tranche.
   
    La tranche change d’état en raison d’une exécution (par exemple, une exécution a démarré, une exécution s’est terminée et a échoué, une exécution s’est terminée et a réussi, etc.).
   
    Cliquez sur le titre des listes ou sur **... (points de suspension)** pour consulter la liste complète des tranches. Cliquez sur l’option **Filtre** de la barre d’outils pour filtrer les tranches.
   
    Pour afficher les tranches de données triées en fonction des heures de début et de fin de tranche, cliquez sur la vignette **Tranches de données (par heure de tranche)** .
7. Dans le panneau **Jeux de données**, cliquez sur **OutputBlobTable**.
   
    ![Tranches OputputBlobTable](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
8. Assurez-vous de voir que les tranches jusqu’à l’heure actuelle sont produites et à l’état **Prêt**. Attendez que les états des tranches jusqu’à l’heure actuelle soient définis sur **Prêt**.
9. Cliquez sur une tranche de données dans la liste pour voir le panneau **TRANCHE DE DONNÉES** .
   
    ![Panneau Tranche de données](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)
   
    Si la tranche n’a pas l’état **Prêt**, vous pouvez voir les tranches en amont qui ne sont pas prêtes et qui empêchent l’exécution de la tranche actuelle dans la liste **Tranches en amont qui ne sont pas prêtes**.
10. Cliquez sur **l’exécution d’activité** dans la liste de la partie inférieure de la fenêtre pour afficher les **détails sur l’exécution d’activité**.
    
    ![Panneau Détails sur l’exécution d’activité](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)
11. Cliquez sur **X** pour fermer tous les panneaux jusqu’à ce que vous 
12. reveniez au panneau d’accueil de l’élément **ADFTutorialOnPremDF**.
13. (facultatif) Cliquez sur **Pipelines**, sur **ADFTutorialOnPremDF**, puis accédez aux tables d’entrée (**Consommé**) ou aux tables de sortie (**Produit**).
14. Utilisez des outils tels que l’ **Explorateur de stockage Azure** pour contrôler la sortie.
    
    ![Explorateur de stockage Azure](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Étapes suivantes
* Consultez l’article [Data Management Gateway](data-factory-data-management-gateway.md) (Passerelle de gestion des données) pour obtenir des informations détaillées sur la passerelle de gestion des données.
* Consultez l’article [Copie de données d’un objet blob vers Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pour en savoir plus sur l’utilisation de l’activité de copie pour déplacer des données d’une banque de données source vers une banque de données récepteur. 

<!--HONumber=Oct16_HO2-->


