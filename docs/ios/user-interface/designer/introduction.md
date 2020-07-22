---
title: iOS Designer の基本
description: このガイドでは、Xamarin Designer for iOS について説明します。 IOS デザイナーを使用して、コントロールを視覚的にレイアウトする方法、コードでコントロールにアクセスする方法、およびプロパティを編集する方法を示します。
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/31/2018
ms.openlocfilehash: e5cbbc10f189abb6d0d0b2ef99b50ae53d1103c2
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572287"
---
# <a name="ios-designer-basics"></a>iOS Designer の基本

_このガイドでは、Xamarin Designer for iOS について説明します。IOS デザイナーを使用して、コントロールを視覚的にレイアウトする方法、コードでコントロールにアクセスする方法、およびプロパティを編集する方法を示します。_

Xamarin Designer for iOS は、Xcode の Interface Builder や Android Designer に似たビジュアルインターフェイスデザイナーです。 多くの機能の中には、Visual Studio for Windows および Mac とのシームレスな統合、ドラッグアンドドロップ編集、イベントハンドラーを設定するためのインターフェイス、カスタムコントロールのレンダリング機能などがあります。

## <a name="requirements"></a>必要条件

IOS デザイナーは、Visual Studio for Mac と Visual Studio 2017 以降の Windows で使用できます。 Visual Studio for Windows では、iOS Designer は適切に構成された Mac ビルドホストへの接続を必要としますが、Xcode は実行されている必要はありません。

このガイドでは、[はじめにガイド](~/ios/get-started/index.md)で説明されている内容を理解していることを前提としています。

<a name="how-it-works"></a>

## <a name="how-the-ios-designer-works"></a>IOS デザイナーの動作

このセクションでは、iOS Designer によるユーザーインターフェイスの作成とコードへの接続を容易にする方法について説明します。

IOS Designer を使用すると、開発者はアプリケーションのユーザーインターフェイスを視覚的にデザインできます。 「ストーリーボードの[概要](~/ios/user-interface/storyboards/index.md)」ガイドで説明したように、ストーリーボードは、アプリを構成する画面 (ビューコントローラー)、それらのビューコントローラーに配置されているインターフェイス要素 (ビュー)、およびアプリ全体のナビゲーションフローを記述します。 

ビューコントローラーには、iOS デザイナーのビジュアル表現と関連付けられた C# クラスの2つの部分があります。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![IOS デザイナーのビューコントローラー](introduction-images/1-storyboardwithviewcontroller-vsmac.png "IOS デザイナーのビューコントローラー")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![ビューコントローラーのコード](introduction-images/2-viewcontrollercode-vsmac.png "ビューコントローラーのコード")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![IOS デザイナーのビューコントローラー](introduction-images/1-storyboardwithviewcontroller-vs.png "IOS デザイナーのビューコントローラー")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![ビューコントローラーのコード](introduction-images/2-viewcontrollercode-vs.png "ビューコントローラーのコード")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

既定の状態では、ビューコントローラーは機能を提供しません。コントロールが設定されている必要があります。 これらのコントロールは、画面のすべてのコンテンツを含む四角形の領域であるビューコントローラーのビューに配置されます。 ほとんどのビューコントローラーには、ボタンを含むビューコントローラーを示す次のスクリーンショットに示すように、ボタン、ラベル、テキストフィールドなどの一般的なコントロールが含まれています。 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![ボタンを含むビューコントローラー](introduction-images/3-viewcontrollerwithbutton-vsmac.png "ボタンを含むビューコントローラー")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![ボタンを含むビューコントローラー](introduction-images/3-viewcontrollerwithbutton-vs.png "ボタンを含むビューコントローラー")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

静的テキストを含むラベルなど、一部のコントロールは、ビューコントローラーに追加できます。 ただし、多くの場合、コントロールはプログラムによってカスタマイズする必要があります。 たとえば、上で追加したボタンは、タップしたときに何かを実行する必要があるので、イベントハンドラーをコードに追加する必要があります。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

コード内のボタンにアクセスして操作するには、一意の識別子を持っている必要があります。 一意の識別子を指定するには、[] ボタンを選択して**Properties Pad**を開き、[**名前**] フィールドに "submitbutton" などの値を設定します。

[![Properties Pad でのボタンの名前の設定](introduction-images/4-settingbuttonname-vsmac.png "Properties Pad でのボタンの名前の設定")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

コード内のボタンにアクセスして操作するには、一意の識別子を持っている必要があります。 一意の識別子を指定するには、ボタンを選択し、[**プロパティ] ウィンドウ**を開き、[**名前**] フィールドに "submitbutton" などの値を設定します。

[![[プロパティ] ウィンドウでのボタンの名前の設定](introduction-images/4-settingbuttonname-vs.png "[プロパティ] ウィンドウでのボタンの名前の設定")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

これで、ボタンに名前が付いたので、コードでアクセスできます。 しかし、これはどのように機能するのでしょうか。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

**Solution Pad**で、 **ViewController.cs**に移動し、公開インジケーターをクリックすると、ビューコントローラーの `ViewController` クラス定義が2つのファイルにまたがり、それぞれに[部分クラス](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)定義が含まれていることがわかります。

[![ViewController クラスを構成する2つのファイル: ViewController.cs と ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "ViewController クラスを構成する2つのファイル: ViewController.cs と ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

**ソリューションエクスプローラー**で、 **ViewController.cs**に移動し、公開インジケーターをクリックすると、ビューコントローラーの `ViewController` クラス定義が2つのファイルにまたがり、それぞれに[部分クラス](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)定義が含まれていることがわかります。

[![ViewController クラスを構成する2つのファイル: ViewController.cs と ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "ViewController クラスを構成する2つのファイル: ViewController.cs と ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs**には、クラスに関連するカスタムコードを入力する必要があり `ViewController` ます。 このファイルでは、 `ViewController` クラスはさまざまな iOS ビューコントローラーのライフサイクルメソッドに応答し、UI をカスタマイズして、ボタンタップなどのユーザー入力に応答できます。

- **ViewController.designer.cs**は、視覚的に構築されたインターフェイスをコードにマップするために iOS デザイナーによって作成された、生成されたファイルです。 このファイルへの変更は上書きされるため、変更しないでください。 このファイルのプロパティ宣言を使用すると、クラス内のコードが `ViewController` 、IOS デザイナーで設定された**名前**でアクセスできるようになります。 **ViewController.designer.cs**を開くと、次のコードがわかります。

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

プロパティの宣言は、 `SubmitButton` `ViewController` **ViewController.designer.cs**ファイルだけでなく、クラス全体をストーリーボードで定義されているボタンに接続します。 **ViewController.cs**はクラスの一部を定義 `ViewController` するため、にアクセス `SubmitButton` できます。

次のスクリーンショットは、IntelliSense が ViewController.cs 内の参照を認識するようになったことを示してい `SubmitButton` ます。 **ViewController.cs**

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![SubmitButton 参照を認識する IntelliSense](introduction-images/6-submitbuttonintellisense-vsmac.png "SubmitButton 参照を認識する IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![SubmitButton 参照を認識する IntelliSense](introduction-images/6-submitbuttonintellisense-vs.png "SubmitButton 参照を認識する IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

このセクションでは、iOS デザイナーでボタンを作成し、コードでそのボタンにアクセスする方法を示しました。

このドキュメントの残りの部分では、iOS Designer の概要について説明します。

## <a name="ios-designer-basics"></a>iOS Designer の基本

このセクションでは、iOS デザイナーの各部分について説明し、その機能について説明します。

### <a name="launching-the-ios-designer"></a>IOS Designer の起動

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で作成した Xamarin iOS プロジェクトには、ストーリーボードが含まれます。 ストーリーボードの内容を表示するには、 **Solution Pad**で、storyboard ファイルをダブルクリックします。

[![IOS デザイナーでストーリーボードが開かれている](introduction-images/7-storyboardopen-vsmac.png "IOS デザイナーでストーリーボードが開かれている")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio で作成されたほとんどの Xamarin. iOS プロジェクトには、ストーリーボードが含まれています。 ストーリーボードの内容を表示するには、**ソリューションエクスプローラー**で、storyboard ファイルをダブルクリックします。

[![IOS デザイナーでストーリーボードが開かれている](introduction-images/7-storyboardopen-vs.png "IOS デザイナーでストーリーボードが開かれている")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"></a>

### <a name="ios-designer-features"></a>iOS デザイナーの機能

IOS Designer には、6つの主要なセクションがあります。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![IOS Designer のセクション](introduction-images/8-sixpartsofiosdesigner-vsmac.png "IOS Designer のセクション")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **デザインサーフェイス**: iOS Designer のプライマリワークスペース。 ドキュメント領域に表示されているので、ユーザーインターフェイスを視覚的に構築できます。
2. **制約ツールバー** –ユーザーインターフェイスに要素を配置するための2つの異なる方法で、フレーム編集モードと制約編集モードを切り替えることができます。
3. [**ツールボックス**] –コントローラー、オブジェクト、コントロール、データビュー、ジェスチャレコグナイザー、ウィンドウ、およびバーが表示されます。これらは、デザイン画面にドラッグしてユーザーインターフェイスに追加できます。
4. **Properties Pad** –選択したコントロールのプロパティ (id、視覚スタイル、アクセシビリティ、レイアウト、動作など) を表示します。
5. [**ドキュメントアウトライン**] –編集中のインターフェイスのレイアウトを構成するコントロールのツリーが表示されます。 ツリー内の項目をクリックすると、iOS デザイナーで項目が選択され、 **Properties Pad**にそのプロパティが表示されます。 これは、深く入れ子になったユーザーインターフェイスで特定のコントロールを選択する場合に便利です。
6. **下部のツールバー** –デバイス、向き、ズームを含む、iOS Designer でのストーリーボードまたは xib ファイルの表示方法を変更するためのオプションが含まれています。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![IOS Designer のセクション](introduction-images/8-sixpartsofiosdesigner-vs.png "IOS Designer のセクション")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **デザインサーフェイス**: iOS Designer のプライマリワークスペース。 ドキュメント領域に表示されているので、ユーザーインターフェイスを視覚的に構築できます。
2. **制約ツールバー** –ユーザーインターフェイスに要素を配置するための2つの異なる方法で、フレーム編集モードと制約編集モードを切り替えることができます。
3. [**ツールボックス**] –コントローラー、オブジェクト、コントロール、データビュー、ジェスチャレコグナイザー、ウィンドウ、およびバーが表示されます。これらは、デザイン画面にドラッグしてユーザーインターフェイスに追加できます。
4. [**プロパティウィンドウ**] –選択したコントロールのプロパティ (id、視覚スタイル、アクセシビリティ、レイアウト、動作など) が表示されます。
5. [**ドキュメントアウトライン**] –編集中のインターフェイスのレイアウトを構成するコントロールのツリーが表示されます。 ツリー内の項目をクリックすると、iOS デザイナーで項目が選択され、そのプロパティが [**プロパティ] ウィンドウ**に表示されます。 これは、深く入れ子になったユーザーインターフェイスで特定のコントロールを選択する場合に便利です。
6. **下部のツールバー** –デバイス、向き、ズームを含む、iOS Designer でのストーリーボードまたは xib ファイルの表示方法を変更するためのオプションが含まれています。

-----

### <a name="design-workflow"></a>ワークフローの設計

#### <a name="adding-a-control-to-the-interface"></a>インターフェイスへのコントロールの追加

インターフェイスにコントロールを追加するには、[**ツールボックス**] からコントロールをドラッグし、デザインサーフェイスにドロップします。 コントロールを追加または配置するときに、垂直方向および水平方向のガイドラインによって、垂直方向の中心、水平方向の中心、余白など、一般的に使用されるレイアウト位置が強調されます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![デザイン画面では、一般的に使用されるレイアウト位置に関するガイドラインが強調表示されます。](introduction-images/9-layoutguides-vsmac.png "デザイン画面では、一般的に使用されるレイアウト位置に関するガイドラインが強調表示されます。")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![デザイン画面では、一般的に使用されるレイアウト位置に関するガイドラインが強調表示されます。](introduction-images/9-layoutguides-vs.png "デザイン画面では、一般的に使用されるレイアウト位置に関するガイドラインが強調表示されます。")

-----

上の例の青い点線は、ボタンの配置を補助するための水平方向の視覚的な配置ガイドラインを示しています。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="context-menu-commands"></a>コンテキスト メニュー コマンド

コンテキストメニューは、デザイン画面と**ドキュメントアウトライン**の両方で使用できます。 このメニューには、選択したコントロールとその親のコマンドが表示されます。入れ子になった階層のビューを操作するときに便利です。

[![デザインサーフェイスのコンテキストメニュー](introduction-images/10-contextmenudesignsurface-vsmac.png "デザインサーフェイスのコンテキストメニュー")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>制約ツールバー

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![制約ツールバー](introduction-images/11-constraintstoolbar-vsmac.png "[制約] ツールバー")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![制約ツールバー](introduction-images/11-constraintstoolbar-vs.png "[制約] ツールバー")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

[制約] ツールバーが更新され、[フレーム編集モード/制約編集モード] と [更新制約/フレームの更新] ボタンの2つのコントロールで構成されるようになりました。

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>フレーム編集モード/制約編集モードの切り替え

以前のバージョンの iOS Designer では、デザイン画面で既に選択されているビューをクリックすると、フレーム編集モードと制約編集モードが切り替わりました。 ここで、[制約] ツールバーのトグルコントロールは、これらの編集モードを切り替えます。

- フレーム編集モード:

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![フレーム編集モードボタン](introduction-images/12a-frameeditingmode-vsmac.png "フレーム編集モードボタン")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![フレーム編集モードボタン](introduction-images/12a-frameeditingmode-vs.png "フレーム編集モードボタン")

-----

- 制約編集モード:

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![制約編集モードボタン](introduction-images/12b-constrainteditingmode-vsmac.png "制約編集モードボタン")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![制約編集モードボタン](introduction-images/12b-constrainteditingmode-vs.png "制約編集モードボタン")

-----

#### <a name="update-constraints--update-frames-button"></a>[制約の更新]/[フレームの更新] ボタン

[更新の制約/フレームの更新] ボタンは、フレーム編集モード/制約編集モードの切り替えの右側にあります。

- フレーム編集モードでは、このボタンをクリックすると、選択した要素のフレームが、その制約に一致するように調整されます。
- 制約編集モードでは、このボタンをクリックすると、選択した要素の制約が、そのフレームに合わせて調整されます。

### <a name="bottom-toolbar"></a>下部ツールバー

下部のツールバーでは、iOS デザイナーでストーリーボードまたは xib ファイルを表示するために使用する、デバイス、向き、およびズームを選択できます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![下部のツールバー。デザイン画面のデバイスと向きを選択するために使用されます。](introduction-images/13-bottomtoolbar-vsmac.png "下部のツールバー。デザイン画面のデバイスと向きを選択するために使用されます。")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![下部のツールバー。デザイン画面のデバイスと向きを選択するために使用されます。](introduction-images/13-bottomtoolbar-vs.png "下部のツールバー。デザイン画面のデバイスと向きを選択するために使用されます。")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>デバイスと向き

展開すると、下部のツールバーに、現在のドキュメントに適用可能なすべてのデバイス、向き、または適応が表示されます。 これらのボタンをクリックすると、デザイン画面に表示されるビューが変わります。 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![デバイスと向きを表示するために展開された下部のツールバー](introduction-images/14-bottomtoolbarexpanded-vsmac.png "デバイスと向きを表示するために展開された下部のツールバー")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![デバイスと向きを表示するために展開された下部のツールバー](introduction-images/14-bottomtoolbarexpanded-vs.png "デバイスと向きを表示するために展開された下部のツールバー")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

デバイスと向きを選択すると、iOS デザイナーがデザインをプレビューする方法のみが変更されることに注意してください。 現在の選択内容に関係なく、新たに追加された制約は、[**特徴の編集**] ボタンを使用して指定しない限り、すべてのデバイスと方向に適用されます。

[サイズクラス](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes)が[有効になって](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes)いる場合は、[**特徴の編集**] ボタンが展開下部のツールバーに表示されます。  [**特徴の編集**] ボタンをクリックすると、選択したデバイスと向きによって表されるサイズクラスに基づいてインターフェイスのバリエーションを作成するためのオプションが表示されます。 次の例を考えてみます。

- [ **IPhone SE**  /  **縦長**] が選択されている場合、segue には、compact width (標準高さサイズ) クラスのインターフェイスのバリエーションを作成するためのオプションが用意されています。 
- **IPad Pro 9.7 "**  /  **横長**  /  **全画面**" が選択されている場合、segue は、標準幅、標準高さサイズクラスのインターフェイスのバリエーションを作成するオプションを提供します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![Size クラスによってインターフェイスを変更するために使用される下部のツールバー](introduction-images/15-edittraitsbutton-vsmac.png "Size クラスによってインターフェイスを変更するために使用される下部のツールバー")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Size クラスによってインターフェイスを変更するために使用される下部のツールバー](introduction-images/15-edittraitsbutton-vs.png "Size クラスによってインターフェイスを変更するために使用される下部のツールバー")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>ズームコントロール

デザイン画面では、いくつかのコントロールを使用したズームがサポートされています。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![下部のツールバーのズームコントロール](introduction-images/16-zoomcontrols-vsmac.png "下部のツールバーのズームコントロール")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![下部のツールバーのズームコントロール](introduction-images/16-zoomcontrols-vs.png "下部のツールバーのズームコントロール")

-----

コントロールには、次のものがあります。

1. 画面のサイズに合わせる
2. 縮小します
3. 拡大します
4. 実際のサイズ (1:1 ピクセルのサイズ)

これらのコントロールは、デザインサーフェイスのズームを調整します。 実行時にアプリケーションのユーザーインターフェイスには影響しません。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

### <a name="properties-pad"></a>Properties Pad

コントロールの id、視覚スタイル、アクセシビリティ、および動作を編集するには、 **Properties Pad**を使用します。 次のスクリーンショットは、ボタンの**Properties Pad**オプションを示しています。

[![ボタンの Properties Pad](introduction-images/17-buttonpropertiespad-vsmac.png "ボタンの Properties Pad")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Properties Pad セクション

**Properties Pad**には、次の3つのセクションが含まれます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>[プロパティ] ウィンドウ

コントロールの id、視覚スタイル、アクセシビリティ、および動作を編集するには、[**プロパティ] ウィンドウ**を使用します。 次のスクリーンショットは、ボタンの [**プロパティ] ウィンドウ**のオプションを示しています。

[![ボタンの [プロパティ] ウィンドウ](introduction-images/17-buttonpropertieswindow-vs.png "ボタンの [プロパティ] ウィンドウ")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>プロパティウィンドウのセクション

[**プロパティ] ウィンドウ**には、次の3つのセクションがあります。

-----

1. **ウィジェット**–名前、クラス、スタイルプロパティなど、コントロールの主要なプロパティです。コントロールのコンテンツを管理するためのプロパティは、通常、ここに配置されます。
2. **Layout** –制約やフレームなど、コントロールの位置とサイズを追跡するプロパティを次に示します。
3. **イベント: イベント**とイベントハンドラーがここで指定されます。 タッチ、タップ、ドラッグなどのイベントを処理する場合に便利です。イベントは、コード内で直接処理することもできます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Properties Pad のプロパティの編集

IOS デザイナーでは、デザイン画面でのビジュアル編集に加えて、 **Properties Pad**のプロパティの編集もサポートされています。 使用可能なプロパティは、次のスクリーンショットに示すように、選択したコントロールに基づいて変化します。

[![ボタンのプロパティ](introduction-images/18a-buttonpropertiespad-vsmac.png "ボタンのプロパティ")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![コントローラーのプロパティを表示する](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "コントローラーのプロパティを表示する")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>[プロパティ] ウィンドウでのプロパティの編集

IOS デザイナーでは、デザイン画面でのビジュアル編集に加えて、[**プロパティ] ウィンドウ**でのプロパティの編集もサポートされています。 使用可能なプロパティは、次のスクリーンショットに示すように、選択したコントロールに基づいて変化します。

[![ボタンのプロパティ](introduction-images/18a-buttonpropertieswindow-vs.png "ボタンのプロパティ")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![コントローラーのプロパティを表示する](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "コントローラーのプロパティを表示する")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Properties Pad の Id セクションに**モジュール**フィールドが表示されるようになりました。 Swift クラスと相互運用する場合にのみ、このセクションに入力する必要があります。 これを使用して、Swift クラスのモジュール名 (名前空間) を入力します。

#### <a name="default-values"></a>既定の値

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

**Properties Pad**の多くのプロパティには、値も既定値も表示されません。 ただし、アプリケーションのコードでは、これらの値が変更される可能性があります。 **Properties Pad**は、コードで設定された値を表示しません。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[**プロパティ] ウィンドウ**の多くのプロパティには、値も既定値も表示されません。 ただし、アプリケーションのコードでは、これらの値が変更される可能性があります。 [**プロパティ] ウィンドウ**には、コードで設定された値は表示されません。

-----

#### <a name="event-handlers"></a>イベント ハンドラー

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

さまざまなイベントのカスタムイベントハンドラーを指定するには、 **Properties Pad**の [**イベント**] タブを使用します。 たとえば、次のスクリーンショットでは、 `HandleClick` メソッドがイベント内のボタンの**タッチアップ**を処理しています。

[![ボタンのイベントハンドラーが設定されている Properties Pad。](introduction-images/19-buttonpropertiespadevents-vsmac.png "ボタンのイベントハンドラーが設定されている Properties Pad。")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

さまざまなイベントのカスタムイベントハンドラーを指定するには、[**プロパティ] ウィンドウ**の [**イベント**] タブを使用します。 たとえば、次のスクリーンショットでは、 `HandleClick` メソッドがイベント内のボタンの**タッチアップ**を処理しています。

[![[プロパティ] ウィンドウ。ボタンのイベントハンドラーが設定されています。](introduction-images/19-buttonpropertieswindowevents-vs.png "[プロパティ] ウィンドウ。ボタンのイベントハンドラーが設定されています。")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

イベントハンドラーを指定したら、同じ名前のメソッドを、対応するビューコントローラークラスに追加する必要があります。 それ以外の場合は、 `unrecognized selector` ボタンがタップされると例外が発生します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![認識されないセレクター例外です。](introduction-images/20-unrecognizedselector-vsmac.png "認識されないセレクター例外です。")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

**Properties Pad**でイベントハンドラーが指定された後、iOS デザイナーは、対応するコードファイルをすぐに開き、メソッド宣言を挿入するように提供します。 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![認識されないセレクター例外です。](introduction-images/20-unrecognizedselector-vs.png "認識されないセレクター例外です。")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

カスタムイベントハンドラーの使用例については、「 [Hello, iOS はじめにガイド」](~/ios/get-started/hello-ios/index.md)を参照してください。

### <a name="outline-view"></a>アウトライン ビュー

IOS デザイナーでは、インターフェイスのコントロール階層をアウトラインとして表示することもできます。 次に示すように、[**ドキュメントアウトライン**] タブを選択すると、アウトラインを表示できます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![ドキュメントアウトライン](introduction-images/21-buttonoutlineview-vsmac.png "ドキュメントアウトライン")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![ドキュメントアウトライン](introduction-images/21-buttonoutlineview-vs.png "ドキュメントアウトライン")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

[アウトライン] ビューで選択したコントロールは、デザインサーフェイス上の選択したコントロールと同期したままになります。  この機能は、深い入れ子になったインターフェイス階層から項目を選択する場合に便利です。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Xcode に戻す

IOS Designer と Xcode Interface Builder、同じ意味で使用することができます。 ストーリーボードまたは xib ファイルを Xcode Interface Builder で開くには、次のスクリーンショットに示すように、ファイルを右クリックし、[ **Interface Builder > で開く**] を選択します。

[![Xcode Interface Builder でストーリーボードを開く](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode Interface Builder でストーリーボードを開く")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode Interface Builder で編集を行った後、ファイルを保存して Visual Studio for Mac に戻ります。 変更は、Xamarin. iOS プロジェクトに同期されます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="revert-to-xcode"></a>Xcode に戻す

IOS Designer と Xcode Interface Builder は同じように使用できますが、Xcode Interface Builder は Mac でのみ使用できます。 Mac 上の Xcode Interface Builder でストーリーボードまたは xib ファイルを開くには、次のスクリーンショットに示すように、 [Visual Studio for Mac](/visualstudio/mac/)で Xamarin の iOS プロジェクトを含むソリューションを開き、ファイルを右クリックして [ **> Xcode Interface Builder で開く**] を選択します。

[![Xcode Interface Builder でストーリーボードを開く](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode Interface Builder でストーリーボードを開く")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode Interface Builder で編集を行った後、ファイルを保存して Visual Studio for Mac に戻ります。 変更は、Xamarin. iOS プロジェクトに同期されます。

-----

## <a name="xib-support"></a>. xib のサポート

IOS デザイナーでは、xib ファイルの作成、編集、および管理がサポートされています。 これらは、アプリケーションのビュー階層に追加できる単一のカスタムビューを送信する XML ファイルです。 Xib ファイルは、通常、アプリケーション内の1つのビューまたは画面のインターフェイスを表します。一方、ストーリーボードは、多くの画面とそれらの間の遷移を表します。

ユーザーインターフェイスの作成と保守に最適なソリューション (xib ファイル、ストーリーボード、コード) については多くの意見があります。 実際には、完全な解決策はありません。ジョブに最適なツールは常に考慮することをお勧めします。 ただし、xib ファイルは、通常、カスタムテーブルビューのセルなど、アプリ内の複数の場所で必要なカスタムビューを表すために使用される場合に最も強力です。 

Xib ファイルの使用に関するその他のドキュメントについては、次のレシピを参照してください。

- [Xib テンプレートの使用](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Xib を使用してカスタム TableViewCell を作成する](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Xib を使用して起動画面を作成する](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

ストーリーボードの使用方法の詳細については、「[ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)」を参照してください。

この iOS デザイナー関連のガイドでは、ほとんどの Xamarin. iOS の新しいプロジェクトテンプレートでストーリーボードが既定で提供されるため、ユーザーインターフェイスを構築するための標準的なアプローチとしてストーリーボードを使用することを示しています。

## <a name="summary"></a>まとめ

このガイドでは、iOS Designer の概要について説明し、機能を説明し、美しいユーザーインターフェイスを設計するために提供するツールの概要を説明しました。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS のデザイン可能コントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS マルチスクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android Designer の概要](~/android/user-interface/android-designer/index.md)
- [部分クラスと部分メソッド](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Xamarin Designer for iOS の進化 2014 (ビデオ)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [IOS Designer を使用した起動画面の作成 (ビデオ)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
