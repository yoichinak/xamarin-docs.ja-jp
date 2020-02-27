---
title: AndroidX
description: Xamarin を使用して AndroidX でアプリの開発を開始する方法について説明します。
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: ad6ea2f68fc01183f7ed42e85094f6be5fb3d9f9
ms.sourcegitcommit: 2836f2003a5b745b042ee6003a3d6a11b9139e44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618912"
---
# <a name="androidx-with-xamarin"></a>Xamarin を使用した AndroidX

_Xamarin を使用して AndroidX でアプリの開発を開始する方法について説明します。_

AndroidX は、管理されなくなった元の Android サポートライブラリの大幅な改善点です。 **Androidx**パッケージは、android アプリケーションで使用できる機能のパリティと新しいライブラリを提供することで、Android サポートライブラリを完全に置き換えます。

AndroidX には、次の機能が含まれています。

- AndroidX 内のすべてのパッケージは、`androidx`で始まる一貫した名前空間を持つようになりました。 これは、すべての Android サポートライブラリパッケージが対応する `androidx.*` パッケージにマップされることを意味します。
- `androidx` パッケージは個別に保持および更新されます。 これは、AndroidX ライブラリを相互に独立して更新できることを意味します。
- Android サポートライブラリの v28 のとき、リリースはこれ以上ありません。 すべての開発は、代わりに `androidx` に含まれます。

![AndroidX ロゴ](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>要件

Xamarin ベースのアプリで AndroidX 機能を使用するには、次の一覧が必要です。

- Visual **studio** -On Windows Update On visual studio 2019 version 16.4 以降。 MacOS で、Visual Studio 2019 for Mac バージョン8.4 以降に更新します。
- **Xamarin.** android 10.0 以降を Visual Studio と共にインストールする必要があります (xamarin android は、Windows 上の .net ワークロード**を使用したモバイル開発**の一部として自動的にインストールされ、 **Visual Studio for Mac インストーラー**の一部としてインストールされます)
- **Java Developer Kit** -Xamarin Android 10.0 の開発には JDK 8 が必要です。 Microsoft の OpenJDK の配布は、Visual Studio の一部として自動的にインストールされます。
- **Android SDK** -Android SDK API 28 以降が Android SDK Manager を使用してインストールされている必要があります。

## <a name="get-started"></a>作業開始

AndroidX の使用を開始するには、Android プロジェクト内に[Androidx NuGet パッケージ](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)を含めます。 [Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio)または[Visual Studio for Mac](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)でパッケージをインストールして使用する方法について説明します。

## <a name="behavior-changes"></a>動作の変更

AndroidX は Android サポートライブラリの再設計であるため、Android サポートライブラリを使用してビルドされた Android アプリケーションに影響する移行手順が含まれています。

### <a name="package-name-change"></a>パッケージ名の変更
パッケージ名は、古いパッケージと新しいパッケージの間で変更されています。 これらの変更の例を以下に示します。

| 以前                    | 新規                    |
| ---------------------- | ---------------------- |
| android. サポート. * *     | androidx。@             |
| android. * *      | .com. android.@ |
| android. サポート. テスト. * * | androidx. test。@       |
| android. * *        | androidx。@             |
| android............... * * | androidx. ルーム。@ |
| android... 永続化. * * | androidx. sqlite。@ |

パッケージの名前付けの詳細については、[次のドキュメントを参照してください](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings)。

## <a name="migration-tooling"></a>移行ツール

アプリケーションでは、3つの移行手順に注意する必要があります。

1. アプリケーションに Android サポートライブラリの名前空間が含まれていて、**それらを AndroidX 名前空間に移行する**場合は、「 **androidx への移行**」ツールを使用して、ほとんどの名前空間のシナリオを処理できます。 

Visual Studio 2019 内で **> Xamarin > Android 設定のツール > オプション**を使用して**Androidx Migrator**を有効にします (Visual Studio for Mac でこの手順を省略できます)。

![AndroidX Migrator を有効にする](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

プロジェクトを右クリックし、 **AndroidX に移行**します。

![AndroidX への移行](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> ツールが対応しないシナリオについては、手動で名前空間を変更する必要があります。 正しいパッケージをマップしますが、プロジェクトの移行に役立つ公式の[アーティファクトマッピング](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings)と[クラスマッピング](https://developer.android.com/jetpack/androidx/migrate/class-mappings)について確認することをお勧めします。

2. **Androidx 名前空間に移行されていない依存関係**がアプリケーションに含まれている場合は、 [Android サポートライブラリを Androidx 移行パッケージに](https://www.nuget.org/packages/Xamarin.AndroidX.Migration)使用する必要があります。
3. アプリケーションに**androidx 名前空間の移行を必要とする依存関係が含まれてい**ない場合は、[現在、NuGet で androidx ライブラリ](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)を使用できます。

## <a name="troubleshooting"></a>トラブルシューティング

- AndroidX 内の特定のアーキテクチャパッケージは、サポートライブラリのバージョンと競合します。 この問題を解決するには、これらのパッケージの AndroidX バージョンを使用し、サポートライブラリのバージョンを削除する必要があります。 たとえば、プロジェクト内の `Xamarin.Android.Arch.Work.Runtime` を参照している場合は、新しく追加した `AndroidX.Work` パッケージの種類と競合します。

## <a name="summary"></a>要約

この記事では、AndroidX を導入し、AndroidX を使用して Xamarin Android 開発用の最新のツールとパッケージをインストールして構成する方法について説明しました。 ここでは、AndroidX の概要について説明しました。 ここには、AndroidX を使用したアプリの作成を開始する際に役立つ API ドキュメントと Android 開発者向けのトピックへのリンクが含まれています。 また、既存のアプリに影響する可能性のある、最も重要な AndroidX の動作の変更とトラブルシューティングのトピックも強調表示されています。

## <a name="related-links"></a>関連リンク

- [AndroidX | の概要Xamarin の表示](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Xamarin AndroidX GitHub リポジトリ](https://github.com/xamarin/AndroidX)
- [Xamarin AndroidX 移行 GitHub リポジトリ](https://github.com/xamarin/XamarinAndroidXMigration)
