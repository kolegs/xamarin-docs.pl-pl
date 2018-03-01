---
title: Publikowanie do witryny Google Play
ms.topic: article
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 74635b10e97513d6b023cb44ede7745448aa153c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-google-play"></a>Publikowanie do witryny Google Play

Choć wiele rynków aplikację do dystrybucji aplikacja, Google Play jest raczej magazynu największych i najczęściej odwiedzanych w świecie dla aplikacji systemu Android. Google Play zapewnia pojedynczą platformę do dystrybucji, reklamy, sprzedaży i analizowania sprzedaży aplikacji systemu Android.

Ta sekcja obejmuje tematów, które są specyficzne dla sklepu Google Play, takie jak rejestrowanie jako wydawca zasoby ułatwiające Google Play wspierania i anonsowanie aplikacji, wskazówki dotyczące klasyfikacji aplikacji w witrynie Google Play i za pomocą filtrów do zbierania Ogranicz wdrożenia aplikacji na niektórych urządzeniach.

<a name="Requirements"  />

## <a name="requirements"></a>Wymagania

Aby rozpowszechnić aplikację za pomocą usługi Google Play, można utworzyć konta dewelopera. To tylko wystarczy wykonać jeden raz i obejmować jeden opłata czasu z $25 USD.

Wszystkie aplikacje muszą być podpisane za pomocą klucza kryptograficznego, który wygasa po 22 października 2033.

Maksymalny rozmiar APK publikowane w witrynie Google Play wynosi 100MB. Jeśli aplikacja przekroczy ten rozmiar, Google Play umożliwia dodatkowe zasoby, które mają być dostarczone przez *APK rozszerzenia plików*. Pliki rozszerzeń android umożliwiają APK mieć rozmiar maksymalnie 2GB 2 dodatkowe pliki, każde z nich. Google Play hosta, a następnie rozpowszechnić te pliki bez ponoszenia kosztów. Rozszerzenia plików zostanie omówiony w innej sekcji.

Google Play nie jest ogólnie dostępna. Niektóre lokalizacje może nie być obsługiwana dystrybucji aplikacji.


<a name="Becoming_a_Publisher"  />

## <a name="becoming-a-publisher"></a>Staje się wydawca

Do publikowania aplikacji w witrynie Google play, należy mieć konto wydawcy. Aby zarejestrować się wydawca konto, wykonaj następujące kroki:

1.  Odwiedź stronę [konsoli dla deweloperów Google Play](https://play.google.com/apps/publish).
1.  Podaj podstawowe informacje o tożsamości developer.
1.  Przeczytaj i zaakceptuj umowę dystrybucji Developer dla ustawień regionalnych użytkownika.
1.  Zapłacić rejestracji $25 USD.
1.  Potwierdź weryfikacji za pośrednictwem poczty e-mail.
1.  Po utworzeniu konta, istnieje możliwość publikowania aplikacji przy użyciu usługi Google Play.


Google Play nie obsługuje pewnych krajach na całym świecie. Najbardziej aktualne listę krajów, można znaleźć w następujących łączy:

1.  [Obsługiwane lokalizacje dla deweloperów &amp; rejestracji sprzedawcy](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; jest lista wszystkich krajów, w której deweloperzy mogą zarejestrować jako sprzedawców i sprzedawać płatnych aplikacji.

1.  [Obsługiwane lokalizacje dla dystrybucji użytkownikom Google Play](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; jest lista wszystkich krajów, w których aplikacje mogą być dystrybuowane.


<a name="Preparing_Promotional_Assets"  />

### <a name="preparing-promotional-assets"></a>Trwa przygotowywanie zasobów promocyjna

Efektywne wspierania i anonsowanie aplikacji w witrynie Google Play, Google umożliwia deweloperom przesłać promocyjna zasoby, takie jak zrzuty ekranu, grafiki i wideo do przesłania. Google Play reklamy i promocji aplikacja będzie używać tych zasobów.


<a name="Launcher_Icons"  />

#### <a name="launcher-icons"></a>Uruchamianie ikon

A *ikonę uruchamiania* jest reprezentującą aplikacji. Każda z ikon uruchamiania powinna być PNG 32-bitowy, z kanału alfa przezroczystości. Aplikacja musi mieć ikon dla wszystkich gęstości uogólniony ekranu, jak przedstawiono na poniższej liście:

-   **ldpi** (120dpi) &ndash; 36 x 36 pikseli
-   **mdpi** (160dpi) &ndash; 48 x 48 pikseli
-   **hdpi** (240dpi) &ndash; 72 x 72 pikseli
-   **xhdpi** (320dpi) &ndash; 96 x 96 pikseli


Ikony uruchamiania są najważniejsze, które użytkownik będzie widział aplikacji w witrynie Google Play, więc należy zwrócić uwagę, aby wizualnie atrakcyjne i łatwy do rozpoznania ikony uruchamiania.

Wskazówki dotyczące uruchamiania ikon:

1.  **Proste i ikonami** &ndash; ikony uruchamiania powinny być przechowywane w prosty i ikonami. Oznacza to, z wyłączeniem nazwę aplikacji z ikony. Ikony prostsze będzie można łatwiej zapamiętać i będzie można łatwiej odróżnić w mniejszych rozmiarach.

1.  **Ikony nie powinien być alokowania** &ndash; zbyt alokowania ikony nie wyróżnienia na wszystkich tła.

1.  **Użyj kanału alfa** &ndash; ikony należy wykonać przy użyciu kanału alfa, a nie powinien być obrazy pełnej w ramce.


<a name="High_Resolution_Application_Icon"  />

#### <a name="high-resolution-application-icons"></a>Ikony aplikacji wysokiej rozdzielczości

Aplikacje w sklepie Google Play wymagają wersji o wysokiej wierności ikony aplikacji. Jest używana tylko przez Google Play, a nie zastępuje ikonę uruchamiania aplikacji. Są to następujące specyfikacje wysokiej rozdzielczości ikony:

1.  32-bitowe PNG z kanału alfa
1.  512 x 512 pikseli
1.  Maksymalny rozmiar 1024KB

[Android Studio zasobów](https://romannurik.github.io/AndroidAssetStudio/) jest przydatne narzędzie do tworzenia odpowiedniego uruchamiania ikony i ikona aplikacji o wysokiej rozdzielczości.


<a name="Screen_shots"  />

#### <a name="screen-shots"></a>Zrzuty ekranu

Google play wymaga co najmniej dwóch i maksymalnie osiem zrzuty ekranu aplikacji. Będą one wyświetlane na stronie szczegółów aplikacji w sklepie Google Play.

Dane techniczne dla zrzuty ekranu są:

1.  bit 24 PNG lub JPG na żaden kanał alfa
1.  320w x 480h lub 480w x 800h lub 480w x 854 h. Obrazy landscaped zostaną przycięte.


<a name="Promotional_Graphic" />

#### <a name="promotional-graphic"></a>Promocyjna grafiki

Jest to opcjonalny obraz używany przez Google Play:

1.  Jest 180w x 120 h 24 bit PNG lub JPG kanałów alfa.
1.  Brak obramowania kompozycji.


<a name="Feature_Graphic" />

#### <a name="feature-graphic"></a>Funkcja grafiki

Używany przez sekcji promocji Google Play. Ta grafika mogą być wyświetlane pojedynczo bez ikonę aplikacji.

1.  1024w x 500h PNG lub JPG nie kanału alfa i bez przezroczystości.
1.  Wszystkie ważne zawartość powinna być w ramce 924 x 500. Pikseli poza tej ramki może zostać obcięty do celów stylistycznych.
1.  Ta grafika może być skalowany w dół: Użyj duży i zachować grafiki proste.


<a name="Video_Link" />

#### <a name="video-link"></a>Łącze wideo

Jest to adres URL wideo pokazujące aplikacji YouTube. Wideo należy 2 minuty długości 30 sekund i pokazują najlepsze części aplikacji.


<a name="pubgp" />

### <a name="publishing-to-google-play"></a>Publikowanie do witryny Google Play

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android 7.0 wprowadza zintegrowane przepływu pracy w przypadku publikowania aplikacji do witryny Google Play, z programu Visual Studio. Jeśli używasz platformy Xamarin Android w wersji starszej niż 7.0 możesz ręcznie przekazać Twojej APK za pomocą konsoli deweloperów Google Play. Ponadto należy skonfigurować co najmniej jeden APK już przekazany przed użyciem zintegrowanego przepływu pracy. Jeśli Twoje pierwsze APK nie zostały jeszcze przekazane możesz przekazać go ręcznie. Aby uzyskać więcej informacji, zobacz [ręcznie przekazać plik APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Tworzenie nowego świadectwa](~/android/deploy-test/signing/index.md#newcert), wyjaśniono, jak utworzyć nowy certyfikat podpisywania aplikacji systemu Android. Następnym krokiem jest do publikowania w podpisanej aplikacji ze sklepu Google Play:

1. Logowanie na koncie Google Play Developer, aby utworzyć nowy projekt, który jest połączony z kontem Google Play Developer.
2. Utwórz **klienta OAuth** który uwierzytelnia aplikacji.
3. Wprowadź wynikowy identyfikator klienta i klucz tajny klienta w programie Visual Studio.
4. Zarejestruj konto przy użyciu programu Visual Studio.
5. Podpisywania aplikacji przy użyciu certyfikatu.
6. Publikowanie podpisanej aplikacji do witryny Google Play.

W [archiwum publikowania](~/android/deploy-test/release-prep/index.md#archive), **kanału dystrybucji** okna dialogowego prezentowane dwa wybory dla dystrybucji: **Ad Hoc** i **Google Play** . Jeśli **tożsamość podpisywania** zamiast tego zostanie wyświetlone okno dialogowe, kliknij przycisk **ponownie** aby powrócić do **kanału dystrybucji** okna dialogowego. Wybierz **Google Play** i kliknij przycisk **dalej**:

[ ![Okno dialogowe kanału dystrybucji](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png)

W **tożsamość podpisywania** okno dialogowe, wybierz tożsamość utworzone w [Tworzenie nowego świadectwa](~/android/deploy-test/signing/index.md#newcert) i kliknij przycisk **Kontynuuj**:

[ ![Okno dialogowe tożsamość podpisywania](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png)

W **Google Play kont** okna dialogowego, kliknij przycisk  **+**  przycisk, aby dodać nowe konto Google Play:

[ ![Okno dialogowe konta usługi Google Play](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png)

W **zarejestrować dostępu do interfejsu API Google** okna dialogowego, należy podać _identyfikator klienta_ i _klucz tajny klienta_ , który zapewnia dostęp API do swojego konta Google Play Developer:

[ ![Okno dialogowe rejestracji dostępu do interfejsu API Google](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png)

Następnej sekcji opisano sposób tworzenia nowego projektu interfejsu API Google i generować wymagane _identyfikator klienta_ i _klucz tajny klienta_.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac ma zintegrowane przepływu pracy w przypadku publikowania aplikacji do witryny Google Play. Jeśli używasz Xamarin Studio w wersji starszej niż 5.9, należy ręcznie przekazać Twojej APK za pomocą konsoli deweloperów Google Play, a następnie użyć **publikowania do witryny Google Play** okna dialogowego podczas kolejnych aktualizacji APK. Ponadto należy skonfigurować co najmniej jeden APK już przekazany przed użyciem **publikowania do witryny Google Play**. Jeśli Twoje pierwsze APK nie zostały jeszcze przekazane możesz przekazać go ręcznie. Aby uzyskać informacje o sposobie ręcznego przekazania APK, zobacz [ręcznie przekazać plik APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Tworzenie nowego świadectwa](~/android/deploy-test/signing/index.md#newcert), omówione creatin nowy certyfikat podpisywania aplikacji systemu Android. Poniższe kroki przedstawiają sposób publikowania aplikacji platformy Xamarin.Android do witryny Google Play:

1. Logowanie na koncie Google Play Developer, aby utworzyć nowy projekt, który jest połączony z kontem Google Play Developer.
2. Utwórz _klienta OAuth_ który uwierzytelnia aplikacji.
3. Wprowadź powstałe w ten sposób _identyfikator klienta_ i _klucz tajny klienta_ do programu Visual Studio dla komputerów Mac.
4. Zarejestruj konto przy użyciu programu Visual Studio dla komputerów Mac.
5. Podpisywanie aplikacji przy użyciu certyfikatu.
6. Opublikuj podpisanej aplikacji do witryny Google Play.

W [archiwum publikowania](~/android/deploy-test/release-prep/index.md#archive), **utworzyć i dystrybuować...**  okna dialogowego prezentowane dwa wybory dla dystrybucji. Wybierz **Google Play**i kliknij przycisk **dalej**:

[ ![Wybierz opcję dystrybucji Android okna dialogowego](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png)

W **Google Play konto interfejsu API** okna dialogowego, należy podać _identyfikator klienta_ i _klucz tajny klienta_ , który zapewnia dostęp API do swojego konta Google Play Developer:

[ ![Okno dialogowe Google Play konto interfejsu API](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png)

Następnej sekcji opisano sposób tworzenia nowego projektu interfejsu API Google i generować wymagane _identyfikator klienta_ i _klucz tajny klienta_.

-----


#### <a name="create-a-google-api-project"></a>Utwórz projekt interfejsu API Google

Po pierwsze, zaloguj się do Twojego [konto Google Play dewelopera](https://play.google.com/apps/publish).
Jeśli nie masz już konto Google Play dewelopera, zobacz [wprowadzenie do publikowania](http://developer.android.com/distribute/googleplay/start.html).
Ponadto Google Play Developer interfejsu API [wprowadzenie](https://developers.google.com/android-publisher/getting_started) wyjaśniono, jak za pomocą interfejsu API Developer odtwarzania Google. Po zalogowaniu się do konsoli Google Play dewelopera, kliknij przycisk **ustawienia**:

[ ![Ikona ustawień](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png)

W **ustawienia** wybierz pozycję **dostępu do interfejsu API**i kliknij przycisk **Tworzenie nowego projektu** przycisk:

[ ![Tworzenie nowego przycisku projektu](images/02-create-new-project-sml.png)](images/02-create-new-project.png)

Po minucie lub to nowy projekt interfejsu API jest automatycznie generowany i połączony z kontem Google Play konsoli dewelopera.

Następnym krokiem jest można utworzyć klienta OAuth dla aplikacji (jeśli jest to jeden nie już utworzono). Gdy użytkownik zażąda dostępu do swoich danych za pomocą aplikacji, identyfikator klienta OAuth służy do uwierzytelniania aplikacji.
Kliknij przycisk **utworzyć klienta OAuth** do utworzenia nowego klienta OAuth:

[ ![Tworzenie klienta OAuth przycisku](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png)

Po kilku sekundach jest generowany nowy identyfikator klienta. Kliknij przycisk **widoku w konsoli deweloperów Google** wyświetlić nowy identyfikator klienta w konsoli dla deweloperów Google:

[ ![Wyświetla identyfikator klienta](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png)

Identyfikator klienta jest wyświetlana wraz z jego nazwy i tworzenia daty. Kliknij przycisk **edytowanie klienta OAuth** ikonę, aby wyświetlić klucz tajny klienta aplikacji:

[ ![Widok aplikacji poświadczeń](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png)

Domyślna nazwa klienta OAuth to *Google Play systemu Android Developer*. Można to zmienić nazwę aplikacji platformy Xamarin.Android lub dowolnego odpowiednią nazwę. W tym przykładzie nazwa klienta OAuth została zmieniona na nazwę aplikacji, **moja_aplikacja**:

[ ![Identyfikator klienta i klucz tajny wyświetlane](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png)

Kliknij przycisk **zapisać**można zapisać zmian. To polecenie zwróci do **poświadczenia** strony, skąd pobrać poświadczenia, klikając **Pobierz JSON** ikonę:

[ ![Pobierz ikonę JSON](images/07-download-json-sml.png)](images/07-download-json.png)

Ten plik JSON zawiera identyfikator klienta i klucz tajny klienta, który można wyciąć i wkleić do **logowania i rozpowszechnij** okna dialogowego w następnym kroku.


#### <a name="register-google-api-access"></a>Rejestrowanie dostępu do interfejsu API Google

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wykonać przy użyciu Identyfikatora klienta i klucz tajny klienta **Google Play konto interfejsu API** okna dialogowego w programie Visual Studio dla komputerów Mac. Można podać opis konta &ndash; umożliwia zarejestrować więcej niż jedno konto Google Play i przekazać przyszłych APK do różnych kont Google Play. Skopiuj identyfikator klienta i klucz tajny klienta do tego okna dialogowego, a następnie kliknij przycisk **zarejestrować**:

[ ![Okno dialogowe rejestracji dostępu do interfejsu API Google](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png)

Przeglądarki sieci web spowoduje uruchomienie i wyświetlenie monitu zaloguj się do konta Google Play dewelopera systemu Android (Jeśli użytkownik nie są już podpisane). Po zalogowaniu się w następujących monit jest wyświetlany w przeglądarce sieci web.
Kliknij przycisk **Zezwalaj** do autoryzacji aplikacji:

[ ![Zezwolić aplikacji w oknie dialogowym](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png)

#### <a name="publish"></a>Publish

Po kliknięciu przycisku **Zezwalaj**, przeglądarki raportów _Odebrano kod weryfikacyjny. Zamykanie..._  i aplikacja zostaje dodany do listy kont Google Play w programie Visual Studio. W **Google Play kont** okna dialogowego, kliknij przycisk **Kontynuuj**:

[ ![Konto dodane do usługi Google Play Acccounts](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png)

Następnie **Google Play Śledź** jest wyświetlone okno dialogowe. Google Play oferuje cztery możliwych ścieżek do przekazywania aplikacji:

* **Alpha** &ndash; używany do przekazywania bardzo wcześniejszą wersję aplikacji do krótką listę testerów.
* **W wersji beta** &ndash; używany do przekazywania do listy większych testerzy wcześniejszą wersję aplikacji.
* **Wdrożenie** &ndash; umożliwia procent użytkownikom odbieranie zaktualizowaną wersję aplikacji; umożliwia ona powoli zwiększać odsetek, 10% użytkowników i zwiększyć do 100% użytkowników podczas żelaza limit błędów.
* **Produkcji** &ndash; zaznacz tej ścieżki, gdy aplikacja jest gotowa do pełnego dystrybucji ze sklepu Google Play.

Wybierz opcję Śledź Google Play, które będzie służyć do przekazywania aplikacji i kliknij pozycję **przekazać**. W przypadku wybrania **wdrożenia**, należy określić wartość procentową:

[ ![Wybierz alfa, Beta, wdrożenia i produkcji](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png)

Aby uzyskać więcej informacji na temat testowania Google Play i wdrożeniach przemieszczanego, zobacz [Konfigurowanie testy alfa/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Okno dialogowe są prezentowane obok, wprowadź hasło dla certyfikatu podpisywania.
Wprowadź hasło i kliknij przycisk **OK**:

[ ![Podpisywanie hasło w oknie dialogowym](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png)

**Menedżera archiwum** Wyświetla postęp przekazywania:

[ ![Przekazywanie postęp APK](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png)

Po zakończeniu przekazywania, stan ukończenia jest wyświetlany w dolnym rogu po lewej stronie programu Visual Studio:

[ ![Komunikat ukończone publikowania projektu](images/vs/11-published-sml.png)](images/vs/11-published.png)


### <a name="troubleshooting"></a>Rozwiązywanie problemów

Należy pamiętać, że jeden APK musi już zostały przesłane do witryny Google Play przechowywania przed **publikowania do witryny Google Play** będzie działać. Jeśli nie zostało już załadowane APK Kreatora publikacji wyświetli następujący błąd w **błędy** okienka:

[ ![Musisz ręcznie przekazać Twojego pierwszego APK dla tej aplikacji](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png)

Podczas tego occures błąd ręcznie przekazać APK (np. kompilacja Ad Hoc) za pomocą konsoli Google Play deweloperów i użyj **kanału dystrybucji** okna dialogowego podczas kolejnych aktualizacji APK.  Aby uzyskać więcej informacji, zobacz [ręcznie przekazać plik APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Kod wersji plik APK musi zmienić z każdym przekazywania, w przeciwnym razie wystąpi następujący błąd:

[ ![APK z kodem wersji (1) został już zaktualizowany.](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png)

Aby rozwiązać ten problem, ponownie skompilować aplikację z numerem wersji o różnych i ponownie prześlij go do witryny Google Play za pośrednictwem **kanału dystrybucji** okna dialogowego.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Wykonać przy użyciu Identyfikatora klienta i klucz tajny klienta **Google Play konto interfejsu API** okna dialogowego w programie Visual Studio dla komputerów Mac. Można podać opis konta &ndash; umożliwia zarejestrować więcej niż jedno konto Google Play i przekazać przyszłych APK do różnych kont Google Play. Skopiuj identyfikator klienta i klucz tajny klienta do tego okna dialogowego, a następnie kliknij przycisk **zarejestrować**:

[ ![Autoryzuj dostęp do okna dialogowego](images/xs/10-register-sml.png)](images/xs/10-register.png)

Jeśli identyfikator klienta i klucz tajny klienta są akceptowane, **pomyślnej rejestracji** zostanie wyświetlony komunikat. Kliknij przycisk **dalej**:

[ ![Pomyślne komunikat dotyczący rejestracji](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png)

W **konta usługi Google Play** okno dialogowe, wybierz konto Google i ścieżki dla przekazywania aplikacji:

[ ![Wybierz w oknie dialogowym konta Google](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png)

Google Play oferuje cztery możliwych ścieżek do przekazywania aplikacji:

-   **Alpha** &ndash; używany do przekazywania bardzo wcześniejszą wersję aplikacji do krótką listę testerów.

-   **W wersji beta** &ndash; używany do przekazywania do listy większych testerzy wcześniejszą wersję aplikacji.

-   **Wdrożenie** &ndash; umożliwia procent użytkownikom odbieranie zaktualizowaną wersję aplikacji; umożliwia ona powoli zwiększać odsetek, 10% użytkowników i zwiększyć do 100% użytkowników podczas żelaza limit błędów.

-   **Produkcji** &ndash; zaznacz tej ścieżki, gdy aplikacja jest gotowa do pełnego dystrybucji ze sklepu Google Play.

Aby uzyskać więcej informacji na temat testowania Google Play i wdrożeniach przemieszczanego, zobacz [Konfigurowanie testy alfa/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Następnie wybierz tożsamości podpisywania, do którego zostanie użyty do podpisania aplikacji.
Wybierz **Użyj istniejącego klucza** Aby użyć istniejącej tożsamości podpisywania, w przeciwnym razie zapoznaj się z podręcznikiem [Tworzenie nowego świadectwa](~/android/deploy-test/signing/index.md#newcert) informacji o tworzeniu nowego klucza. Po wybraniu certyfikat do podpisania aplikacji, kliknij przycisk **dalej**:

[ ![Android dialogowe tożsamość podpisywania](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png)

W tym momencie można przekazać aplikację do witryny Google Play. **Publikowania do witryny Google Play** okno dialogowe zawiera podsumowanie informacji na temat aplikacji &ndash; kliknij **publikowania** do publikowania aplikacji ze sklepu Google Play:

[ ![Publikowanie do okna dialogowego Google Play](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png)

Należy pamiętać, że jeden APK musi już zostały przesłane do witryny Google Play przechowywania przed **publikowania do witryny Google Play** będzie działać. Jeśli nie zostało załadowane APK może wystąpić następujący błąd:

> _Google Play wymaga ręcznego przekazania Twojego pierwszego APK dla tej aplikacji. W tym służy APK ad hoc._

lub

> _Żadna aplikacja nie znaleziono nazwy danego pakietu. [404]_

Aby rozwiązać ten problem, ręcznie przekazać APK (np. kompilacja Ad Hoc) za pomocą konsoli Google Play deweloperów i użyj **publikowania do witryny Google Play** okna dialogowego podczas kolejnych aktualizacji APK. Aby uzyskać informacje o sposobie ręcznego przekazania APK, zobacz [ręcznie przekazać plik APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
