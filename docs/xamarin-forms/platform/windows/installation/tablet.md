---
title: Windows アプリを追加します。
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0d2ef44896c9352776443c2fec318d40d27d7539
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-app"></a>Windows アプリを追加します。


1. **ソリューションを右クリックして > 追加 > 新しいプロジェクト.**を追加し、 **Blank App (Windows)**

 ![](tablet-images/add-wu.png "新しいプロジェクト ダイアログ ボックスを追加します。")

2. **プロジェクトを右クリックして > NuGet パッケージの管理.**を追加し、 **Xamarin.Forms**パッケージです。

3. **プロジェクトを右クリックして > 追加 > 参照**し共有 Xamarin.Forms アプリケーション プロジェクトにプロジェクト参照を作成します。

  ![](tablet-images/addref.png "参照マネージャー ダイアログ ボックス")

4. 編集**App.xaml.cs**に含める、`Init()`内のメソッドの呼び出し、 `OnLaunched` 65 'system.ftpserver メソッド

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . 編集**MainPage.xaml** -ルート要素を変更`<Page`に`<forms:WindowsPage`*と*定義、`xmlns:forms`のために使用します。

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . 編集**MainPage.xaml.cs**を削除する、`: Page`クラス名の指定子を継承します。

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . まだ**MainPage.xaml.cs**、追加、`LoadApplication`で呼び出し、 `MainPage` ('system.ftpserver 28) コンス トラクター Xamarin.Forms アプリを起動します。

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . ダブルクリックして**Package.appxmanifest**を必要とされる多くの場合、これらの機能を設定します。

  機能を設定します。

  * インターネット (クライアント)
  * 場所

9 . 最後に、(ローカル リソースを追加します。 イメージ ファイル) に必要な既存のプラットフォーム プロジェクトからです。

