---
title: Specify fonts to embed
seo-title: Specify fonts to embed
description: Learn how to specify fonts to embed.
seo-description: Learn how to specify fonts to embed.
uuid: 76af3efa-5057-49b5-b472-c7d90a1565d9
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_output
products: SG_EXPERIENCEMANAGER/6.4/FORMS
discoiquuid: 62e4b677-6327-40fe-be10-1e28a74404c1
index: y
internal: n
snippet: y
---

# Specify fonts to embed{#specify-fonts-to-embed}

You can specify which fonts are always embedded or never embedded with the forms used by Output. Embedding fonts increases the file size of the forms. Embed unusual fonts that users are not likely to have on their systems, and do not embed common fonts that they will have installed.

>[!NOTE]
>
>If you have specified a custom XCI file for Output, the embed font option in the XCI file overrides these settings. (See [Specify file locations for Output](../../../forms/using/admin-help/specify-file-locations-output.md#specify-file-locations-for-output).)

1. In administration console, click Services &gt; output.
1. Under Font Embedding Settings, in the Always Embed Fonts box, type the names of the fonts to embed with the forms, separated by commas. The fonts that you specify are only embedded in the generated form if they are used in the form. This setting is ignored if the embed font option has been turned on in the XCI file passed to the service. In that case, all fonts used in the PDF are always embedded.
1. In the Never Embed Fonts box, type the names of the fonts not to embed with the forms, separated by commas. The fonts that you specify are not embedded in the PDF, even if they are used in the generated PDF. This setting is ignored if the embed font option has been turned off in the XCI file passed to the service. In that case, none of the fonts used in the PDF are embedded.
1. Click Save.
