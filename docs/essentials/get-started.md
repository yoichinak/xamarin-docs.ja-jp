---
title: Xamarin.Essentials
description: Xamarin.Essentials、iOS、Android、または UWP で動作する単一のクロス プラットフォーム API を提供するアプリケーションからアクセスできるユーザー インターフェイスを作成する方法に関係なくコードを共有します。
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 700c148424da7e50a519659bf9766ce248e66dda
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials を概要します。

![プレリリース NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials、iOS、Android、または UWP で動作する単一のクロス プラットフォーム API を提供するアプリケーションからアクセスできるユーザー インターフェイスを作成する方法に関係なくコードを共有します。

## <a name="platform-support"></a>プラットフォームのサポート

Xamarin.Essentials には、次のプラットフォームやオペレーティング システムがサポートされています。

| プラットフォーム | Version |
| --- | --- |
| Android | 4.4 (API 19) またはそれ以降 |
| iOS |10.0 以上 |
| UWP | 10.0.6299.0 またはそれ以降 |

## <a name="installation"></a>インストール

Xamarin.Essentials は Visual Studio を使用して、既存または新規のプロジェクトに追加できる NuGet パッケージとして入手できます。

1. ダウンロードしてインストール[Visual Studio](http://visualstudio.com)で、 [Xamarin 用の Visual Studio tools](~/cross-platform/get-started/installation/index.md)です。

2. 既存のプロジェクトを開くか、下にある空のアプリケーション テンプレートを使用して新しいプロジェクトを作成**Visual Studio c#** (Android、iPhone および iPad、またはクロス プラットフォーム)。 **重要な**: かどうか UWP プロジェクトに追加することを確認 16299 またはそれ以降のビルドがプロジェクトのプロパティで設定します。

3. 追加、 **Xamarin.Essentials**の各プロジェクトに NuGet パッケージ。

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    ソリューション エクスプ ローラー パネルで、ソリューション名を右クリックし、選択**NuGet パッケージの管理**です。 検索**Xamarin.Essentials**にパッケージをインストールおよび**すべて**Android、iOS、UWP、および標準の .NET ライブラリを含むプロジェクトです。

    > [!TIP]
    > チェック、 **Include prerelease**中にボックス、 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials)はプレビュー段階です。

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    ソリューション エクスプ ローラー パネルで、プロジェクト名を右クリックし、選択**追加 > NuGet パッケージを追加しています.**.検索**Xamarin.Essentials**にパッケージをインストールおよび**すべて**Android、iOS、および標準の .NET ライブラリを含むプロジェクトです。

    > [!TIP]
    > チェック、**プレリリースのパッケージを表示**中にボックス、 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials)はプレビュー段階です。

    -----

4. Api を参照するすべての c# クラス内で Xamarin.Essentials への参照を追加します。

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials には、プラットフォーム固有のセットアップが必要です。

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Android プロジェクトの`MainLauncher`または any`Activity`つまりで起動した Xamarin.Essentials を初期化する必要があります、`OnCreate`メソッド。

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Android でのランタイム アクセス許可を処理する Xamarin.Essentials 受け取る必要がありますいずれかの`OnRequestPermissionsResult`します。 次のコードをすべて追加`Activity`クラス。

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

6. 以下の[Xamarin.Essentials ガイド](index.md)をコピーして各機能のコード スニペットを貼り付けることができます。

## <a name="other-resources"></a>その他の参照情報

Xamarin のアクセスに慣れていない開発者ことをお勧め[Xamarin 開発を開始する](~/cross-platform/getting-started/index.md)です。

参照してください、 [Xamarin.Essentials GitHub リポジトリ](http://github.com/xamarin/Essentials)して、現在のソース コード、今後の次に、サンプルを実行し、リポジトリを閉じます。 コミュニティの皆様がようこそ!

目を通す、 [API ドキュメント](xref:Xamarin.Essentials)Xamarin.Essentials のすべての機能です。
