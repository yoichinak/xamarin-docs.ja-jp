---
title: MonoGame UWP プロジェクトを作成します。
description: ユニバーサル Windows プラットフォーム、コードベースのいずれかで複数のデバイスを対象としてコンテンツのセットを 1 つのゲームやアプリを作成する MonoGame を使用できます。
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 9f39580d282defed354f3b9e5cbe4eb1cdec4796
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070931"
---
# <a name="creating-a-monogame-uwp-project"></a>MonoGame UWP プロジェクトを作成します。

_ユニバーサル Windows プラットフォーム、コードベースのいずれかで複数のデバイスを対象としてコンテンツのセットを 1 つのゲームやアプリを作成する MonoGame を使用できます。_

このチュートリアルでは、MonoGame ユニバーサル Windows プラットフォーム (UWP) プロジェクトの作成とコンテンツの読み込みについて説明します。 UWP アプリは、デスクトップ、タブレット、Windows スマート フォン、Xbox One を含め、すべての Windows 10 デバイスで実行できます。

このチュートリアルを表示する空のプロジェクトの作成、 *コーンフラワー ブルー*背景 (XNA アプリの従来の背景色)。

## <a name="requirements"></a>必要条件

MonoGame の UWP アプリの開発が必要です。

- Windows 10 オペレーティング システム
- Visual Studio 2017 の任意のバージョン
- Windows 10 開発者ツール
- 開発者モードにデバイスを設定
- [Visual Studio の MonoGame 3.7.1](http://community.monogame.net/t/monogame-3-7-1-release/11173)以降

詳細については、この参照してください。 [Windows 10 UWP 開発用の設定に関するページ](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)します。

小売 Xbox One のハードウェアでは、Xbox One のゲームを開発できます。 開発の PC と Xbox One の両方で追加のソフトウェアが必要です。 Xbox One のゲーム開発用の構成方法の詳細については、このページを参照して[、Xbox One を設定](https://msdn.microsoft.com/windows/uwp/xbox-apps/index)します。

## <a name="creating-an-empty-template"></a>空のテンプレートを作成します。

すべての必要なリソースがインストールされているし、Windows 10 コンピューターで開発者モードが有効になって、次の手順に従って Visual Studio を使用して、新しい MonoGame プロジェクトを作成できます。

1. 選択**ファイル** > **新しい** > **プロジェクト.**
1. 選択、**インストール** > **テンプレート** > **Visual C#**   >  **MonoGame**カテゴリ:

    ![](uwp-images/image1.png "MonoGame カテゴリ")

1. 選択、 **MonoGame Windows 10 のユニバーサル プロジェクト**オプション。

    ![](uwp-images/image2.png "MonoGame Windows 10 のユニバーサル プロジェクト オプションを選択します。")

1. 新しいプロジェクトの名前を入力し、クリックして**OK**します。
Visual Studio では、[ok] をクリックした後、エラーが表示されている場合は、Windows 10 のツールがインストールされていることと、デバイスが開発者モードであることを確認します。

Visual Studio では、テンプレートの作成が完了すると、実行を実行している空のプロジェクトを確認できます。

![](uwp-images/image3.png "Visual Studio では、テンプレートの作成が完了するを実行して、実行している空のプロジェクトを参照してください。")

コーナーの番号は、診断情報を提供します。 内のコードを削除することによって、この情報を削除できる`App.xaml.cs`で、`DEBUG`ブロック、`OnLaunched`メソッド。


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

## <a name="running-on-xbox-one"></a>Xbox One で実行されています。

UWP プロジェクトは、同じプロジェクトから任意の Windows 10 デバイスに展開できます。 Windows 10 の開発用コンピューターおよび Xbox One を設定したら、リモート コンピューターに、ターゲットの切り替え、Xbox One の IP アドレスを入力して UWP アプリを展開できます。

![](uwp-images/remote.png "リモート コンピューターへのターゲットの切り替えを Xbox の IP アドレスを入力することによって、UWP アプリを展開できます。")

Xbox One では、白い境界線は、Tv の非セーフ領域を表します。 詳細については、次を参照してください。、[安全領域セクション](#safe-area-on-xbox-one)します。

![](uwp-images/safearea.png "Xbox One で白い境界線が Tv の非セーフ領域を表します")

### <a name="safe-area-on-xbox-one"></a>Xbox One での安全な領域

コンソールのゲームの開発には、UI または HUD) などのすべての重要なビジュアルが含まれている画面の中央の領域には、安全な領域を考慮する必要があります。 安全な領域の外側の領域は、この領域に配置するビジュアルが部分的または完全に表示されている一部のディスプレイがない可能性がありますので、すべてのテレビに表示されるは保証されません。

Xbox One の MonoGame テンプレートでは、安全領域が考慮され、白い枠線として表示します。 この領域は、ゲームの実行時にも反映されます。`Window.ClientBounds`プロパティの Visual Studio の [ウォッチ] ウィンドウには、この図のようにします。 クライアントの境界の高さが 1920 x 1080 のディスプレイの解像度に関係なく、1016 であることを確認します。

![](uwp-images/clientbounds.png "クライアントの境界の高さが 1920 x 1080 のディスプレイの解像度に関係なく、1016 ことに注意してください。")

## <a name="referencing-content-in-uwp-projects"></a>UWP プロジェクト内のコンテンツを参照します。

ファイルから直接または MonoGame プロジェクト内のコンテンツを参照することができます、 [MonoGame コンテンツ パイプライン](~/graphics-games/cocossharp/content-pipeline/index.md)します。 小さなゲーム プロジェクト ファイルからの読み込みの簡潔さが挙げられます。 大規模なプロジェクトは、コンテンツのサイズを小さくして読み込み時間を最適化するために、コンテンツ パイプラインを使用して得られます。 Xbox 360 に XNA とは異なり、`System.IO.File`クラスは Xbox の 1 つの UWP アプリで使用できます。

コンテンツ パイプラインを使用してコンテンツを読み込む詳細については、次を参照してください。、[コンテンツ パイプライン ガイド](~/graphics-games/cocossharp/content-pipeline/index.md)します。

### <a name="loading-content-from-file"></a>ファイルからコンテンツを読み込む

IOS と Android とは異なり、UWP プロジェクトは、実行可能ファイルを基準としたファイルを参照できます。 単純なゲームは、しなくてもこの手法の負荷のコンテンツを使用して変更し、コンテンツ パイプライン プロジェクトをビルドします。

読み込み、`Texture2D`ファイルから。

1. UWP プロジェクト内のコンテンツのフォルダーには、.png ファイルを追加します。 コンテンツを追加するコンテンツのフォルダーには、XNA と MonoGame 規則です。
1. 新しく追加した PNG を右クリックし、[プロパティ] を選択します。
1. 変更、**出力ディレクトリにコピー**に**新しい場合はコピー**します。
1. 次のコードを読み込むには、ゲームの Initialize メソッドに追加、 `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

使用しての詳細については、`Texture2D`を参照してください、 [MonoGame ガイド入門](~/graphics-games/monogame/introduction/index.md)します。

## <a name="summary"></a>まとめ

このガイドでは、ファイルの読み込み時に、新しい UWP プロジェクトと UWP 固有の考慮事項を作成する方法について説明します。 完全な UWP ゲームを作成したい開発者は、詳細を読み取ることができますで MonoGame について、 [MonoGame ガイドの概要](~/graphics-games/monogame/introduction/index.md)します。
