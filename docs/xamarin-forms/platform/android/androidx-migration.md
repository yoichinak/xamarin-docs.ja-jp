---
title: Xamarin. Forms での AndroidX の移行
description: この記事では、AndroidX が存在する理由と、Xamarin. Forms アプリで AndroidX に移行する方法について説明します。
ms.prod: xamarin
ms.assetid: 98884003-E65A-4EB4-842D-66CFE27344A4
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/22/2020
ms.openlocfilehash: 13fb802dec326cdb82bac8825ca84343ef85b13e
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646654"
---
# <a name="androidx-migration-in-xamarinforms"></a>Xamarin. Forms での AndroidX の移行

AndroidX は、Android サポートライブラリを置き換えます。 この記事では、AndroidX が存在する理由、Xamarin に与える影響、および AndroidX ライブラリを使用するようにアプリケーションを移行する方法について説明します。

## <a name="history-of-androidx"></a>AndroidX の履歴

Android サポートライブラリは、古いバージョンの Android に新しい機能を提供するために作成されました。 これは、開発者が Android オペレーティングシステムのすべてのバージョンに存在しない機能を使用し、古いバージョンに対して正常なフォールバックを行うことができる互換性レイヤーです。 サポートライブラリには、便利なヘルパークラス、デバッグツールとユーティリティツール、および他のサポートライブラリクラスに依存する高度なクラスも含まれています。

サポートライブラリはもともと1つのバイナリですが、ライブラリのスイートに拡張され、進化しています。これは、最新のアプリの開発において非常に重要です。 サポートライブラリの一般的に使用される機能を次に示します。

- `Fragment` サポートクラス。
- 長い一覧を管理するために使用される `RecyclerView`です。
- 65536を超えるメソッドを使用したアプリのマルチ dex サポート。
- `ActivityCompat` クラスです。

AndroidX はサポートライブラリに代わるものであり、これは管理されなくなりました。すべての新しいライブラリの開発が AndroidX ライブラリで行われます。 AndroidX は、セマンティックバージョン管理を使用し、パッケージ名を明確にし、一般的なアプリケーションアーキテクチャパターンのサポートを強化する、再設計されたライブラリです。 AndroidX バージョン1.0.0 は、ライブラリバージョン28.0.0 のサポートに相当するバイナリです。 サポートライブラリから AndroidX へのクラスマッピングの完全な一覧については、「 [Support library class mappings](https://developer.android.com/jetpack/androidx/migrate/class-mappings) on developer.android.com」を参照してください。

Google は、AndroidX と Jetifier という移行プロセスを作成しました。 Jetifier は、ビルドプロセス中に jar バイトコードを検査し、アプリコードと依存関係の両方で、サポートされているライブラリ参照を AndroidX と同等のものに再マップします。

Android Java アプリの場合と同様に、Xamarin のフォームアプリでは、jar の依存関係を AndroidX に移行する必要があります。 ただし、適切な基になる jar ファイルを指すように Xamarin バインドも移行する必要があります。 バージョン4.5 での自動 AndroidX 移行のサポートが追加されました。

AndroidX の詳細については、developer.android.com の「 [Androidx の概要](https://developer.android.com/jetpack/androidx)」を参照してください。

## <a name="automatic-migration-in-xamarinforms"></a>Xamarin. フォームでの自動移行

AndroidX に自動的に移行するには、次のようにする必要があります。

- Android API バージョン29以上を対象とします。
- Xamarin. Forms バージョン4.5 以上を使用します。

プロジェクトでこれらの設定を確認したら、Visual Studio 2019 で Android アプリをビルドします。 ビルドプロセス中に中間言語 (IL) が検査され、ライブラリの依存関係をサポートし、バインドが AndroidX 依存関係にスワップされます。 アプリケーションに、ビルドに必要なすべての AndroidX 依存関係がある場合、ビルドプロセスに違いはありません。

> [!NOTE]
> サポートライブラリへの参照をプロジェクトに保持する必要があります。 これらは、移行プロセスが結果の IL を検査して依存関係を変換する前に、アプリケーションをコンパイルするために使用されます。

プロジェクトに含まれていない AndroidX 依存関係が検出されると、ビルドエラーが報告され、どの AndroidX パッケージが不足しているかが示されます。 ビルドエラーの例を次に示します。

```
Could not find 37 AndroidX assemblies, make sure to install the following NuGet packages:
- Xamarin.AndroidX.Lifecycle.LiveData
- Xamarin.AndroidX.Browser
- Xamarin.Google.Android.Material
- Xamarin.AndroidX.Legacy.Supportv4
You can also copy and paste the following snippit into your .csproj file:
 <PackageReference Include="Xamarin.AndroidX.Lifecycle.LiveData" Version="2.1.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Browser" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.Google.Android.Material" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Legacy.Support.V4" Version="1.0.0-rc1" />
```

不足している NuGet パッケージは、Visual Studio の NuGet パッケージマネージャーを使用してインストールすることも、Android の .csproj ファイルを編集してエラーに記載されている `PackageReference` XML 項目を含めることによってインストールすることもできます。

見つからないパッケージが解決されると、プロジェクトを再構築すると、不足しているパッケージが読み込まれます。プロジェクトは、ライブラリの依存関係をサポートするのではなく、AndroidX 依存関係を使用してコンパイルされます

> [!NOTE]
> プロジェクトとプロジェクトの依存関係が Android サポートライブラリを参照していない場合、移行プロセスでは何も行われず、実行されません。

## <a name="related-links"></a>関連リンク

- Developer.android.com の[Android サポートライブラリの概要](https://developer.android.com/topic/libraries/support-library/index)
- Developer.android.com の[Androidx の概要](https://developer.android.com/jetpack/androidx)
