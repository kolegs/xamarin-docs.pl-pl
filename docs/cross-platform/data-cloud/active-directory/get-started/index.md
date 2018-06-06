---
title: Azure Active Directory
description: Tym dokumencie opisano kroki, które musi występować umożliwia aplikacji mobilnej do uwierzytelniania w usłudze Azure Active Directory.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780072"
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
