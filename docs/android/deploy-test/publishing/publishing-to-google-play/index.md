---
title: Publikowanie w sklepie Google Play
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3525541ba0795f4e0b174b155c0ca219e3257bac
ms.sourcegitcommit: 6433b424410a850f504e0f934bbb5baf8f093e49
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067363"
---
# <a name="publishing-to-google-play"></a>Publikowanie w sklepie Google Play

Chociaż istnieje wiele rynków aplikację do dystrybucji aplikacji sklepu Google Play jest prawdopodobnie największy i najczęściej odwiedzanych magazynu na świecie aplikacji systemu Android. Google Play zapewnia platformę jednym dystrybucję, reklamy, sprzedaży i analizowania sprzedaży aplikacji systemu Android.

Ta sekcja obejmuje tematów, które są specyficzne dla sklepu Google Play, takich jak rejestrowanie zostać wydawcą zasoby ułatwiające Google Play, podwyższanie poziomu i anonsowanie aplikacji, wytyczne dotyczące oceny aplikacji ze sklepu Google Play i przy użyciu filtrów do zbierania Ogranicz wdrożenia aplikacji na niektórych urządzeniach.


## <a name="requirements"></a>Wymagania

Aby rozpowszechnić aplikację za pośrednictwem sklepu Google Play, można utworzyć konta dewelopera. To tylko należy wykonać jeden raz i obejmują jeden czas opłata w wysokości 25 USD.

Wszystkie aplikacje muszą być podpisane za pomocą klucza kryptograficznego, który wygasa po 22 października 2033 roku.

Maksymalny rozmiar pliku APK, opublikowane w witrynie Google Play wynosi 100MB. Jeśli aplikacja przekroczy ten rozmiar, Google Play umożliwi dodatkowe zasoby, które mają zostać dostarczone za pośrednictwem *pliki rozszerzania zestawu APK*. Pliki rozszerzania android zezwolić APK 2 dodatkowe pliki, każde z nich znajdują się maksymalnie 2GB. Google Play będzie hostować i rozpowszechniać następujące pliki bez ponoszenia kosztów. Pliki rozszerzania zostanie dokładnie omówione w innej sekcji.

Google Play nie jest dostępna globalnie. Niektórych miejscach jest nieobsługiwana dla dystrybucji aplikacji.



## <a name="becoming-a-publisher"></a>Staje się z wydawcą

Aby opublikować aplikacje ze sklepu Google play, jest konieczne konta wydawcy. Aby się zarejestrować dla wydawcy konto, wykonaj następujące kroki:

1.  Odwiedź stronę [konsoli dewelopera Google Play](https://play.google.com/apps/publish).
1.  Wprowadzanie podstawowych informacji o tożsamości dla deweloperów.
1.  Przeczytaj i zaakceptuj umowę dystrybucji dla deweloperów dla ustawień regionalnych.
1.  Uiszczenie rejestracji 25 USD.
1.  Potwierdź weryfikację za pośrednictwem poczty e-mail.
1.  Po utworzeniu konta istnieje możliwość publikowania aplikacji przy użyciu sklepu Google Play.


Google Play nie obsługuje wszystkich krajów na całym świecie. Najbardziej aktualne listę krajów, można znaleźć w następujących łączy:

1.  [Obsługiwane lokalizacje dla dewelopera &amp; rejestracja sprzedawcy](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; jest to lista wszystkich krajów, w której deweloperzy mogą zarejestrować jako temu handlowcy mogą tworzyć i sprzedawać aplikacje płatne.

1.  [Obsługiwane lokalizacje dla dystrybucji użytkownikom sklepu Google Play](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; jest to lista wszystkich krajach, w których aplikacje mogą być rozpowszechniane.



### <a name="preparing-promotional-assets"></a>Trwa przygotowywanie promocyjne zasobów

Skutecznie promowania i anonsowanie aplikacji ze sklepu Google Play, Google umożliwia deweloperom przesyłanie promocyjne zasoby, takie jak zrzuty ekranu, grafiki i wideo do przesłania. Google Play reklamy i promocji aplikacja będzie używać tych zasobów.



#### <a name="launcher-icons"></a>Ikony uruchamianie programu

A *ikonę uruchamiania* jest reprezentującą aplikację. Każda ikona uruchamiania powinna być 32-bitowych PNG z kanału alfa przezroczystości. Aplikacja powinna mieć ikon dla wszystkich gęstości uogólnionego ekranu, tak jak pokazano na poniższej liście:

-   **ldpi** (120dpi) &ndash; 36 x 36 piks.
-   **mdpi** (160dpi) &ndash; 48 x 48 piks.
-   **hdpi** (240dpi) &ndash; 72 x 72 pikseli
-   **xhdpi** (320dpi) &ndash; 96 x 96 pikseli


Uruchamianie ikony są pierwszych czynności, które użytkownik będzie widział aplikacje ze sklepu Google Play, więc należy z rozwagą się ikony uruchamianie wizualnie atrakcyjne i zrozumiała.

Porady dotyczące uruchamiania ikon:

1.  **Proste i pozostał** &ndash; ikony uruchamianie powinny być przechowywane, prosty i pozostał. Oznacza to, z wyłączeniem nazwę aplikacji z ikony. Prostsze ikony będzie łatwiej zapamiętać, a będzie można łatwiej odróżnić na mniejsze rozmiary.

1.  **Ikony nie powinny być elastycznie** &ndash; nadmiernie alokowania elastycznego ikony nie będą wyróżniały się również na wszystkim programistom.

1.  **Przy użyciu kanału alfa** &ndash; ikony należy wprowadzić użytkowania kanał alfa, i nie powinny być obrazów pełnej w ramce.



#### <a name="high-resolution-application-icons"></a>Ikony aplikacji o wysokiej rozdzielczości

Aplikacje ze sklepu Google Play wymagają o dużej wierności wersję ikony aplikacji. Jest używana tylko przez Google Play, a nie zastępuje ikonę uruchamiania aplikacji. Wymagania techniczne dla ikony o wysokiej rozdzielczości są:

1.  32-bitowych PNG z kanał alfa
1.  512 x 512 pikseli
1.  Maksymalny rozmiar 1024KB

[Android Studio zasobów](https://romannurik.github.io/AndroidAssetStudio/) jest przydatne narzędzie do tworzenia, uruchamiania odpowiednie ikony i ikony aplikacji o wysokiej rozdzielczości.



#### <a name="screen-shots"></a>Zrzuty ekranu

Sklep Google play wymaga co najmniej dwóch i maksymalnie osiem zrzuty ekranu aplikacji. Będą one wyświetlane na stronie szczegółów aplikacji w sklepie Google Play.

Specyfikacje dla zrzuty ekranu są następujące:

1.  24 bity PNG lub JPG o żaden kanał alfa
1.  320w x 480h lub 480w x 800h lub 480w x 854 h. Obrazy landscaped zostaną przycięte.



#### <a name="promotional-graphic"></a>Promocyjna grafiki

Jest to opcjonalne obrazu użytego przez Google Play:

1.  Jest 180w x 120 h 24 bit PNG lub JPG żaden kanał alfa.
1.  Brak obramowania w grafikę.



#### <a name="feature-graphic"></a>Funkcja grafiki

Używane przez sekcji promocji Sklepu Google Play. Ta grafika mogą pojawiać się samodzielnie bez ikony aplikacji.

1.  1024w x 500 godzin PNG lub JPG żaden kanał alfa i bez przezroczystości.
1.  Wszystkie ważne zawartość powinna być w ramce 924 x 500. Piksele spoza tej ramki może zostać obcięty do celów stylistycznych.
1.  Ta grafika można skalować w dół: Użyj dużego tekstu i grafiki proste.



#### <a name="video-link"></a>Link do wideo

Jest to adres URL do usługi YouTube wideo przedstawiające aplikacji. Film wideo powinien być 30 sekund do 2 minut długości i prezentowania najlepsze części aplikacji.



### <a name="publishing-to-google-play"></a>Publikowanie w sklepie Google Play

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Program Xamarin Android 7.0 wprowadza zintegrowany przepływ pracy w przypadku publikowania aplikacji do sklepu Google Play z programu Visual Studio. Korzystania z platformy Xamarin Android w wersji starszej niż 7.0 możesz ręcznie przekazać swoje APK za pośrednictwem konsoli programisty Google Play. Ponadto musisz mieć co najmniej jednego pliku APK już przekazany, zanim będzie można użyć zintegrowany przepływ pracy. Jeśli nie została jeszcze przesłana Twojego pierwszego pliku APK, możesz przekazać go ręcznie. Aby uzyskać więcej informacji, zobacz [ręczne przekazywanie zestawu APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Tworzenie nowego certyfikatu](~/android/deploy-test/signing/index.md#newcert), wyjaśniono, jak utworzyć nowy certyfikat do podpisywania aplikacji systemu Android. Następnym krokiem jest do publikowania podpisaną aplikację do sklepu Google Play:

1. Zaloguj się do swojego konta Google Play Developer, aby utworzyć nowy projekt, który jest połączony z Twoim kontem Google Play Developer.
2. Tworzenie **klienta OAuth** uwierzytelnia się aplikacji.
3. Wprowadź wynikowy Identyfikatora klienta oraz klucz tajny klienta w programie Visual Studio.
4. Zarejestruj swoje konto programu Visual Studio.
5. Podpisz aplikację za pomocą certyfikatu.
6. Opublikuj Twoją podpisaną aplikację do sklepu Google Play.

W [archiwum dla publikowania](~/android/deploy-test/release-prep/index.md#archive), **kanał dystrybucji** okna dialogowego wyświetlone dwa wybory dla dystrybucji: **Ad-Hoc** i **sklepu Google Play** . Jeśli **tożsamość podpisywania** zamiast tego zostanie wyświetlone okno dialogowe, kliknij przycisk **ponownie** aby powrócić do **kanał dystrybucji** okna dialogowego. Wybierz **sklepu Google Play** i kliknij przycisk **dalej**:

[![Okno dialogowe kanał dystrybucji](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

W **tożsamość podpisywania** okno dialogowe, wybierz opcję tożsamości są tworzone w [Tworzenie nowego certyfikatu](~/android/deploy-test/signing/index.md#newcert) i kliknij przycisk **Kontynuuj**:

[![Okno dialogowe tożsamości podpisywania](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

W **kont usługi Google Play** okno dialogowe, kliknij przycisk **+** przycisk, aby dodać nowe konto Google Play:

[![Okno dialogowe kont usługi Google Play](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

W **zarejestrować Google dostęp do interfejsu API** okno dialogowe, należy podać _identyfikator klienta_ i _klucz tajny klienta_ zapewniający dostęp do interfejsu API do swojego konta Google Play Developer:

[![Okno dialogowe rejestracji dostęp do interfejsu API Google](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

W następnej sekcji objaśniono sposób tworzenia nowego projektu interfejsu API Google i wygenerować potrzebną _identyfikator klienta_ i _klucz tajny klienta_.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Program Visual Studio dla komputerów Mac zawiera zintegrowany przepływ pracy w przypadku publikowania aplikacji do sklepu Google Play. Jeśli używasz programu Xamarin Studio w wersji starszej niż 5.9. należy ręcznie przekazać swoje APK za pośrednictwem konsoli programisty Google Play, a następnie użyć **Publikuj w sklepie Google Play** okno dialogowe podczas kolejnych aktualizacji pliku APK. Ponadto musisz mieć co najmniej jednego pliku APK już przekazany, zanim będzie można użyć **Publikuj w sklepie Google Play**. Jeśli nie została jeszcze przesłana Twojego pierwszego pliku APK, możesz przekazać go ręcznie. Aby uzyskać informacje o sposobie ręcznego przekazania pliku APK, zobacz [ręczne przekazywanie zestawu APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Tworzenie nowego certyfikatu](~/android/deploy-test/signing/index.md#newcert)omawianej, tworząc nowy certyfikat do podpisywania aplikacji systemu Android. Poniższe kroki przedstawiają sposób publikowania aplikacji platformy Xamarin.Android w sklepie Google Play:

1. Zaloguj się do swojego konta Google Play Developer, aby utworzyć nowy projekt, który jest połączony z Twoim kontem Google Play Developer.
2. Tworzenie _klienta OAuth_ uwierzytelnia się aplikacji.
3. Wprowadź wartość wynikowa _identyfikator klienta_ i _klucz tajny klienta_ do programu Visual Studio dla komputerów Mac.
4. Zarejestruj swoje konto programu Visual Studio dla komputerów Mac.
5. Utwórz aplikację za pomocą certyfikatu.
6. Opublikuj swoje podpisaną aplikację do sklepu Google Play.

W [archiwum dla publikowania](~/android/deploy-test/release-prep/index.md#archive), **podpisywanie i dystrybucja...**  okna dialogowego wyświetlone dwa wybory dla dystrybucji. Wybierz **sklepu Google Play**i kliknij przycisk **dalej**:

[![Okno dialogowe wyboru dystrybucji dla systemu Android](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

W **konto interfejsu API Google Play** okno dialogowe, należy podać _identyfikator klienta_ i _klucz tajny klienta_ zapewniający dostęp do interfejsu API do swojego konta Google Play Developer:

[![Okno dialogowe konto interfejsu API Google Play](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

W następnej sekcji objaśniono sposób tworzenia nowego projektu interfejsu API Google i wygenerować potrzebną _identyfikator klienta_ i _klucz tajny klienta_.

-----


#### <a name="create-a-google-api-project"></a>Utwórz projekt interfejsu API Google

Najpierw zaloguj się do Twojej [konta Google Play Developer](https://play.google.com/apps/publish).
Jeśli nie masz jeszcze konta Google Play Developer, zobacz [wprowadzenie do publikowania](http://developer.android.com/distribute/googleplay/start.html).
Ponadto Google Play Developer interfejsu API [wprowadzenie](https://developers.google.com/android-publisher/getting_started) wyjaśnia, jak używać interfejsu API dla deweloperów Google Play. Po zalogowaniu się do konsoli programisty Google Play, kliknij przycisk **ustawienia**:

[![Ikona ustawienia](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png#lightbox)

W **ustawienia** wybierz opcję **dostęp do interfejsu API**i kliknij przycisk **Tworzenie nowego projektu** przycisku:

[![Przycisk Utwórz nowy projekt](images/02-create-new-project-sml.png)](images/02-create-new-project.png#lightbox)

Po około minucie nowy projekt interfejsu API jest automatycznie generowany i połączone z Twoim kontem konsoli programisty Google Play.

Następnym krokiem jest do utworzenia klienta OAuth dla aplikacji (jeśli jeden nie utworzono już). Użytkownicy żądania dostępu do prywatnych danych przy użyciu aplikacji, Identyfikatora klienta uwierzytelniania OAuth jest używany do uwierzytelniania aplikacji.
Kliknij przycisk **Utwórz klienta OAuth** do utworzenia nowego klienta OAuth:

[![Tworzenie przycisku klienta OAuth](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

Po kilku sekundach jest generowany nowy identyfikator klienta. Kliknij przycisk **widoku w konsoli deweloperów Google** Aby wyświetlić nowy identyfikator klienta w taki sposób, w konsoli dewelopera Google:

[![Wyświetla identyfikator klienta](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

Identyfikator klienta jest wyświetlany wzdłuż daty jego nazwę i tworzenia. Kliknij przycisk **Edycja klienta OAuth** ikonę, aby wyświetlić klucz tajny klienta aplikacji:

[![Widok aplikacji poświadczeń](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

Domyślna nazwa klienta OAuth to *Google Play Android Developer*. Można to zmienić nazwę aplikacji platformy Xamarin.Android lub odpowiednią nazwę. W tym przykładzie nazwa klienta OAuth została zmieniona na nazwę aplikacji, **MyApp**:

[![Identyfikator klienta i wpis tajny wyświetlane](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

Kliknij przycisk **Zapisz**można zapisać zmian. Nastąpi powrót do **poświadczenia** strony, skąd pobrać poświadczenia, klikając **Pobierz JSON** ikony:

[![Pobierz ikonę JSON](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

Ten plik JSON zawiera identyfikator klienta i klucz tajny klienta, który można wyciąć i wkleić do **logowania i rozłożenie** okna dialogowego w następnym kroku.


#### <a name="register-google-api-access"></a>Zarejestruj dostęp do interfejsu API Google

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Użyj Identyfikatora klienta oraz klucz tajny klienta, aby ukończyć **konto interfejsu API Google Play** okna dialogowego w programie Visual Studio dla komputerów Mac. Można nadać kontu opis &ndash; dzięki temu można zarejestrować więcej niż jedno konto Google Play i przekaż przyszłych APK do różnych kont Google Play. Skopiuj identyfikator klienta oraz klucz tajny klienta do tego okna dialogowego, a następnie kliknij przycisk **zarejestrować**:

[![Okno dialogowe rejestracji dostęp do interfejsu API Google](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Przeglądarki sieci web otworzy się i wyświetlenie monitu zaloguj się do swojego konta Google Play Developer systemu Android (Jeśli użytkownik są nie już zalogowany). Po zalogowaniu się w następujący monit jest wyświetlany w przeglądarce sieci web.
Kliknij przycisk **Zezwalaj** Aby autoryzować aplikację:

[![Autoryzowanie aplikacji w oknie dialogowym](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>Publikowanie

Po kliknięciu przycisku **Zezwalaj**, raporty przeglądarki _otrzymały kod weryfikacyjny. Zamykanie..._  i aplikacja zostanie dodany do listy kont Google Play w programie Visual Studio. W **kont usługi Google Play** okno dialogowe, kliknij przycisk **Kontynuuj**:

[![Konto zostało dodane do usługi Google Play Acccounts](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

Następnie **Google Play śledzenie** zobaczy okno dialogowe. Google Play oferuje cztery możliwe ścieżki przekazywania aplikacji:

* **Alfa** &ndash; używany do przekazywania wczesnym wersję aplikacji do krótką listę testerów.
* **W wersji beta** &ndash; używany do przekazywania wcześniejszą wersję aplikacji do większą listę testerów.
* **Wdrożenie** &ndash; umożliwia procent użytkowników, którzy otrzymają zaktualizowanej wersji aplikacji, a dzięki temu możliwe powoli zwiększenie procent użytkowników, że 10% i zwiększyć do 100% użytkowników podczas żelaza się błędy.
* **Produkcji** &ndash; wybierz tę ścieżkę, gdy aplikacja jest gotowa na pełne dystrybucji ze sklepu Google Play.

Wybierz Ścieżka usługi Google Play, która będzie służyć do przekazywania aplikacji i kliknij pozycję **przekazywanie**. Jeśli wybierzesz **wdrożenia**, należy się upewnić, że wartość procentowa:

[![Wybierz alfa, Beta, wprowadzania lub produkcji](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png#lightbox)

Aby uzyskać więcej informacji o testowaniu sklepu Google Play i wdrożeniach przygotowanych, zobacz [skonfigurować testy alfa/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Okno dialogowe jest przedstawiony następnie wprowadź hasło dla certyfikatu podpisywania.
Wprowadź hasło, a następnie kliknij przycisk **OK**:

[![Okno dialogowe dotyczące hasła logowania](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

**Menedżer archiwów** Wyświetla postęp przekazywania:

[![Plik APK postęp przekazywania pliku](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

Po zakończeniu przekazywania, stan ukończenia zostały przedstawione w dolnym lewym dolnym rogu programu Visual Studio:

[![Komunikat ukończone publikowania projektu](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)


### <a name="troubleshooting"></a>Rozwiązywanie problemów

Należy pamiętać, że jednego pliku APK musi już zostały przesłane do sklepu Google Play Przechowuj przed **Publikuj w sklepie Google Play** będzie działać. Jeśli plik APK nie została już przekazana Kreatora publikacji wyświetli się następujący błąd w **błędy** okienka:

[![Należy ręcznie przekazać Twojego pierwszego pliku APK dla tej aplikacji](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

Po tym occures błąd ręcznie przekazać pakiet APK (na przykład kompilacji Ad-Hoc) za pośrednictwem konsoli programisty Google Play i używać **kanał dystrybucji** okno dialogowe podczas kolejnych aktualizacji pliku APK.  Aby uzyskać więcej informacji, zobacz [ręczne przekazywanie zestawu APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Kod wersji zestawu APK zmienić za pomocą każdego przekazywania, w przeciwnym razie wystąpi następujący błąd:

[![Plik APK z kodem wersji (1) został już zaktualizowany.](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

Aby rozwiązać ten problem, ponownie kompilować aplikacji z liczbą inną wersję i ponownie prześlij go do usługi Google Play za pośrednictwem **kanał dystrybucji** okna dialogowego.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Użyj Identyfikatora klienta oraz klucz tajny klienta, aby ukończyć **konto interfejsu API Google Play** okna dialogowego w programie Visual Studio dla komputerów Mac. Można nadać kontu opis &ndash; dzięki temu można zarejestrować więcej niż jedno konto Google Play i przekaż przyszłych APK do różnych kont Google Play. Skopiuj identyfikator klienta oraz klucz tajny klienta do tego okna dialogowego, a następnie kliknij przycisk **zarejestrować**:

[![Autoryzuj dostęp do okna dialogowego](images/xs/10-register-sml.png)](images/xs/10-register.png#lightbox)

Jeśli identyfikator klienta oraz klucz tajny klienta są akceptowane, **pomyślnej rejestracji** zostanie wyświetlony komunikat. Kliknij przycisk **dalej**:

[![Komunikat o pomyślnej rejestracji](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png#lightbox)

W **konto Google Play** okno dialogowe, wybierz konto Google i Śledź przekazywania aplikacji:

[![Wybierz okno dialogowe konta Google](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png#lightbox)

Google Play oferuje cztery możliwe ścieżki przekazywania aplikacji:

-   **Alfa** &ndash; używany do przekazywania wczesnym wersję aplikacji do krótką listę testerów.

-   **W wersji beta** &ndash; używany do przekazywania wcześniejszą wersję aplikacji do większą listę testerów.

-   **Wdrożenie** &ndash; umożliwia procent użytkowników, którzy otrzymają zaktualizowanej wersji aplikacji, a dzięki temu możliwe powoli zwiększenie procent użytkowników, że 10% i zwiększyć do 100% użytkowników podczas żelaza się błędy.

-   **Produkcji** &ndash; wybierz tę ścieżkę, gdy aplikacja jest gotowa na pełne dystrybucji ze sklepu Google Play.

Aby uzyskać więcej informacji o testowaniu sklepu Google Play i wdrożeniach przygotowanych, zobacz [skonfigurować testy alfa/beta](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Następnie wybierz tożsamości podpisywania, będzie ono używane do podpisania aplikacji.
Wybierz **Użyj istniejącego klucza** użyć istniejącej tożsamości podpisywania, a w przeciwnym razie sprawdź w przewodniku [Tworzenie nowego certyfikatu](~/android/deploy-test/signing/index.md#newcert) informacji o tworzeniu nowego klucza. Po wybraniu certyfikat do podpisania aplikacji, kliknij przycisk **dalej**:

[![Android dialogowe tożsamości podpisywania](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png#lightbox)

W tym momencie można przekazać aplikacji do sklepu Google Play. **Publikuj w sklepie Google Play** okno dialogowe zawiera podsumowanie informacji na temat aplikacji &ndash; kliknij **Publikuj** do publikowania aplikacji do sklepu Google Play:

[![Publikowanie w sklepie Google Play w oknie dialogowym](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png#lightbox)

Należy pamiętać, że jednego pliku APK musi już zostały przesłane do sklepu Google Play Przechowuj przed **Publikuj w sklepie Google Play** będzie działać. Jeśli nie zostało załadowane przez plik APK może wystąpić następujący błąd:

> _Sklep Google Play wymaga ręcznego przekazania Twojego pierwszego pliku APK dla tej aplikacji. W tym, można użyć pliku APK ad hoc._

lub

> _Żadna aplikacja nie został znaleziony dla nazwy danego pakietu. [404]_

Aby rozwiązać ten problem, ręcznie przekazać pakiet APK (na przykład kompilacji Ad-Hoc) za pośrednictwem konsoli programisty Google Play i użyj **Publikuj w sklepie Google Play** okno dialogowe podczas kolejnych aktualizacji pliku APK. Aby uzyskać informacje o sposobie ręcznego przekazania pliku APK, zobacz [ręczne przekazywanie zestawu APK](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
