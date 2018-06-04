---
title: Edytowanie właściwości urządzenia wirtualnego systemu Android
description: W tym artykule wyjaśniono, jak edytować właściwości profilu wirtualnego urządzenia z systemem Android za pomocą Menedżera urządzeń systemu Android.
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 75ac85c67825e5db1b663d00f10eee6d093bfc1f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734045"
---
# <a name="editing-android-virtual-device-properties"></a>Edytowanie właściwości urządzenia wirtualnego systemu Android

_W tym artykule wyjaśniono, jak edytować właściwości profilu wirtualnego urządzenia z systemem Android za pomocą Menedżera urządzeń systemu Android._


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Menedżera urządzeń Android** obsługuje edycję właściwości profilu poszczególne urządzenia wirtualnego systemu Android. **Nowe urządzenie** i **Edytuj urządzenia** ekrany listy właściwości urządzenia wirtualnego w pierwszej kolumnie z odpowiednimi wartościami każdej właściwości w drugiej kolumnie (taki jak pokazano w tym przykładzie): 

[![Przykład ekranu nowego urządzenia](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

Po wybraniu właściwości szczegółowy opis tej właściwości jest wyświetlana po prawej stronie. Można zmodyfikować *właściwości profilu sprzętu* i *AVD właściwości*. Właściwości profilu sprzętu (takich jak `hw.ramSize` i `hw.accelerometer`) opisano charakterystyki fizycznej emulowanej urządzenia. Te właściwości obejmują rozmiaru ekranu, ilość dostępnej pamięci RAM, czy występuje przyspieszeniomierza. AVD właściwości określ operacji AVD, po uruchomieniu. Na przykład aby określić, jak AVD używa karty graficznej komputer deweloperski do renderowania można skonfigurować AVD właściwości.

Możesz zmienić właściwości, korzystając z poniższych wskazówek:

-   Aby zmienić właściwości typu boolean, kliknij znacznik wyboru z prawej strony właściwości typu boolean:

    ![Zmiana właściwości typu boolean](device-properties-images/win/02-boolean-value.png)

-   Aby zmienić *wyliczenia* właściwość (wyliczany), kliknij strzałkę w dół na prawo od właściwości i wybierz nową wartość.

    ![Zmiana właściwości wyliczenia](device-properties-images/win/04-enum-value.png)

-   Aby zmienić właściwość ciąg lub liczba całkowita, kliknij dwukrotnie bieżące ustawienie ciąg lub liczba całkowita w kolumnie wartość i wprowadź nową wartość.

    ![Zmiana właściwością liczby całkowitej.](device-properties-images/win/03-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Menedżera urządzeń Android** obsługuje edycję właściwości profilu poszczególne urządzenia wirtualnego systemu Android. **Nowe urządzenie** i **Edytuj urządzenia** ekrany listy właściwości urządzenia wirtualnego w pierwszej kolumnie z odpowiednimi wartościami każdej właściwości w drugiej kolumnie (taki jak pokazano w tym przykładzie): 

[![Przykład ekranu nowego urządzenia](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

Po wybraniu właściwości szczegółowy opis tej właściwości jest wyświetlana po prawej stronie. Można zmodyfikować *właściwości profilu sprzętu* i *AVD właściwości*. Właściwości profilu sprzętu (takich jak `hw.ramSize` i `hw.accelerometer`) opisano charakterystyki fizycznej emulowanej urządzenia. Te właściwości obejmują rozmiaru ekranu, ilość dostępnej pamięci RAM, czy występuje przyspieszeniomierza. AVD właściwości określ operacji AVD, po uruchomieniu. Na przykład aby określić, jak AVD używa karty graficznej komputer deweloperski do renderowania można skonfigurować AVD właściwości.

Możesz zmienić właściwości, korzystając z poniższych wskazówek:

-   Aby zmienić właściwości typu boolean, kliknij znacznik wyboru z prawej strony właściwości typu boolean:

    ![Zmiana właściwości typu boolean](device-properties-images/mac/02-boolean-value.png)

-   Aby zmienić *wyliczenia* właściwość (wyliczany), kliknij menu rozwijane z prawej strony właściwości i wybierz nową wartość.

    ![Zmiana właściwości wyliczenia](device-properties-images/mac/04-enum-value.png)

-   Aby zmienić właściwość ciąg lub liczba całkowita, kliknij dwukrotnie bieżące ustawienie ciąg lub liczba całkowita w kolumnie wartość i wprowadź nową wartość.

    ![Zmiana właściwością liczby całkowitej.](device-properties-images/mac/03-integer-value.png)

-----

W poniższej tabeli przedstawiono szczegółowy opis właściwości wymienionych w **nowe urządzenie** i **Edytor urządzenia** ekrany:

[!include[](~/android/includes/emulator-properties.md)]

Aby uzyskać więcej informacji na temat tych właściwości, zobacz [właściwości profilu sprzętu](https://developer.android.com/studio/run/managing-avds.html#hpproperties).

