---
title: App Center で Xamarin.Forms テストを自動化する
description: Xamarin の UITest コンポーネントを Xamarin.Forms で使用して、数百台のデバイスを対象にクラウドで実行する UI テストを作成できます。
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/31/2016
ms.openlocfilehash: dc43d8b5623b83be16d437e30290bc8b059be4bb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2018
ms.locfileid: "35242948"
---
# <a name="automate-xamarinforms-testing-with-app-center"></a>App Center で Xamarin.Forms テストを自動化する

_Xamarin の UITest コンポーネントを Xamarin.Forms で使用して、数百台のデバイスを対象にクラウドで実行する UI テストを作成できます。_

## <a name="overview"></a>概要

**[App Center Test](/appcenter/test-cloud/)** を利用すると、開発者は iOS アプリと Android アプリのために自動化されたユーザー インターフェイス テストを記述できます。 同じテスト コードの共有など、若干の微調整を行うことで Xamarin.Forms アプリを Xamarin.UITest でテストできます。 この記事では、Xamarin.Forms で Xamarin.UITest を利用するためのヒントを紹介します。

このガイドは、Xamarin.UITest の知識を前提としています。 Xamarin.UITest に関する知識を得る場所として次のガイドを推奨しています。

- [App Center Test の概要](/appcenter/test-cloud/)
- [UITest の概要](/appcenter/test-cloud/preparing-for-upload/uitest/)

UITest プロジェクトを Xamarin.Forms に追加すると、Xamarin.Forms アプリケーションのテストを記述し、実行するための手順が Xamarin.Android または Xamarin.iOS アプリケーションの場合と同じになります。

## <a name="requirements"></a>必要条件

[Xamarin.UITest](/appcenter/test-cloud/uitest/) に関するセクションを参照して、プロジェクトで自動 UI テストを実施する準備ができているかを確認してください。

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Xamarin.Forms アプリに UITest サポートを追加する

UITest はユーザー インターフェイスを自動化します。画面上のコントロールを起動し、ユーザーが通常、そのアプリケーションと対話するあらゆる場所で入力を実行します。 *ボタンを押す*か、*ボックスにテキストを入力する*テストを有効にするには、画面上のコントロールを特定する方法をテスト コードに与える必要があります。

コントロールを参照する UITest コードを有効にするには、各コントロールに一意の識別子を与える必要があります。 Xamarin.Forms では、この識別子を設定する推奨方法は、下の画像のように `AutomationId` プロパティを使用することです。

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

`AutomationId` プロパティも XAML に設定できます。

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

テストに必要なあらゆるコントロールを一意の `AutomationId` に追加してください (ボタン、テキスト エントリ、値を問い合わせる可能性があるラベルなど)。

> [!NOTE]
> `Element` の `AutomationId` プロパティを複数回設定しようとすると、`InvalidOperationException` がスローされます。

### <a name="ios-application-project"></a>iOS アプリケーション プロジェクト

iOS でテストを実行するには、[Xamarin Test Cloud Agent NuGet パッケージ](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/)をプロジェクトに追加する必要があります。 追加されたら、次のコードを `AppDelegate.FinishedLaunching` メソッドにコピーします。

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

Calabash アセンブリは非公開の Apple API を使用します。この API により、アプリは App Store に却下されます。 しかしながら、明示的にコードから参照されていないのであれば、Xamarin.iOS リンカーが最終 IPA から Calabash アセンブリを削除します。

> [!NOTE]
>  リリース ビルドには `ENABLE_TEST_CLOUD` コンパイラ変数がありません。Calabash アセンブリがアプリ バンドルから取り除かれます。 しかしながら、デバッグ ビルドにはコンパイラ ディレクティブが定義されており、リンカーがアセンブリを削除するのを防ぎます。

次のスクリーンショットでは、デバッグ ビルドに設定されている `ENABLE_TEST_CLOUD` コンパイラ変数を確認できます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "ビルド オプション")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "コンパイラ オプション")

-----

### <a name="android-application-project"></a>Android アプリケーション プロジェクト

iOS とは異なり、Android プロジェクトには特別なスタートアップ コードは必要ありません。

## <a name="writing-uitests"></a>UITests を記述する

UITests を記述する方法については、[UITest ドキュメント](/appcenter/test-cloud/preparing-for-upload/uitest/)を参照してください。 以下の手順はまとめです。特に、[Xamarin.Forms デモ **UsingUITest**](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) のビルド方法についてまとめています。

### <a name="use-automationid-in-the-xamarinforms-ui"></a>Xamarin.Forms UI の AutomationId を使用する

UITests を記述するには、Xamarin.Forms アプリケーション ユーザー インターフェイスをスクリプト実行可能にする必要があります。 テスト コードで参照できるように、ユーザー インターフェイスのすべてのコントロールに `AutomationId` を与えます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-a-uitest-project-to-a-new-solution"></a>UITest プロジェクトを新しいソリューションに追加する

Visual Studio for Mac を利用して新しい Xamarin.Forms プロジェクトを作成するとき、**[Xamarin Test Cloud: 自動 UI テスト プロジェクトを追加します]** を選択し、新しい UITest プロジェクトをソリューションに追加できます。

![](uitest-and-test-cloud-images/01-new-solution-xs.png "新しいプロジェクトを構成します")

Xamarin.Forms アプリケーションに対して Xamarin.UITest を実行するように、新しいソリューションが自動的に構成されます。

-----

#### <a name="referring-to-the-automationid-in-uitests"></a>UITests の AutomationId を参照する

UITests を記述するとき、`AutomationId` 値はプラットフォームごとに異なる方法で公開されます。

- **iOS** は `id` フィールドを使用します。
- **Android** は `label` フィールドを使用します。

iOS と Android の両方で `AutomationId` を見つけるプラットフォーム非依存 UITests を記述するには、`Marked` テスト クエリを使用します。

```csharp
app.Query(c=>c.Marked("MyButton"))
```

省略形 `app.Query("MyButton")` も機能します。

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>既存のソリューションに UITest プロジェクトを追加する

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio には、Xamarin.UITest プロジェクトを既存の Xamarin.Forms ソリューションに追加するためのテンプレートがあります。

1. ソリューションを右クリックし、**[ファイル]、[新しいプロジェクト]** の順に選択します。
1. **Visual C#** テンプレートから、**テスト** カテゴリを選択します。 **[UI テスト アプリ]、[クロスプラットフォーム]** テンプレートの順に選択します。

    ![](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "新しいプロジェクトを追加します")

    これで **NUnit**、**Xamarin.UITest**、**NUnitTestAdapter** NuGet パッケージを含む新しいプロジェクトがソリューションに追加されます。

    ![](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "NuGet パッケージ マネージャー")

    **NUnitTestAdapter** は、Visual Studio から NUnit テストを実行することを可能にするサードパーティ製のテスト ランナーです。

    新しいプロジェクトには 2 つのクラスも含まれています。 **AppInitializer** には、テストを初期化し、セットアップするコードが含まれています。 もう 1 つのクラスである **Tests** は、UITests を起動させるスケルトン コードが含まれています。

1. UITest プロジェクトから Xamarin.Android プロジェクトにプロジェクト参照を追加します。

    ![](uitest-and-test-cloud-images/10-test-apps-vs.png "プロジェクト参照マネージャー")

    これで **NUnitTestAdapter** は Visual Studio から Android アプリの UITests を実行できるようになります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

既存のソリューションに新しい Xamarin.UITest プロジェクトを手動で追加できます。

1. 最初に新しいプロジェクトを追加します。ソリューションを選択し、**[ファイル]、[新しいプロジェクトの追加]** の順にクリックします。 **[新しいプロジェクト]** ダイアログで、**[クロスプラットフォーム]、[テスト]、[Xamarin Test Cloud]、[UI テスト アプリ]** を選択します。

    ![](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "テンプレートを選択します")

    これで **NUnit** と **Xamarin.UITest** NuGet のパッケージがソリューションに既に含まれる新しいプロジェクトが追加されます。

    ![](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Xamarin UITest NuGet パッケージ")

    新しいプロジェクトには 2 つのクラスも含まれています。 **AppInitializer** には、テストを初期化し、セットアップするコードが含まれています。 もう 1 つのクラスである **Tests** は、UITests を起動させるスケルトン コードが含まれています。

1. **[表示]、[パッド]、[単体テスト]** の順に選択し、単体テスト パッドを表示します。 **[UsingUITest]、[UsingUITest.UITests]、[アプリのテスト]** の順に展開します。

    ![](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "単体テスト パッド")

1. **[アプリのテスト]** を右クリックし、**[アプリ プロジェクトの追加]** をクリックし、表示されたダイアログで iOS プロジェクトと Android プロジェクトを選択します。

    ![](uitest-and-test-cloud-images/05-add-test-apps-xs.png "[アプリのテスト] ダイアログ")

    **単体テスト** パッドに、iOS プロジェクトと Android プロジェクトの参照が含まれているはずです。 これで Visual Studio for Mac のテスト ランナーは 2 つの Xamarin.Forms プロジェクトに対して UITests をローカル実行できます。

#### <a name="adding-uitest-to-the-ios-app"></a>UITest を iOS アプリに追加する

Xamarin.UITest を機能させるには、いくつかの点で iOS をさらに変更する必要があります。

1. **Xamarin Test Cloud Agent** NuGet パッケージを追加します。 **[パッケージ]** を右クリックし、**[パッケージの追加]** を選択し、NuGet で **Xamarin Test Cloud Agent** を検索し、それを Xamarin.iOS プロジェクトに追加します。

    ![](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "NuGet パッケージを追加します")

1. iOS アプリケーションの起動時に Xamarin Test Cloud Agent を初期化し、ビューの `AutomationId` プロパティを設定するように、**AppDelegate** クラスの `FinishedLaunching` メソッドを編集します。 `FinishedLaunching` メソッドは次のコード例のようになります。

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
        #if ENABLE_TEST_CLOUD
        Xamarin.Calabash.Start();
        #endif

        global::Xamarin.Forms.Forms.Init();

        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
}
```

-----

Xamarin.UITest を Xamarin.Forms ソリューションに追加した後、UITests を作成し、ローカルで実行し、Xamarin Test Cloud に送信できます。

## <a name="summary"></a>まとめ

Xamarin.Forms アプリケーションは、テスト自動化のための一意の表示 ID として `AutomationId` を公開する単純なメカニズムを利用し、**Xamarin.UITest** で簡単にテストできます。 UITest プロジェクトを Xamarin.Forms に追加すると、Xamarin.Forms アプリケーションのテストを記述し、実行するための手順が Xamarin.Android または Xamarin.iOS アプリケーションの場合と同じになります。

テストを App Center Test に送信する方法については、[UITests の送信](/appcenter/test-cloud/preparing-for-upload/uitest/)に関するページを参照してください。 UITest の詳細については、[App Center Test のドキュメント](/appcenter/test-cloud/)を参照してください。


## <a name="related-links"></a>関連リンク

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Xamarin.Forms のサンプル](https://github.com/xamarin/xamarin-forms-samples)
- [App Center テスト](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
