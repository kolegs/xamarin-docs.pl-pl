---
ms.assetid: 4D47185C-8998-4903-AE64-7E2A67F9DF7A
title: Porównanie formantów interfejsu użytkownika
description: Ten dokument zawiera porównanie kontrolek interfejsu użytkownika między platformy Xamarin.Forms, formularze systemu Windows i WPF. Zawiera on również linki do dokumentacji, która porównuje WPF do platformy Xamarin.Forms.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: d97756952d7de724e1ef573092456e24294a74db
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782349"
---
# <a name="ui-controls-comparison"></a>Porównanie formantów interfejsu użytkownika

Poniżej znajduje się porównanie platformy Xamarin.Forms formantów formularzy systemu Windows i WPF, na podstawie [tej tabeli](/dotnet/framework/wpf/advanced/windows-forms-controls-and-equivalent-wpf-controls).

Przeczytaj więcej na temat [podobieństwa i różnice między WPF i platformy Xamarin.Forms](wpf.md) Aby zaktualizować wiedzy użytkownika pulpitu do tworzenia aplikacji mobilnych.

|Windows Forms|WPF|Xamarin.Forms|
|--- |--- |--- |
|[BindingNavigator](https://msdn.microsoft.com/library/system.windows.forms.bindingnavigator(v=vs.110).aspx)|-|-|
|[BindingSource](https://msdn.microsoft.com/library/system.windows.forms.bindingsource(v=vs.110).aspx)|[CollectionViewSource](https://msdn.microsoft.com/library/system.windows.data.collectionviewsource(v=vs.110).aspx)|Powiązania właściwości, np. BindingContext|
|[Przycisk](https://msdn.microsoft.com/library/system.windows.forms.button(v=vs.110).aspx)|[Przycisk](https://msdn.microsoft.com/library/system.windows.controls.button(v=vs.110).aspx)|Przycisk|
|[CheckBox](https://msdn.microsoft.com/library/system.windows.forms.checkbox(v=vs.110).aspx)|[CheckBox](https://msdn.microsoft.com/library/system.windows.controls.checkbox(v=vs.110).aspx)|Przełącznik|
|[CheckedListBox —](https://msdn.microsoft.com/library/system.windows.forms.checkedlistbox(v=vs.110).aspx)|[ListBox](https://msdn.microsoft.com/library/system.windows.controls.listbox(v=vs.110).aspx) z kompozycji.|Element ListView z kompozycji.|
|[ColorDialog](https://msdn.microsoft.com/library/system.windows.forms.colordialog(v=vs.110).aspx)|-|-|
|[ComboBox](https://msdn.microsoft.com/library/system.windows.forms.combobox(v=vs.110).aspx)|[ComboBox](https://msdn.microsoft.com/library/system.windows.controls.combobox(v=vs.110).aspx) (nie obsługuje automatycznego zakończenia)|Selektor|
|[ContextMenuStrip](https://msdn.microsoft.com/library/system.windows.forms.contextmenustrip(v=vs.110).aspx)|[ContextMenu](https://msdn.microsoft.com/library/system.windows.controls.contextmenu(v=vs.110).aspx)|-|
|[Formant DataGridView](https://msdn.microsoft.com/library/system.windows.forms.datagridview(v=vs.110).aspx)|[DataGrid](https://msdn.microsoft.com/library/system.windows.controls.datagrid(v=vs.110).aspx)|-|
|[Element DateTimePicker](https://msdn.microsoft.com/library/system.windows.forms.datetimepicker(v=vs.110).aspx)|[DatePicker](https://msdn.microsoft.com/library/system.windows.controls.datepicker(v=vs.110).aspx)|Selektora daty & TimePicker|
|[DomainUpDown](https://msdn.microsoft.com/library/system.windows.forms.domainupdown(v=vs.110).aspx)|[Pole tekstowe](https://msdn.microsoft.com/library/system.windows.controls.textbox(v=vs.110).aspx) i dwa [RepeatButton](https://msdn.microsoft.com/library/system.windows.controls.primitives.repeatbutton(v=vs.110).aspx) kontrolki.|Stepper|
|[ErrorProvider](https://msdn.microsoft.com/library/system.windows.forms.errorprovider(v=vs.110).aspx)|-|-|
|[FlowLayoutPanel](https://msdn.microsoft.com/library/system.windows.forms.flowlayoutpanel(v=vs.110).aspx)|[WrapPanel](https://msdn.microsoft.com/library/system.windows.controls.wrappanel(v=vs.110).aspx) lub [Panel stosu](https://msdn.microsoft.com/library/system.windows.controls.stackpanel(v=vs.110).aspx)|StackLayout lub WrapLayout formantu niestandardowego|
|[FolderBrowserDialog —](https://msdn.microsoft.com/library/system.windows.forms.folderbrowserdialog(v=vs.110).aspx)|-|-|
|[FontDialog](https://msdn.microsoft.com/library/system.windows.forms.fontdialog(v=vs.110).aspx)|-|-|
|[Formularz](https://msdn.microsoft.com/library/system.windows.forms.form(v=vs.110).aspx)|[Window](https://msdn.microsoft.com/library/system.windows.window(v=vs.110).aspx)|Strona|
|[GroupBox](https://msdn.microsoft.com/library/system.windows.forms.groupbox(v=vs.110).aspx)|[GroupBox](https://msdn.microsoft.com/library/system.windows.controls.groupbox(v=vs.110).aspx)|-|
|[HelpProvider](https://msdn.microsoft.com/library/system.windows.forms.helpprovider(v=vs.110).aspx)|Brak równoważne kontroli (Użyj etykietki narzędzi).|-|
|[HScrollBar](https://msdn.microsoft.com/library/system.windows.forms.hscrollbar(v=vs.110).aspx)|[Pasek przewijania](https://msdn.microsoft.com/library/system.windows.controls.primitives.scrollbar(v=vs.110).aspx) (przewijanie jest wbudowana w kontenerze formantów)|Użyj ScrollView|
|[ImageList](https://msdn.microsoft.com/library/system.windows.forms.imagelist(v=vs.110).aspx)|-|-|
|[Etykieta](https://msdn.microsoft.com/library/system.windows.forms.label(v=vs.110).aspx)|[Etykieta](https://msdn.microsoft.com/library/system.windows.controls.label(v=vs.110).aspx)|Etykieta|
|[Linklabel —](https://msdn.microsoft.com/library/system.windows.forms.linklabel(v=vs.110).aspx)|Kontrolka nie równoważne (można użyć [Hyperlink](https://msdn.microsoft.com/library/system.windows.documents.hyperlink(v=vs.110).aspx) klasy hiperłącza hosta w ramach dowolnej zawartości).|-|
|[ListBox](https://msdn.microsoft.com/library/system.windows.forms.listbox(v=vs.110).aspx)|[ListBox](https://msdn.microsoft.com/library/system.windows.controls.listbox(v=vs.110).aspx)|Użyj widoku listy.|
|[ListView](https://msdn.microsoft.com/library/system.windows.forms.listview(v=vs.110).aspx)|[ListView](https://msdn.microsoft.com/library/system.windows.controls.listview(v=vs.110).aspx)|ListView|
|[MaskedTextBox](https://msdn.microsoft.com/library/system.windows.forms.maskedtextbox(v=vs.110).aspx)|-|-|
|[MenuStrip](https://msdn.microsoft.com/library/system.windows.forms.menustrip(v=vs.110).aspx)|[Menu](https://msdn.microsoft.com/library/system.windows.controls.menu(v=vs.110).aspx)|Należy wziąć pod uwagę MasterDetailPage lub TabbedPage|
|[MonthCalendar](https://msdn.microsoft.com/library/system.windows.forms.monthcalendar(v=vs.110).aspx)|[Kalendarz](https://msdn.microsoft.com/library/system.windows.controls.calendar(v=vs.110).aspx)|-|
|[NotifyIcon](https://msdn.microsoft.com/library/system.windows.forms.notifyicon(v=vs.110).aspx)|-|-|
|[NumericUpDown](https://msdn.microsoft.com/library/system.windows.forms.numericupdown(v=vs.110).aspx)|[Pole tekstowe](https://msdn.microsoft.com/library/system.windows.controls.textbox(v=vs.110).aspx) i dwa [RepeatButton](https://msdn.microsoft.com/library/system.windows.controls.primitives.repeatbutton(v=vs.110).aspx) kontrolki.|Stepper|
|[OpenFileDialog](https://msdn.microsoft.com/library/system.windows.forms.openfiledialog(v=vs.110).aspx)|[OpenFileDialog](https://msdn.microsoft.com/library/microsoft.win32.openfiledialog(v=vs.110).aspx)|-|
|[PageSetupDialog —](https://msdn.microsoft.com/library/system.windows.forms.pagesetupdialog(v=vs.110).aspx)|-|-|
|[Panel](https://msdn.microsoft.com/library/system.windows.forms.panel(v=vs.110).aspx)|[Kanwa](https://msdn.microsoft.com/library/system.windows.controls.canvas(v=vs.110).aspx)|Widok lub AbsoluteLayout|
|[PictureBox](https://msdn.microsoft.com/library/system.windows.forms.picturebox(v=vs.110).aspx)|[Obraz](https://msdn.microsoft.com/library/system.windows.controls.image(v=vs.110).aspx)|Obraz|
|[PrintDialog](https://msdn.microsoft.com/library/system.windows.forms.printdialog(v=vs.110).aspx)|[PrintDialog](https://msdn.microsoft.com/library/system.windows.controls.printdialog(v=vs.110).aspx)|-|
|[PrintDocument —](https://msdn.microsoft.com/library/system.drawing.printing.printdocument(v=vs.110).aspx)|-|-|
|[Printpreviewcontrol —](https://msdn.microsoft.com/library/system.windows.forms.printpreviewcontrol(v=vs.110).aspx)|[DocumentViewer](https://msdn.microsoft.com/library/system.windows.controls.documentviewer(v=vs.110).aspx)|-|
|[PrintPreviewDialog](https://msdn.microsoft.com/library/system.windows.forms.printpreviewdialog(v=vs.110).aspx)|-|-|
|[ProgressBar](https://msdn.microsoft.com/library/system.windows.forms.progressbar(v=vs.110).aspx)|[ProgressBar](https://msdn.microsoft.com/library/system.windows.controls.progressbar(v=vs.110).aspx)|ProgressBar|
|[PropertyGrid](https://msdn.microsoft.com/library/system.windows.forms.propertygrid(v=vs.110).aspx)|-|-|
|[RadioButton](https://msdn.microsoft.com/library/system.windows.forms.radiobutton(v=vs.110).aspx)|[RadioButton](https://msdn.microsoft.com/library/system.windows.controls.radiobutton(v=vs.110).aspx)|-|
|[RichTextBox](https://msdn.microsoft.com/library/system.windows.forms.richtextbox(v=vs.110).aspx)|[RichTextBox](https://msdn.microsoft.com/library/system.windows.controls.richtextbox(v=vs.110).aspx)|Edytor nie obsługuje tekstu sformatowanego (sformatowanych), wpis pojedynczy wiersz tekstu|
|[SaveFileDialog](https://msdn.microsoft.com/library/system.windows.forms.savefiledialog(v=vs.110).aspx)|[SaveFileDialog](https://msdn.microsoft.com/library/microsoft.win32.savefiledialog(v=vs.110).aspx)|-|
|[Elementu ScrollableControl](https://msdn.microsoft.com/library/system.windows.forms.scrollablecontrol(v=vs.110).aspx)|[ScrollViewer](https://msdn.microsoft.com/library/system.windows.controls.scrollviewer(v=vs.110).aspx)|ScrollView|
|[SoundPlayer](https://msdn.microsoft.com/library/system.media.soundplayer(v=vs.110).aspx)|[MediaPlayer](https://msdn.microsoft.com/library/system.windows.media.mediaplayer(v=vs.110).aspx)|-|
|[SplitContainer](https://msdn.microsoft.com/library/system.windows.forms.splitcontainer(v=vs.110).aspx)|[GridSplitter](https://msdn.microsoft.com/library/system.windows.controls.gridsplitter(v=vs.110).aspx)|Należy wziąć pod uwagę MasterDetailPage|
|[StatusStrip](https://msdn.microsoft.com/library/system.windows.forms.statusstrip(v=vs.110).aspx)|[StatusBar](https://msdn.microsoft.com/library/system.windows.controls.primitives.statusbar(v=vs.110).aspx)|-|
|[TabControl](https://msdn.microsoft.com/library/system.windows.forms.tabcontrol(v=vs.110).aspx)|[TabControl](https://msdn.microsoft.com/library/system.windows.controls.tabcontrol(v=vs.110).aspx)|TabbedPage|
|[TableLayoutPanel](https://msdn.microsoft.com/library/system.windows.forms.tablelayoutpanel(v=vs.110).aspx)|[Siatka](https://msdn.microsoft.com/library/system.windows.controls.grid(v=vs.110).aspx)|Siatka|
|[TextBox](https://msdn.microsoft.com/library/system.windows.forms.textbox(v=vs.110).aspx)|[TextBox](https://msdn.microsoft.com/library/system.windows.controls.textbox(v=vs.110).aspx)|Edytor nie obsługuje tekstu sformatowanego (sformatowanych)|
|[Timer](https://msdn.microsoft.com/library/system.windows.forms.timer(v=vs.110).aspx)|[DispatcherTimer](https://msdn.microsoft.com/library/system.windows.threading.dispatchertimer(v=vs.110).aspx)|Device.StartTime()|
|[Pasek narzędzi](https://msdn.microsoft.com/library/system.windows.forms.toolstrip(v=vs.110).aspx)|[ToolBar](https://msdn.microsoft.com/library/system.windows.controls.toolbar(v=vs.110).aspx)|Page.ToolbarItems i ToolbarItem|
|[ToolStripContainer](https://msdn.microsoft.com/library/system.windows.forms.toolstripcontainer(v=vs.110).aspx), [ToolStripDropDown](https://msdn.microsoft.com/library/system.windows.forms.toolstripdropdown(v=vs.110).aspx), [ToolStripDropDownMenu](https://msdn.microsoft.com/library/system.windows.forms.toolstripdropdownmenu(v=vs.110).aspx), [ToolStripPanel](https://msdn.microsoft.com/library/system.windows.forms.toolstrippanel(v=vs.110).aspx)|[Pasek narzędzi](https://msdn.microsoft.com/library/system.windows.controls.toolbar(v=vs.110).aspx) z kompozycji.|Page.ToolbarItems i ToolbarItem z kompozycji|
|[Etykietka narzędzia](https://msdn.microsoft.com/library/system.windows.forms.tooltip(v=vs.110).aspx)|[Etykietka narzędzia](https://msdn.microsoft.com/library/system.windows.controls.tooltip(v=vs.110).aspx)|Korzystanie z funkcji ułatwień dostępu|
|[TrackBar](https://msdn.microsoft.com/library/system.windows.forms.trackbar(v=vs.110).aspx)|[Slider](https://msdn.microsoft.com/library/system.windows.controls.slider(v=vs.110).aspx)|Suwak|
|[TreeView](https://msdn.microsoft.com/library/system.windows.forms.treeview(v=vs.110).aspx)|[TreeView](https://msdn.microsoft.com/library/system.windows.controls.treeview(v=vs.110).aspx)|Należy wziąć pod uwagę hierarchiczny widok listy w NavigationPage|
|[UserControl](https://msdn.microsoft.com/library/system.windows.forms.usercontrol(v=vs.110).aspx)|[UserControl](https://msdn.microsoft.com/library/system.windows.controls.usercontrol(v=vs.110).aspx)|Widok, a także niestandardowe moduły renderowania|
|[Vscrollbar —](https://msdn.microsoft.com/library/system.windows.forms.vscrollbar(v=vs.110).aspx)|[ScrollBar](https://msdn.microsoft.com/library/system.windows.controls.primitives.scrollbar(v=vs.110).aspx)|Użyj ScrollView|
|[WebBrowser](https://msdn.microsoft.com/library/system.windows.forms.webbrowser(v=vs.110).aspx)|WebBrowser|Widok sieci Web|
