---
title: "Rejestrowanie przy użyciu linii papilarnych"
description: "Jak ustawić Konfigurowanie blokada ekranu i rejestrowanie na urządzeniu z systemem Android lub w emulatorze przy użyciu linii papilarnych."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20e6d693f2a3eba54afaf1d3c7054ad75d7a7610
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="enrolling-a-fingerprint"></a>Rejestrowanie przy użyciu linii papilarnych

## <a name="enrolling-a-fingerprint-overview"></a>Rejestrowanie Omówienie linii papilarnych

Jego jest możliwe tylko dla aplikacji systemu Android, aby wykorzystać odcisku palca uwierzytelniania, jeśli urządzenie zostało skonfigurowane przy użyciu odcisku palca uwierzytelniania. W tym przewodniku będzie omawiać temat rejestrowanie na urządzeniu z systemem Android lub w emulatorze przy użyciu linii papilarnych. Emulatory bez rzeczywistego sprzętu skanowanie linii papilarnych, ale można symulować odcisk palca skanowania za pomocą mostka debugowania systemu Android (opisanych poniżej).  W tym przewodniku będzie omawiać temat Włączanie blokada ekranu na urządzeniu z systemem Android i rejestracja przy użyciu linii papilarnych do uwierzytelniania.

## <a name="requirements"></a>Wymagania

Aby zarejestrować się przy użyciu linii papilarnych, muszą mieć urządzenia z systemem Android lub emulator uruchomiony interfejs API na poziomie 23 (Android 6.0).

Korzystanie z systemem Android debugowania Mostek (ADB) wymaga znajomości wiersza polecenia i **adb** pliku wykonywalnego musi być w ŚCIEŻCE programu Bash, programu PowerShell, lub polecenie monitu środowiska.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Konfigurowanie funkcji blokady ekranu i rejestrowanie przy użyciu linii papilarnych 

Aby skonfigurować blokady ekranu, wykonaj następujące czynności:

1. Przejdź do **Ustawienia > Zabezpieczenia**i wybierz **blokada ekranu**:

    ![Lokalizacja wyboru blokady ekranu na ekranie zabezpieczeń](enrolling-fingerprint-images/testing-01.png)

2. Kolejnym ekranie będzie pozwoli wybierz i skonfiguruj jedno z metody zabezpieczeń blokady ekranu: 

    ![Wybierz Przejdź, wzorzec, PIN lub hasła](enrolling-fingerprint-images/testing-02.png)

   Wybierz i wykonaj jedną z metod blokadę ekranu dostępne.

3. Po skonfigurowaniu screenlock, wróć do **Ustawienia > Zabezpieczenia** i wybrać opcję **odcisk palca**:

    ![Lokalizacja zaznaczenia odcisku palca na ekranie zabezpieczeń](enrolling-fingerprint-images/testing-03.png)

4. Z tego miejsca wykonaj sekwencji można dodać przy użyciu linii papilarnych do urządzenia:

    [![Sekwencja zrzuty ekranu do dodawania przy użyciu linii papilarnych do urządzenia](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Na ekranie końcowym zostanie wyświetlony monit o umieść palca na linii papilarnych: 

    ![Ekran z monitem o zawiesić palca czujnika](enrolling-fingerprint-images/testing-05.png)

    Jeśli używasz urządzenia z systemem Android, dokończ proces przez dotknięcie palca do skanera. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Symuluje skanowania odcisku palca w emulatorze

Na emulatorze systemu Android jest możliwe do symulowania odcisk palca skanowania przy użyciu mostka debugowania systemu Android. Po uruchomieniu OS X sesję terminala znajduje się w systemie Windows uruchom wiersz polecenia lub sesji programu Powershell i uruchom `adb`:

```shell
$ adb -e emu finger touch 1
```

Wartość **1** jest _palca\_identyfikator_ dla linii papilarnych, przeskanowanej "". To unikatowa liczba całkowita, która przypisanie dla każdego wirtualnego linii papilarnych. W przyszłości, gdy aplikacja jest uruchomiona, można można uruchomić tego samego ADB polecenie emulator po każdej aktualizacji monituje przy użyciu linii papilarnych, można uruchomić `adb` polecenia i przekaż go _palca\_identyfikator_ do symulowania skanowanie linii papilarnych .

Po zakończeniu skanowania odcisk palca Android zostanie wyświetlona dodano odcisk palca:  

![Wyświetlanie linii papilarnych dodany ekran!](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Podsumowanie 

W tym przewodniku opisano jak ustawienia ekranu blokady i rejestrowanie przy użyciu linii papilarnych na urządzeniu z systemem Android lub w emulatorze systemu Android. 

