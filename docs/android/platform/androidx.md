---
title: AndroidX
description: Xamarin.Android を使用して AndroidX でアプリの開発を始める方法。
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: ad6ea2f68fc01183f7ed42e85094f6be5fb3d9f9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "77618912"
---
# <a name="androidx-with-xamarin"></a>Xamarin を使用した AndroidX

_Xamarin.Android を使用して AndroidX でアプリの開発を始める方法。_

AndroidX は、維持されなくなった元の Android サポート ライブラリを大幅に改良したものです。 **AndroidX** パッケージを使うと、Android アプリケーションで使用できる機能パリティと新しいライブラリを提供することで、Android サポート ライブラリが完全に置き換えられます。

AndroidX には、次の機能があります。

- AndroidX 内のすべてのパッケージは、`androidx` で始まる一貫した名前空間を持つようになりました。 これは、すべての Android サポート ライブラリ パッケージが対応する `androidx.*` パッケージにマップされることを意味します。
- `androidx` パッケージは個別に維持され更新されます。 これは、AndroidX ライブラリを相互に独立して更新できることを意味します。
- Android サポート ライブラリは v28 の時点で、これ以上リリースされることはありません。 代わりにすべての開発が `androidx` に含まれます。

![AndroidX ロゴ](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで AndroidX の機能を使用するのに必要な一覧を次に示します。

- **Visual Studio** - Windows 上では、Visual Studio 2019 バージョン 16.4 以降に更新します。 macOS 上では、Visual Studio 2019 for Mac バージョン 8.4 以降に更新します。
- **Xamarin.Android** - Xamarin.Android 10.0 以降を Visual Studio と共にインストールする必要があります (Xamarin.Android は、Windows 上で **[.NET によるモバイル開発]** ワークロードの一部として、また、**Visual Studio for Mac インストーラー**の一部として自動的にインストールされます)。
- **Java Developer Kit** - Xamarin.Android 10.0 の開発には JDK 8 が必要です。 Microsoft の OpenJDK の配布は、Visual Studio の一部として自動的にインストールされます。
- **Android SDK** - Android SDK マネージャーを使用して Android SDK API 28 以降をインストールする必要があります。

## <a name="get-started"></a>作業開始

AndroidX の使用を開始するには、Android プロジェクト内に [AndroidX NuGet パッケージ](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)を含めます。 [Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio) または [Visual Studio for Mac](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac) でのパッケージのインストールと使用について詳しく確認します

## <a name="behavior-changes"></a>動作の変更

AndroidX は Android サポート ライブラリの再設計であるため、Android サポート ライブラリを使用してビルドされた Android アプリケーションに影響する移行手順が含まれています。

### <a name="package-name-change"></a>パッケージ名の変更
パッケージ名は、旧パッケージと新パッケージの間で変更されています。 これらの変更の例を以下に示します。

| 旧                    | 新規作成                    |
| ---------------------- | ---------------------- |
| android.support.**     | androidx.@             |
| android.design.**      | com.google.android.material.@ |
| android.support.test.** | androidx.test.@       |
| android.arch.**        | androidx.@             |
| android.arch.persistence.room.** | androidx.room.@ |
| android.arch.persistence.** | androidx.sqlite.@ |

パッケージの名前付けの詳細については、[次のドキュメントを参照してください](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings)。

## <a name="migration-tooling"></a>移行ツール

アプリケーションでは、3 つの移行手順に注意する必要があります。

1. アプリケーションに **Android サポート ライブラリの名前空間が含まれていて、それらを AndroidX 名前空間に移行する**場合、 **[AndroidX への移行]** IDE ツールを使用して、ほとんどの名前空間のシナリオを処理できます。 

Visual Studio 2019 内の **[ツール] > [オプション] > [Xamarin] > [Android 設定]** から **AndroidX Migrator** を有効にします (Visual Studio for Mac ではこの手順をスキップできます)。

![AndroidX Migrator を有効にする](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

プロジェクトを右クリックし、 **[AndroidX への移行]** を選択します。

![AndroidX への移行](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> ツールで対応されないシナリオについては、手動で名前空間を変更する必要があります。 自動的に正しいパッケージがマップされますが、プロジェクトの移行に役立てるために、公式の[アーティファクト マッピング](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings)と[クラス マッピング](https://developer.android.com/jetpack/androidx/migrate/class-mappings)を参照することをお勧めします。

2. **AndroidX 名前空間に移行されていない依存関係**がアプリケーションに含まれている場合は、[Android サポート ライブラリを AndroidX 移行パッケージに使用する必要があります。](https://www.nuget.org/packages/Xamarin.AndroidX.Migration)
3. アプリケーションに **AndroidX 名前空間の移行**を必要とする依存関係が含まれていない場合は、[今すぐ NuGet の AndroidX ライブラリを使用できます](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)。

## <a name="troubleshooting"></a>トラブルシューティング

- AndroidX 内の特定のアーキテクチャ パッケージは、サポート ライブラリのバージョンと競合します。 この問題を解決するには、これらのパッケージの AndroidX バージョンを使用し、サポート ライブラリのバージョンを削除する必要があります。 たとえば、プロジェクト内の `Xamarin.Android.Arch.Work.Runtime` を参照している場合は、新しく追加した `AndroidX.Work` パッケージの種類と競合します。

## <a name="summary"></a>まとめ

この記事では、AndroidX について紹介し、AndroidX での Xamarin.Android 開発用に最新のツールとパッケージをインストールして構成する方法について紹介しました。 これにより、AndroidX の概要が示されました。 AndroidX を使用してアプリを作り始めるために役立つ、API ドキュメントと Android デベロッパー トピックへのリンクが記載されています。 また、既存のアプリに影響する可能性のある最も重要な AndroidX の動作の変更点とトラブルシューティングに関するトピックについても説明しました。

## <a name="related-links"></a>関連リンク

- [AndroidX の概要 | Xamarin Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Xamarin AndroidX GitHub リポジトリ](https://github.com/xamarin/AndroidX)
- [Xamarin AndroidX の移行 GitHub リポジトリ](https://github.com/xamarin/XamarinAndroidXMigration)
