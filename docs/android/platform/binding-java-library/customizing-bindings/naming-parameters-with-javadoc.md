---
title: Nazwy parametrów z Javadoc
description: W tym artykule opisano sposób odzyskiwania nazwy parametrów w projekcie powiązanie Java za pomocą Javadoc generowane na podstawie projektu języka Java.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 7517e46c5b66123dc4e12fb5562c59f569f249aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766897"
---
# <a name="naming-parameters-with-javadoc"></a>Nazwy parametrów z Javadoc

_W tym artykule opisano sposób odzyskiwania nazwy parametrów w projekcie powiązanie Java za pomocą Javadoc generowane na podstawie projektu języka Java._


## <a name="overview"></a>Omówienie

Podczas tworzenia wiązania istniejącej biblioteki języka Java, niektóre metadane dotyczące interfejsu API powiązane zostaną utracone. W szczególności nazwy parametrów metod. Nazwy parametrów będą wyświetlane jako `p0`, `p1`itp. Jest to spowodowane Java `.class` plików nie zachowuj nazwy parametrów, które były używane w kodzie źródłowym Java. 

Projekt platformy Xamarin.Android Java powiązanie zapewniają nazwy parametrów, jeśli ma dostęp do Javadoc HTML z oryginalna Biblioteka. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrowanie Javadoc HTML Java, powiązania projektu

Integrowanie Javadoc HTML Java powiązania projektu jest ręczny proces składający się z następujących czynności: 

1.  Pobierz Javadoc biblioteki
2.  Edytuj `.csproj` plik i dodać `<JavaDocPaths>` właściwości:
3.  Czyszczenie i skompiluj ponownie projekt

Po zakończeniu oryginalnej nazwy parametrów Java powinny znajdować się w interfejsach API powiązany projekt powiązanie Java. 


> [!NOTE]
> Istnieje bardzo duże odchylenia w danych wyjściowych JavaDoc. . Łańcuch narzędzi powiązanie JAR nie obsługuje każdego pojedynczego permutacji możliwe i w związku z tym niektóre parametru może nie być poprawnie o nazwie.


## <a name="summary"></a>Podsumowanie

W tym artykule objętych jak stosowanie Javadoc w projekcie powiązanie Java nadać nazwy parametru znaczenie powiązanych interfejsów API. 

