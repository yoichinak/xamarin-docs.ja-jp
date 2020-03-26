---
title: Android レイアウトの診断
description: Android レイアウトの診断と開始方法について説明します。
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 5c29a1a80d8c1f599f0bbc750d22d8334ddb3494
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247816"
---
# <a name="android-layout-diagnostics"></a>Android レイアウトの診断

Android レイアウト診断は、一般的な品質の問題や便利な最適化を強調することで、Android レイアウトファイルの品質向上に役立つように設計されています。 この機能は、Visual Studio 16.5 + と Visual Studio for Mac 8.5 + の両方で使用できます。

さまざまな問題に対して既定のアナライザーのセットが用意されており、それぞれをカスタマイズして、プロジェクト固有のニーズに対応することができます。 アナライザーは、Android のシステムに大まかに基づいています。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Visual Studio 2019 で Android レイアウト診断を有効にする

レイアウト診断設定 **[レイアウト診断を有効にする]** が有効になっていることを確認してください。 このオプションページにアクセスするには、 **[ツール]**  >  **[オプション]** の順に選択し、 **[テキストエディター]**  > [ **Android XML** > **詳細設定**] を選択します。

![診断オプションを有効にする方法を示す [オプション] ダイアログ](diagnostics-images/AndroidDiagnosticsEnableOption.png)

有効にすると、Android レイアウトエディターに問題が表示されます。

![Visual Studio 2019 で有効になっている Android 診断](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Visual Studio for Mac で Android レイアウト診断を有効にする

レイアウト診断設定 **[レイアウト診断を有効にする]** が有効になっていることを確認してください。 このオプションページにアクセスするには、[ **Visual Studio** > の**基本設定...** ] を選択し、[**テキストエディター** > **Android XML**] を選択します。

![診断オプションを有効にする方法を示す [基本設定] ダイアログ](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

有効にすると、Android レイアウトエディターに問題が表示されます。

![Visual Studio for Mac で有効になっている Android 診断](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>[機能]

以下のセクションでは、Android レイアウト診断で使用できる機能の概要を説明します。

### <a name="analyzers"></a>アナライザー

アナライザーは、レイアウトファイルの問題を検出するために使用されます。 ハードコードされた値を減らす方法、パフォーマンスを向上させる方法、エラーにフラグを付ける方法があります。

### <a name="diagnostic-configuration"></a>診断構成

アナライザーは XML ファイルを使用して構成できます。これにより、既定の重大度レベルを変更し、特定のファイルを無視して、変数を渡すことができます。

複数の Android アプリ間で共有する一連の構成がある場合は、ベースラインファイルを使用できます。 この機能を使用するには、新しい構成ファイルを作成し、ファイル名に `-baseline` を追加します。 ベースライン構成が最初に適用され、その後、残りの構成ファイルが適用されます。

> [!TIP]
> これは、新規または既存の Android アプリで一連の問題を無視する場合に便利です。

形式は次のようになります:

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<configuration>
    <issue id="DuplicateIDs" severity="warning">
        <ignore path="Resources/layout/layout1.xml" />
    </issue>
    <issue id="HardcodedText" severity="informational">
        <ignore path="Resources/layout/layout1.xml" />
        <ignore path="Resource/layout/layout2.xml" />
    </issue>
    <issue id="TooManyViews">
        <variable name="MAX_VIEW_COUNT" value="12" />
    </issue>
    <issue id="TooDeepLayout">
        <variable name="MAX_DEPTH" value="12" />
    </issue>
</configuration>
```

> [!NOTE]
> 現在、唯一の変数は `MAX_VIEW_COUNT` (既定値:80) で、`MAX_DEPTH` (既定:10) が `TooManyViews` と `TooDeepLayout` にそれぞれ使用されます。

重大度レベルは次のとおりです。

- 推奨事項
- Info
- 警告
- エラー
- Ignore

### <a name="add-a-configuration-file"></a>構成ファイルを追加する

Android アプリプロジェクトのルートに新しい XML ファイルを作成します。 ファイル名は重要ではありませんが、この例では `AndroidLayoutDiagnostics.xml`を使用します。

![新しい項目の追加](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

新しい XML ファイルが追加されると、Android アプリプロジェクトツリーに表示されます。

![Android アプリプロジェクトツリー](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

プロパティ パネルで、ビルド アクションが  **AndroidResourceAnalysisConfig** に設定されていることを確認します。
新しいファイルのプロパティパネルをプルする最も簡単な方法は、ファイルを右クリックし、[プロパティ] を選択することです。 [プロパティ] パネルが表示されたら、**ビルドアクション**を**AndroidResourceAnalysisConfig**に変更する必要があります。

![項目のプロパティでのビルドアクションの設定](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

空の XML ファイルを作成したので、`<configuration>` ルート要素を追加する必要があります。 この時点で、サポートされている問題の既定の動作を調整できます。
ハードコーディングされた文字列がエラーとして扱われるようにするには、次のように追加します。

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![診断構成ファイル](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

ハードコーディングされたテキストはエラーと見なされるようになったので、次はレイアウトエディターで赤い波線でフラグが設定されています。

![診断構成を使用したレイアウト](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> 新しい構成ファイルの変更が有効になるようにするには、現在開いているすべてのレイアウトファイルを再度開く必要があります。
>

## <a name="troubleshooting"></a>トラブルシューティング

いくつかの一般的な問題を次に示します。

- XML 形式のエラーがないことを確認します。
- ビルドアクションは**AndroidResourceAnalysisConfig**に正しく設定されています。

## <a name="known-issues"></a>既知の問題

- エラーパッドは、ファイルが最初に変更されるまで設定されません。

## <a name="related-links"></a>関連リンク

- [Android のけばチェック](http://tools.android.com/tips/lint-checks)
- [糸くずによるチェックでコードを改良する](https://developer.android.com/studio/write/lint)
