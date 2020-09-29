---
ms.assetid: 4D47185C-8998-4903-AE64-7E2A67F9DF7A
title: UI コントロールの比較
description: このドキュメントでは、Xamarin、Windows フォーム、WPF の UI コントロールの比較について説明します。 また、WPF と Xamarin を比較するその他のドキュメントへのリンクもあります。
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: b4cffd9e95f24dea9fc5fed5a6badeec624a4e25
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453091"
---
# <a name="ui-controls-comparison"></a>UI コントロールの比較

次に示すのは、 [このテーブル](/dotnet/framework/wpf/advanced/windows-forms-controls-and-equivalent-wpf-controls)に基づく WINDOWS フォームと WPF を使用した Xamarin コントロールの比較です。

詳細については、「」を参照してください。これにより、モバイルアプリ開発用のデスクトップナレッジを更新でき[ます。](wpf.md)

|Windows フォーム|WPF|Xamarin.Forms|
|--- |--- |--- |
|[BindingNavigator](/dotnet/api/system.windows.forms.bindingnavigator)|-|-|
|[BindingSource](/dotnet/api/system.windows.forms.bindingsource)|[CollectionViewSource](/dotnet/api/system.windows.data.collectionviewsource)|バインディングプロパティ (例) BindingContext|
|[ボタン](/dotnet/api/system.windows.forms.button)|[ボタン](/dotnet/api/system.windows.controls.button)|Button|
|[CheckBox](/dotnet/api/system.windows.forms.checkbox)|[CheckBox](/dotnet/api/system.windows.controls.checkbox)|Switch|
|[CheckedListBox](/dotnet/api/system.windows.forms.checkedlistbox)|コンポジションを含む[ListBox](/dotnet/api/system.windows.controls.listbox) 。|コンポジションを含む ListView。|
|[ColorDialog](/dotnet/api/system.windows.forms.colordialog)|-|-|
|[ComboBox](/dotnet/api/system.windows.forms.combobox)|[ComboBox](/dotnet/api/system.windows.controls.combobox) (オートコンプリートをサポートしていません)|ピッカー|
|[ContextMenuStrip](/dotnet/api/system.windows.forms.contextmenustrip)|[ContextMenu](/dotnet/api/system.windows.controls.contextmenu)|-|
|[DataGridView](/dotnet/api/system.windows.forms.datagridview)|[DataGrid](/dotnet/api/system.windows.controls.datagrid)|-|
|[DateTimePicker](/dotnet/api/system.windows.forms.datetimepicker)|[DatePicker](/dotnet/api/system.windows.controls.datepicker)|DatePicker & TimePicker|
|[DomainUpDown](/dotnet/api/system.windows.forms.domainupdown)|[TextBox](/dotnet/api/system.windows.controls.textbox) と2つの [repeatbutton](/dotnet/api/system.windows.controls.primitives.repeatbutton) コントロール。|ステッパ|
|[ErrorProvider](/dotnet/api/system.windows.forms.errorprovider)|-|-|
|[FlowLayoutPanel](/dotnet/api/system.windows.forms.flowlayoutpanel)|[WrapPanel](/dotnet/api/system.windows.controls.wrappanel) または [StackPanel](/dotnet/api/system.windows.controls.stackpanel)|StackLayout または FlexLayout|
|[FolderBrowserDialog](/dotnet/api/system.windows.forms.folderbrowserdialog)|-|-|
|[FontDialog](/dotnet/api/system.windows.forms.fontdialog)|-|-|
|[形式](/dotnet/api/system.windows.forms.form)|[ウィンドウ](/dotnet/api/system.windows.window)|Page|
|[GroupBox](/dotnet/api/system.windows.forms.groupbox)|[GroupBox](/dotnet/api/system.windows.controls.groupbox)|-|
|[HelpProvider](/dotnet/api/system.windows.forms.helpprovider)|同等のコントロールはありません (ツールヒントを使用します)。|-|
|[Hscrollbar コントロール](/dotnet/api/system.windows.forms.hscrollbar)|[ScrollBar](/dotnet/api/system.windows.controls.primitives.scrollbar) (スクロールはコンテナーコントロールに組み込まれています)|ScrollView を使用する|
|[リスト](/dotnet/api/system.windows.forms.imagelist)|-|-|
|[Label](/dotnet/api/system.windows.forms.label)|[Label](/dotnet/api/system.windows.controls.label)|ラベル|
|[LinkLabel](/dotnet/api/system.windows.forms.linklabel)|同等のコントロールはありません ( [Hyperlink](/dotnet/api/system.windows.documents.hyperlink) クラスを使用して、フローコンテンツ内のハイパーリンクをホストできます)。|-|
|[ListBox](/dotnet/api/system.windows.forms.listbox)|[ListBox](/dotnet/api/system.windows.controls.listbox)|ListView の使用|
|[ListView](/dotnet/api/system.windows.forms.listview)|[ListView](/dotnet/api/system.windows.controls.listview)|ListView|
|[MaskedTextBox](/dotnet/api/system.windows.forms.maskedtextbox)|-|-|
|[MenuStrip](/dotnet/api/system.windows.forms.menustrip)|[Menu](/dotnet/api/system.windows.controls.menu)|Masterのページまたは TabbedPage を検討する|
|[MonthCalendar](/dotnet/api/system.windows.forms.monthcalendar)|[Calendar](/dotnet/api/system.windows.controls.calendar)|-|
|[NotifyIcon](/dotnet/api/system.windows.forms.notifyicon)|-|-|
|[NumericUpDown](/dotnet/api/system.windows.forms.numericupdown)|[TextBox](/dotnet/api/system.windows.controls.textbox) と2つの [repeatbutton](/dotnet/api/system.windows.controls.primitives.repeatbutton) コントロール。|ステッパ|
|[OpenFileDialog](/dotnet/api/system.windows.forms.openfiledialog)|[OpenFileDialog](/dotnet/api/microsoft.win32.openfiledialog)|-|
|[PageSetupDialog](/dotnet/api/system.windows.forms.pagesetupdialog)|-|-|
|[Panel](/dotnet/api/system.windows.forms.panel)|[キャンバス](/dotnet/api/system.windows.controls.canvas)|View または AbsoluteLayout|
|[PictureBox](/dotnet/api/system.windows.forms.picturebox)|[Image](/dotnet/api/system.windows.controls.image)|Image|
|[PrintDialog](/dotnet/api/system.windows.forms.printdialog)|[PrintDialog](/dotnet/api/system.windows.controls.printdialog)|-|
|[PrintDocument](/dotnet/api/system.drawing.printing.printdocument)|-|-|
|[PrintPreviewControl](/dotnet/api/system.windows.forms.printpreviewcontrol)|[DocumentViewer](/dotnet/api/system.windows.controls.documentviewer)|-|
|[PrintPreviewDialog](/dotnet/api/system.windows.forms.printpreviewdialog)|-|-|
|[ProgressBar](/dotnet/api/system.windows.forms.progressbar)|[ProgressBar](/dotnet/api/system.windows.controls.progressbar)|ProgressBar|
|[PropertyGrid](/dotnet/api/system.windows.forms.propertygrid)|-|-|
|[RadioButton](/dotnet/api/system.windows.forms.radiobutton)|[RadioButton](/dotnet/api/system.windows.controls.radiobutton)|-|
|[RichTextBox](/dotnet/api/system.windows.forms.richtextbox)|[RichTextBox](/dotnet/api/system.windows.controls.richtextbox)|エディターは、リッチ (書式設定された) テキストをサポートしていません。単一行テキストの入力です|
|[SaveFileDialog](/dotnet/api/system.windows.forms.savefiledialog)|[SaveFileDialog](/dotnet/api/microsoft.win32.savefiledialog)|-|
|[ScrollableControl](/dotnet/api/system.windows.forms.scrollablecontrol)|[ScrollViewer](/dotnet/api/system.windows.controls.scrollviewer)|ScrollView|
|[SoundPlayer](/dotnet/api/system.media.soundplayer)|[MediaPlayer](/dotnet/api/system.windows.media.mediaplayer)|-|
|[SplitContainer](/dotnet/api/system.windows.forms.splitcontainer)|[GridSplitter](/dotnet/api/system.windows.controls.gridsplitter)|Masterのページを検討する|
|[StatusStrip](/dotnet/api/system.windows.forms.statusstrip)|[StatusBar](/dotnet/api/system.windows.controls.primitives.statusbar)|-|
|[TabControl](/dotnet/api/system.windows.forms.tabcontrol)|[TabControl](/dotnet/api/system.windows.controls.tabcontrol)|TabbedPage|
|[TableLayoutPanel](/dotnet/api/system.windows.forms.tablelayoutpanel)|[Grid](/dotnet/api/system.windows.controls.grid)|グリッド|
|[TextBox](/dotnet/api/system.windows.forms.textbox)|[TextBox](/dotnet/api/system.windows.controls.textbox)|エディターはリッチ (書式設定された) テキストをサポートしていません|
|[Timer](/dotnet/api/system.windows.forms.timer)|[DispatcherTimer](/dotnet/api/system.windows.threading.dispatchertimer)|デバイス. StartTime ()|
|[ToolStrip](/dotnet/api/system.windows.forms.toolstrip)|[ToolBar](/dotnet/api/system.windows.controls.toolbar)|ページの ToolbarItems と ToolbarItem|
|[ToolStripContainer](/dotnet/api/system.windows.forms.toolstripcontainer)、 [ToolStripDropDown](/dotnet/api/system.windows.forms.toolstripdropdown)、 [タイミング](/dotnet/api/system.windows.forms.toolstripdropdownmenu)、 [ToolStripPanel](/dotnet/api/system.windows.forms.toolstrippanel)|コンポジションを含む[ツールバー](/dotnet/api/system.windows.controls.toolbar) 。|コンポジションを使用した ToolbarItems と ToolbarItem|
|[ToolTip](/dotnet/api/system.windows.forms.tooltip)|[ToolTip](/dotnet/api/system.windows.controls.tooltip)|ユーザー補助機能を使用する|
|[TrackBar](/dotnet/api/system.windows.forms.trackbar)|[スライダー](/dotnet/api/system.windows.controls.slider)|Slider|
|[TreeView](/dotnet/api/system.windows.forms.treeview)|[TreeView](/dotnet/api/system.windows.controls.treeview)|NavigationPage での階層的な ListView の検討|
|[UserControl](/dotnet/api/system.windows.forms.usercontrol)|[UserControl](/dotnet/api/system.windows.controls.usercontrol)|ビューとカスタムレンダラー|
|[VScrollBar](/dotnet/api/system.windows.forms.vscrollbar)|[ScrollBar](/dotnet/api/system.windows.controls.primitives.scrollbar)|ScrollView を使用する|
|[WebBrowser](/dotnet/api/system.windows.forms.webbrowser)|[WebBrowser](/dotnet/api/system.windows.controls.webbrowser)|WebView|