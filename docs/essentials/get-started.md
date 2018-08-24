---
title: Xamarin.Essentials を概要します。
description: Xamarin.Essentials、iOS、Android、または UWP で動作する 1 つのクロス プラットフォーム API を提供するアプリケーションからアクセスできるユーザー インターフェイスを作成する方法に関係なく、コードを共有します。
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c72c1c66a465075770ce739270cb4b1f2c6fba7a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353777"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials を概要します。

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

Xamarin.Essentials、iOS、Android、または UWP で動作する 1 つのクロス プラットフォーム API を提供するアプリケーションからアクセスできるユーザー インターフェイスを作成する方法に関係なく、コードを共有します。

## <a name="platform-support"></a>プラットフォームのサポート

Xamarin.Essentials には、次のプラットフォームとオペレーティング システムがサポートされています。

| プラットフォーム | Version |
| --- | --- |
| Android | 4.4 (API 19) またはそれ以降 |
| iOS |10.0 以上 |
| UWP | 10.0.16299.0 以降 |

## <a name="installation"></a>インストール

Xamarin.Essentials は Visual Studio を使用して既存または新規のプロジェクトに追加できる NuGet パッケージとして入手できます。

1. ダウンロードしてインストール[Visual Studio](http://visualstudio.com)で、 [Visual Studio tools for Xamarin](~/cross-platform/get-started/installation/index.md)します。

2. 既存のプロジェクトを開くか、下の空のアプリ テンプレートを使用して新しいプロジェクトを作成する**Visual Studio c#** (Android、iPhone と iPad、またはクロス プラットフォーム)。 **重要な**: UWP プロジェクトに追加することがビルド 16299 以降、プロジェクトのプロパティが設定されているようにします。

3. 追加、 **Xamarin.Essentials**の各プロジェクトに NuGet パッケージ。

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    ソリューション エクスプ ローラー パネルで、ソリューション名を右クリックし、選択**NuGet パッケージの管理**します。 検索**Xamarin.Essentials**にパッケージをインストールおよび**すべて**Android、iOS、UWP、および .NET Standard ライブラリを含むプロジェクト。

    > [!TIP]
    > チェック、**プレリリースを含める**ボックス中に、 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials)はプレビュー段階です。

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    ソリューション エクスプ ローラー パネルで、プロジェクト名を右クリックし、選択**追加 > NuGet パッケージを追加しています.**.検索**Xamarin.Essentials**にパッケージをインストールおよび**すべて**Android、iOS、および .NET Standard ライブラリを含むプロジェクト。

    > [!TIP]
    > チェック、**プレリリース パッケージを表示する**ボックス中に、 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials)はプレビュー段階です。

    -----

4. Api を参照するすべての c# クラスで Xamarin.Essentials への参照を追加します。

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials では、プラットフォーム固有のセットアップが必要です。

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials が最小の Android バージョン 4.4、API レベル 19 に対応するをサポートしていますが、コンパイルのターゲット Android バージョンは、8.1 である必要があります、API レベル 27 に対応します。 (Visual Studio でのこれら 2 つのバージョンが、Android マニフェスト タブで、Android プロジェクトのプロジェクトのプロパティ ダイアログで設定されます。Visual studio for Mac では、これらいるに設定、Android アプリケーション タブで、Android プロジェクトのプロジェクト オプション ダイアログ。) 
    
    Xamarin.Essentials には、必要な xamarin.android.support についてライブラリのバージョン 27.0.2.1 がインストールされます。 アプリケーションを必要とする他の Xamarin.Android.Support ライブラリは、NuGet パッケージ マネージャーを使用して 27.0.2.1 のバージョンにも更新されます。 アプリケーションで使用されるすべての Xamarin.Android.Support ライブラリは同じである必要があり、以上である必要があります 27.0.2.1 のバージョン。 参照してください、[トラブルシューティング ページ](troubleshooting.md)Xamarin.Essentials NuGet を追加または更新して、ソリューションで Nuget の問題がある場合。

    Android プロジェクトの`MainLauncher`または any`Activity`つまりで起動した Xamarin.Essentials を初期化する必要があります、`OnCreate`メソッド。

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Android ランタイムのアクセス許可を処理する Xamarin.Essentials 受け取る必要がありますすべて`OnRequestPermissionsResult`します。 次のコードをすべて追加`Activity`クラス。

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    追加の設定が必要です。

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    追加の設定が必要です。

    -----

6. に従って、 [Xamarin.Essentials ガイド](index.md)をコピーして各機能のコード スニペットを貼り付けることができます。

## <a name="other-resources"></a>その他の参照情報

新しい Xamarin 訪問する開発者をお勧め[Xamarin 開発を開始する](~/cross-platform/getting-started/index.md)します。

参照してください、 [Xamarin.Essentials GitHub リポジトリ](http://github.com/xamarin/Essentials)して、現在のソース コード、今後の次に、サンプルを実行して、リポジトリを閉じます。 コミュニティへの投稿も歓迎します!

どうぞ、 [API ドキュメント](xref:Xamarin.Essentials)Xamarin.Essentials のすべての機能です。
