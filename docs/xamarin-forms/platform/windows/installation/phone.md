---
title: Windows Phone アプリを追加します。
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 55bd4bdcfde4c91ad5c9b94bef486207466e135d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-phone-app"></a>Windows Phone アプリを追加します。


まず Xamarin.Forms PCL テンプレートを使用する場合は、[プロファイルを更新](~/xamarin-forms/platform/windows/installation/index.md)、し、次の手順に従います。

1. **ソリューションを右クリックして > 追加 > 新しいプロジェクト.**を追加し、 **Blank App (Windows Phone)**

  ![](phone-images/add-wp81.png "新しいプロジェクト ダイアログ ボックスを追加します。")

2. **新しく作成されたプロジェクトを右クリックして > NuGet パッケージの管理.**を追加し、 **Xamarin.Forms**パッケージです。

3. **プロジェクトを右クリックして > 追加 > 参照**し共有 Xamarin.Forms アプリケーション プロジェクトにプロジェクト参照を作成します。

  ![](phone-images/addref.png "参照マネージャー ダイアログ ボックス")

4. 編集**App.xaml.cs**に含める、`Init()`メソッドの呼び出しで、 `OnLaunched` 67 'system.ftpserver メソッド。

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . 編集**MainPage.xaml** -ルート要素を変更`<Page`に`<forms:WindowsPhonePage`*と*定義、`xmlns:forms`のために使用します。

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . 編集**MainPage.xaml.cs**を削除する、`: PhonePage`クラス名の指定子を継承します。

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . まだ**MainPage.xaml.cs**、追加、`LoadApplication`で呼び出し、 `MainPage` ('system.ftpserver 28) コンス トラクター Xamarin.Forms アプリを起動します。

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . ダブルクリックして**Package.appxmanifest**を必要とされる多くの場合、これらの機能を設定します。

  * インターネット (クライアントとサーバー)

9 . 最後に、(ローカル リソースを追加します。 イメージ ファイル) に必要な既存のプラットフォーム プロジェクトからです。

