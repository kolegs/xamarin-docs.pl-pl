---
title: Ustawianie wartości ułatwień dostępu w elementach interfejsu użytkownika
description: W tym artykule wyjaśniono, jak używać klasy AutomationProperties tak, aby czytnik ekranu mogą mówić o elementów na stronie.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 576fe34b0536c83825aa39bdd7111143d9474225
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998918"
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Ustawianie wartości ułatwień dostępu w elementach interfejsu użytkownika

_Zestaw narzędzi Xamarin.Forms umożliwia wartości ułatwień dostępu, należy ustawić na elementy interfejsu użytkownika przy użyciu dołączone właściwości z klasy AutomationProperties, które z kolei Ustawianie wartości ułatwień dostępu w macierzystym. W tym artykule wyjaśniono, jak używać klasy AutomationProperties tak, aby czytnik ekranu mogą mówić o elementów na stronie._

## <a name="overview"></a>Omówienie

Zestaw narzędzi Xamarin.Forms umożliwia wartości ułatwień dostępu, należy ustawić na elementy interfejsu użytkownika za pośrednictwem następujących właściwości dołączone:

- `AutomationProperties.IsInAccessibleTree` — Wskazuje, czy element jest dostępny do dostępnej aplikacji. Aby uzyskać więcej informacji, zobacz [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` — Krótki opis elementu, który służy jako speakable identyfikator elementu. Aby uzyskać więcej informacji, zobacz [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` — dłuższy opis elementu, który można traktować jako tekst etykietki narzędzia skojarzone z elementem. Aby uzyskać więcej informacji, zobacz [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` — zezwala na inny element zdefiniować informacje o ułatwieniach dostępu dla bieżącego elementu. Aby uzyskać więcej informacji, zobacz [AutomationProperties.LabeledBy](#labeledby).

Dołączony te wartości dostępności natywny zestaw właściwości, tak, aby czytnik ekranu mogą mówić o elemencie. Aby uzyskać więcej informacji na temat właściwości dołączone zobacz [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Za pomocą `AutomationProperties` dołączone właściwości mogą mieć wpływ na wykonywanie testów interfejsu użytkownika w systemie Android. [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` i `AutomationProperties.HelpText` obie właściwości ustawi natywnych `ContentDescription` właściwości, za pomocą `AutomationProperties.Name` i `AutomationProperties.HelpText` wartości właściwość, pierwszeństwo `AutomationId`wartości (jeśli oba `AutomationProperties.Name` i `AutomationProperties.HelpText` są ustawione wartości zostaną połączone). Oznacza to, że żadne testy, wyszukiwanie `AutomationId` zakończy się niepowodzeniem, jeśli `AutomationProperties.Name` lub `AutomationProperties.HelpText` również są ustawione dla elementu. W tym scenariuszu, należy zmienić testy interfejsu użytkownika do wyszukania wartość `AutomationProperties.Name` lub `AutomationProperties.HelpText`, lub połączenie obu tych elementów.

Czytnik zawartości ekranu w różnych komentować wartości dostępności dotyczy wszystkich platform:

- dla systemu iOS zawiera VoiceOver. Aby uzyskać więcej informacji, zobacz [testów dostępności na swoje urządzenie przy użyciu VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) na developer.apple.com.
- Android ma TalkBack. Aby uzyskać więcej informacji, zobacz [testowanie aplikacji ułatwień dostępu](https://developer.android.com/training/accessibility/testing.html#talkback) na developer.android.com.
- Windows ma Narrator. Aby uzyskać więcej informacji, zobacz [sprawdzić scenariusze głównego aplikacji za pomocą programu Narrator](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Jednak dokładne zachowanie czytnika ekranu zależy od oprogramowania oraz jego konfigurację użytkownika. Na przykład większość czytniki zawartości ekranu odczytać tekst skojarzony z formantem, po odebraniu fokus, umożliwiające użytkownikom zorientowania się zgodnie z ich poruszać się między formantów na stronie. Czytniki ekranu również przeczytać całą aplikację interfejsu użytkownika podczas zostanie wyświetlona strona, która umożliwia użytkownikowi otrzymywać wszystkie dostępne informacyjny zawartość strony ma przed podjęciem próby nawigować po nim.

Czytniki zawartości ekranu, zobacz temat wartości różnej dostępności. W przykładowej aplikacji:

- VoiceOver odczyta `Placeholder` wartość `Entry`, a następnie instrukcje dotyczące korzystania z formantu.
- Odczyta talkBack `Placeholder` wartość `Entry`, a następnie `AutomationProperties.HelpText` wartości, a następnie instrukcje dotyczące korzystania z formantu.
- Narrator odczyta `AutomationProperties.LabeledBy` wartość `Entry`, a następnie zgodnie z instrukcjami opisanymi na temat korzystania z formantu.

Ponadto program Narrator powoduje zwiększenie priorytetu `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, a następnie `AutomationProperties.HelpText`. W systemie Android może łączyć TalkBack `AutomationProperties.Name` i `AutomationProperties.HelpText` wartości. W związku z tym zaleca się, że testy dostępności dokładnego odbywa się na każdej platformie, aby upewnić się, zapewnienia optymalnego działania.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` Jest dołączona właściwość `boolean` określający, jeśli element jest dostępny i dlatego są widoczne, dla czytników zawartości ekranu. Musi być ustawione na `true` używać innych ułatwień dostępu dołączonych właściwości. Można to zrobić w XAML w następujący sposób:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternatywnie można ją ustawić w języku C# w następujący sposób:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Należy pamiętać, że [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metodę można również ustawić `AutomationProperties.IsInAccessibleTree` dołączona właściwość — `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Wartość dołączonej właściwości musi być ciągiem krótki i opisowy tekst korzystającą z czytnika zawartości ekranu w celu anonsowania elementu. Ta właściwość powinna być ustawiona dla elementów, które mają znaczenie, które są ważne dla zrozumienie zawartości lub interakcję z interfejsem użytkownika. Można to zrobić w XAML w następujący sposób:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Alternatywnie można ją ustawić w języku C# w następujący sposób:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Należy pamiętać, że [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metodę można również ustawić `AutomationProperties.Name` dołączona właściwość — `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Dołączonych właściwość powinna być ustawiona na tekst, który opisuje elementu interfejsu użytkownika i może być myślenia o jako tekst etykietki narzędzia skojarzony z elementem. Można to zrobić w XAML w następujący sposób:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Alternatywnie można ją ustawić w języku C# w następujący sposób:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Należy pamiętać, że [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metodę można również ustawić `AutomationProperties.HelpText` dołączona właściwość — `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Na niektórych platformach do edycji kontroluje takich jak [ `Entry` ](xref:Xamarin.Forms.Entry), `HelpText` właściwość czasami może być pominięty i zastąpić tekst symbolu zastępczego. Na przykład "Wprowadź tutaj nazwę" jest dobrym kandydatem do umieszczenia [ `Entry.Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) właściwość, która umieszcza tekst w kontrolce przed rzeczywistych danych wejściowych użytkownika.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Dołączoną właściwość zezwala na inny element zdefiniować informacje o ułatwieniach dostępu dla bieżącego elementu. Na przykład [ `Label` ](xref:Xamarin.Forms.Label) obok [ `Entry` ](xref:Xamarin.Forms.Entry) może służyć do opisywania, co `Entry` reprezentuje. Można to zrobić w XAML w następujący sposób:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Alternatywnie można ją ustawić w języku C# w następujący sposób:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Należy pamiętać, że [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metodę można również ustawić `AutomationProperties.IsInAccessibleTree` dołączona właściwość — `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak ustawić wartości ułatwień dostępu użytkownika elementów interfejsu w aplikacji platformy Xamarin.Forms przy użyciu dołączonych właściwości z `AutomationProperties` klasy. Dołączyć te wartości dostępności natywny zestaw właściwości, tak, aby czytnik ekranu mogą mówić o elementów na stronie.


## <a name="related-links"></a>Linki pokrewne

- [Dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md)
- [Ułatwienia dostępu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
