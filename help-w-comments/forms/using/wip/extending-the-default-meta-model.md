---
title: [DO NOT PUBLISH] Extending the default meta-model
seo-title: [DO NOT PUBLISH] Extending the default meta-model
description: null
seo-description: null
page-status-flag: never-activated
uuid: 927d16aa-3c36-410d-816f-4dd816a75a5b
contentOwner: khsingh
discoiquuid: 5a28f62c-9e53-4f9d-a47a-e2509cc18507
index: y
internal: n
snippet: y
---

# [DO NOT PUBLISH] Extending the default meta-model{#do-not-publish-extending-the-default-meta-model}

Automated Forms Conversion service identifies and extracts form objects from legacy forms. Meta-model helps the service to decide how the extracted objects are represented in an adaptive form. For example, a legacy form can have many different types of representations of a date and the meta-model helps map all the representations of date with date component of adaptive forms. Meta-model also allows the service to pre-configure validations, rules, data patterns, help text, and accessibility properties of adaptive form components.

![](assets/meta-model.gif)

Automated Forms Conversion service provides a default meta-model. The meta-model is a JSON schema. It defines mapping for all the adaptive form components. For example, a legacy form field with keyword phone, telephone, mobile phone, work phone, home phone, telephone number, telephone no, and phone number are mapped to the adaptive form’s telephone component. You can extend the default the meta-model to add mapping and properties specific to your organization.