---
title: Add Comment to Sample Page
seo-title: Add Comment to Sample Page
description: null
seo-description: Add Custom Comments to a page
uuid: d632de1a-8712-4e91-b8a0-0f57b5d42690
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: developing
content-type: reference
discoiquuid: c5324790-fb27-4964-b0b2-0bb93597e946
isreadyforlocalization: false
index: y
internal: n
snippet: y
---

# Add Comment to Sample Page{#add-comment-to-sample-page}

| ** [⇐ Create the Components](../../communities/using/extend-create-components.md)** |** [Alter the Appearance ⇒](../../communities/using/extend-alter-appearance.md)** |
|---|---|

Now that the components for the custom comment system are in place in the application directory (/apps), it is possible to make use of the extended component. The instance of the comment system in a website to be affected must set its resourceType to be the custom comment system and include all necessary client libraries.

### Identify Required Clientlibs {#identify-required-clientlibs}

The client libraries necessary for the style and functioning of the default Comments are also necessary for extended Comments.

The [Community Components Guide](../../communities/using/components-guide.md) identifies the required client libraries. Browse to the Component Guide and view the Comments component, for example :

[http://localhost:4502/content/community-components/en/comments.html](http://localhost:4502/content/community-components/en/comments.html)

Note the three client libraries required for Comments to render and function properly. These will need to be included where the extended Comments is referenced, as well as the [extended Comments' client library](../../communities/using/extend-create-components.md#createaclientlibraryfolder) ( `apps.custom.comments`).

![](assets/chlimage_1-46.png)

### Add Custom Comments to a Page {#add-custom-comments-to-a-page}

As there can be only one Comment system per page, it is simpler to create a sample page as described in the short [Create a Sample Page](../../communities/using/create-sample-page.md) tutorial.

Once created, enter Design mode and make available the Custom component group to allow the `Alt Comments` component to be added to the page.

In order for the Comment to appear and function properly, the client libraries for Comments must be added to the clientlibslist for the page (see [Clientlibs for Communities Components](../../communities/using/clientlibs.md)).

#### Comments Clientlibs on Sample Page {#comments-clientlibs-on-sample-page}

![Comments Clientlibs on Sample Page](assets/chlimage_1-47.png)

#### Author : Alt Comment on Sample Page {#author-alt-comment-on-sample-page}

![Alt Comment on Sample Page](assets/chlimage_1-48.png)

#### Author : Sample Page Comments Node {#author-sample-page-comments-node}

You can verify the resourceType in CRXDE by viewing the properties of the comments node for the sample page at `/content/sites/sample/en/jcr:content/content/primary/comments`.

![](assets/chlimage_1-49.png)

#### Publish Sample Page {#publish-sample-page}

After the custom component is added to the page, it is also necessary to (re) [publish the page](../../communities/using/sites-console.md#publishingthesite).

#### Publish : Alt Comment on Sample Page {#publish-alt-comment-on-sample-page}

After publishing both the custom application and the sample page, it should be possible to enter a comment. When signed in, either with a [demo user](../../communities/using/tutorials.md#demousers) or admin, it should be possible to post a comment.

Here is aaron.mcdonald@mailinator.com posting a comment :

![](assets/chlimage_1-50.png) ![](assets/chlimage_1-51.png)

Now that it appears the extended component is working correctly with the default appearance, it is time to modify the appearance.

| ** [⇐ Create the Components](../../communities/using/extend-create-components.md)** |** [Alter the Appearance ⇒](../../communities/using/extend-alter-appearance.md)** |
|---|---|
