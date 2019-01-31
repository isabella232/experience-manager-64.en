---
title: Saving a task or form as a draft
seo-title: Saving a task or form as a draft
description: null
seo-description: Steps to save a draft copy of a task or a form in the AEM Forms app
uuid: f5a31397-f3d6-40a4-89c9-1d723cb5f419
acrolinxdate: 2016-05-31T06 29 03.427-0400
acrolinxlastcheckedby: vishgupt
acrolinxpagestatus: green
acrolinxreporturl: http //checkstyle.corp.adobe.com 8031/output/en/save_as_draft_admin_5e12de0b318c6865_1930_report.xml
acrolinxsentences: 33
acrolinxwords: 499
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: forms-app
discoiquuid: c6ea272c-b91b-4222-a917-e40dc4f0a50e
firstpublishqadate: 2015-12-07T08 21 42.542-0500
isreadyforlocalization: false
lastpublishqadate: 2015-12-08T05 26 14.326-0500
locpublishdate: 09/06/13
locpublishoption: later
publishqadate: 2015-12-08T05 26 14.326-0500
publishqaurl: https //helpx.stage.adobe.com/aem-forms/6-2/save-as-draft.html
index: y
internal: n
snippet: y
---

# Saving a task or form as a draft{#saving-a-task-or-form-as-a-draft}

The save as draft option saves a snapshot of a task or form along with the data filled in the associated form. You can also create a draft from a template. The drafts are saved in the mobile device, and synced with Adobe Experience Manager Forms server for a later retrieval.

You can [update the form](../../forms/using/working-with-form.md), [annotate it](../../forms/using/add-attachments.md) with photographs, and scribble notes. As you continue to update a form, it is recommended to save it as a draft. For situations, where you decide to submit a filled form at a later point in time, saving it as a draft is helpful.

To enable save as draft feature for forms saved on forms portal, see [Saving an HTML5 form as a draft](../../forms/using/saving-html5-form-draft.md).   
To configure submission of adaptive forms, see [Drafts and submissions component](../../forms/using/draft-submission-component.md). (Not valid for forms synced with AEM Forms JEE server.)

To create a draft, open the form and tap the **Save as Draft** ![](assets/save-as-draft.png). Provide the name of the draft and tap **Save**. The draft is saved in the Drafts folder and synced with the server. It is saved in the Outbox folder if the app is offline.

If you update the corresponding form afterwards, the changes are reflected immediately. On synchronizing the AEM Forms app with AEM Forms server, the draft is uploaded to AEM Forms server. In addition, the draft is moved from the Outbox to the Tasks or Drafts folder. An edit icon appears next to it.

As you continue to work on multiple tasks and start points and save them, the drafts are saved. Each time your app is synchronized with the AEM Forms server, the draft is saved to the server. It ensures that at any time you can recover the drafts based on the last saved date and time. For example, if you reinstall the app or change your mobile device, you can download the draft from the server.

### Delete a draft {#delete-a-draft}

The drafts folder lists all the drafts. You can use the Delete Draft option to permanently delete the drafts from the mobile device and server.

The option to delete drafts created from a task is not available. On deleting a draft created from a task, the task is abandoned.

You can discard drafts in both offline and online mode. On discarding the drafts in offline mode, the drafts are deleted from the server only when the connection with the server is restored.

Perform the following steps to delete a draft:

1. In the AEM Forms app, navigate to **Forms. **
1. Select **Drafts** from the drop-down next to Search. 
1. A form with the edit icon  ![](assets/edit-draft-app.png)

   denotes a draft. Tap the horizontal ellipsis next to the draft. 
1. In the options that appear when you tap the horizontal ellipsis, tap **Delete Draft**.

[**Contact Support**](https://www.adobe.com/account/sign-in.supportportal.html)

>[!MORE_LIKE_THIS]
>
>* [Opening a task](../../forms/using/open-task.md)
>* [Working with a Form](../../forms/using/working-with-form.md)
>* [Adding attachments](../../forms/using/add-attachments.md)
>* [Saving a task (Save as Draft)](../../forms/using/save-as-draft.md)