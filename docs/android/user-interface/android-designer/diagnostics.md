---
title: アンドロイドレイアウト診断
description: Android レイアウト診断と開始方法について説明します。
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 746f74e68fa4816f1f7979980af9506dc0173542
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987583"
---
# <a name="android-layout-diagnostics"></a>アンドロイドレイアウト診断

Android レイアウト診断は、一般的な品質の問題と役に立つ最適化を強調することにより、Android レイアウト ファイルの品質を向上させるために設計されています。 この機能は、Visual Studio 16.5+ と Mac 8.5+ 用の両方で使用できます。

問題の広い範囲に対して、デフォルトのアナライザーセットが用意されており、プロジェクト固有のニーズに合わせてカスタマイズできます。 アナライザーは、Androidのリンティングシステムに基づいています。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Visual Studio 2019 で Android レイアウト診断を有効にする

レイアウト診断設定の [**レイアウト診断を有効にする**] が有効になっていることを確認します。 このオプション ページにアクセスするには、[**ツール** > **オプション]** を選択し、[**テキスト エディター** > **の Android XML** > **詳細設定**] を選択します。

![診断オプションを有効にする方法を示すオプション ダイアログ](diagnostics-images/AndroidDiagnosticsEnableOption.png)

有効にすると、Android レイアウト エディターに問題が表示されます。

![Visual Studio 2019 で有効な Android 診断](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Mac 用の Visual Studio で Android レイアウト診断を有効にする

レイアウト診断設定の [**レイアウト診断を有効にする**] が有効になっていることを確認します。 このオプション ページにアクセスするには **、[Visual Studio** > **Text Editor** > **Android XML****の基本設定.**

![診断オプションを有効にする方法を示す環境設定ダイアログ](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

有効にすると、Android レイアウト エディターに問題が表示されます。

![Mac 用の Visual Studio で有効になっている Android 診断](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>特徴

次のセクションでは、Android レイアウト診断で使用可能な機能の概要を説明します。

### <a name="analyzers"></a>アナライザー

アナライザーは、レイアウト ファイルの問題の検出、ハードコードされた値の削減、パフォーマンスの向上、およびエラーのフラグの設定に使用されます。 アナライザーの一覧については[、Android デザイナーの診断アナライザーを](diagnostic-analyzers.md)参照してください。

### <a name="diagnostic-configuration"></a>診断構成

アナライザーは XML ファイルを使用して構成できるため、デフォルトの重大度レベルの変更、特定のファイルの無視、変数の受け渡しを行うことができます。

複数の Android アプリで共有する構成のセットがある場合は、ベースライン ファイルを使用できます。 この機能を使用するには、新しい構成ファイルを作成`-baseline`し、ファイル名に追加します。 最初にベースライン構成が適用され、次に残りの構成ファイルが適用されます。

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
> 現在、`MAX_VIEW_COUNT`変数は(デフォルト: 80)`MAX_DEPTH`と (デフォルト: 10)`TooManyViews`と`TooDeepLayout`の変数だけです。

重大度レベルは次のとおりです。

- 推奨事項
- Info
- 警告
- エラー
- Ignore

### <a name="add-a-configuration-file"></a>構成ファイルを追加する

Android アプリ プロジェクトのルートに新しい XML ファイルを作成します。 ファイルの名前は重要ではありませんが、この例では次のコードを`AndroidLayoutDiagnostics.xml`使用します。

![新しい項目の追加](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

新しい XML ファイルが追加されると、Android アプリのプロジェクト ツリーに表示されます。

![アンドロイドアプリプロジェクトツリー](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

ビルド アクションがプロパティ パネルで **[AndroidResourceAnalysisConfig]** に設定されていることを確認します。
新しいファイルのプロパティ パネルを表示する最も簡単な方法は、ファイルを右クリックしてプロパティを選択することです。 プロパティ パネルが表示されたら、**ビルド アクション**を変更する必要**があります**。

![アイテムプロパティでのビルド アクションの設定](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

空の XML ファイルが作成できたので、`<configuration>`ルート要素を追加する必要があります。 この時点で、サポートされている問題の既定の動作を調整できます。
ハードコードされた文字列がエラーとして扱われるようにしたい場合は、次のように追加します。

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![診断構成ファイル](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

ハードコードされたテキストがエラーと見なされるようになりましたが、レイアウトエディタで赤い波線でフラグが付けられます。

![診断構成を使用したレイアウト](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> 新しい構成ファイルの変更を有効にするには、現在開いているレイアウト ファイルを再び開く必要があります。
>

## <a name="troubleshooting"></a>トラブルシューティング

考えられる一般的な問題を次に示します。

- XML 形式エラーがないことを確認してください。
- ビルド アクションが正しく設定**されている。**

## <a name="known-issues"></a>既知の問題

- エラー パッドは、ファイルが最初に変更されるまで設定されません。

## <a name="related-links"></a>関連リンク

- [アンドロイドリントチェック](http://tools.android.com/tips/lint-checks)
- [lint チェックでコードを改善する](https://developer.android.com/studio/write/lint)
