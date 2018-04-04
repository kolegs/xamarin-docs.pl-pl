---
title: Ustawienie wartości dostępności na elementy interfejsu użytkownika
description: Platformy Xamarin.Forms umożliwia wartości dostępności ma być ustawiony na elementy interfejsu użytkownika przy użyciu dołączone właściwości z klasy AutomationProperties, które z kolei wartości dostępności natywny zestaw. W tym artykule opisano sposób użycia klasy AutomationProperties, dzięki czemu czytnika ekranu można mowy o elementach na stronie.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: cf9071684061b584e1cb75cfd50b33212f42bf79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Ustawienie wartości dostępności na elementy interfejsu użytkownika

_Platformy Xamarin.Forms umożliwia wartości dostępności ma być ustawiony na elementy interfejsu użytkownika przy użyciu dołączone właściwości z klasy AutomationProperties, które z kolei wartości dostępności natywny zestaw. W tym artykule opisano sposób użycia klasy AutomationProperties, dzięki czemu czytnika ekranu można mowy o elementach na stronie._

## <a name="overview"></a>Omówienie

Platformy Xamarin.Forms umożliwia wartości dostępności ma być ustawiony na elementy interfejsu użytkownika za pomocą następujących dołączone właściwości:

- `AutomationProperties.IsInAccessibleTree` — Wskazuje, czy element jest dostępny do dostępnych aplikacji. Aby uzyskać więcej informacji, zobacz [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` — Krótki opis elementu, który służy jako speakable identyfikator dla elementu. Aby uzyskać więcej informacji, zobacz [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` — Opis dłużej element, który można traktować jako tekst etykietki narzędzia skojarzone z elementem. Aby uzyskać więcej informacji, zobacz [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` — Umożliwia inny element zdefiniować informacje dotyczące ułatwień dostępu dla bieżącego elementu. Aby uzyskać więcej informacji, zobacz [AutomationProperties.LabeledBy](#labeledby).

Wartości dostępności natywny zestaw właściwości je dołączyć tak, aby do odczytywania zawartości ekranu mogą mowy o elemencie. Aby uzyskać więcej informacji na temat dołączone właściwości, zobacz [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Przy użyciu `AutomationProperties` dołączone właściwości mogą mieć wpływ na wykonanie testu interfejsu użytkownika w systemie Android. [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` i `AutomationProperties.HelpText` obie właściwości ustawi natywnego `ContentDescription` właściwości, z `AutomationProperties.Name` i `AutomationProperties.HelpText` wartości właściwości pierwszeństwo `AutomationId`wartość (jeśli obie `AutomationProperties.Name` i `AutomationProperties.HelpText` są ustawione wartości zostaną połączone). Oznacza to, że wszystkie testy wyszukiwanie `AutomationId` zakończy się niepowodzeniem, jeśli `AutomationProperties.Name` lub `AutomationProperties.HelpText` również są ustawione w elemencie. W tym scenariuszu, należy zmienić testów interfejsu użytkownika do wyszukania wartość `AutomationProperties.Name` lub `AutomationProperties.HelpText`, lub łączenia obu.

Dotyczy wszystkich platform inny czytnik komentować wartości dostępności:

- iOS ma VoiceOver. Aby uzyskać więcej informacji, zobacz [testu dostępności na tym urządzeniu za pomocą VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) na developer.apple.com.
- Android ma TalkBack. Aby uzyskać więcej informacji, zobacz [testowania aplikacji na dostępność](https://developer.android.com/training/accessibility/testing.html#talkback) na developer.android.com.
- System Windows ma Narrator. Aby uzyskać więcej informacji, zobacz [Sprawdź scenariusze aplikacji głównej, przy użyciu Narrator](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Jednak dokładne zachowanie czytnika ekranu zależy od oprogramowania oraz jego konfigurację użytkownika. Na przykład większość czytników ekranu odczytać tekst skojarzony z formantem po odebraniu fokus, umożliwiając użytkownikom zorientowania się, jak przechodzą między formantami na stronie. Czytniki ekranu, zobacz temat interfejsu użytkownika całej aplikacji kiedy zostanie wyświetlona strona, który umożliwia użytkownikowi uprawnia do wszystkich dostępnych informacyjną zawartość strony ma przed próbą przejścia go.

Czytników ekranu, zobacz temat ułatwień dostępu w różnych wartości. W przykładowej aplikacji:

- Odczytuje voiceOver `Placeholder` wartość `Entry`, a następnie instrukcje dotyczące używania formantu.
- Odczytuje talkBack `Placeholder` wartość `Entry`, a następnie `AutomationProperties.HelpText` wartość następuje instrukcje dotyczące używania formantu.
- Narrator odczyta `AutomationProperties.LabeledBy` wartość `Entry`, a następnie instrukcje dotyczące używania formantu.

Ponadto priorytetów Narrator `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, a następnie `AutomationProperties.HelpText`. W systemie Android, może łączyć TalkBack `AutomationProperties.Name` i `AutomationProperties.HelpText` wartości. W związku z tym zaleca się, że testowania dostępności dokładnego odbywa się na każdej platformie zapewnienie zapewnienia optymalnego działania.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` Jest dołączona właściwość `boolean` Określa, czy element jest dostępny, a więc widoczne dla czytników ekranu. Należy wybrać opcję `true` do używania innych dostępność dołączone właściwości. Można to zrobić w języku XAML w następujący sposób:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternatywnie można ją ustawić w języku C# w następujący sposób:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Należy pamiętać, że [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody można również ustawić `AutomationProperties.IsInAccessibleTree` dołączona właściwość — `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Wartość dołączona właściwość powinna być ciąg krótki i opisowy tekst, który używa czytniki ogłaszamy elementu. Tej właściwości należy ustawić dla elementów, które mają znaczenie, które są ważne dla opis zawartości lub interakcję z interfejsem użytkownika. Można to zrobić w języku XAML w następujący sposób:

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
> Należy pamiętać, że [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody można również ustawić `AutomationProperties.Name` dołączona właściwość — `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Dołączonych właściwości należy ustawić wartość na tekst, który opisuje elementu interfejsu użytkownika i może być myśl z jako tekst etykietki narzędzia skojarzone z elementem. Można to zrobić w języku XAML w następujący sposób:

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
> Należy pamiętać, że [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody można również ustawić `AutomationProperties.HelpText` dołączona właściwość — `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Na niektórych platformach do edycji kontrolki takich jak [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), `HelpText` właściwości czasami może być pominięty i zastąpione tekst zastępczy. Na przykład "Wprowadź tutaj nazwę", są odpowiednimi kandydatami do [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) właściwości, które umieszcza tekst w formancie przed rzeczywistych danych wejściowych użytkownika.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Dołączona właściwość zezwala na inny element zdefiniować informacje dotyczące ułatwień dostępu dla bieżącego elementu. Na przykład [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) obok [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) może służyć do Opisz, co `Entry` reprezentuje. Można to zrobić w języku XAML w następujący sposób:

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
> Należy pamiętać, że [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody można również ustawić `AutomationProperties.IsInAccessibleTree` dołączona właściwość — `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak ustawić wartości dostępności użytkownika elementów interfejsu w aplikacji platformy Xamarin.Forms przy użyciu dołączone właściwości z `AutomationProperties` klasy. Wartości dostępności natywny zestaw właściwości je dołączyć tak, aby do odczytywania zawartości ekranu mogą mowy o elementach na stronie.


## <a name="related-links"></a>Linki pokrewne

- [Dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md)
- [Ułatwienia dostępu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
