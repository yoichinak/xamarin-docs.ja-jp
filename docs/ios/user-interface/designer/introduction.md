---
title: "iOS デザイナーの基礎"
description: "このガイドでは、iOS 用の Xamarin デザイナーについて説明します。 IOS デザイナーを使用してコントロールを視覚的にレイアウトする方法、コードでは、これらのコントロールにアクセスする方法、およびプロパティを編集する方法を示しています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: a2445e49005175f62e4d7cd8aadccb5f596177bf
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="ios-designer-basics"></a>iOS デザイナーの基礎

_このガイドでは、iOS 用の Xamarin デザイナーについて説明します。IOS デザイナーを使用してコントロールを視覚的にレイアウトする方法、コードでは、これらのコントロールにアクセスする方法、およびプロパティを編集する方法を示しています。_

IOS 用の Xamarin デザイナーは、Xcode のインターフェイスのビルダーのようなビジュアル インターフェイス デザイナーと、Android デザイナーです。 多数の機能の一部には、Visual Studio for Mac と Visual Studio 2015 および 2017、ドラッグ アンド ドロップ編集、イベント ハンドラーを設定するためのインターフェイスをカスタム コントロールを表示する機能とのシームレスな統合が含まれます。

## <a name="requirements"></a>必要条件

IOS デザイナーは、windows や、Visual Studio 2015、2017 Mac 用の Visual Studio で使用します。 Visual Studio 2015 または 2017 では、iOS デザイナーでは、Xcode が実行されていない必要がある場合に、正しく構成されている Mac ビルド ホストへの接続が必要です。

このガイドで説明した内容に関する知識を前提と、[概要ガイド](~/ios/get-started/index.md)です。

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>IOS デザイナーのしくみ

このセクションでは、ユーザー インターフェイスを作成して、コードに接続する iOS デザイナーを容易にする方法について説明します。

IOS デザイナーでは、アプリケーションのユーザー インターフェイスを視覚的にデザインをすることができます。 」の説明に従って、[ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)ガイド、ストーリー ボード画面について説明 (コント ローラーの表示)、アプリを構成するこれらのビューのコント ローラー、およびアプリの全体的なナビゲーション フロー上に配置インターフェイス要素 (ビュー). 

ビューのコント ローラーが 2 つの部分: iOS デザイナーの視覚的に表現し、関連付けられている c# クラス。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![IOS デザイナーでビュー コント ローラー](introduction-images/1-storyboardwithviewcontroller-vsmac.png "iOS デザイナーでビュー コント ローラー")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![ビューのコント ローラーのコード](introduction-images/2-viewcontrollercode-vsmac.png "ビュー コント ローラーのコード")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IOS デザイナーでビュー コント ローラー](introduction-images/1-storyboardwithviewcontroller-vs.png "iOS デザイナーでビュー コント ローラー")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![ビューのコント ローラーのコード](introduction-images/2-viewcontrollercode-vs.png "ビュー コント ローラーのコード")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

ビュー コント ローラーで、既定の状態です。 機能が提供しません。コントロールに入力する必要があります。 これらのコントロールは、ビュー コント ローラーの表示、すべての画面のコンテンツを含む四角形の領域に配置されます。 ほとんどコント ローラーの表示は、ボタンを含むビュー コント ローラーを示しています。 次のスクリーン ショットに示すように、ボタン、ラベル、およびテキスト フィールドなどのコモン コントロールを含めます。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ボタンを含むビュー コント ローラー](introduction-images/3-viewcontrollerwithbutton-vsmac.png "ボタンを含むビュー コント ローラー")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ボタンを含むビュー コント ローラー](introduction-images/3-viewcontrollerwithbutton-vs.png "ボタンを含むビュー コント ローラー")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

静的テキストを含むラベルなど、一部のコントロールをビュー コント ローラーに追加し、単独でのままです。 ただし、ほとんどの場合、制御する必要がありますのカスタマイズ プログラムでします。 たとえば、ボタンの上に追加する必要があります操作してみましょうタップしたときにため、コードでイベント ハンドラーを追加する必要があります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

アクセスし、コードでボタンを操作するために一意の識別子が必要です。 開く ボタンを選択し、一意の識別子を指定、**プロパティ パッド**、設定とその**名前**「ボタン」などの値をフィールド。

[![プロパティ パッドで、ボタンの名前を設定する](introduction-images/4-settingbuttonname-vsmac.png "プロパティ パッドで、ボタンの名前を設定します。")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

アクセスし、コードでボタンを操作するために一意の識別子が必要です。 開く ボタンを選択し、一意の識別子を指定、**プロパティ ウィンドウ**、設定とその**名前**「ボタン」などの値をフィールド。

[![[プロパティ] ウィンドウで、ボタンの名前を設定する](introduction-images/4-settingbuttonname-vs.png "プロパティ ウィンドウで、ボタンの名前を設定します。")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

これで、ボタンは、名前を持つ、コードでアクセスできます。 しかし、動作のでしょうか。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**ソリューション パッド**に間を移動する、 **ViewController.cs**によってわかりました漏えいインジケーターをクリックすると、ビューのコント ローラーの`ViewController`2 つのクラス定義のスパン ファイルは、それぞれの含まれています、[部分クラス](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)定義。

[![2 つのファイルの ViewController クラスを構成する: ViewController.cs と ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "ViewController クラスを構成する 2 つファイル: ViewController.cs と ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**ソリューション エクスプ ローラー**に間を移動する、 **ViewController.cs**によってわかりました漏えいインジケーターをクリックすると、ビューのコント ローラーの`ViewController`クラス定義が、それぞれの 2 つのファイルにまたがる含む、[部分クラス](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)定義。

[![2 つのファイルの ViewController クラスを構成する: ViewController.cs と ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "ViewController クラスを構成する 2 つファイル: ViewController.cs と ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs**に関連するカスタム コードを代入するか、`ViewController`クラスです。 このファイルで、`ViewController`クラスは、さまざまな iOS にコント ローラーのライフ サイクル メソッドのビューを応答、UI のカスタマイズ、およびユーザーがボタンのタップなどの入力に応答します。

- **ViewController.designer.cs** iOS コードを視覚的に構築されたインターフェイスにマップするデザイナーによって作成された、生成されたファイルです。 このファイルへの変更は上書きされるためには変更できません。 このファイル内のプロパティの宣言を行うでコードを`ViewController`にアクセスしてクラス**名前**、iOS デザイナーでの設定を制御します。 開く**ViewController.designer.cs**次のコードを表示します。

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

`SubmitButton`プロパティの宣言には接続全体`ViewController`クラスでなくだけ**ViewController.designer.cs**ファイル: ストーリー ボードで定義されているボタンにします。 **ViewController.cs**の一部を定義、`ViewController`へのアクセス権を持つクラス、`SubmitButton`です。

次のスクリーン ショットは、IntelliSense が認識されるようになりましたことを示しています、`SubmitButton`内で参照**ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![損害賠償参照を認識する IntelliSense](introduction-images/6-submitbuttonintellisense-vsmac.png "損害賠償参照を認識する IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![損害賠償参照を認識する IntelliSense](introduction-images/6-submitbuttonintellisense-vs.png "損害賠償参照を認識する IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

このセクションの内容が示されているどの iOS デザイナーでボタンを作成し、そのボタンのコードにアクセスします。

このドキュメントの残りの部分では、iOS デザイナーのさらに概要を説明します。

## <a name="ios-designer-basics"></a>iOS デザイナーの基礎

このセクションでは、iOS デザイナーの各部分を紹介し、その機能のツアーを提供します。

### <a name="launching-the-ios-designer"></a>IOS デザイナーを起動します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio で作成した Xamarin.iOS プロジェクトには、ストーリー ボードが含まれます。 ストーリー ボードの内容を表示するで .storyboard ファイルをダブルクリック、**ソリューション パッド**:

[![ストーリー ボードが iOS デザイナーで開いて](introduction-images/7-storyboardopen-vsmac.png "iOS デザイナーで、ストーリー ボードを開く")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2015 または 2017 年 1 で作成されたほとんどの Xamarin.iOS プロジェクトには、ストーリー ボードが含まれます。 ストーリー ボードの内容を表示するで .storyboard ファイルをダブルクリック、**ソリューション エクスプ ローラー**:

[![ストーリー ボードが iOS デザイナーで開いて](introduction-images/7-storyboardopen-vs.png "iOS デザイナーで、ストーリー ボードを開く")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS デザイナーの機能

IOS デザイナーでは、6 つの主要セクションがあります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![セクションでは、iOS デザイナーの](introduction-images/8-sixpartsofiosdesigner-vsmac.png "iOS デザイナーのセクション")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **デザイン画面**– iOS デザイナーのプライマリ ワークスペースです。 ユーザー インターフェイスの視覚的に作成でき、ドキュメントの領域に表示できます。
2. **制約ツールバー** – モードと編集モードでの制約は、2 つの方法をユーザー インターフェイスで要素を配置する編集フレームの切り替えを許可します。
3. **ツールボックス**– コント ローラー、オブジェクト、コントロール、データ ビュー、ジェスチャ レコグナイザー、windows を示し、バーに、デザイン サーフェイスにドラッグ アンド ドロップできるユーザー インターフェイスに追加します。
4. **プロパティのパッド**– id、visual スタイル、アクセシビリティ、レイアウト、および動作を含む、選択したコントロールのプロパティを表示します。
5. **ドキュメント アウトライン**: 編集されているインターフェイスのレイアウトを作成するコントロールのツリーを示しています。 IOS デザイナーで選択およびそのプロパティを示しています、ツリー内の項目をクリックすると、**プロパティ パッド**です。 これは、深く入れ子になったユーザー インターフェイスで特定のコントロールを選択するため便利です。
6. **ツールバーの下**– iOS デザイナーでのデバイス、向き、およびズームを含む、.storyboard または .xib ファイルの表示方法を変更するためのオプションが含まれています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![セクションでは、iOS デザイナーの](introduction-images/8-sixpartsofiosdesigner-vs.png "iOS デザイナーのセクション")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **デザイン画面**– iOS デザイナーのプライマリ ワークスペースです。 ユーザー インターフェイスの視覚的に作成でき、ドキュメントの領域に表示できます。
2. **制約ツールバー** – モードと編集モードでの制約は、2 つの方法をユーザー インターフェイスで要素を配置する編集フレームの切り替えを許可します。
3. **ツールボックス**– コント ローラー、オブジェクト、コントロール、データ ビュー、ジェスチャ レコグナイザー、windows を示し、バーに、デザイン サーフェイスにドラッグ アンド ドロップできるユーザー インターフェイスに追加します。
4. **[プロパティ] ウィンドウ**– id、visual スタイル、アクセシビリティ、レイアウト、および動作を含む、選択したコントロールのプロパティを表示します。
5. **ドキュメント アウトライン**: 編集されているインターフェイスのレイアウトを作成するコントロールのツリーを示しています。 IOS デザイナーで選択およびそのプロパティを示しています、ツリー内の項目をクリックすると、**プロパティ ウィンドウ**します。 これは、深く入れ子になったユーザー インターフェイスで特定のコントロールを選択するため便利です。
6. **ツールバーの下**– iOS デザイナーでのデバイス、向き、およびズームを含む、.storyboard または .xib ファイルの表示方法を変更するためのオプションが含まれています。

-----

### <a name="design-workflow"></a>ワークフローを設計します。

#### <a name="adding-a-control-to-the-interface"></a>インターフェイスへのコントロールの追加

インターフェイスにコントロールを追加するからをドラッグ、**ツールボックス**し、デザイン画面にドロップします。 追加するか、コントロールを配置、垂直および水平方向のガイドラインには、垂直方向の中央、水平方向の中央、および余白などのための一般的なレイアウトの配置が強調表示します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![デザイン サーフェイス上のガイドラインには一般的に使用されるレイアウトの配置が強調表示](introduction-images/9-layoutguides-vsmac.png "デザイン サーフェイス上のガイドラインには一般的に使用されるレイアウトの配置が強調表示")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![デザイン サーフェイス上のガイドラインには一般的に使用されるレイアウトの配置が強調表示](introduction-images/9-layoutguides-vs.png "デザイン サーフェイス上のガイドラインには一般的に使用されるレイアウトの配置が強調表示")

-----

上記の例で青色の点線は、ボタンの配置に役立てるために水平方向の中央 visual 配置ガイドラインを示します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>コンテキスト メニュー コマンド

コンテキスト メニューは、デザイン サーフェイスとの両方に使用可能な**ドキュメント アウトライン**です。 このメニューのコマンドには、選択したコントロールとその親では、入れ子になった階層内のビューを使用する場合に便利です。

[![デザイン サーフェイスのコンテキスト メニュー](introduction-images/10-contextmenudesignsurface-vsmac.png "デザイン サーフェイスのコンテキスト メニュー")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>制約のツールバー

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![制約ツールバー](introduction-images/11-constraintstoolbar-vsmac.png "制約ツールバー")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![制約ツールバー](introduction-images/11-constraintstoolbar-vs.png "制約ツールバー")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

制約ツールバーが更新されており、ここでは 2 つのコントロールで構成されます: 編集モード フレーム/制約が編集モードの切り替えおよび更新制約/フレーム ボタンを更新します。

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>編集モードのフレームの制約を編集モードの切り替え/

以前のバージョンの iOS デザイナー、デザイン画面で、選択したビューをクリックすると、モードと編集モードの制約を編集フレームの間で切り替えます。 ここで、制約のツールバーで切り替えコントロールは、これらの編集モードの間で切り替わります。

- フレームの編集モード:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![編集モード ボタン フレーム](introduction-images/12a-frameeditingmode-vsmac.png "フレームの編集モード ボタン")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![編集モード ボタン フレーム](introduction-images/12a-frameeditingmode-vs.png "フレームの編集モード ボタン")

-----

- 制約の編集モード:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![編集モード ボタン制約](introduction-images/12b-constrainteditingmode-vsmac.png "制約編集モード ボタン")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![編集モード ボタン制約](introduction-images/12b-constrainteditingmode-vs.png "制約編集モード ボタン")

-----

#### <a name="update-constraints--update-frames-button"></a>制約の更新し、更新プログラムのフレーム ボタン

更新制約更新プログラムが編集モードのフレームの右ボタンに配置をフレーム/制約編集モードの切り替え/です。

- 編集モードのフレームでは、このボタンをクリックすると、制約に一致するように選択した要素のフレームを調整します。
- 制約の編集モードでは、このボタンをクリックすると、フレームを一致するように選択した要素の制約を調整します。

### <a name="bottom-toolbar"></a>下部のツールバー

下部のツールバーでは、デバイス、向き、およびズームのストーリー ボードまたは .xib ファイル iOS デザイナーで表示するために使用を選択する方法を提供します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![下部のツールバーで、デバイスと、デザイン画面の向きを選択するために使用](introduction-images/13-bottomtoolbar-vsmac.png "下部のツールバーで、デバイスと、デザイン画面の向きを選択するために使用")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![下部のツールバーで、デバイスと、デザイン画面の向きを選択するために使用](introduction-images/13-bottomtoolbar-vs.png "下部のツールバーで、デバイスと、デザイン画面の向きを選択するために使用")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>デバイスと印刷の向き

展開すると、下部のツールバーでは、すべてのデバイス、向き、および製を採用した現在のドキュメントには適用を表示します。 クリックすると、デザイン サーフェイスに表示されるビューを変更します。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![下部のツールバーで、展開と向きがデバイスを表示する](introduction-images/14-bottomtoolbarexpanded-vsmac.png "下部のツールバーで、展開と向きがデバイスを表示するには")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![下部のツールバーで、展開と向きがデバイスを表示する](introduction-images/14-bottomtoolbarexpanded-vs.png "下部のツールバーで、展開と向きがデバイスを表示するには")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

デバイスを選択するとし、印刷の向きは、iOS デザイナーがデザインをプレビューする方法のみを変更します。 現在の選択項目に関係なく新しく追加された場合を除き、すべてのデバイスとの向きで制約が適用されて、**編集 Traits**  ボタンを指定しない場合に使用されています。

ときに[クラスのサイズを](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes)は[有効になっている](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes)、**特徴 (traits) を編集**拡張下部のツールバーにボタンが表示されます。  クリックすると、**特徴 (traits) 編集**ボタンには、選択したデバイスの向きで表されたサイズ クラスに基づくインターフェイス バリエーションを作成するためのオプションが表示されます。 次に例を示します。

- 場合**iPhone SE** / **縦**を選択すると、重なっては compact の幅、正規の高さのサイズのクラス、インターフェイスのバリエーションを作成するオプションを提供します。 
- 場合**iPad Pro 9.7"** / **ランドス ケープ** / **全画面表示**は選択すると、重なってはインターフェイスのバリエーションを作成するオプションを提供します。正規の幅、正規の高さのサイズのクラスです。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![下部のツールバーで、インターフェイスを変更するサイズ クラスによって使用されている](introduction-images/15-edittraitsbutton-vsmac.png "下部のツールバーで、インターフェイスを変更するサイズ クラスによって使用されています。")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![下部のツールバーで、インターフェイスを変更するサイズ クラスによって使用されている](introduction-images/15-edittraitsbutton-vs.png "下部のツールバーで、インターフェイスを変更するサイズ クラスによって使用されています。")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>ズーム コントロール

デザイン画面では、いくつかのコントロールを使用してズームはサポートします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![下部のツールバーのズーム コントロール](introduction-images/16-zoomcontrols-vsmac.png "下部のツールバーのズーム コントロール")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![下部のツールバーのズーム コントロール](introduction-images/16-zoomcontrols-vs.png "下部のツールバーのズーム コントロール")

-----

コントロールは次のとおりです。

1. ウィンドウに合わせる
2. ズーム アウト
3. 拡大表示
4. 実際のサイズ (1:1 ピクセルのサイズ)

これらのコントロールでは、デザイン サーフェイスのズームを調整します。 実行時にアプリケーションのユーザー インターフェイスには影響しません。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="properties-pad"></a>プロパティの埋め込み

使用して、**プロパティ パッド**id、visual スタイル、アクセシビリティ、およびコントロールの動作を編集します。 次のスクリーン ショットを示しています、**プロパティ パッド**ボタンのオプション。

[![ボタンのプロパティ パッド](introduction-images/17-buttonpropertiespad-vsmac.png "ボタンの プロパティ パッド")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>埋め込みのセクションではプロパティ

**プロパティ パッド**3 つのセクションが含まれています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>[プロパティ] ウィンドウ

使用して、**プロパティ ウィンドウ**id、visual スタイル、アクセシビリティ、およびコントロールの動作を編集します。 次のスクリーン ショットを示しています、**プロパティ ウィンドウ**ボタンのオプション。

[![ボタンのプロパティ ウィンドウ](introduction-images/17-buttonpropertieswindow-vs.png "ボタンの [プロパティ] ウィンドウ")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>ウィンドウのセクションではプロパティ

**プロパティ ウィンドウ**3 つのセクションが含まれています。

-----

1.  **ウィジェット**– 名、クラス、スタイル プロパティなど、コントロールの主要なプロパティです。コントロールのコンテンツを管理するためのプロパティは通常は、ここに配置されます。
2.  **レイアウト**– 制約と、フレームを含む、コントロールのサイズと位置を追跡するプロパティが次に示します。
3.  **イベント**– イベントとイベント ハンドラーがここで指定します。 タッチ、タップ、ドラッグなどのイベントを処理するために便利です。イベントは、コード内で直接処理できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>プロパティのパッドでプロパティを編集

ビジュアル編集、デザイン サーフェイスに加えて、iOS デザイナーがサポートでプロパティを編集、**プロパティ パッド**です。 次のスクリーン ショットに示すように、選択したコントロールに基づく利用可能なプロパティの変更:

[![プロパティをボタン](introduction-images/18a-buttonpropertiespad-vsmac.png "プロパティをボタン")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![コント ローラーのプロパティを表示](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "コント ローラーのプロパティを表示")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>[プロパティ] ウィンドウでプロパティを編集

ビジュアル編集、デザイン サーフェイスに加えて、iOS デザイナーがサポートでプロパティを編集、**プロパティ ウィンドウ**します。 次のスクリーン ショットに示すように、選択したコントロールに基づく利用可能なプロパティの変更:

[![プロパティをボタン](introduction-images/18a-buttonpropertieswindow-vs.png "プロパティをボタン")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![コント ローラーのプロパティを表示](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "コント ローラーのプロパティを表示")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> プロパティ パッド今番組の Identity セクション、**モジュール**フィールドです。 Swift クラスと相互運用する場合にのみ、このセクションで入力する必要があります。 これを使用して、Swift クラス、名前空間内のモジュール名を入力します。

#### <a name="default-values"></a>既定の値

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

多くのプロパティを**プロパティ パッド**値がないか、既定値を表示します。 ただし、アプリケーションのコードでは、これらの値が変更も可能性があります。 **プロパティ パッド**コードで設定された値は表示されません。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

多くのプロパティを**プロパティ ウィンドウ**値がないか、既定値を表示します。 ただし、アプリケーションのコードでは、これらの値が変更も可能性があります。 **プロパティ ウィンドウ**コードで設定された値は表示されません。

-----

#### <a name="event-handlers"></a>イベント ハンドラー

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

さまざまなイベントのカスタム イベント ハンドラーを指定するには、使用、**イベント**のタブ、**プロパティ パッド**です。 たとえば、以下のスクリーン ショットで、`HandleClick`ボタンの処理**タッチを内部**イベント。

[![ボタンの設定、イベント ハンドラーで、プロパティ パッド](introduction-images/19-buttonpropertiespadevents-vsmac.png "ボタンの設定、イベント ハンドラーで、のプロパティ パッド")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

さまざまなイベントのカスタム イベント ハンドラーを指定するには、使用、**イベント**のタブ、**プロパティウィンドウ**します。 たとえば、以下のスクリーン ショットで、`HandleClick`ボタンの処理**タッチを内部**イベント。

[![[プロパティ] ウィンドウのボタンの設定、イベント ハンドラーを持つ](introduction-images/19-buttonpropertieswindowevents-vs.png "[プロパティ] ウィンドウのイベント ハンドラー ボタンの設定")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

イベント ハンドラーを指定すると、同じ名前のメソッドがビュー コント ローラーの対応するクラスに追加する必要があります。 それ以外の場合、 `unrecognized selector`  ボタンをタップすると、例外が発生します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![認識されないセレクター例外](introduction-images/20-unrecognizedselector-vsmac.png "認識されないセレクター例外")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

イベント ハンドラーの後で指定されていることに注意してください、**プロパティ パッド**、iOS デザイナーはすぐに対応するコード ファイルを開き、メソッドの宣言を挿入するを提供します。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![認識されないセレクター例外](introduction-images/20-unrecognizedselector-vs.png "認識されないセレクター例外")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

カスタム イベント ハンドラーを使用する例を参照してください、[こんにちは, iOS Getting Started Guide](~/ios/get-started/hello-ios/index.md)です。

### <a name="outline-view"></a>アウトライン表示

IOS デザイナーは、アウトラインとしてコントロールのインターフェイスの階層を表示することもできます。 選択して、アウトラインが利用可能な**[ドキュメント アウトライン**] タブで次のようにします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ドキュメント アウトライン](introduction-images/21-buttonoutlineview-vsmac.png "ドキュメント アウトライン")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ドキュメント アウトライン](introduction-images/21-buttonoutlineview-vs.png "ドキュメント アウトライン")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

アウトライン ビューで選択したコントロールは、デザイン画面で選択したコントロールと同期して維持されます。  この機能は、深く入れ子になったインターフェイスの階層構造から項目を選択する場合に便利です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Xcode に戻す

IOS デザイナーと Xcode インターフェイスのビルダーを置き換えて使用することができます。 Xcode インターフェイス ビルダーで開く、ストーリー ボードまたは .xib ファイルをクリックし、ファイルを右クリックし**ファイルを開く > Xcode インターフェイス ビルダー**、次のスクリーン ショットに示すようにします。

[![Xcode インターフェイス ビルダーでストーリー ボードを開く](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode インターフェイス ビルダーでストーリー ボードを開く")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode インターフェイス ビルダーでの編集を行った後、ファイルを保存し、for mac を Visual Studio に戻る Xamarin.iOS プロジェクトに変更を同期させます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>.xib サポート

IOS デザイナーは、作成、編集、および .xib ファイルの管理をサポートします。 これらの XML ファイルは、そのを表す単一のカスタム ビュー、アプリケーションの 階層の表示に追加することができます。 ストーリー ボードは、多くの画面とそれらの間の遷移を表します、.xib ファイルは一般的に 1 つのビューまたはアプリケーションでは、画面のインターフェイスを表します。

どの – .xib ファイル、ストーリー ボード、またはコード – ソリューションに最適の作成とユーザー インターフェイスを維持する多くの意見があります。 実際で、完全な解決策はありませんし、ジョブを手元に最適なツールを検討する価値は常にします。 ただし、.xib ファイルは一般的に最も強力なアプリケーションでは、カスタム テーブル ビューのセルなどの複数の場所に必要なカスタム ビューを表すために使用する場合。 

.Xib ファイルの使用についての他のドキュメントは、次のレシピで見つかります。

- [ビュー .xib テンプレートを使用します。](https://developer.xamarin.com/recipes/ios/general/templates/using_the_ios_view_xib_template/)
- [.Xib を使用してカスタム TableViewCell を作成します。](https://developer.xamarin.com/recipes/ios/content_controls/tables/custom-tableviewcell/)
- [.Xib を使用して、起動画面を作成します。](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)

ストーリー ボードの使用に関する詳細についてを参照してください、[ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)です。

これは、およびその他の iOS デザイナー関連ガイドは、Xamarin.iOS 新しいのほとんどのプロジェクト テンプレートが既定では、ストーリー ボードを使用できないために、ユーザー インターフェイスを構築するための標準的手法としてストーリー ボードの使用を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、iOS デザイナー、その機能を記述して、素晴らしいユーザー インターフェイスの設計には、そのツールのアウトライン表示に紹介します。

## <a name="related-links"></a>関連リンク

- [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)
- [iOS 設計可能なコントロールのチュートリアル](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS マルチスクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [部分クラスと部分メソッド](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [IOS - 進化 2014 (ビデオ) 用の Xamarin デザイナーに進む](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [IOS デザイナーを使用して (ビデオ) の起動画面を作成するには](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
