---
title: "Połączona usług w programie Visual Studio dla komputerów Mac"
description: "Dodawanie magazynu danych Azure, uwierzytelniania i powiadomień wypychanych do aplikacji mobilnych z poziomu programu Visual Studio dla komputerów Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: cc2deb11d544bc4e933e690d6089eb001a186c79
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="connected-services-walkthrough"></a>Wskazówki podłączonych usług

Przepływ pracy usług połączonych powoduje przeniesienie przepływu pracy portalu Azure do programu Visual Studio dla komputerów Mac, dzięki czemu nie trzeba pozostaw projekt, aby dodać usługi.

W tym przewodniku przedstawiono sposób dodawania usługi zaplecza Azure udostępnia magazyn danych w chmurze, uwierzytelnianie i wysyłanie powiadomień wypychanych do aplikacji platformy Xamarin.Forms przenośnej biblioteki klasy (PCL) i platform.


1.  Uruchomić przez dwukrotne kliknięcie **usług połączonych** węzła w rozwiązania, które zostaną wyświetlone **galerii usług**.
  To jest lista wszystkich usług dostępnych dla tego typu aplikacji. Wybierz usługę (takich jak **zaplecza aplikacji mobilnych w usłudze aplikacji Azure**), klikając go.

  [ ![](connected-services-images/image001-sml.png "Połączone węzła usług w programie Visual Studio dla komputerów Mac")](connected-services-images/image001.png)

2. Na stronie Szczegóły usługi zawiera opis usługi i zależności do zainstalowania.
  Kliknij przycisk **Dodaj** przycisk, aby dodać zależności do aplikacji:

  [ ![](connected-services-images/image002-sml.png "Zaplecze aplikacji mobilnej przy użyciu platformy Azure")](connected-services-images/image002.png)

3. Zależności należy dodać do PCL i projekty specyficzne dla platformy do pracy.
  Zaznacz pola wyboru, aby dodać usługę do każdego projektu odwołującego się on (bezpośrednio lub pośrednio):

  [ ![](connected-services-images/image003-sml.png "Sprawdź wszystkie projekty, które powinien odwoływać się usługi")](connected-services-images/image003.png)

4. Wybierz **Akceptuj** na **akceptacji licencji** okien dialogowych dla pakietów NuGet.
  Mogą być dwa okna dialogowe, aby zaakceptować, jeden dla MobileClient i zależności i drugi dla SQLiteStore, co jest niezbędne do synchronizacji danych w trybie offline:

  [ ![](connected-services-images/image004-sml.png "Zaakceptuj umowy licencyjne")](connected-services-images/image004.png)

  ![](connected-services-images/image005.png "Okno akceptacji licencji")

5. Po dodaniu zależności poprosimy Cię o Zaloguj się przy użyciu konta, które ma być używany do komunikacji z platformy Azure.
  Jeśli użytkownik jest już zalogowany za pomocą Identyfikatora Microsoft, Visual Studio for Mac podejmie próbę pobrania subskrypcji platformy Azure i usług aplikacji skojarzonych z nimi. Jeśli nie masz żadnych subskrypcji, możesz dodać kategorię skorzystania z bezpłatnej wersji próbnej lub zakup planu subskrypcji w portalu Azure.

6. Wybierz usługę aplikacji z listy. To spowoduje wypełnienie kod szablonu `MobileServiceClient` obiektu z odpowiedni adres URL usługi app Service na platformie Azure:

  [ ![](connected-services-images/image006-sml.png "Wybierz usługę aplikacji z listy")](connected-services-images/image006.png)

  Jeśli nie ma na liście usług, kliknij przycisk **nowy** przycisk (zobacz krok 9).

7. Skopiuj kod szablonu `MobileServiceClient` do PCL. Aplikacji lokalizacji pliku nie ważne, jak długo istnieje tylko jedno wystąpienie.
  Zalecanym podejściem jest utworzenie `AzureService` klasy, która obsługuje wszystkie interakcje Azure i używa `MobileServiceClient`:

  ![](connected-services-images/image007.png "Skopiuj kod konfiguracji do aplikacji")

8. Postępuj zgodnie z dokumentacją w **następne kroki** dodawania danych, synchronizacja w trybie offline, uwierzytelniania i powiadomienia wypychane do aplikacji:

  [ ![](connected-services-images/image008-sml.png "Przejrzyj następnej instrukcji kroki")](connected-services-images/image008.png)

10. Jeśli nie masz żadnych istniejących usług aplikacji, można utworzyć nowych usług z poziomu programu Visual Studio dla komputerów Mac.
  Kliknij przycisk **nowy** przycisk w lewym dolnym rogu listy usług, aby otworzyć **nowej aplikacji usługi** okna dialogowego:

  [ ![](connected-services-images/image009-sml.png "Utwórz nową usługę aplikacji w programie Visual Studio dla komputerów Mac")](connected-services-images/image009.png)

Nowa usługa wymaga następujących parametrów:

-   **Nazwa aplikacji usługi** — Unikatowy identyfikator/nazwę planu
-   **Subskrypcja** — subskrypcji chcesz płacisz za usługę
-   **Grupa zasobów** — lub sposób organizowania wszystkich zasobów platformy Azure dla projektu. Opcja Użyj istniejących lub Utwórz nową. Jeśli jest to pierwszy usługi Azure, Utwórz nową.
-   **Planowanie usługi** — Określa lokalizację i koszt wszystkich zasobów, które go używają. Opcja Użyj istniejących lub Utwórz nową. Jeśli jest to pierwszy usługi Azure, użyj ustawienia domyślnego lub Utwórz nową w warstwie bezpłatna (F1).

Odwiedź stronę [dokumentacji usługi Azure App Service](https://docs.microsoft.com/azure/app-service/) Aby uzyskać więcej informacji


## <a name="related-links"></a>Linki pokrewne

- [Usługa aplikacji Azure](https://docs.microsoft.com/en-us/azure/app-service/)
- [Uwagi do wersji](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
