---
title: Application Manager Client JavaAPI Quick Start(SOAP)
seo-title: Application Manager Client JavaAPI Quick Start(SOAP)
description: null
seo-description: null
uuid: cf2802e8-313c-44dd-90e4-748bf895f116
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: develop
discoiquuid: 3a95c586-cb55-4a39-8b38-6f58003f9b09
index: y
internal: n
snippet: y
---

# Application Manager Client JavaAPI Quick Start(SOAP){#application-manager-client-javaapi-quick-start-soap}

The following Java API Quick Start(SOAP) are available for the Application Manager Client.

[Quick Start (SOAP mode): Creating AEM Forms Application Version using the Java API](application-manager-client-java-api#quick_start_soap_mode_creating_application_version_using_the_java_api)

[Quick Start (SOAP mode): Exporting applications using the Java API](application-manager-client-java-api#quick_start_soap_mode_exporting_applications_using_the_java_api)

[Quick Start (SOAP mode): Importing applications using the Java API](application-manager-client-java-api#quick_start_soap_mode_importing_applications_using_the_java_api)

[Quick Start (SOAP mode): Getting a AEM Forms Application using the Java API](application-manager-client-java-api#quick_start_soap_mode_getting_a_application_using_the_java_api)

[Quick Start (SOAP mode): Getting the applications using the Java API](application-manager-client-java-api#quick_start_soap_mode_getting_the_applications_using_the_java_api)

[Quick Start (SOAP mode): Getting status of applications using Java API](application-manager-client-java-api#quick_start_soap_mode_getting_status_of_applications_using_java_api)

[Quick Start (SOAP mode):Previewing the AEM Forms and later application archive using the Java API](application-manager-client-java-api#quick_start_soap_mode_previewing_the_livecycle_es2_and_later_application_archive_using_the_java_api)

[Quick Start (SOAP mode):Deleting the AEM Forms application archive using the Java API](application-manager-client-java-api#quick_start_soap_mode_deleting_the_application_archive_using_the_java_api)

AEM Forms operations can be performed using the AEM Forms strongly-typed API and the connection mode should be set to SOAP.

***Note**: Quick start located in Programming with AEM Forms are based on the Forms Server being deployed on JBoss and the Windows operating system. However, if you are using another operating system, such as Unix, replace windows-specific paths with paths supported by the applicable operating system. Likewise, if you are using another J2EE application server, then ensure that you specify valid connection properties. (See [Setting connection properties](#unresolvedlink-lc-in-invoke-using-java-iu.xml#ws624e3cba99b79e12e69a9941333732bac8-7fd6).)*

## Quick Start (SOAP mode): Creating Application Version using the Java API {#quick-start-soap-mode-creating-application-version-using-the-java-api}

The following Java code example creates an application using the JAVA API.

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. adobe-repository-client.jar 
 * 5. activation.jar (required for SOAP mode) 
 * 6. axis.jar (required for SOAP mode) 
 * 7. commons-codec-1.3.jar (required for SOAP mode) 
 * 8. commons-collections-3.1.jar  (required for SOAP mode) 
 * 9. commons-discovery.jar (required for SOAP mode) 
 * 10. commons-logging.jar (required for SOAP mode) 
 * 11. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 12. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 13. jaxrpc.jar (required for SOAP mode) 
 * 14. log4j.jar (required for SOAP mode) 
 * 15. mail.jar (required for SOAP mode) 
 * 16. saaj.jar (required for SOAP mode) 
 * 17. wsdl4j.jar (required for SOAP mode) 
 * 18. xalan.jar (required for SOAP mode) 
 * 19. xbean.jar (required for SOAP mode) 
 * 20. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ 
package com.adobe.idp.dsc.applicationmanager; 
import java.util.Properties; 
import java.util.Random; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.repository.bindings.ResourceRepositoryDelegate; 
import com.adobe.repository.bindings.dsc.client.ResourceRepositoryClient; 
public class CreateApplicationVersion_SOAP { 
    private static String applicationFolder = "Applications"; 
    private static String defaultAppVersion = "1.0"; 
    public static void main(String[] args) { 
        // Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", 
                "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", 
                ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL); 
        connectionProps.setProperty("DSC_SERVER_TYPE", 
                ServiceClientFactoryProperties.DSC_JBOSS_SERVER_TYPE); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
        // Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory 
                .createInstance(connectionProps); 
        // Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient( 
                myFactory); 
        // Create ResourceRepositoryDelegate object 
        ResourceRepositoryDelegate repositoryClient = new ResourceRepositoryClient( 
                myFactory); 
        final Random num = new Random(); 
        String appName = "App" + num.nextInt(); 
        String newAppName = null; 
        try { 
            // Create application with default application version 
            newAppName = appClient.createApplication(appName); 
            if (repositoryClient.resourceExists("/" + applicationFolder + "/" 
                    + appName.toString() + "/" + defaultAppVersion)) { 
                System.out.println("Application with name: " + appName + "/" 
                        + defaultAppVersion + " is created succesfully!"); 
            } 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
        try { 
            // Create version of the new application 
            appName = appClient.createApplicationVersion(newAppName, 
                    defaultAppVersion, "2.0", "version increment"); 
            if (repositoryClient.resourceExists("/" + applicationFolder + "/" 
                    + appName.toString() + "/" + "2.0")) { 
                System.out.println("Application version 2.0 created : " 
                        + appName + "/" + "2.0"); 
            } 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

## Quick Start (SOAP mode): Exporting Applications using the Java API {#quick-start-soap-mode-exporting-applications-using-the-java-api}

The following Java code example exports an application using the JAVA API.

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ 
package com.adobe.idp.dsc.applicationmanager; 
import java.io.File; 
import java.io.FileInputStream; 
import java.util.ArrayList; 
import java.util.List; 
import java.util.Properties; 
import com.adobe.idp.Document; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
public class ExportLCA_SOAP { 
    public static void main(String[] args) { 
        // Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", 
                "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", 
                ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL); 
        connectionProps.setProperty("DSC_SERVER_TYPE", 
                ServiceClientFactoryProperties.DSC_JBOSS_SERVER_TYPE); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
        // Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory 
                .createInstance(connectionProps); 
        // Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient( 
                myFactory); 
        Document doc = null; 
        try { 
            final FileInputStream fileApp = new FileInputStream( 
                    "C:\\ImportSampleApp2.lca"); 
            doc = new Document(fileApp); 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
        String resourceID = null; 
        try { 
            // Import the application into the LC server 
            resourceID = appClient.importApplication(doc); 
            System.out.println("Import application with resource ID:" 
                    + resourceID + " is completed successfully!"); 
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace(); 
        } 
        final String sampleAppName = "ExportSampleApp2"; 
        try { 
            final List<String> eReqList = new ArrayList<String>(); 
            eReqList.add(resourceID); 
            // Export the application imported above 
            doc = appClient.export(eReqList, "lcaDescription"); 
            // Save into local LCA file 
            final String archiveName = "C:/" + sampleAppName + "-" + "1.0" 
                    + ".lca"; 
            final File fTemp = new File(archiveName); 
            doc.copyToFile(fTemp); 
            System.out.println("Export application completed with name: " 
                    + archiveName); 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

## Quick Start (SOAP mode): Importing Applications using the Java API {#quick-start-soap-mode-importing-applications-using-the-java-api}

The following Java code example imports an application using the JAVA API.

>[!NOTE]
>
>The Java API importApplication() replaces existing applications of the same name with newer application. To update an exsiting application, use API importApplication() in place of API updateApplication().

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ 
package com.adobe.idp.dsc.applicationmanager; 
import java.io.FileInputStream; 
import java.util.Properties; 
import com.adobe.idp.Document; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
public class ImportLCA_SOAP { 
    public static void main(String[] args) { 
        // Set connection properties required to invoke AEM FOrms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", 
                "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", 
                ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL); 
        connectionProps.setProperty("DSC_SERVER_TYPE", 
                ServiceClientFactoryProperties.DSC_JBOSS_SERVER_TYPE); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
        // Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory 
                .createInstance(connectionProps); 
        // Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient( 
                myFactory); 
        Document doc = null; 
        try { 
            final FileInputStream fileApp = new FileInputStream( 
                    "C:\\ImportSampleApp2.lca"); 
            doc = new Document(fileApp); 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
        try { 
            // Import the application into the LC server 
            final String resourceID = appClient.importApplication(doc); 
            System.out.println("Import application with resource ID:" 
                    + resourceID + " is completed successfully!"); 
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

## Quick Start (SOAP mode): Getting a Application using the Java API {#quick-start-soap-mode-getting-a-application-using-the-java-api}

The following Java code example gets an application using the Java API.

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ 
package com.adobe.idp.dsc.applicationmanager; 
 
import java.io.FileInputStream; 
import java.util.Properties; 
 
import com.adobe.idp.Document; 
import com.adobe.idp.applicationmanager.application.Application; 
import com.adobe.idp.applicationmanager.application.ApplicationId; 
import com.adobe.idp.applicationmanager.application.impl.ApplicationIdImpl; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
 
public class GetApplication_SOAP { 
 
    public static void main(String[] args) { 
        //Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);          
        connectionProps.setProperty("DSC_SERVER_TYPE", "JBoss"); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
 
        //Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
        //Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient(myFactory); 
         
        Document doc = null; 
        try { 
             
            final FileInputStream fileApp = new FileInputStream("C:\\appraisal.lca"); 
            doc = new Document(fileApp); 
             
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
        final String sampleAppName = "Samples - Performance Appraisal"; 
        try { 
         
            //Import the application into the LC server 
            final String resourceID = appClient.importApplication(doc); 
            System.out.println("Import application with resource ID: " + resourceID + " is completed successfully!"); 
            //Deploy the application 
            boolean result = appClient.deployApplication(sampleAppName); 
            if (result) { 
                System.out.println("Imported application is deployed."); 
            } 
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace();     
        } 
         
        //Initialize the ApplicationId instance 
        ApplicationId appId = new ApplicationIdImpl(); 
        appId.setApplicationName(sampleAppName); 
 
        try { 
                //Get the application by application id 
                Application app = appClient.getApplication(appId); 
                System.out.println("Get application with name: " + app.getApplicationId().getApplicationName()); 
                 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

## Quick Start (SOAP mode): Getting the applications using the Java API {#quick-start-soap-mode-getting-the-applications-using-the-java-api}

The following Java code example gets the applications using the Java API.

***note**: Getting AEM Forms Application API, getApplications(), returns only deployed applications. *

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ package com.adobe.idp.dsc.applicationmanager; 
 
import java.io.FileInputStream; 
import java.util.List; 
import java.util.Properties; 
 
import com.adobe.idp.Document; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
 
public class GetApplications_SOAP { 
 
    public static void main(String[] args) { 
        //Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);          
        connectionProps.setProperty("DSC_SERVER_TYPE", "JBoss"); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
 
        //Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
        //Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient(myFactory); 
         
        Document doc = null; 
        try { 
             
            final FileInputStream fileApp = new FileInputStream("C:\\appraisal.lca"); 
            doc = new Document(fileApp); 
             
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
         
        try { 
         
            //Import the application into the LC server 
            final String resourceID = appClient.importApplication(doc); 
            System.out.println("Import application with resource ID: " + resourceID + " is completed successfully!"); 
         
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace();     
        } 
 
        try { 
                //Get applications from LC server 
                List appList = appClient.getApplications(); 
                System.out.println("Get applications from LC Server: " + appList.size()); 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

## Quick Start (SOAP mode): Getting status of applications using Java API {#quick-start-soap-mode-getting-status-of-applications-using-java-api}

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ 
package com.adobe.idp.dsc.applicationmanager; 
 
import java.io.FileInputStream; 
import java.util.Properties; 
 
import com.adobe.idp.Document; 
import com.adobe.idp.applicationmanager.application.ApplicationId; 
import com.adobe.idp.applicationmanager.application.ApplicationStatus; 
import com.adobe.idp.applicationmanager.application.impl.ApplicationIdImpl; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
 
public class GetApplicationStatus_SOAP { 
 
    public static void main(String[] args) { 
        //Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);          
        connectionProps.setProperty("DSC_SERVER_TYPE", "JBoss"); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
 
        //Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
        //Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient(myFactory); 
         
        Document doc = null; 
        try { 
             
            final FileInputStream fileApp = new FileInputStream("C:\\appraisal.lca"); 
            doc = new Document(fileApp); 
             
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
        final String sampleAppName = "Samples - Performance Appraisal"; 
        try { 
         
            //Import the application into the LC server 
            final String resourceID = appClient.importApplication(doc); 
            System.out.println("Import application with resource ID: " + resourceID + " is completed successfully!"); 
            //Deploy the application 
            boolean result = appClient.deployApplication(sampleAppName); 
            if (result) { 
                System.out.println("Imported application is deployed."); 
            } 
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace();     
        } 
         
        //Initialize the ApplicationId instance 
        ApplicationId appId = new ApplicationIdImpl(); 
        appId.setApplicationName(sampleAppName); 
 
        try { 
                //Get the application by application id 
                ApplicationStatus status = appClient.getApplicationStatus(appId); 
                System.out.println("Get application status with code: " + status.getStatusCode()); 
                 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

## Quick Start (SOAP mode):Previewing the LiveCycle ES2 and later application archive using the Java API {#quick-start-soap-mode-previewing-the-livecycle-es2-and-later-application-archive-using-the-java-api}

The following Java code example is for prevewing AEM Forms and later application archive using the Java API.

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
*/ 
package com.adobe.idp.dsc.applicationmanager; 
import java.io.FileInputStream; 
import java.util.Properties; 
import com.adobe.idp.Document; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
public class PreviewLCA_SOAP { 
    public static void main(String[] args) { 
        //Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);          
        connectionProps.setProperty("DSC_SERVER_TYPE", "JBoss"); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
        //Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
        //Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient(myFactory); 
         
        Document doc = null; 
        try { 
             
            final FileInputStream fileApp = new FileInputStream("C:\\appraisal.lca"); 
            doc = new Document(fileApp); 
             
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
         
        try { 
         
            //Preview the LCA 
            final Document newDoc = appClient.previewLCA(doc); 
                     
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace();     
        } 
    } 
}
```

## Quick Start (SOAP mode):Deleting the Application archive using the Java API {#quick-start-soap-mode-deleting-the-application-archive-using-the-java-api}

The following Java code example is for deleting an application archive.

```as3
/* 
 * This Java Quick Start uses the SOAP mode and contains the following JAR files 
 * in the class path: 
 * 1. adobe-livecycle-client.jar 
 * 2. adobe-usermanager-client.jar 
 * 3. adobe-application-remote-client.jar 
 * 4. activation.jar (required for SOAP mode) 
 * 5. axis.jar (required for SOAP mode) 
 * 6. commons-codec-1.3.jar (required for SOAP mode) 
 * 7. commons-collections-3.1.jar  (required for SOAP mode) 
 * 8. commons-discovery.jar (required for SOAP mode) 
 * 9. commons-logging.jar (required for SOAP mode) 
 * 10. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
 * 11. jaxen-1.1-beta-9.jar (required for SOAP mode) 
 * 12. jaxrpc.jar (required for SOAP mode) 
 * 13. log4j.jar (required for SOAP mode) 
 * 14. mail.jar (required for SOAP mode) 
 * 15. saaj.jar (required for SOAP mode) 
 * 16. wsdl4j.jar (required for SOAP mode) 
 * 17. xalan.jar (required for SOAP mode) 
 * 18. xbean.jar (required for SOAP mode) 
 * 19. xercesImpl.jar (required for SOAP mode) 
 * 
 * These JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/common 
 * 
 * 
 * SOAP required JAR files are located in the following path: 
 * <install directory>/sdk/client-libs/thirdparty 
 * 
 * 
 */ 
package com.adobe.idp.dsc.applicationmanager; 
 
import java.io.FileInputStream; 
import java.util.Properties; 
 
import com.adobe.idp.Document; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClient; 
import com.adobe.livecycle.applicationmanager.client.ApplicationManagerClientException; 
 
public class DeleteApplication_SOAP { 
 
    public static void main(String[] args) { 
        //Set connection properties required to invoke AEM Forms 
        Properties connectionProps = new Properties(); 
        connectionProps.setProperty("DSC_DEFAULT_SOAP_ENDPOINT", "http://[server]:[port]"); 
        connectionProps.setProperty("DSC_TRANSPORT_PROTOCOL", ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);          
        connectionProps.setProperty("DSC_SERVER_TYPE", "JBoss"); 
        connectionProps.setProperty("DSC_CREDENTIAL_USERNAME", "administrator"); 
        connectionProps.setProperty("DSC_CREDENTIAL_PASSWORD", "password"); 
 
        //Create ServiceClientFactory object 
        ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
        //Create ApplicationManagerClient object 
        ApplicationManagerClient appClient = new ApplicationManagerClient(myFactory); 
         
        Document doc = null; 
        try { 
             
            final FileInputStream fileApp = new FileInputStream("C:\\appraisal.lca"); 
            doc = new Document(fileApp); 
             
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
         
        try { 
         
            //Import the application into the LC server 
            final String resourceID = appClient.importApplication(doc); 
            System.out.println("Import application with resource ID: " + resourceID + " is completed successfully!"); 
         
        } catch (ApplicationManagerClientException e) { 
            e.printStackTrace();     
        } 
        final String sampleAppName = "Samples - Performance Appraisal"; 
        try { 
                //Delete the application imported above 
                appClient.deleteApplication(sampleAppName, "1.0"); 
                System.out.println("Delete application completed with name: " + sampleAppName); 
                 
        } catch (Exception e) { 
            e.printStackTrace(); 
        } 
    } 
}
```
