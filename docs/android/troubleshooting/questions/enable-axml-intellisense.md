---
title: Jak włączyć Intellisense w plikach Android .axml?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 8278576355299894eab1c7c6d3ceaadb607fe915
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Jak włączyć Intellisense w plikach Android .axml?

Intellisense dla systemu android w programie Visual Studio wymaga dwóch plików schematów XML działała prawidłowo. Aby znaleźć te pliki, przejdź do tego folderu:

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (Wcześniej: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

Wewnątrz dostępne są dwa pliki XSD:

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio zachowuje wszystkich schematów XML znajduje się w folderze odpowiednich:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

Można przenieść te dwa pliki XSD do powyższej lokalizacji, lub wystarczy po prostu dodaj te schematów w programie Visual Studio. Użytkownik może następnie "Dodaj" tych schematów w programie Visual Studio za pomocą **XML > Schematy > Dodaj** okna dialogowego.






