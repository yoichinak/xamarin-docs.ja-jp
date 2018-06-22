---
title: MonoGame UWP プロジェクトの作成
description: MonoGame は、ユニバーサル Windows プラットフォーム、コードベースのいずれかで複数のデバイスを対象としてコンテンツのセットを 1 つのゲームやアプリを作成するために使用します。
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: f539ede3bb90f216463a55cca30554a5f50e2386
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922679"
---
# <a name="creating-a-monogame-uwp-project"></a>MonoGame UWP プロジェクトの作成

_MonoGame は、ユニバーサル Windows プラットフォーム、コードベースのいずれかで複数のデバイスを対象としてコンテンツのセットを 1 つのゲームやアプリを作成するために使用します。_

このチュートリアルでは、MonoGame ユニバーサル Windows プラットフォーム (UWP) プロジェクトの作成およびコンテンツの読み込みについて説明します。 UWP アプリは、デスクトップ、タブレット、Windows Phone、および Xbox One を含め、すべての Windows 10 デバイスで実行できます。

このチュートリアルを表示する空のプロジェクトを作成、 *コーンフラワー ブルー*背景 (XNA アプリの従来の背景色)。

## <a name="requirements"></a>要件

MonoGame UWP アプリの開発が必要です。

- Windows 10 オペレーティング システム
- Visual Studio 2015 の任意のバージョン
- Windows 10 開発者ツール
- 開発者モードに設定デバイス
- [Visual Studio の MonoGame 3.5](http://www.monogame.net/2016/03/17/monogame-3-5/)以降

詳細については、これを参照して[の Windows 10 UWP 開発に関して設定に関するページ](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)です。

Xbox One ゲームは、小売 Xbox One ハードウェアで開発できます。 開発 PC と Xbox One の両方では、追加のソフトウェアが必要です。 構成、Xbox 1 つのゲームの開発方法の詳細については、このページを参照して[Xbox One を設定する](https://msdn.microsoft.com/windows/uwp/xbox-apps/index)です。

## <a name="creating-an-empty-template"></a>空のテンプレートを作成します。

すべての必要なリソースがインストールされているし、開発者モードが Windows 10 コンピューターで有効になって、次の手順で Visual Studio を使用して新しい MonoGame プロジェクトを作成できます。

1. 選択**ファイル** > **新しい** > **プロジェクト.**
1. 選択、**インストール** > **テンプレート** > **Visual c#** > **MonoGame**カテゴリ。 

    ![](uwp-images/image1.png "MonoGame カテゴリ")

1. 選択、 **MonoGame Windows 10 のユニバーサル プロジェクト**オプション。 

    ![](uwp-images/image2.png "MonoGame Windows 10 のユニバーサル プロジェクト オプションを選択します。")

1. 新しいプロジェクトの名前を入力し、クリックして**OK**です。
Visual Studio では、[ok] をクリックすると、エラーが表示されている場合は、Windows 10 のツールがインストールされていることと、デバイスが開発者モードであるを確認します。

Visual Studio では、テンプレートの作成が終了したを実行している、空のプロジェクトを表示を実行できます。

![](uwp-images/image3.png "Visual Studio では、テンプレートの作成が終了したを実行して、実行されている空のプロジェクトを参照してください。")

コーナーの番号は、診断情報を提供します。 内のコードを削除することによって、この情報を削除することができます`App.xaml.cs`で、`DEBUG`のブロック、`OnLaunched`メソッド。


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

## <a name="running-on-xbox-one"></a>1 つの Xbox で実行されています。

UWP プロジェクトは、同じプロジェクトから任意の Windows 10 デバイスに展開できます。 Windows 10 の開発用コンピューターおよび Xbox One をセットアップした後は、リモート コンピューターに、ターゲットの切り替え、Xbox One の IP アドレスを入力して UWP アプリを展開できます。

![](uwp-images/remote.png "リモート コンピューターに、ターゲットの切り替えと Xbox の IP アドレスを入力して、UWP アプリを展開すること")

Xbox One では、空白の枠線は、Tv の非セーフ領域を表します。 詳細については、次を参照してください。、[セーフ エリア セクション](#Safe_Area_on_Xbox_One)です。

![](uwp-images/safearea.png "Xbox One に白の罫線が Tv の非セーフ領域を表す")

### <a name="safe-area-on-xbox-one"></a>Xbox 1 セーフ領域

コンソールのゲームの開発には、UI または HUD) などのすべての重要なビジュアルが含まれている画面の中央の領域には、安全な領域を考慮する必要があります。 安全なエリアの外側の領域は、ビジュアルのこの領域に配置されますが部分的または完全に表示されている一部のモニターでない可能性がありますので、すべてのテレビに表示するは保証されません。

Xbox One の MonoGame テンプレートは、安全な領域が考慮され、白い罫線としてレンダリングします。 この領域は、ゲームの実行時にも反映されます。`Window.ClientBounds`プロパティの Visual Studio で [ウォッチ] ウィンドウには、この図のようにします。 クライアントの境界の高さが 1920 x 1080 ディスプレイの解像度に関係なく、1016 であることを確認します。

![](uwp-images/clientbounds.png "クライアントの境界の高さが 1920 x 1080 ディスプレイの解像度に関係なく、1016 ことに注意してください。")

## <a name="referencing-content-in-uwp-projects"></a>UWP プロジェクトでコンテンツを参照します。

ファイルから直接または MonoGame プロジェクト内のコンテンツを参照できる、 [MonoGame コンテンツ パイプライン](~/graphics-games/cocossharp/content-pipeline/index.md)です。 小規模なゲーム プロジェクト ファイルからの読み込みのシンプルさからパフォーマンスが向上します。 大規模なプロジェクトとして、コンテンツ サイズを小さくし、読み込み時間を最適化するために、コンテンツのパイプラインを使用するメリットがあります。 Xbox 360 では、XNA とは異なり、`System.IO.File`クラスは Xbox の 1 つの UWP アプリで使用できます。

コンテンツのパイプラインを使用して、コンテンツの読み込みの詳細については、次を参照してください。、[コンテンツ パイプライン ガイド](~/graphics-games/cocossharp/content-pipeline/index.md)です。 

### <a name="loading-content-from-file"></a>ファイルからコンテンツを読み込む

IOS および Android とは異なり、UWP プロジェクトは実行可能ファイルを基準としたファイルを参照できます。 シンプルなゲームは、変更およびコンテンツのパイプライン プロジェクトをビルドすることがなくこの手法を読み込みコンテンツを使用できます。

読み込み、`Texture2D`ファイルから。

1. UWP プロジェクトでのコンテンツ フォルダーのファイルは .png ファイルを追加します。 コンテンツのフォルダーにコンテンツの追加は、XNA および MonoGame 規則です。
1. 新しく追加した PNG を右クリックし、プロパティを選択します。
1. 変更、**出力ディレクトリにコピー**に**新しい場合はコピー**です。
1. 次のコードを読み込むには、ゲームの初期化メソッドを追加、 `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

使用する方法について、`Texture2D`を参照してください、 [MonoGame guide 入門](~/graphics-games/monogame/introduction/index.md)です。

## <a name="summary"></a>まとめ

このガイドでは、ファイルを読み込むときに、新しい UWP プロジェクトと UWP に固有の考慮事項を作成する方法について説明します。 完全 UWP ゲームを作成する関心のある開発者は、詳細を読み取ることができますに MonoGame に関する、 [MonoGame ガイドの概要](~/graphics-games/monogame/introduction/index.md)です。
