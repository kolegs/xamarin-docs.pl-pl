---
title: "Krok 2. Konfigurowanie usługi dostępu do aplikacji mobilnej"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: a679d404eace121b38463e35350b8b85ec08b1e0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Krok 2. Konfigurowanie usługi dostępu do aplikacji mobilnej

Zawsze, gdy żadnych zasobów, np. aplikacji sieci web, usługi sieci web, itp. musi być zabezpieczone przez usługę Azure Active Directory, musi zostać zarejestrowany. Zabezpieczenia aplikacji lub usług, można wyświetlić w obszarze **aplikacji** kartę. Tutaj możesz wybierać aplikacji, którą należy będą uzyskiwać dostęp do aplikacji mobilnej i przyznać dostęp do niego.

1. Na **Konfiguruj** zlokalizuj **uprawnień dotyczących innych aplikacji** sekcji:

  ![](configure-images/2.1-configure.png "Na karcie Konfigurowanie Znajdź uprawnienia do innych części aplikacji")

2.  Polecenie **Dodaj aplikację** przycisku. Na następnym ekranie wyskakujących powinna zostać wyświetlona lista wszystkich aplikacji, które są zabezpieczone przez usługę Azure Active Directory. Wybierz aplikacje, które należy udostępnić z aplikacji mobilnej.

  ![](configure-images/2.2-add-application.png "Wybierz aplikacje, które należy udostępnić z aplikacji mobilnej")

3. Po wybraniu aplikacji, ponownie wybrać do aplikacji nowo dodanych w **uprawnień dotyczących innych aplikacji** sekcji i przypisz odpowiednie uprawnienia.

  ![](configure-images/2.3-permissions.png "Po wybraniu aplikacji, ponownie wybierz aplikację, nowo dodanych w uprawnienia do innych części aplikacji i przypisz odpowiednie uprawnienia")

4. Na koniec **zapisać** konfiguracji. Te usługi powinny być teraz dostępne w aplikacjach mobilnych!



## <a name="related-links"></a>Linki pokrewne

- [Przykładowe NativeClient firmy Microsoft](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
