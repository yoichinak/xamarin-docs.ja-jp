---
title: iOS Designer の基本
description: このガイドでは、iOS 用の Xamarin デザイナーについて説明します。 IOS Designer を使用してコントロールを視覚的にレイアウトする方法、コードでは、それらのコントロールにアクセスする方法、およびプロパティを編集する方法を示します。
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/31/2018
ms.openlocfilehash: e9c2a42b9108c04f18252a410d40dbc03013f6dd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123753"
---
# <a name="ios-designer-basics"></a>iOS Designer の基本

_このガイドでは、iOS 用の Xamarin デザイナーについて説明します。IOS Designer を使用してコントロールを視覚的にレイアウトする方法、コードでは、それらのコントロールにアクセスする方法、およびプロパティを編集する方法を示します。_

IOS 用の Xamarin デザイナーとは、Xcode の Interface Builder のようなビジュアル インターフェイス デザイナーと Android デザイナーです。 多数の機能の一部には、Visual Studio for Mac と Visual Studio 2015 および 2017、ドラッグ アンド ドロップ編集、イベントのハンドラーを設定するためのインターフェイス、およびカスタム コントロールをレンダリングする機能とシームレスな統合が含まれます。

## <a name="requirements"></a>必要条件

IOS デザイナーは、Windows でおよび Visual Studio 2015 と 2017 の Visual Studio for Mac で利用できます。 Visual Studio 2015 または 2017 では、iOS Designer では、Xcode が実行されていない必要がある場合に、適切に構成された Mac ビルド ホストへの接続が必要です。

このガイドで説明した内容を熟知することを前提としています、[ガイド ファースト](~/ios/get-started/index.md)します。

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>IOS Designer の動作方法

このセクションでは、ユーザー インターフェイスを作成し、コードに接続して iOS Designer を容易にする方法について説明します。

IOS Designer では、アプリケーションのユーザー インターフェイスを視覚的に設計できます。 」の説明に従って、[ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)ガイド、ストーリー ボード画面について説明します、(コント ローラーの表示)、アプリを構成するこれらのビュー コント ローラーと、アプリの全体的なナビゲーションのフローに格納されて、インターフェイス要素 (ビュー). 

ビュー コント ローラーが 2 つの部分: iOS Designer でビジュアルな表示と関連付けられている c# クラス。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![IOS Designer のビュー コント ローラー](introduction-images/1-storyboardwithviewcontroller-vsmac.png "iOS Designer のビュー コント ローラー")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![ビュー コント ローラーのコード](introduction-images/2-viewcontrollercode-vsmac.png "ビュー コント ローラーのコード")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IOS Designer のビュー コント ローラー](introduction-images/1-storyboardwithviewcontroller-vs.png "iOS Designer のビュー コント ローラー")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![ビュー コント ローラーのコード](introduction-images/2-viewcontrollercode-vs.png "ビュー コント ローラーのコード")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

既定の状態では、ビュー コント ローラーによって; 任意の機能が備わっていません。コントロールに入力する必要があります。 これらのコントロールは、ビュー コント ローラーのビューでは、すべての画面のコンテンツを含む四角形の領域に配置されます。 ほとんどのビュー コント ローラーには、ボタンを含むビュー コント ローラーを示しています。 次のスクリーン ショットに示すようにボタン、ラベル、テキスト フィールドなどの一般的なコントロールが含まれます。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ボタンを含むビュー コント ローラー](introduction-images/3-viewcontrollerwithbutton-vsmac.png "ボタンを含むビュー コント ローラー")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ボタンを含むビュー コント ローラー](introduction-images/3-viewcontrollerwithbutton-vs.png "ボタンを含むビュー コント ローラー")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

静的テキストを含むラベルなど、一部のコントロールをビュー コント ローラーに追加し、単独でのままです。 ただし、たいてい、制御する必要がありますのカスタマイズ プログラムを使用します。 たとえば、ボタンの上に追加する必要がありますを行いますタップしたときにため、コードでイベント ハンドラーを追加する必要があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

コードでボタンの操作にアクセスし、するには、一意の識別子が必要です。 開く ボタンを選択し、一意の識別子を指定、 **Properties Pad**、設定とその**名前**「ボタン」などの値にフィールド。

[![Properties Pad でボタンの名前を設定](introduction-images/4-settingbuttonname-vsmac.png "Properties Pad でボタンの名前を設定します。")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

コードでボタンの操作にアクセスし、するには、一意の識別子が必要です。 開く ボタンを選択し、一意の識別子を指定、**プロパティ ウィンドウ**、設定とその**名前**「ボタン」などの値にフィールド。

[![[プロパティ] ウィンドウで、ボタンの名前を設定](introduction-images/4-settingbuttonname-vs.png "プロパティ ウィンドウで、ボタンの名前を設定します。")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

これで、ボタンは、名前を持つ、コードでアクセスできます。 しかし、動作のでしょうか。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**Solution Pad**に移動する、 **ViewController.cs**わかりました漏えいインジケーターをクリックすると、ビュー コント ローラーの`ViewController`2 つのクラス定義の範囲がファイルの含まれています、[部分クラス](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)定義。

[![2 つのファイル、ViewController クラスを構成する: ViewController.cs と ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "2 つのファイルをことで、ViewController クラス: ViewController.cs と ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**ソリューション エクスプ ローラー**に移動する、 **ViewController.cs**わかりました漏えいインジケーターをクリックすると、ビュー コント ローラーの`ViewController`クラス定義が、それぞれの 2 つのファイルにまたがる含まれています、[部分クラス](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)定義。

[![2 つのファイル、ViewController クラスを構成する: ViewController.cs と ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "2 つのファイルをことで、ViewController クラス: ViewController.cs と ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs**に関連するカスタム コードを代入するか、`ViewController`クラス。 このファイルで、`ViewController`クラスは、さまざまな iOS ビュー コント ローラーのライフ サイクル メソッドの応答、UI のカスタマイズ、およびユーザーがボタンのタップなどの入力に応答します。

- **ViewController.designer.cs**は iOS コードを視覚的に構築されたインターフェイスにマップするデザイナーによって作成された、生成されたファイルです。 このファイルに対する変更は上書きされるためには変更できません。 このファイルのプロパティの宣言を行うでコードを`ViewController`によってアクセス、クラス**名前**、iOS Designer での設定を制御します。 開く**ViewController.designer.cs**次のコードが表示されます。

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

`SubmitButton`プロパティの宣言全体を接続する`ViewController`クラスには、 **ViewController.designer.cs**ストーリー ボードで定義されているボタンに – ファイル。 **ViewController.cs**の一部を定義、`ViewController`クラスへのアクセスが`SubmitButton`します。

次のスクリーン ショットは、IntelliSense が認識されるようになりましたことを示しています、`SubmitButton`で参照**ViewController.cs**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![損害賠償参照を認識する IntelliSense](introduction-images/6-submitbuttonintellisense-vsmac.png "損害賠償参照を認識する IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![損害賠償参照を認識する IntelliSense](introduction-images/6-submitbuttonintellisense-vs.png "損害賠償参照を認識する IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

このセクションで説明しました iOS Designer でボタンを作成し、コードでは、そのボタンにアクセスする方法。

このドキュメントの残りの部分では、iOS Designer の詳細の概要を示します。

## <a name="ios-designer-basics"></a>iOS Designer の基本

このセクションでは、iOS Designer の一部を紹介し、その機能のツアーを提供します。

### <a name="launching-the-ios-designer"></a>IOS Designer を起動します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で作成された Xamarin.iOS プロジェクトには、ストーリー ボードが含まれます。 ストーリー ボードの内容を表示するで .storyboard ファイルをダブルクリック、 **Solution Pad**:

[![IOS Designer でストーリー ボードが開いて](introduction-images/7-storyboardopen-vsmac.png "iOS Designer でストーリー ボードを開きます")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 2015 または 2017 で作成されたほとんどの Xamarin.iOS プロジェクトには、ストーリー ボードが含まれます。 ストーリー ボードの内容を表示するで .storyboard ファイルをダブルクリック、**ソリューション エクスプ ローラー**:

[![IOS Designer でストーリー ボードが開いて](introduction-images/7-storyboardopen-vs.png "iOS Designer でストーリー ボードを開きます")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS デザイナーの機能

IOS Designer では、6 つの主要なセクションがあります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![IOS Designer のセクションでは](introduction-images/8-sixpartsofiosdesigner-vsmac.png "iOS Designer のセクション")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **デザイン画面**– iOS デザイナーのプライマリ ワークスペースです。 ユーザー インターフェイスのビジュアルの構築、ドキュメント領域に示すようにできます。
2. **制約ツールバー** – モードと編集モードでの制約は、2 つの方法をユーザー インターフェイス要素を配置する編集フレーム間の切り替えを使用します。
3. **ツールボックス**– コント ローラー、オブジェクト、コントロール、データ ビュー、ジェスチャ レコグナイザー、windows の一覧し、バーに、デザイン サーフェイスにドラッグし、ユーザー インターフェイスに追加できます。
4. **Properties Pad** – identity、視覚スタイル、アクセシビリティ、レイアウト、および動作を含む、選択したコントロールのプロパティを表示します。
5. **ドキュメント アウトライン**– 編集されているインターフェイスのレイアウトを作成するコントロールのツリーを示しています。 IOS Designer でその選択し、そのプロパティを示しています、ツリー内の項目をクリックすると、 **Properties Pad**します。 これは、深く入れ子になったユーザー インターフェイスで特定のコントロールを選択するのに便利です。
6. **下のツールバー** – iOS Designer でのデバイス、向き、ズームなど、.storyboard または .xib ファイルの表示方法を変更するためのオプションが含まれています。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IOS Designer のセクションでは](introduction-images/8-sixpartsofiosdesigner-vs.png "iOS Designer のセクション")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **デザイン画面**– iOS デザイナーのプライマリ ワークスペースです。 ユーザー インターフェイスのビジュアルの構築、ドキュメント領域に示すようにできます。
2. **制約ツールバー** – モードと編集モードでの制約は、2 つの方法をユーザー インターフェイス要素を配置する編集フレーム間の切り替えを使用します。
3. **ツールボックス**– コント ローラー、オブジェクト、コントロール、データ ビュー、ジェスチャ レコグナイザー、windows の一覧し、バーに、デザイン サーフェイスにドラッグし、ユーザー インターフェイスに追加できます。
4. **[プロパティ] ウィンドウ**– identity、視覚スタイル、アクセシビリティ、レイアウト、および動作を含む、選択したコントロールのプロパティを表示します。
5. **ドキュメント アウトライン**– 編集されているインターフェイスのレイアウトを作成するコントロールのツリーを示しています。 IOS Designer でその選択し、そのプロパティを示しています、ツリー内の項目をクリックすると、**プロパティ ウィンドウ**します。 これは、深く入れ子になったユーザー インターフェイスで特定のコントロールを選択するのに便利です。
6. **下のツールバー** – iOS Designer でのデバイス、向き、ズームなど、.storyboard または .xib ファイルの表示方法を変更するためのオプションが含まれています。

-----

### <a name="design-workflow"></a>ワークフローを設計します。

#### <a name="adding-a-control-to-the-interface"></a>インターフェイスにコントロールの追加

インターフェイスにコントロールを追加するからをドラッグ、**ツールボックス**し、デザイン画面にドロップします。 追加またはコントロールを配置、垂直および水平方向のガイドラインに垂直方向の中心、水平方向の中央、および余白などのよく使われるレイアウト位置が強調表示します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
 
![ガイドラインをデザイン サーフェイスには、一般的に使用されるレイアウトの配置を強調表示](introduction-images/9-layoutguides-vsmac.png "ガイドラインをデザイン サーフェイスには、一般的に使用されるレイアウトの配置を強調表示")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![ガイドラインをデザイン サーフェイスには、一般的に使用されるレイアウトの配置を強調表示](introduction-images/9-layoutguides-vs.png "ガイドラインをデザイン サーフェイスには、一般的に使用されるレイアウトの配置を強調表示")

-----

上記の例では青い点線は、ボタンの配置を支援する水平方向の中央 visual 配置ガイドラインを示します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="context-menu-commands"></a>コンテキスト メニュー コマンド

コンテキスト メニューがデザイン サーフェイスとで使用可能な**ドキュメント アウトライン**します。 このメニューには、選択したコントロールとその親は、入れ子になった階層内のビューを使用する場合に便利ですコマンドが提供されます。

[![デザイン サーフェイスのコンテキスト メニュー](introduction-images/10-contextmenudesignsurface-vsmac.png "デザイン サーフェイスのコンテキスト メニュー")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>制約ツールバー

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
 
[![制約ツールバー](introduction-images/11-constraintstoolbar-vsmac.png "制約ツールバー")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![制約ツールバー](introduction-images/11-constraintstoolbar-vs.png "制約ツールバー")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

制約ツールバーが更新され、ここでは 2 つのコントロールで構成されます。 編集モード フレーム/制約モードの切り替えと更新制約を編集/フレーム ボタンを更新します。

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>編集モードのフレーム]、[制約の編集モードの切り替え

IOS Designer の以前のバージョンで既に選択されているビューは、デザイン サーフェイスをクリックすると、モードと編集モードの制約を編集フレームの間で切り替えます。 ここで、制約のツールバーのトグル コントロールは、これらの編集モードの間で切り替わります。

- フレームの編集モード。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![編集モード ボタン フレーム](introduction-images/12a-frameeditingmode-vsmac.png "フレームの編集モード ボタン")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![編集モード ボタン フレーム](introduction-images/12a-frameeditingmode-vs.png "フレームの編集モード ボタン")

-----

- 制約の編集モード:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![編集モード ボタン制約](introduction-images/12b-constrainteditingmode-vsmac.png "制約編集モード ボタン")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![編集モード ボタン制約](introduction-images/12b-constrainteditingmode-vs.png "制約編集モード ボタン")

-----

#### <a name="update-constraints--update-frames-button"></a>制約の更新/フレーム ボタンを更新

更新制約]、[更新プログラムが編集モードのフレームの右側にボタンの位置をフレーム/制約モードの切り替えを編集します。

- 編集モードのフレームでは、このボタンをクリックすると、制約に一致するように選択した要素のフレームを調整します。
- 制約の編集モードでは、このボタンをクリックすると、フレームを一致するように選択した要素の制約を調整します。

### <a name="bottom-toolbar"></a>下部ツールバー

下部のツールバーには、デバイス、向き、および zoom が iOS Designer でストーリー ボードまたは .xib ファイルを表示するために使用を選択する方法を提供します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![下部にあるツールバーで、デバイスと、デザイン画面の向きを選択するために使用](introduction-images/13-bottomtoolbar-vsmac.png "下部にあるツールバーで、デバイスと、デザイン画面の向きを選択するために使用")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![下部にあるツールバーで、デバイスと、デザイン画面の向きを選択するために使用](introduction-images/13-bottomtoolbar-vs.png "下部にあるツールバーで、デバイスと、デザイン画面の向きを選択するために使用")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>デバイスや向き

展開すると、すべてのデバイス、向き、および適応の現在のドキュメントに適用できる下部ツールバーが表示されます。 クリックしてデザイン サーフェイスに表示されるビューを変更します。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![下部のツールバーに展開デバイスと向きを表示する](introduction-images/14-bottomtoolbarexpanded-vsmac.png "下部にあるツールバーで、展開のデバイスや向きを表示するには")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![下部のツールバーに展開デバイスと向きを表示する](introduction-images/14-bottomtoolbarexpanded-vs.png "下部にあるツールバーで、展開のデバイスや向きを表示するには")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

デバイスを選択することに注意してください。 し、印刷の向きが iOS Designer で設計をプレビューする方法だけを変更します。 現在の選択に関係なく新しく追加された場合を除き、すべてのデバイスや向きの制約が適用されます、**編集 Traits**  ボタンを指定しない場合に使用されています。

ときに[サイズ クラス](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes)は[有効になっている](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes)、**編集 Traits**ボタンが展開されている下部のツールバーに表示されます。  クリックすると、**編集 Traits**ボタンには、選択したデバイスや向きによって表されるサイズ クラスに基づく、インターフェイスのバリエーションを作成するためのオプションが表示されます。 次に例を示します。

- 場合**iPhone SE** / **縦**が選択すると、ポップ オーバーは compact の幅、高さの通常サイズ クラス、インターフェイスのバリエーションを作成するオプションが提供されます。 
- 場合**iPad Pro 9.7 インチ** / **ランドス ケープ** / **全画面表示**は選択すると、ポップ オーバー オプションが提供されるインターフェイスのバリエーションを作成するには通常の幅、高さの通常サイズ クラス。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![インターフェイスを異なるサイズ クラスによって使用されている下部ツールバー](introduction-images/15-edittraitsbutton-vsmac.png "インターフェイスを異なるサイズ クラスによって使用されている下部ツールバー")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![インターフェイスを異なるサイズ クラスによって使用されている下部ツールバー](introduction-images/15-edittraitsbutton-vs.png "インターフェイスを異なるサイズ クラスによって使用されている下部ツールバー")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>ズーム コントロール

デザイン画面では、いくつかのコントロールを使用してズームがサポートされています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
 
![下部のツールバーで、ズーム コントロール](introduction-images/16-zoomcontrols-vsmac.png "下部ツールバーのズーム コントロール")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![下部のツールバーで、ズーム コントロール](introduction-images/16-zoomcontrols-vs.png "下部ツールバーのズーム コントロール")

-----

コントロールを以下に示します。

1. ウィンドウに合わせる
2. ズーム アウト
3. 拡大表示
4. 実際のサイズ (1 対 1 のピクセル サイズ)

これらのコントロールは、デザイン サーフェイスのズームを調整します。 実行時にアプリケーションのユーザー インターフェイスには影響しません。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="properties-pad"></a>Properties Pad

使用して、 **Properties Pad** id、視覚スタイル、アクセシビリティ、およびコントロールの動作を編集します。 次のスクリーン ショットを示しています、 **Properties Pad**ボタンのオプション。

[![ボタンの Properties Pad](introduction-images/17-buttonpropertiespad-vsmac.png "ボタンの Properties Pad")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>プロパティ パッド セクション

**Properties Pad** 3 つのセクションが含まれています。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>[プロパティ] ウィンドウ

使用して、**プロパティ ウィンドウ**id、視覚スタイル、アクセシビリティ、およびコントロールの動作を編集します。 次のスクリーン ショットを示しています、**プロパティ ウィンドウ**ボタンのオプション。

[![ボタンのプロパティ ウィンドウ](introduction-images/17-buttonpropertieswindow-vs.png "ボタン、[プロパティ] ウィンドウ")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>プロパティ ウィンドウのセクション

**プロパティ ウィンドウ**3 つのセクションが含まれています。

-----

1.  **ウィジェット**– (名、クラス、スタイル プロパティなど)、コントロールの主要なプロパティ。コントロールのコンテンツを管理するためのプロパティは通常は、ここに配置されます。
2.  **レイアウト**– の制約とフレームを含む、コントロールのサイズと位置を追跡するプロパティがここに一覧表示します。
3.  **イベント**– イベントとイベント ハンドラーがここで指定します。 (タッチ、タップ、ドラッグなど) のイベントを処理するために便利です。イベントを処理して、コード内で直接こともできます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Properties Pad でプロパティを編集

IOS Designer がでプロパティを編集をサポート ビジュアル編集、デザイン サーフェイスに加え、 **Properties Pad**します。 次のスクリーン ショットに示すように、選択したコントロールに基づいて使用可能なプロパティの変更:

[![プロパティをボタン](introduction-images/18a-buttonpropertiespad-vsmac.png "プロパティをボタン")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![コント ローラーのプロパティを表示](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "コント ローラーのプロパティを表示")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>[プロパティ] ウィンドウでプロパティを編集

IOS Designer がでプロパティを編集をサポート ビジュアル編集、デザイン サーフェイスに加え、**プロパティ ウィンドウ**します。 次のスクリーン ショットに示すように、選択したコントロールに基づいて使用可能なプロパティの変更:

[![プロパティをボタン](introduction-images/18a-buttonpropertieswindow-vs.png "プロパティをボタン")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![コント ローラーのプロパティを表示](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "コント ローラーのプロパティを表示")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Properties Pad ここで示していますの Identity セクション、**モジュール**フィールド。 Swift クラスと相互運用する場合にのみ、このセクションで入力する必要があります。 Swift クラス、名前空間内のモジュール名を入力するのにには、これを使用します。

#### <a name="default-values"></a>既定の値

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

多くのプロパティを**Properties Pad**値または既定値を表示します。 ただし、アプリケーションのコードでは、これらの値が変更も可能性があります。 **Properties Pad**コードで設定された値は表示されません。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

多くのプロパティを**プロパティ ウィンドウ**値または既定値を表示します。 ただし、アプリケーションのコードでは、これらの値が変更も可能性があります。 **プロパティ ウィンドウ**コードで設定された値は表示されません。

-----

#### <a name="event-handlers"></a>イベント ハンドラー

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

さまざまなイベントのカスタム イベント ハンドラーを指定するには、使用、**イベント**のタブ、 **Properties Pad**します。 次のスクリーン ショット、`HandleClick`メソッド処理のボタンの**タッチを内部**イベント。

[![[プロパティ] タブのボタンの設定をイベント ハンドラーで](introduction-images/19-buttonpropertiespadevents-vsmac.png "ボタンの設定をイベント ハンドラーで、Properties Pad")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

さまざまなイベントのカスタム イベント ハンドラーを指定するには、使用、**イベント**のタブ、**プロパティウィンドウ**します。 次のスクリーン ショット、`HandleClick`メソッド処理のボタンの**タッチを内部**イベント。

[![[プロパティ] ウィンドウのボタンの設定、イベント ハンドラーを持つ](introduction-images/19-buttonpropertieswindowevents-vs.png "[プロパティ] ウィンドウのイベント ハンドラー ボタンの設定")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

イベント ハンドラーを指定すると、同じ名前のメソッドが対応するビュー コント ローラー クラスに追加する必要があります。 それ以外の場合、`unrecognized selector`ボタンがタップされたときに例外が発生します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![認識されないセレクター例外](introduction-images/20-unrecognizedselector-vsmac.png "セレクターが認識されていない例外")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

イベント ハンドラーの後で指定されていること、 **Properties Pad**、iOS デザイナーがすぐに対応するコード ファイルを開き、メソッド宣言を挿入して提供します。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![認識されないセレクター例外](introduction-images/20-unrecognizedselector-vs.png "セレクターが認識されていない例外")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

カスタム イベント ハンドラーを使用する例を参照してください、[こんにちは, iOS Getting Started Guide](~/ios/get-started/hello-ios/index.md)します。

### <a name="outline-view"></a>アウトライン ビュー

IOS Designer では、アウトラインとしてもコントロールのインターフェイスの階層を表示できます。 選択して、アウトラインは利用可能な**ドキュメント アウトライン**タブの次に示すよう。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ドキュメント アウトライン](introduction-images/21-buttonoutlineview-vsmac.png "ドキュメント アウトライン")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ドキュメント アウトライン](introduction-images/21-buttonoutlineview-vs.png "ドキュメント アウトライン")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

アウトライン ビューの選択したコントロールがデザイン画面で選択されているコントロールとの同期が維持されます。  この機能は、深く入れ子になったインターフェイスの階層から項目を選択するために便利です。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Xcode を元に戻す

IOS デザイナーと Xcode の Interface Builder を同じ意味で使用することになります。 Xcode の Interface Builder では、ストーリー ボードまたは .xib ファイルを開く、ファイルを選択します右クリックして**ファイルを開く > Xcode の Interface Builder**次のスクリーン ショットに示しますように。

[![Xcode の Interface Builder のストーリー ボードを開く](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode の Interface Builder のストーリー ボードを開く")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode の Interface Builder での編集を行った後ファイルを保存し、Visual studio for mac を返す 変更は、Xamarin.iOS プロジェクトに同期されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-----

## <a name="xib-support"></a>.xib サポート

IOS Designer は、作成、編集、および .xib ファイルの管理をサポートします。 これらの XML ファイルを表します、1 つのカスタム表示するアプリケーションのビュー階層に追加できるは。 .Xib ファイルは、ストーリー ボードは、多くの画面とそれらの間の遷移を表しますが、一般に 1 つのビューまたはアプリケーションでは、画面のインターフェイスを表します。

多くの意見について – .xib ファイル、ストーリー ボード、またはコード – ソリューションに最適の作成と管理ユーザー インターフェイスがあります。 実際には、完全なソリューションではありませんし、手元のジョブに最適なツールを検討する価値は常にします。 ただし、.xib ファイルは通常、最も強力なカスタム table view cell をなど、アプリで複数の場所で必要なカスタム ビューを表すために使用する場合。 

.Xib ファイルの使用についての他のドキュメントは、次のレシピにあります。

- [ビューの .xib テンプレートを使用します。](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [.Xib を使用してカスタム TableViewCell を作成します。](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [.Xib を使用して、起動画面を作成します。](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

ストーリー ボードの使用に関する詳細についてを参照してください、[ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)します。

これと他の iOS デザイナー関連ガイドは、Xamarin.iOS 新しいのほとんどのプロジェクト テンプレートが既定では、ストーリー ボードを提供するためのユーザー インターフェイスを構築するため、標準的なアプローチとしてストーリー ボードの使用を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、iOS Designer、その機能を記述して、アウトラインの美しいユーザー インターフェイスの設計を提供するツールの概要を提供します。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS デザイン可能なコントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS マルチスクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [部分クラスと部分メソッド](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [IOS - Evolve 2014 (ビデオ) 用の Xamarin デザイナーについてください。](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [IOS Designer を使用して (ビデオ) 起動画面を作成するには](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
