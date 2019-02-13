---
title: SAML 2.0 Authentication Handler
seo-title: SAML 2.0 Authentication Handler
description: Learn about the SAML 2.0 Authentication Handler in AEM.
seo-description: Learn about the SAML 2.0 Authentication Handler in AEM.
uuid: 319cd061-7930-44b9-b080-a5205523c4a8
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: Security
content-type: reference
discoiquuid: d6be4a6b-41c8-427e-ae03-6070fd18545b
index: y
internal: n
snippet: y
---

# SAML 2.0 Authentication Handler{#saml-authentication-handler}

<!--
Comment Type: remark
Last Modified By: unknown unknown (colligno@adobe.com)
Last Modified Date: 2018-03-14T13:20:43.299-0400
<p><strong>Issue</strong> tracked by https://issues.adobe.com/browse/DOC-2769</p>
<p>Dev <strong>draft</strong> <a href="https://zerowing.corp.adobe.com/display/granite/SAML+2.0+Authentication+Handler">https://zerowing.corp.adobe.com/display/granite/SAML+2.0+Authentication+Handler</a></p>
-->

AEM ships with a [SAML](http://saml.xml.org/saml-specifications) authentication handler. This handler provides support for the [SAML](http://saml.xml.org/saml-specifications) 2.0 Authentication Request Protocol (Web-SSO profile) using the `HTTP POST` binding.

It supports:

* signing and encryption of messages
* automatic creation of users  
* synching groups to existsing ones in AEM
* Service Provider and Identity Provider initiated authentication

This handler stores the encrypted SAML response message in the user-node ( `usernode/samlResponse`) to facilitate communication with a third-party Service Provider.

>[!NOTE]
>
>See [a demonstration of AEM and SAML integration](http://helpx.adobe.com/cq/kb/saml-demo.html).
>
>To read an end to end community article, click: [Integrating SAML with Adobe Experience Manager](https://helpx.adobe.com/experience-manager/using/aem63_saml.html).

## Configuring The SAML 2.0 Authentication Handler {#configuring-the-saml-authentication-handler}

The [Web console](../../../sites/deploying/using/configuring-osgi.md) provides access to the [SAML](http://saml.xml.org/saml-specifications) 2.0 Authentication Handler configuration called **Adobe Granite SAML 2.0 Authentication Handler**. The following properties can be set.

>[!NOTE]
>
>The SAML 2.0 Authentication Handler is disabled by default. You must set at least one of the following properties in order to enable the handler: 
>
>* The Identity Provider POST URL.  
>* The Service Provider Entity ID.
>

>[!NOTE]
>
>SAML assertions are signed and may optionally be encrypted. In order for this to work you have to provide at least the public certificate of the Indentity Provider in the TrustStore. See [Adding the IdP certificate to the TrustStore](../../../sites/administering/using/saml-2-0-authenticationhandler.md#addtheidpcertificatetotheaemtruststore) section for more information.

**Path** Repository path for which this authentication handler should be used by Sling. If this is empty, the authentication handler will be disabled.

**Service Ranking** OSGi Framework Service Ranking value to indicate the order in which to call this service. This is an integer value where higher values designate higher precedence.

**IDP Certificate Alias** The alias of the IdP's certificate in the global truststore. If this property is empty the authentication handler is disabled. See the "Add the IdP Certificate to the AEM TrustStore" chapter below on how to set it up.

**Identity Provider URL** URL of the IDP where the SAML Authentication Request should be sent to. If this property is empty the authentication handler is disabled.

>[!CAUTION]
>
>The Identity Provider hostname must be added to the **Apache Sling Referrer Filter** OSGi configuration. See the [Web console](../../../sites/deploying/using/configuring-osgi.md) section for more information.

**Service Provider Entity ID** ID which uniquely identifies this service provider with the identity provider. If this property is empty the authentication handler is disabled.

**Default Redirect** The default location to redirect to after successful authentication.

>[!NOTE]
>
>This location is only used if the `request-path` cookie is not set. If you request any page below the configured path without a valid login-token, the requested path is stored in a cookie  
>and the browser will be redirected to this location again after successful authentication.

**User-ID Attribute** The name of the attribute containing the user ID used to authenticate and create the user in the CRX repository.

>[!NOTE]
>
>The user ID will not be taken from the `saml:Subject` node of the SAML assertion but from this `saml:Attribute`.

**Use Encryption** Whether or not this authentication handler expects encrypted SAML assertions.

**Autocreate CRX Users** Whether or not to automatically create non-existing users in the repository after successful authentication.

>[!CAUTION]
>
>If the automatic creation of CRX users is disabled, the users will have to be created manually.

**Add to Groups** Whether or not a user should be automatically added to CRX groups after successful authentication.

**Group Membership** The name of the saml:Attribute containing a list of CRX groups this user should be added to.

## Add the IdP Certificate to the AEM TrustStore {#add-the-idp-certificate-to-the-aem-truststore}

SAML assertions are signed and may optionally be encrypted. In order for this to work you have to provide at least the public certificate of the IdP in the repository. In order to do this you need to:

1. Go to: [http://localhost:4502/libs/granite/security/content/useradmin.html](http://localhost:4502/libs/granite/security/content/useradmin.html)
1. Click on any of the users in the list.
1. Go under **Account Settings**, and press the **Create TrustStore** link.
1. Enter the password for the TrustStore and press **Save**.
1. Click on **Manage TrustStore**.
1. Upload the IdP certificate.
1. Take note of the certificate Alias. The alias is **admin#1436172864930** in the example below.

   ![](assets/chlimage_1-459.png)

## Add the Service Provider key and certificate chain to the AEM keystore {#add-the-service-provider-key-and-certificate-chain-to-the-aem-keystore}

>[!NOTE]
>
>The below steps are mandatory, otherwise the following exception will be thrown: `com.adobe.granite.keystore.KeyStoreNotInitialisedException: Uninitialised system trust store`

1. Go to: [http://localhost:4502/libs/granite/security/content/useradmin.html](http://localhost:4502/libs/granite/security/content/useradmin.html)
1. Edit the `authentication-service` user.
1. Create a KeyStore by clicking **Create KeyStore** under **Account Settings**.

>[!NOTE]
>
>The below steps are required only if handler should be able to sign or decrypt messages.

1. Upload the Private key file by clicking **Select Private Key File**. The key meeds to be in PKCS#8 format with DER encoding. 
1. Upload the certificate file by clicking **Select Certificate Chain Files**.
1. Assign an Alias, as shown below:

   ![](assets/chlimage_1-460.png)

## Configure a Logger for SAML {#configure-a-logger-for-saml}

You can set up a Logger in order to debug any issues that might arise from misconfiguring SAML. You can do this by:

1. Going to the Web Console, at *http://localhost:4502/system/console/configMgr*
1. Search for and click on the entry called **Apache Sling Logging Logger Configuration**
1. Create a logger with the following configuration:

    * **Log Level:** Debug
    * **Log File:** logs/saml.log
    * **Logger:** com.adobe.granite.auth.saml
