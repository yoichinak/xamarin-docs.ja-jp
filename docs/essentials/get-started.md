---
title: Xamarin.Essentials の概要
description: Xamarin.Essentials には、任意の iOS、Android、または UWP アプリケーションと連携する単一のクロスプラットフォーム API が用意されています。ユーザー インターフェイスの作成方法に関係なく、共有コードからアクセスできます。
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 05/11/2020
ms.openlocfilehash: 944b01d67fb09f9a21a19fb2ede9eb217d89732a
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149981"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials の概要

Xamarin.Essentials には、任意の iOS、Android、または UWP アプリケーションと連携する単一のクロスプラットフォーム API が用意されています。ユーザー インターフェイスの作成方法に関係なく、共有コードからアクセスできます。 サポートされているオペレーティング システムについて詳しくは、[プラットフォームと機能のサポート ガイド](platform-feature-support.md)に関するページをご覧ください。

## <a name="installation"></a>インストール

Xamarin.Essentials は、NuGet パッケージとして入手でき、Visual Studio のすべての新しいプロジェクトに含まれています。 また、次の手順で Visual Studio を使用して既存のプロジェクトに追加することもできます。

1. [Xamarin 用の Visual Studio ツール](~/get-started/installation/index.md)と共に、[Visual Studio](https://visualstudio.microsoft.com/) をダウンロードしてインストールします。

2. 既存のプロジェクトを開くか、**Visual Studio C#** (Android、iPhone と iPad、またはクロスプラットフォーム) の下の空のアプリ テンプレートを使用して新しいプロジェクトを作成します。

    > [!IMPORTANT]
    > UWP プロジェクトに追加する場合、プロジェクトのプロパティにビルド 16299 以上が設定されていることを確認します。

3. 各プロジェクトに [**Xamarin.Essentials**](https://www.nuget.org/packages/Xamarin.Essentials/) の NuGet パッケージを追加します。

    <!--markdownlint-disable MD023 -->
    # <a name="visual-studio"></a>[Visual Studio](#tab/windows)

    ソリューション エクスプローラー パネルでソリューション名を右クリックし、 **[NuGet パッケージの管理]** を選択します。 **Xamarin.Essentials** を検索し、Android、iOS、UWP、.NET Standard ライブラリなど、**すべての**プロジェクトにパッケージをインストールします。

    # <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

    ソリューション エクスプローラー パネルでプロジェクト名を右クリックし、 **[追加] > [Add NuGet Packages...]\(NuGet パッケージの追加...\)** を選択します。**Xamarin.Essentials** を検索し、Android、iOS、.NET Standard ライブラリなど、**すべての**プロジェクトにパッケージをインストールします。

    -----

4. Xamarin.Essentials への参照を任意の C# クラスに追加して、API を参照します。

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials では、プラットフォーム固有の設定が必要です。

    # <a name="android"></a>[Android](#tab/android)

    Xamarin.Essentials では最小の Android バージョン 4.4 (API レベル 19 に対応) がサポートされていますが、コンパイルのターゲットの Android バージョンは 9.0 または 10.0 (API レベル 28 およびレベル 29 に対応) である必要があります。 (Visual Studio では、これらの 2 つのバージョンは、[Android マニフェスト] タブ内の、Android プロジェクトの [プロジェクトのプロパティ] ダイアログで設定されます。Visual Studio for Mac では、これらは [Android アプリケーション] タブ内の、Android プロジェクトの [プロジェクト オプション] ダイアログで設定されます。)

    Android 9.0 に対してコンパイルする場合、Xamarin.Essentials では、必要なバージョン 28.0.0.3 の Xamarin.Android.Support ライブラリがインストールされます。 アプリケーションで必要となるその他の Xamarin.Android.Support ライブラリも、NuGet パッケージ マネージャーを使用してバージョン 28.0.0.3 に更新する必要があります。 アプリケーションで使用される Xamarin.Android.Support ライブラリはすべて同じであり、かつ少なくともバージョン 28.0.0.3 以上である必要があります。 Xamarin.Essentials の NuGet の追加、またはソリューションでの NuGet の更新に関して問題が発生した場合は、[トラブルシューティングのページ](troubleshooting.md)を参照してください。

    バージョン 1.5.0 以降では、Android 10.0 に対してコンパイルする場合、Xamarin.Essentials では、必要な AndroidX サポート ライブラリがインストールされます。 まだ移行を行っていない場合は、[AndroidX のドキュメント](https://docs.microsoft.com/xamarin/android/platform/androidx)に目を通してください。

    Android プロジェクトの `MainLauncher`、または起動されるすべての `Activity` では、`OnCreate` メソッド内で Xamarin.Essentials を初期化する必要があります。

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState) {
        //...
        base.OnCreate(savedInstanceState);
        Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
        //...
    ```

    Android 上で実行時のアクセス許可を処理するには、Xamarin.Essentials がすべての `OnRequestPermissionsResult` を受け取る必要があります。 すべての `Activity` クラスに次のコードを追加します。

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="ios"></a>[iOS](#tab/ios)

    追加の設定は必要ありません。

    # <a name="uwp"></a>[UWP](#tab/uwp)

    追加の設定は必要ありません。

    -----

6. [Xamarin.Essentials のガイド](index.md)に従ってください。各機能に対するコード スニペットをコピーして貼り付けることができます。

## <a name="xamarinessentials---cross-platform-apis-for-mobile-apps-video"></a>Xamarin.Essentials - モバイル アプリ用クロスプラットフォーム API (ビデオ)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Snack-Pack-XamarinEssentials-Cross-Platform-APIs-for-Mobile-Apps/player]

## <a name="other-resources"></a>その他の参照情報

Xamarin を使用したことがない開発者は、[Xamarin 開発の概要](~/cross-platform/getting-started/index.md)に関するページを参照することをお勧めします。

現在のソース コード、今後の内容、サンプルの実行、リポジトリの複製について確認するには、[Xamarin.Essentials の GitHub リポジトリ](https://github.com/xamarin/Essentials)をご覧ください。 コミュニティへの投稿も歓迎します。

Xamarin.Essentials のすべての機能の [API ドキュメント](xref:Xamarin.Essentials)を参照します。
