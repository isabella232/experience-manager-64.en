---
title: Searching for process instances
seo-title: Searching for process instances
description: null
seo-description: Use the Process Search page to enter search criteria for finding a process instance.
uuid: 9e21f99a-d85d-41e7-a035-36dad9f8e266
acrolinxdate: 2016-05-31T07 02 56.569-0400
acrolinxlastcheckedby: vishgupt
acrolinxpagestatus: red
acrolinxreporturl: http //checkstyle.corp.adobe.com 8031/output/en/searching_process_instances_admin_5e12de0b318c6865_2362_report.xml
acrolinxsentences: 31
acrolinxwords: 441
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms_workflow
products: SG_EXPERIENCEMANAGER/6.4/FORMS
discoiquuid: 86a6710f-ea7c-4d50-a409-ded232484323
isreadyforlocalization: false
index: y
internal: n
snippet: y
---

# Searching for process instances{#searching-for-process-instances}

Use the Process Search page to enter search criteria for finding a process instance. You can access the Process Search page from the forms workflow page or by clicking Search on the Process Instance page.

You can enter basic criteria to perform a general search, specific attributes to perform a detailed search, or a combination of basic criteria and specific attributes to perform a combined search.

## Perform a general search {#perform-a-general-search}

A general search for a process is most appropriate if you know the process ID of the process instance, if you are looking for a group of related process instances, or if only a few process instances are running.

Enter basic criteria to perform a general search. If you enter multiple criteria, the search is performed with an implied AND condition.

1. In administration console, click Services &gt; Forms workflow &gt; Process Search.
1. On the Process Search page, under General Search, provide the following criteria:

    * **Process ID: **The positive integer that identifies each unique process instance.
    * **Process Status: **Select a status from the list.
    * **Application: **Select an application from the list. Only deployed applications are shown.
    * **Process Name - Version: **Select a process name from the menu. Only deployed processes are shown.

1. Click Search. The Process Instance page appears, listing the found instances.

## Perform a detailed search for a process {#perform-a-detailed-search-for-a-process}

You can enter specific attributes to perform a detailed search. A detailed search is most appropriate if you have many process instances running and you need to narrow the possible finds by certain criteria.

1. In administration console, click Services &gt; Forms workflow &gt; Process Search.
1. On the Process Search page, under Detailed Search, specify your first criteria set:

    * In the Attribute list, select an attribute.
    * In the Filter list, select an operator.
    * In the Value box, type a value appropriate to the attribute you selected.

1. To add another row, select More Filters. Another set of Attribute, Filter, and Value lists appears, as well as a Condition list.
1. Under Condition, select AND or OR. Repeat steps 1 - 3 as required to narrow your search further.
1. To add or remove rows, click More Filters or Fewer Filters. You can have from one to four rows.
1. Click Search. The Process Instance page appears, listing the found instances.

[About process instance statuses](../../../forms/using/admin-help/processes.md#about-process-instance-statuses)

## Perform a combined search for a process {#perform-a-combined-search-for-a-process}

To create a search based on both a general search and a detailed search, with an implied AND between the areas, enter your search criteria in both the General Search and Detailed Search areas on the Process Search page.

If the search is too narrow, no instances will be found.