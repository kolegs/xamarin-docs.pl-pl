---
title: Azure Active Directory
description: Rejestrowanie aplikacji przy użyciu usługi Azure Active Directory
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: decc069bd2fe31d54c886793ae4c94935b23ad02
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Rejestrowanie aplikacji przy użyciu usługi Azure Active Directory_

Usługa Azure Active Directory umożliwia deweloperom bezpiecznych zasobów, takich jak pliki, łączy i interfejsów API sieci Web przy użyciu tego samego konta organizacyjnego pracownicy korzystają na zalogowanie się do systemów lub sprawdź ich wiadomości e-mail.

Opracowywanie aplikacji mobilnych, które mogą uwierzytelniać za pomocą usługi Azure Active Directory obejmuje trzy kroki.
Pierwsze dwa kroki, zazwyczaj są takie same, niezależnie od tego, jakie usługi ma być używany. Trzeci krok jest różne dla każdego typu usług:

  1. [Rejestrowanie w usłudze Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) na *windowsazure.com* portalu, a następnie
  2. [Konfigurowanie usług](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Opracowywanie aplikacji mobilnych, korzystanie z usług.

Przykłady różnych usług, który można uzyskać dostępu do:

- [Interfejs API programu Graph](~/cross-platform/data-cloud/active-directory/graph.md)
- Interfejs API sieci Web
- Office365


## <a name="conclusion"></a>Wniosek

Korzystając z procedury powyżej, które można uwierzytelniać Twojej aplikacji mobilnych w usłudze Azure Active Directory. Biblioteki uwierzytelniania usługi Active Directory (ADAL) ułatwia przy mniejszej liczbie wierszy kodu, przy zachowaniu większość kodu takie same i w związku z tym ułatwiając możliwe do udostępnienia platformach.



## <a name="related-links"></a>Linki pokrewne

- [Przykładowe NativeClient firmy Microsoft](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
