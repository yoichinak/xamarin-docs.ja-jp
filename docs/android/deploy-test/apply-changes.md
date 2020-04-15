---
title: 変更の適用
description: Xamarin.Android プロジェクトで変更の適用を開始する方法について説明します。
ms.assetid: 38950B0C-8880-448F-B12E-08D5BFE87D0D
author: JonDouglas
ms.author: jodou
ms.date: 03/09/2020
ms.openlocfilehash: 03bace97908a53ac6112cd2600b20d429fe2f521
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "80215314"
---
# <a name="apply-changes"></a>変更の適用

変更の適用により、アプリを再起動せずに、実行中のアプリにリソースの変更をプッシュできるようになります。 これは、デバイスまたはエミュレーターの現在の状態を維持しながら、小規模な増分変更を配置してテストする場合に、アプリをどの程度再起動するかを制御するのに役立ちます。

変更の適用は、Android 8.0 (API レベル 26) 以降を実行しているデバイスまたはエミュレーターでサポートされている [Android JVMTI 実装](https://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#bci)の機能を使用します。

## <a name="requirements"></a>必要条件

次の一覧は、変更の適用を使用するための要件を示したものです。

- **Visual Studio** - Windows 上では、Visual Studio 2019 バージョン 16.5 以降に更新します。 macOS 上では、Visual Studio 2019 for Mac バージョン 8.5 以降に更新します。
- **Xamarin.Android** - Xamarin.Android 10.2 以降を Visual Studio と共にインストールする必要があります (Xamarin.Android は、Windows 上で **[.NET によるモバイル開発]** ワークロードの一部として、また、**Visual Studio for Mac インストーラー**の一部として自動的にインストールされます)。
- **Android SDK** - Android SDK マネージャーを使用して Android API 28 以降をインストールする必要があります。
- **ターゲット デバイスまたはエミュレーター** - デバイスまたはエミュレーターでは、Android 8.0 (API レベル 26) 以降を実行する必要があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="get-started"></a>作業開始

変更の適用を開始するには、デバイスまたはエミュレーターが Android 8.0 (API レベル 26) 以降を実行していることを確認する必要があります。 次に、デバッグを使用して、または使用せずに、Android アプリケーションを実行します。

その後、次の方法で変更の適用を操作することができます。

1. **ツール バー アイコン。** [変更の適用] ツール バー アイコンをクリックすると、ターゲット デバイスまたはエミュレーターに変更を適用できます。

    [![変更の適用 - ツール バー アイコン](apply-changes-images/Apply-Changes-Toolbar.png)](apply-changes-images/Apply-Changes-Toolbar.png#lightbox)

2. **キーボード ショートカット。** **Shift + Alt + F5** キーボード ショートカットを使用して、ターゲット デバイスまたはエミュレーターに変更を適用できます。
3. **[デバッグ] メニュー。** **[デバッグ]、[変更の適用]** メニュー項目を使用すると、ターゲット デバイスまたはエミュレーターに変更を適用できます。

    [![[変更の適用] - [デバッグ] メニュー](apply-changes-images/Apply-Changes-Debug-Menu.png)](apply-changes-images/Apply-Changes-Debug-Menu.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="get-started"></a>作業開始

変更の適用を開始するには、デバイスまたはエミュレーターが Android 8.0 (API レベル 26) 以降を実行していることを確認する必要があります。 次に、デバッグを使用して、または使用せずに、Android アプリケーションを実行します。

その後、次の方法で変更の適用を操作することができます。

1. **キーボード ショートカット。** **⌥ + ⇧ + R** キーボード ショートカットを使用して、ターゲット デバイスまたはエミュレーターに変更を適用できます。
2. **[実行] メニュー。** **[実行]、[変更の適用]** メニュー項目を使用すると、ターゲット デバイスまたはエミュレーターに変更を適用できます。

    [![[変更の適用] - [デバッグ] メニュー](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png)](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png#lightbox)

-----

## <a name="limitations"></a>制限事項

次の変更を行うには、アプリケーションを再起動する必要があります。

- C# コードの変更。
- リソースの追加または削除。
- AndroidManifest.xml ファイルの変更。
- ネイティブライブラリ (.so ファイル) の変更。

## <a name="related-links"></a>関連リンク

- [変更の適用](https://developer.android.com/studio/run#apply-changes)
