---
title: モノゲーム UWP プロジェクトの作成
description: モノゲームを使用すると、ユニバーサル Windows プラットフォーム用のゲームとアプリを作成し、1つのコードベースと1つのコンテンツセットを持つ複数のデバイスを対象にすることができます。
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 5d970c596d403c7d55ccc23bb5e9ba7e5fbd623a
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431970"
---
# <a name="creating-a-monogame-uwp-project"></a>モノゲーム UWP プロジェクトの作成

_モノゲームを使用すると、ユニバーサル Windows プラットフォーム用のゲームとアプリを作成し、1つのコードベースと1つのコンテンツセットを持つ複数のデバイスを対象にすることができます。_

このチュートリアルでは、モノゲームユニバーサル Windows プラットフォーム (UWP) プロジェクトの作成とコンテンツの読み込みについて説明します。 UWP アプリは、デスクトップ、タブレット、Windows Phone、Xbox One を含むすべての Windows 10 デバイスで実行できます。

このチュートリアルでは、 *コーンフラワーブルー blue* バックグラウンド (XNA アプリの従来の背景色) を表示する空のプロジェクトを作成します。

## <a name="requirements"></a>要件

モノゲーム UWP アプリの開発には、次のものが必要です。

- Windows 10 オペレーティング システム
- 任意のバージョンの Visual Studio 2017
- Windows 10 開発者ツール
- デバイスを開発者モードに設定する
- Visual Studio またはそれ以降[のモノゲーム 3.7.1](http://community.monogame.net/t/monogame-3-7-1-release/11173)

詳細については、 [Windows 10 UWP 開発のセットアップに関するページ](/windows/uwp/get-started/get-set-up)を参照してください。

Xbox One のゲームは、リテール版の Xbox One ハードウェアで開発できます。 開発用の PC と Xbox の両方で、追加のソフトウェアが必要です。 Xbox one をゲーム開発用に構成する方法の詳細については、 [Xbox one](/windows/uwp/xbox-apps/)のセットアップに関するページを参照してください。

## <a name="creating-an-empty-template"></a>空のテンプレートを作成する

必要なすべてのリソースがインストールされ、Windows 10 コンピューターで開発者モードが有効になったら、Visual Studio を使用して新しいモノゲームプロジェクトを作成できます。そのためには、次の手順を実行します。

1. **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト]** の順に選択します。
1. **インストールされている**  >  **テンプレート**  >  **Visual C#**  >  **モノゲーム**カテゴリを選択します。

    ![モノゲームカテゴリ](uwp-images/image1.png)

1. [ **モノゲーム Windows 10 ユニバーサルプロジェクト** ] オプションを選択します。

    ![[モノゲーム Windows 10 ユニバーサルプロジェクト] オプションを選択します。](uwp-images/image2.png)

1. 新しいプロジェクトの名前を入力し、[ **OK]** をクリックします。
[OK] をクリックした後に Visual Studio でエラーが表示された場合は、Windows 10 ツールがインストールされていることと、デバイスが開発者モードであることを確認します。

Visual Studio でテンプレートの作成が完了したら、次のように実行して、空のプロジェクトが実行されていることを確認できます。

![Visual Studio でテンプレートの作成が完了したら、それを実行して、空のプロジェクトが実行されていることを確認します。](uwp-images/image3.png)

角の数値は、診断情報を提供します。 この情報を削除するには、 `App.xaml.cs` メソッドでブロック内のコードを削除し `DEBUG` `OnLaunched` ます。

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

## <a name="running-on-xbox-one"></a>Xbox One での実行

UWP プロジェクトは、同じプロジェクトから任意の Windows 10 デバイスに配置できます。 Windows 10 開発用コンピューターと Xbox One を設定した後、ターゲットをリモートコンピューターに切り替え、Xbox One の IP アドレスを入力することにより、UWP アプリをデプロイできます。

![UWP アプリをデプロイするには、ターゲットをリモートコンピューターに切り替え、Xbox の IP アドレスを入力します。](uwp-images/remote.png)

Xbox One では、白い境界線は Tv の安全でない領域を表します。 詳細については、「 [safe area」セクション](#safe-area-on-xbox-one)を参照してください。

![Xbox One では、白い境界線は Tv の安全でない領域を表します。](uwp-images/safearea.png)

### <a name="safe-area-on-xbox-one"></a>Xbox One の安全領域

コンソール用のゲームを開発するには、安全な領域を考慮する必要があります。これは、すべての重要なビジュアル (UI や HUD など) を含む、画面の中央にある領域です。 安全な領域の外側にある領域は、すべてのテレビに表示されるとは限りません。そのため、この領域に配置されているビジュアルは一部またはすべてのディスプレイで完全に非表示になる場合があります。

Xbox One のモノゲームテンプレートでは、安全な領域が考慮され、白の境界線としてレンダリングされます。 この領域は `Window.ClientBounds` 、Visual Studio の [ウォッチ] ウィンドウの次の図に示すように、実行時にゲームのプロパティにも反映されます。 1920 x 1080 の表示解像度に関係なく、クライアントの境界の高さが1016であることに注意してください。

![1920 x 1080 の表示解像度に関係なく、クライアントの境界の高さが1016であることに注意してください。](uwp-images/clientbounds.png)

## <a name="referencing-content-in-uwp-projects"></a>参照 (UWP プロジェクトのコンテンツを)

モノゲームプロジェクトのコンテンツは、ファイルから直接参照することも、 [モノゲームコンテンツパイプライン](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)を使用して参照することもできます。 小規模なゲームプロジェクトでは、ファイルからの読み込みを簡単に行うことができます。 大規模なプロジェクトでは、コンテンツパイプラインを使用してコンテンツを最適化し、サイズと読み込み時間を短縮できるというメリットがあります。 Xbox 360 の XNA とは異なり、 `System.IO.File` クラスは Xbox ONE UWP アプリで使用できます。

コンテンツパイプラインを使用したコンテンツの読み込みの詳細については、「 [コンテンツパイプラインガイド](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)」を参照してください。

### <a name="loading-content-from-file"></a>ファイルからコンテンツを読み込んでいます

IOS や Android とは異なり、UWP プロジェクトは実行可能ファイルを基準としたファイルを参照できます。 単純なゲームでは、コンテンツパイプラインプロジェクトを変更してビルドしなくても、コンテンツを読み込むことができます。

ファイルからを読み込むには `Texture2D` :

1. UWP プロジェクトのコンテンツフォルダーに .png ファイルを追加します。 コンテンツフォルダーにコンテンツを追加することは、XNA およびモノゲームの慣例です。
1. 新しく追加された PNG を右クリックし、[プロパティ] を選択します。
1. [**新しい場合は**コピーする**出力ディレクトリ**にコピーする。
1. を読み込むには、ゲームの Initialize メソッドに次のコードを追加し `Texture2D` ます。

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

の使用方法の詳細については、 `Texture2D` 「 [入門ゲームガイド](~/graphics-games/monogame/introduction/index.md)」を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、ファイルの読み込み時に新しい UWP プロジェクトを作成する方法と UWP 固有の考慮事項について説明します。 UWP ゲーム全体の作成に関心がある開発者は、「 [モノの概要」ゲームガイド](~/graphics-games/monogame/introduction/index.md)のモノゲームの詳細を参照できます。