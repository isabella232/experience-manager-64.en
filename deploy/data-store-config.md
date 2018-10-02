---
uuid: d8fa0ade-c28e-481c-bba2-bc93656f0bb7
index: y
internal: n
snippet: y
translate: y
---

# Configuring node stores and data stores in AEM 6

---

## Introduction

In Adobe Experience Manager (AEM), binary data can be stored independently from the content nodes. The binary data is stored in a data store, whereas content nodes are stored in a node store.

Both data stores and node stores can be configured using OSGi configuration. Each OSGi configuration is referenced using a persistent identifier (PID).

---

## Configuration steps

To configure both the node store and the data store, perform these steps:

1. Copy the AEM quickstart JAR file to its installation directory.

1. Create a folder `crx-quickstart/install` in the installation directory.

1. First, configure the node store by creating a configuration file with the name of the node store option you want to use in the `crx-quickstart/install` directory.

   For example, the Document node store (which is the basis for AEM's MongoMK implementation) uses the file `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`.

1. Edit the file, and set your configuration options.

1. Create a configuration file with the PID of the data store you want to use. Edit the file to set the configuration options. 

   >[!NOTE]
   >
   ><p>See <a href="#NodeStoreConfigurations">Node Store Configurations</a> and <a href="#DataStoreConfigurations">Data Store Configurations</a> for configuration options.<br> </p> 

1. Start AEM.

---

## Node Store Configurations

>[!CAUTION]
>
><p>Newer versions of Oak employ a new naming scheme and format for OSGi configuration files. The new naming scheme requires that the configuration file be named <strong>.config</strong> and the new format requires values to be typed and is <a href="https://sling.apache.org/documentation/development/slingstart.html#default-configuration-format">documented here</a>.</p> <p>If you upgrade from an older version of Oak, ensure that you make a backup of the <span class="code">crx-quickstart/install </span>folder first. After the upgrade, restore the contents of the folder to the upgraded installation and modify the extension of the configuration files from <strong>.cfg</strong> to <strong>.config</strong>.</p> <p>In case you are reading this article in preparation for an upgrade from an <strong>AEM 5.x</strong> installation, ensure that you consult the <a href="https://docs.adobe.com/content/docs/en/aem/6-0/deploy/upgrade.html">upgrade</a> documentation first.</p>

#### Segment Node Store

The segment node store is the basis of Adobe's TarMK implementation in AEM6. It uses the `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService` PID for configuration.

>[!CAUTION]
>
><p>The PID for the Segment node store has changed from&nbsp;<span class="code">org.apache.jackrabbit.oak.<b>plugins</b>.segment.SegmentNodeStoreService in previous versions</span> of AEM 6 to&nbsp;<span class="code">org.apache.jackrabbit.oak.segment.SegmentNodeStoreService</span> in AEM 6.3. Make sure you make the necessary configuration adjustments to reflect this change.</p>

You can configure the following options:

* `repository.home`: Path to repository home under which repository-related data is stored. By default, segment files are stored under the `crx-quickstart/segmentstore` directory.
* `tarmk.size`: Maximum size of a segment in MB. The default maximum is 256MB.
* `customBlobStore`: Boolean value indicating that a custom data store is used. The default value is true for AEM 6.3 and later versions. Prior to AEM 6.3 the default was false.

The following is a sample `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config` file:

```shell
#Path to repo
repository.home="crx-quickstart/repository"

#Max segment size
tarmk.size=I"256"

#Custom data store
customBlobStore=B"true"
```

#### Document Node Store

The document node store is the basis of AEM's MongoMK implementation. It uses the `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService`* *PID. The following configuration options are available:

* `mongouri`: The [MongoURI](http://docs.mongodb.org/manual/reference/connection-string/) required to connect to Mongo Database. The default is `mongodb://localhost:27017`
* `db`: Name of the Mongo database. The default is **Oak** ``. However, new AEM 6 installations use **aem-author** ``as the default database name.
* `cache`: The cache size in MB. This is distributed among various caches used in DocumentNodeStore. The default is `256`
* `changesSize`: Size in MB of capped collection used in Mongo for caching the diff output. The default is `256`
* `customBlobStore`: Boolean value indicating that a custom data store will be used. The default is `false`.

The following is a sample `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config` file:

```shell
#Mongo server details
mongouri="mongodb://localhost:27017"

#Name of Mongo database to use
db="aem-author"

#Store binaries in custom BlobStore
customBlobStore=B"false"
```

---

## Data Store Configurations

>[!CAUTION]
>
><p>In AEM, online compaction is paused by default. However, a known issue with AEM where an incomplete overridden configuration will enable compaction. To workaround this issue always specify&nbsp;pauseCompaction=B&quot;true&quot; for every overridden configuration.</p> <p>&nbsp;</p>

When dealing with large number of binaries, it is recommended that an external data store be used instead of the default node stores in order to maximize performance.

For example, if your project requires a large number of media assets, storing them under the File or S3 Data Store will make accessing them faster than storing them directly inside a MongoDB.

The File Data Store provides better performance than MongoDB, and Mongo backup and restore operations are also slower with large number of assets.

Details on the different data stores and configurations are described below.

>[!NOTE]
>
><p>In order to enable custom Data Stores, you need to make sure that <span class="code">customBlobStore</span> is set to <span class="code">true</span> in the respective Node Store configuration file (<a href="/content/help/en/experience-manager/6-4/sites/deploying/using/data-store-config.html#main-pars_title_1">segment node store</a> or <a href="/content/help/en/experience-manager/6-4/sites/deploying/using/data-store-config.html#main-pars_title_4">document node store</a>).</p>

#### Oak File Blobstore

The Oak File Blobstore uses the `org.apache.jackrabbit.oak.spi.blob.FileBlobStore` PID for configuration.

These options are available:

* `repository.home`: Path to repository home under which the binary data is stored. Blob files would be stored under * `crx-quickstart/datastore`* directory.
* `blockSize`: Binary content is broken in chunks and stored on file system. This value allows to configure the size of the chunks. The default value in bytes is `2097152` (2MB).
* `blockSizeMin`: The minimum size of the binary chunks in bytes. Content less than this value ``would be inlined. The default value is `4096`.

#### File Data Store

This is the implementation of [FileDataStore](http://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/core/data/FileDataStore.html) present in Jackrabbit 2. It provides a way to store the binary data as normal files on the file system. It uses the `org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore` PID.

These configuration options are available:

* `repository.home`: Path to repository home under which various repository related data is stored. By default, binary files would be stored under `crx-quickstart/repository/datastore` directory
* `path`: Path to the directory under which the files would be stored. If specified then it takes precedence over `repository.home` value
* `minRecordLength`: The minimum size in bytes of a file stored in the data store. Binary content less than this value would be inlined.

>[!NOTE]
>
><p>The file data store makes use of memory mapped files for fast I/O operation when persisting data to disk. Make sure you use this data store for performance sensitive AEM applications.<br> </p> 

>[!NOTE]
>
><p>When using a NAS to store shared file data stores, make sure you use only high performing devices in order to avoid performance issues.</p>

---

## Amazon S3 Data Store

AEM can be configured to store data in Amazon's Simple Storage Service (S3). It uses the `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` PID for configuration.

In order to enable the S3 data store functionality, a feature pack containing the S3 Datastore Connector needs to be downloaded and installed. Go to the [Adobe Repository](https://repo.adobe.com/nexus/content/groups/public/com/adobe/granite/com.adobe.granite.oak.s3connector/ ) and download the latest version from the 1.8.x versions of the feature pack (for example, com.adobe.granite.oak.s3connector-1.8.0.zip).

Aditionally, you also need to download and install the latest Oak hotfix as listed on the [AEM 6.3 Oak Cumulative Fix Pack](/content/help/en/experience-manager/kb/aem63-available-hotfixes/OakCumulativeFixPack) page.

>[!NOTE]
>
><p>When using AEM 6.4 with TarMK, binaries will be stored by default in the <span class="code">FileDataStore</span>. To use TarMK with the S3 Datastore, you need to start AEM using the&nbsp;<span class="code">crx3tar-nofds</span> runmode, for example:</p>

```shell
java -jar aem6.4.jar -r crx3tar-nofds
```

Once downloaded, you can install and configure the S3 Connector as follows:

1. Extract the contents of the feature pack zip file to a temporary folder. 

1. Go to the temporary folder and navigate to the following location:

   ```xml
   jcr_root/libs/system/install
   ```

   Copy all the contents from the above location to `**<aem-install>**/crx-quickstart/install.`

1. If AEM is already configured to work with the Tar or MongoDB storage, remove any existing configuration files from the ***&lt;aem-install&gt;***/*crx-quickstart*/*install* folder before proceeding. The files that need to be removed are:

    * `For MongoMK: org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`    
    * `For TarMK: org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`

1. Return to the temporary location where the feature pack has been extracted, and copy the contents of the following folder:

    * `jcr_root/libs/system/config`

   to

    * `**<aem-install>**/crx-quickstart/install`

   Make sure you only copy the configuration files needed by your current configuration. For both a dedicated data store and a shared data store setup copy the `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` file.

   >[!NOTE]
   >
   ><p>In a cluster setup, perform above steps on all nodes of cluster one by one. Also, make sure to use same S3 settings for all nodes.</p> 

1. Edit the file and add the configuration options required by your setup.

1. Start AEM.

>[!NOTE]
>
><p>The S3 connector package includes the <span class="code">oak-blob-cloud</span> bundle. As a general rule, the Oak version used should be greater than the version of the <span class="code">oak-blob-cloud</span> bundle. For example, for S3 connector version 1.4.2 (that contains oak-blob-cloud version 1.4.4), Oak 1.4.4 or higher should be used. Make sure to install the latest S3 connector package as it also includes the necessary Oak version.</p> <p>&nbsp;</p>

#### Upgrading to a new version of the 1.8.x S3 Connector

If you need to upgrade to a new version of the 1.8.x S3 connector (for example, from 1.8.0 to 1.8.1) follow these steps:

1. Stop the AEM instance. 

1. Navigate to `**<aem-install>**/crx-quickstart/install/15` in the AEM installation folder and make a backup of its contents.

1. After the backup, delete the old version of the S3 Connector and its dependencies by deleting all the jar files in the `**<aem-install>**/crx-quickstart/install/15` folder, for example:

    * **oak-blob-cloud-1.6.1.jar** 
    * **aws-java-sdk-osgi-1.10.76.jar**

   >[!NOTE]
   >
   ><p>The file names presented above are used for illustration purposes only and are not definitive.</p> 

1. Download the latest version of the 1.8.x feature pack from the [Adobe Repository](https://repo.adobe.com/nexus/content/groups/public/com/adobe/granite/com.adobe.granite.oak.s3connector/).

1. Unzip the contents to a separate folder, then navigate to `jcr_root/libs/system/install/15`.

1. Copy the jar files to **&lt;aem-install&gt;**/crx-quickstart/install/15 in the AEM installation folder. 

1. Start AEM and check the connector functionality.

You can use the configuration file with the following options:

* accessKey: The AWS access key.
* secretKey: The AWS secret access key.
* s3Bucket: The bucket name.
* s3Region: The bucket region.
* path: The path of the data store. The default is **&lt;AEM install folder&gt;/repository/datastore**
* minRecordLength: The minimum size of an object that should be stored in the data store. The minimum/default is **16KB.**
* maxCachedBinarySize: Binaries with size less than or equal to this size will be stored in memory cache. The size is in bytes. The default is **17408 **(17 KB).
* cacheSize: The size of the cache. The value is specified in bytes. The default is **64GB**.
* secret: Only to be used if using binaryless replication for shared datastore setup.
* stagingSplitPercentage: The percentage of cache size configured to be used for staging asynchronous uploads. The default value is **10**.
* uploadThreads: The number of uploads threads that are used for asynchronous uploads. The default value is **10**.
* stagingPurgeInterval: The interval in seconds for purging finished uploads from the staging cache. The default value is **300** seconds (5 minutes).
* stagingRetryInterval: The retry interval in seconds for failed uploads. The default value is **600** seconds (10 minutes).

>[!NOTE]
>
><p>The same configuration options are available for the shared S3 data store configuration file under the name of <span class="code">org.apache.jackrabbit.oak.plugins.blob.datastore.SharedS3DataStore.config</span>. For more information, see Configuring a <a href="/content/help/en/experience-manager/6-4/sites/deploying/using/data-store-config.html#main-pars_title_816869532">Shared Data Store</a>.<br> </p>

#### Bucket region options

| US Standard | `us-standard` |
|---|---|
| US West | `us-west-2` |
| US West (Northern California) | `us-west-1` |
| EU (Ireland)  | `EU` |
| Asia Pacific (Singapore)  | `ap-southeast-1` |
| Asia Pacific (Sydney)  | `ap-southeast-2` |
| Asia Pacific (Tokyo) | `ap-northeast-1` |
| South America (Sao Paolo)  | `sa-east-1` |

#### Migrating from CRX2 to CRX3 with S3 Datastore

1. Unpack the AEM 6.3 quickstart:

   ```shell
   java --Xmx4096m -jar aem-quickstart-6.3.0.jar -unpack
   ```

1. Download the latest version of the 1.6.x feature pack from the [Adobe Repository](https://repo.adobe.com/nexus/content/groups/public/com/adobe/granite/com.adobe.granite.oak.s3connector/).

1. Extract the contents of the feature pack zip file to a temporary folder.

1. Go to the temporary folder and navigate to the following location:

   ```
   jcr_root/libs/system/install
   ```

   Copy all the contents from the above location to `**<aem-install>**/crx-quickstart/install.`

1. If AEM is already configured to work with the Tar or MongoDB storage, remove any existing configuration files from the `**<aem-install>**/crx-quickstart/install` folder before proceeding. The files that need to be removed are:

    * For TarMK: `org.apache.jackrabbit.oak.document.DocumentNodeStoreService.config`    
    * For MongoMK: `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config from crx-quickstart/install`

1. Return to the temporary location where the feature pack has been extracted, and copy the contents of the following folder:

    * `jcr_root/libs/system/config`

   to

    * `**<aem-install>**/crx-quickstart/install`

   Make sure you only copy the configuration files needed by your current configuration. See the details below:

    * If a dedicated data store setup copy the `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` file.    
    * If a shared data store setup copy the `org.apache.jackrabbit.oak.plugins.blob.datastore.SharedS3DataStore.config` file.

1. Rename all files with the extension . `config`. `sample` to have the extension . `config`.

1. Refer to the entries in `org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` and populate their values from **repository.xml** or **aws.properties** files.

1. You can perform the migration differently, depending on the persistence type you want to choose as destination. Below is a list of all possible combinations.

   **For migrating to TarMK with an S3 Data Store:**

   ```shell
   java -Xmx4096m -jar aem-quickstart-6.3.0.jar -v -x crx2oak -xargs -- --load-profile segment-custom-ds
   ```

   **For migrating to MongoMK with an S3 Data Store:**

   ```shell
   java -Xmx4096m -jar aem-quickstart-6.3.0.jar -v -x crx2oak -xargs -- --load-profile mongo-custom-ds -T mongo-uri=mongo://mongo-host:mongo-port -T mongo-db=mongo-database-name
   ```

   Where:

    * **mongo-host **is the MongoDB server IP (for example: *127.0.0.1*)    
    * **mongo-port **is the MongoDB server port (for example:** ***27017*)    
    * **mongo-database-name** is the Mongo database name (for example: *aem-author*)

   In this step, a helper utility `crx2oak-quickstart-extension.jar` is called, which generates the file `crx2oak.properties` under `crx-quickstart/crx2oak/`.

   Review the newly-generated file, and edit its properties if you require different values to suit your migration requirements.

1. Run AEM and verify that everything works as expected.

#### The Amazon S3 Connector Features

**Local Cache**

After installing the connector for the first time, all the data will be uploaded from the local datastore. After this first run, the local datastore will be used as a local cache. All operations (with the exception of writes) will first check for the item in the local cache and if no record is found, it will be accessed from S3.

The local cache has a size limit, specifiable by the `cacheSize` parameter. When the size of the cache exceeds the limit, it will automatically undergo purging in order to clear older items and reclaim space. During purging, the local cache makes sure that it doesn’t delete any in-progress asynchronous uploads.

Things to note about the local cache mechanism:

1. It can be disabled by setting the `cacheSize` parameter to **0**. In this case, all operations will be performed directly in the S3 cloud and the local cache completely ignored.
1. If the size of a file exceeds the size of the local cache, it will be served directly from S3.
1. When the cache is being purged, it will not be available to the S3 data store. Files from the local cache that have pending uploads will still be available.
1. Files deleted from the S3 data store will also be deleted from the local cache.

**Multi-Threaded Content migration from FileSystem DataStore to S3**

Multi-threading can be configured in order to speed up file operations to or from the S3 data store. This can be particularly useful for initial migrations from a local datastore where large amounts of data need to be uploaded.

**Asynchronous Upload to S3**

The `asyncUploadLimit` parameter limits the number of asynchronous uploads to the S3 data store. Once this limit is reached, the next upload will be synchronous until one of asynchronous uploads completes. To disable this feature the `asyncUploadLimit` parameter can be set to **0**. The default value is **100**.

**Asynchronous Upload Cache**

The connector also uses a upload cache for asynchronous uploads. It tracks their status and removes finished uploads or adds new ones to the cache when necessary.

**DataStore Caching**

>[!NOTE]
>
><p>The DataStore implementations of <span class="code">S3DataStore</span>, <span class="code">CachingFileDataStore</span>&nbsp;and&nbsp;<span class="code">AzureDataStore</span>&nbsp;support local file system caching. The&nbsp;<span class="code">CachingFileDataStore</span> implementation is useful when the DataStore is on NFS (Network File System).&nbsp;</p> <p>&nbsp;</p>

When upgrading from an older cache implementation (pre Oak 1.6) there is a difference in the structure of the local file system cache directory. In the old cache structure both the downloaded and the uploaded files were put directly under the cache path. The new structure segregates the downloads and uploads and stores them in two directories named `upload` and `download` under cache path. The upgrade process should be seamless and any pending uploads should be scheduled for upload and any previously downloaded files in the cache will be put in the cache on initialization.

You can also upgrade the cache offline by using the `datastorecacheupgrade` command of oak-run. For details on how to execute the command, check the [readme](https://svn.apache.org/repos/asf/jackrabbit/oak/trunk/oak-run/README.md) for the oak-run module.

The cache has a size limit and it can be configured by using the cacheSize parameter.

**Downloads**

The local cache will be checked for the record of the requested file/blob before accessing it from the DataStore. When the cache exceeds the configured limit (see the `cacheSize` parameter) while adding a file into the cache, then some of the file(s) will be evicted to reclaim space.

**Asynchronous Upload**

The cache supports asynchronous uploads to the DataStore. The files are staged locally, in the cache (on the file system), and an asynchronous job starts to upload the file. The number of asynchronous uploads is limited by the size of the staging cache. The size of the staging cache is configured by using the `stagingSplitPercentage` parameter. This parameter defines the percentage of cache size to be used for the staging cache. Also, the percentage of cache available for downloads is calculated as **(100 - `stagingSplitPercentage`) * `cacheSize`**.

The asynchronous uploads are multi-threaded and the number of threads is configured by using the `uploadThreads` parameter.

The files are moved to the main download cache after the uploads are complete. When the staging cache size exceeds its limit, the files are uploaded synchronously to the DataStore until the previous asynchronous uploads are complete and space is again available in the staging cache. The uploaded files are removed from the staging area by a periodic job whose interval is configured by the `stagingPurgeInterval` parameter.

Failed uploads (for example, because of a network disruption) are put on a retry queue and retried periodically. The retry interval is configured by using the `stagingRetryInterval parameter`.

#### Configuring binaryless replication with Amazon S3

In order to configure binaryless replication with S3, the following steps are required:

1. Install the author and publish instances and make sure they are started properly.

1. Go to the replication agent settings, by opening a page to *http://localhost:4502/etc/replication/agents.author/publish.html*.

1. Press the **Edit** button in the **Settings** section.

1. Change the **Serialization** type option to **Binary less**. 

1. Add the parameter " `binaryless`= `true`" in the transport uri. After the change, the uri should look similar to the following:

   *http://localhost:4503/bin/receive?sling:authRequestLogin=1&binaryless=true*

1. Restart all author and publish instances to let the changes take effect.

#### Creating a cluster using S3 and MongoDB

1. Unpack CQ quickstart using the following command:

   `java -jar cq-quickstart.jar -unpack`

1. After AEM has been unpacked, create a folder inside the installation directory *crx-quickstart*/*install*. 

1. Create these two files inside the `crx-quickstart` folder:

    * *org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService*.*config* 
    * *org.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore*.*config*

   After the files have been created, add the configuration options as needed.

1. Install the two bundles required for the S3 data store as explained above.

1. Make sure MongoDB is installed and an instance of `mongod` is running.

1. Start AEM with the following command:

   `java -Xmx1024m -XX:MaxPermSize=256M -jar cq-quickstart.jar -r crx3,crx3mongo`

1. Repeat steps 1 through 4 for the second AEM instance.

1. Start the second AEM instance.

#### Configuring a Shared Data Store

1. First, create the data store configuration file on each instances that is required to share the data store:

    * If you are using a `FileDataStore`, create a file named `org.apache.jackrabbit.oak.plugins.blob.datastore.FileDataStore.config` and place it in the `<aem-install>/crx-quickstart/install` folder.    
    * If using S3 as the data store, create a file named o `rg.apache.jackrabbit.oak.plugins.blob.datastore.S3DataStore.config` in the `<aem-install>/crx-quickstart/install` folder as above.

1. Modify the data store configuration files on each instance to point to the same data store. For more information, see [this article](data-store-config.md#main-pars_title_5).

1. If the instance has been cloned from an existing server, you need to remove the `clusterId` of the new instance by using the latest oak-run tool while the repository is offline. The command you need to run is:

   ```xml
   java -jar oak-run.jar resetclusterid < repository path | Mongo URI >
   ```

   >[!NOTE]
   >
   ><p>If a Segment node store is configured then the repository path needs to be specified. By default, the path is <span class="code">&lt;aem-install-folder&gt;/crx-quickstart/repository/segmentstore.</span> If a&nbsp; Document node store is configured you can use a <a href="https://docs.mongodb.org/manual/reference/connection-string/" >Mongo Connection String URI</a>.</p> 

   >[!NOTE]
   >
   ><p>The Oak-run tool can be downloaded from this location:</p> <p><a href="http://mvnrepository.com/artifact/org.apache.jackrabbit/oak-run/">http://mvnrepository.com/artifact/org.apache.jackrabbit/oak-run/</a></p> <p>Be aware that different versions of the tool need to be used depending on the Oak version you use with your AEM installation. Please check the version requirements list below before using the tool:</p> <ul> <li>For Oak versions <b>1.2.x</b> use the Oak-run <b>1.2.12 or newer</b><br> </li> <li>For Oak versions <b>newer than the above</b>, use the version of Oak-run that matches the Oak core of your AEM installation.</li> </ul> 

1. Lastly, validate the configuration. In order to do this, you need to look for a unique file added to the data store by each repository that is sharing it. The format of the files is `repository-[UUID]`, where the UUID is a unique identifier of each individual repository.

   Therefore, a proper configuration should have as many unique files as there are repositories sharing the data store.

   The files are stored differently, depending on the data store:

    * For the `FileDataStore` the files are created under the root path of the data store folder.    
    * For the `S3DataStore` the files are created in the configured S3 bucket under the `META` folder.

>[!NOTE]
>
><p>The shared data store feature is available with Oak version <b>1.2.12</b><br> </p> 

>[!NOTE]
>
><p>If you are planning to share an S3 data store between repositories, make sure you only use the <span class="code">org.apache.jackrabbit.oak.plugins.blob.datastore.SharedS3DataStore.config</span> file to configure all your instances. Otherwise, data loss might occur during garbage collection operations.</p>

---

## Azure Data Store

AEM can be configured to store data in Microsoft's Azure storage service. It uses the `org.apache.jackrabbit.oak.plugins.blob.datastore.AzureDataStore.config` PID for configuration.

In order to enable the Azure data store functionality, a feature pack containing the Azure Connector needs to be downloaded and installed. Go to the [Adobe Repository](https://repo.adobe.com/nexus/content/groups/public/com/adobe/granite/com.adobe.granite.oak.azureblobconnector/) and download the latest version from the 1.6.x versions of the feature pack (for example, com.adobe.granite.oak.azureblobconnector-1.6.3.zip).

>[!NOTE]
>
><p>When using AEM 6.4 with TarMK, binaries will be stored by default in the&nbsp;FileDataStore. To use TarMK with the Azure DataStore, you need to start AEM using the&nbsp;<span class="code">crx3tar-nofds</span>&nbsp;runmode, for example:</p>

```shell
java -jar aem6.4.jar -r crx3tar-nofds
```

Once downloaded, you can install and configure the Azure connector as follows:

1. Extract the contents of the feature pack zip file to a temporary folder. 

1. Go to the temporary folder and copy the contents of `jcr_root/libs/system/install` to the `<aem-install>crx-quickstart/install` folder.

1. If AEM is already configured to work with the Tar or MongoDB storage, remove any existing configuration files from the `/crx-quickstart/install` folder before proceeding. The files that need to be removed are:

   ForMongoMK:

   `org.apache.jackrabbit.oak.plugins.document.DocumentNodeStoreService.config`

   For TarMK:

   `org.apache.jackrabbit.oak.segment.SegmentNodeStoreService.config`

1. Return to the temporary location where the feature pack has been extracted and copy the contents of `jcr_root/libs/system/config` to the `<aem-install>/crx-quickstart/install` folder.

1. Edit the configuration file and add the configuration options required by your setup.

1. Start AEM.

You can use the configuration file with the following options:

* azureSas="": In version 1.6.3 of the connector, Azure Shared Access Signature (SAS) support was added. **If both SAS and storage credentials exists in the configuration file, SAS has priority.** For more information about SAS see the [official documentation](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1). Ensure that the '=' character is escaped like '\='.
* azureBlobEndpoint="": The Azure Blob Endpoint. For example, https://&lt;storage-account&gt;.blob.core.windows.net.
* accessKey="": The storage account name. For more details about the Microsoft Azure authentication credentials, see the [official documentation](https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account).
* secretKey="": The storage access key. Ensure that the '=' character is escaped like '\='.
* container="": The Microsoft Azure blob storage container name. The container is a grouping of a set of blobs. For additional details, read the [official documentation](https://msdn.microsoft.com/en-us/library/dd135715.aspx).
* maxConnections="": The concurrent number of simultaneous requests per operation. The default value is 1.
* maxErrorRetry="": Number of retries per request. The default value is 3.
* socketTimeout="": The timeout interval, in milliseconds, used for the request. The default value is 5 minutes.

Besides the settings above, the following settings can also be configured:

* path: The path of the data store. The default is `<aem-install>/repository/datastore.`
* RecordLength: The minimum size of an object that should be stored in the data store. The default is 16KB.
* maxCachedBinarySize: Binaries with size less than or equal to this size will be stored in memory cache. The size is in bytes. The default is 17408 (17 KB).
* cacheSize: The size of the cache. The value is specified in bytes. The default is 64GB.
* secret: Only to be used if using binaryless replication for shared datastore setup.
* stagingSplitPercentage: The percentage of cache size configured to be used for staging asynchronous uploads. The default value is 10.
* uploadThreads: The number of uploads threads that are used for asynchronous uploads. The default value is 10.
* stagingPurgeInterval: The interval in seconds for purging finished uploads from the staging cache. The default value is 300 seconds (5 minutes).
* stagingRetryInterval: The retry interval in seconds for failed uploads. The default value is 600 seconds (10 minutes).

>[!NOTE]
>
><p>&nbsp;All settings should be put between quotes, for example:</p>

```shell
accessKey="ASDASDERFAERAER"
secretKey="28932hfjlkwdo8fufsdfas\=\="
```

---

## Data store garbage collection

>[!NOTE]
>
><p>This feature is only available in AEM 6.1 running on Apache Oak versions 1.2.x and higher.<br> </p>

The data store garbage collection process is used to remove any unused files in the data store, thus freeing up valuable disk space in the process.

You can run data store garbage collection by:

1. Going to the JMX console located at *http://&lt;serveraddress:port&gt;/system/console/jmx*

1. Searching for **RepositoryManagement.** Once you find the Repository Manager MBean, click it to bring up the available options.

1. Scroll to the end of the page, and click the **startDataStoreGC(boolean markOnly)** link.

1. In the following dialogue, enter `**false**` for the `markOnly` parameter, then click **Invoke**:
   ![](assets/chlimage_1.png) 
   >[!NOTE]
   >
   ><p>The <span class="code">markOnly</span> paramater signifies whether the sweep phase of garbage collection will run or not.</p>

---

## Data Store Garbage Collection for a Shared Data Store

>[!NOTE]
>
><p>When performing garbage collection in a clustered or shared data store setup (with Mongo or Segment Tar) the log might display warnings about the inability to delete certain blob IDs. This happens because blob IDs deleted in a previous garbage collection are incorrectly referenced again&nbsp;by other cluster or shared nodes which do not have information about the ID deletions. As a result, when garbage collection is performed it logs a warning when it tries to delete an ID that has already been deleted in the last run. This behaviour does not affect performance or functionality.&nbsp;</p>

With newer versions of AEM, data store garbage collection can also be run on data stores shared by more than one repository. In order to be able to run data store garbage collection on a shared data store, take the following steps:

1. Make sure that any maintenance tasks configured for the data store garbage collection are disabled on all repository instances sharing the data store.

1. Run the steps mentioned in [Binary Garbage Collection](data-store-config.md#main-pars_title_81065249) individually on **all** repository instances sharing the data store. However, make sure to enter `**true**` for the `markOnly` parameter before clicking the Invoke button:
   ![](assets/chlimage_1.png)
1. After completing the above procedure on all instances, run the data store garbage collect again from **any** of the instances:

    1. Go to the JMX console and select the Repository Manager Mbean.    
    1. Click on the **Click startDataStoreGC(boolean markOnly)** link.    
    1. In the following dialogue, enter `**false**` for the `markOnly` parameter again.

   This will collate all the files found using the mark phase used before and delete the rest that are unused from the data store.
