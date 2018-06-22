---
title: Przy użyciu ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b9b6340d4aaf386c7b4be8ebf366589582771be2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763296"
---
# <a name="using-a-contentprovider"></a>Przy użyciu ContentProvider

CursorAdapters można również wyświetlić dane z ContentProvider.
ContentProviders pozwalają na dostęp do danych udostępnianych przez inne aplikacje (włącznie z danymi systemu Android, takie jak kontakty, nośniki i kalendarza informacji).

Jest preferowany sposób dostępu ContentProvider z CursorLoader, przy użyciu LoaderManager. LoaderManager został wprowadzony w 3.0 dla systemu Android (11 poziom interfejsu API, pustakowym) aby przenieść blokowania zadania poza głównym wątku i przy użyciu CursorLoader pozwala danych musi być załadowany w wątku, aby związany ListView do wyświetlenia.

Zapoznaj się [wprowadzenie do usługi ContentProviders](~/android/platform/content-providers/index.md) Aby uzyskać więcej informacji.

