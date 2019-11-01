---
title: Xamarin での watchOS ローカライズの使用
description: このドキュメントでは、Xamarin でビルドされた watchOS アプリをローカライズする方法について説明します。 監視アプリ、ウォッチ拡張機能、コード内の文字列、ストーリーボードテキスト、テストなどについて説明します。
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 82b739697705ac4c90390a36405d755a5f523159
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028411"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>Xamarin での watchOS ローカライズの使用

_WatchOS アプリを複数の言語向けに適応する_

![](localization-images/both-languages-sml.png "Apple Watch displaying localized content")

watchOS アプリは、標準の iOS メソッドを使用してローカライズされます。

- Storyboard 要素で**ローカライズ ID**を使用する
- **。** ストーリーボードに関連付けられている文字列ファイル
- コードで使用されるテキストのローカライズ可能な**文字列**ファイルです。

既定のストーリーボードとリソースは**ベース**ディレクトリにあり、言語固有の翻訳やその他のリソースは、 **lproj**ディレクトリに格納されます。
iOS および Watch OS は、ユーザーの言語選択を自動的に使用して、正しい文字列とリソースを読み込みます。

Apple Watch アプリには、2つのパーツウォッチアプリと Watch 拡張機能があるため、使用方法に応じて、ローカライズされた文字列リソースが2か所で必要になります。

ローカライズされたテキストとリソースは、watch アプリと watch 拡張機能では*異なり*ます。

## <a name="watch-app"></a>アプリを見る

Watch アプリには、アプリのユーザーインターフェイスを説明するストーリーボードが含まれています。 ローカライズをサポートするコントロール (`Label`、`Image`など) には、**ローカライズ ID**があります。

各言語固有の**lproj**ディレクトリには、各要素 (**ローカライズ ID**を使用) の翻訳を含む**文字列**ファイルと、ストーリーボードによって参照されるイメージが含まれている必要があります。

## <a name="watch-extension"></a>Watch 拡張機能

Watch の拡張機能は、アプリのコードを実行します。 コードからユーザーに表示されるテキストは、ウォッチアプリではなく、拡張機能でローカライズする必要があります。

拡張機能には、言語固有の**ディレクトリも**含まれている必要がありますが、文字列ファイルには、コードで使用されるテキストの翻訳のみが必要**です**。

## <a name="globalizing-the-watch-solution"></a>Watch ソリューションのグローバル化

グローバリゼーションとは、アプリケーションをローカライズ可能にするプロセスのことです。
Watch アプリの場合、これは、さまざまなテキスト長を考慮してストーリーボードを設計し、表示されるテキストに応じて各画面レイアウトが適切に調整されるようにすることを意味します。 また、watch 拡張コードで参照されている文字列を `LocalizedString` メソッドを使用して変換できることを確認する必要もあります。

### <a name="watch-app"></a>アプリを見る

既定では、ウォッチアプリはローカライズ用に構成されていません。 既定のストーリーボードファイルを移動し、翻訳用に他のディレクトリを作成する必要があります。

1. **Base. lproj**ディレクトリを作成し、**インターフェイス**をその中に移動します。

2. サポートする言語ごとに、 **\<言語 >** のディレクトリを作成します。

3. **Lproj**ディレクトリには、 **Interface. 文字列**テキストファイルが含まれている必要があります (ファイル名は storboard の名前と一致している必要があります)。 必要に応じて、これらのディレクトリでローカライズが必要なイメージを配置することもできます。

これらの変更が行われると、watch アプリプロジェクトは次のようになります (英語とスペイン語のファイルのみが追加されています)。

  ![](localization-images/watchapp-solution.png "The watch app project with English and Spanish language files")

#### <a name="storyboard-text"></a>ストーリーボードのテキスト

ストーリーボードを編集するときに、各要素を選択し、 **[プロパティ]** パッドに表示される**ローカライズ ID**を確認します。

  [![](localization-images/storyboard-sml.png "The Localization ID that appears in the Properties pad")](localization-images/storyboard.png#lightbox)

基本の**lproj**フォルダーで、次に示すようにキーと値のペアを作成します。キーは、**ローカライズ ID**とコントロールのプロパティ名で形成され、ドット (`.`) で結合されます。

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

この例では、**ローカライズ ID**は単純な数値文字列であることに注意してください (例として、 "0"、"1" など)、またはより複雑な文字列 ("AgC-eL-Hgc" など)。 `Label` コントロールには `Text` プロパティがあり、`Button`には、ローカライズされた値の設定方法に反映される `Title` プロパティがあります。上記の例に示すように、小文字のプロパティ名を使用してください。

ストーリーボードがウォッチにレンダリングされると、ユーザーが選択した言語に従って、正しい値が自動的に抽出されて表示されます。

#### <a name="storyboard-images"></a>ストーリーボードイメージ

このソリューションの例には、各言語フォルダー内の **gradient@2x.png** イメージも含まれています。 このイメージは、言語によって異なる場合があります (例として、 変換が必要なテキストが埋め込まれている場合や、ローカライズされた iconography を使用している場合があります)。

ストーリーボードに画像の**画像**のプロパティを設定するだけで、ユーザーが選択した言語に応じて、正しい画像がウォッチにレンダリングされます。

![](localization-images/storyboard-image.png "Set the images Image property in the storyboard")

注: すべての Apple Watch に Retina が表示されるため、イメージの **@2x** バージョンのみが必要です。 ストーリーボードで **@2x** を指定する必要はありません。

### <a name="watch-extension"></a>Watch 拡張機能

Watch 拡張機能では、ローカライズをサポートするために同様のディレクトリ構造が必要ですが、ストーリーボードはありません。 拡張機能のローカライズされた文字列は、コードC#によって参照される文字列のみです。

![](localization-images/watchextension-solution.png "The watch extension directory structure to support localization")

#### <a name="strings-in-code"></a>コード内の文字列

ローカライズ可能な文字列ファイルの構造は、ストーリーボードに関連付けられている場合と若干異なります **。** この場合は、任意の "key" 文字列を選択できます。Apple では、既定の言語で表示される実際のテキストを反映するキーを使用することをお勧めします。

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` メソッドを使用して、次のコードに示すように、変換された対応する文字列に文字列を解決します。

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>コード内の画像

コードによって設定されたイメージは、次の2つの方法で設定できます。

1. `Image` コントロールを変更するには、その値を Watch アプリに既に存在するイメージの文字列名に設定します。たとえば、

    ```csharp
    displayImage.SetImage("gradient"); // image in Watch App (as shown above)
    ```

2. `FromBundle` を使用して拡張機能からウォッチにイメージを移動すると、アプリはユーザーの言語選択に適したイメージを自動的に選択します。 この例のソリューションでは、各言語フォルダーに **language@2x.png** イメージがあり、次のコードを使用して `DetailController` に表示されます。

    ```csharp
    using (var image = UIImage.FromBundle ("language")) {
        displayImage.SetImage (image);
    }
    ```

    イメージのファイル名を参照するときは、 **@2x** を指定する必要がないことに注意してください。

2番目の方法は、リモートサーバーからイメージをダウンロードしてウォッチにレンダリングする場合にも適用できます。ただし、この場合は、ダウンロードしたイメージがユーザーの設定に従って正しくローカライズされていることを確認する必要があります。

## <a name="localization"></a>ローカリゼーション

ソリューションを構成したら、翻訳者は、サポートする言語ごとに、**文字列**ファイルとイメージを処理する必要があります。

必要な数 (サポートされる言語ごとに1つ) のディレクトリを作成**できます。** これらの名前には、 **en**、 **es**、 **de**、 **ja**、 **pt、BR**などの言語コードを使用して名前が付けられます (英語、スペイン語、ドイツ語、日本語、ポルトガル語 (ブラジル) など)。

添付サンプルは、(コンピューターによって生成された) 翻訳を使用して、watchOS アプリをローカライズする方法を示しています。

### <a name="watch-app"></a>アプリを見る

これらの値は、watch アプリのストーリーボードで定義されているユーザーインターフェイスを変換するために使用されます。 *キー*値は、各ストーリーボードコントロールの**ローカライズ ID**と変換されるプロパティの組み合わせです。

翻訳の内容を翻訳者が認識できるように、元のテキストを含むコメントをファイルに追加することをお勧めします。

#### <a name="eslprojinterfacestrings"></a>es. lproj/Interface. 文字列

(機械翻訳) ストーリーボードのスペイン語の文字列を次に示します。 各行にコメントを追加すると便利です。これは、**ローカライズ ID**がどのようなものを参照しているかを把握するのが難しいためです。

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Watch 拡張機能

これらの値は、ユーザーに表示される前に情報を変換するためにコード内で使用されます。 *キー*はコードを記述するときに開発者によって選択され、通常は翻訳される実際の文字列が含まれます。

#### <a name="eslprojlocalizablestrings-file"></a>es。ローカライズ可能な文字列ファイル

(機械翻訳) Spansish 言語文字列:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>テスト中

言語の設定を変更する方法は、シミュレーターと物理デバイスで異なります。

### <a name="simulator"></a>シミュレーター

シミュレーターで、iOS**設定**アプリ (シミュレーターのホーム画面の灰色の歯車アイコン) を使用してテストする言語を選択します。

  ![](localization-images/sim-settings-sml.png "The iOS Settings app Localization settings")

### <a name="watch-device"></a>デバイスの監視

ウォッチを使用してテストする場合は、ペアになっている iPhone の**Apple Watch**アプリでウォッチの言語を変更します。

  ![](localization-images/phone-settings-sml.png "Change the watch's language in the Apple Watch app on the paired iPhone")

## <a name="related-links"></a>関連リンク

- [WatchLocalization (サンプル)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchLocalization/)
