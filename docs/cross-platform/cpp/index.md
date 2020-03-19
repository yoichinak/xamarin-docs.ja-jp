---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Xamarin で C/C++ ライブラリを使用する
description: Visual Studio for Mac を Xamarin および C# とともに使用すると、クロスプラットフォームの C/C++ コードをビルドし、Android と iOS 用のモバイル アプリに統合できます。 この記事では、Xamarin アプリで C++ プロジェクトを設定およびデバッグする方法について説明します。
author: mikeparker104
ms.author: miparker
ms.date: 11/07/2019
ms.openlocfilehash: 42a59570d727657b2f3c23bd9d1f37e1205717d0
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73842813"
---
# <a name="use-cc-libraries-with-xamarin"></a>Xamarin で C/C++ ライブラリを使用する

## <a name="overview"></a>概要

Xamarin を使用すると、開発者は Visual Studio でクロスプラットフォームのネイティブ モバイル アプリを作成できます。 一般的に、C# バインドは、既存のプラットフォーム コンポーネントを開発者に公開するために使用されます。 しかし、Xamarin アプリで既存のコードベースを使用することが必要な場合もあります。 チームには、十分にテストされて高度に最適化された大規模なコードベースを C# に移植するための時間、予算、リソースがない場合があります。

[クロスプラットフォームのモバイル開発用の Visual C++](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) を使用すると、C/C++ と C# コードを同じソリューションの一部としてビルドできるため、統合されたデバッグ エクスペリエンスなどの多くの利点が得られます。 Microsoft は、[Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) や [Pix カメラ](https://www.microsoft.com/microsoftpix)などのアプリを配信するために、このように C/C++ と Xamarin を使用してきました。

ただし、場合によっては、既存の C/C++ のツールとプロセスを保持し、ライブラリ コードをアプリケーションから切り離して、ライブラリをサードパーティ製のコンポーネントと同様に扱うことが望ましい (あるいは必要になる) ことがあります。 このような状況では、関連するメンバーを C# に公開するだけでなく、ライブラリを依存関係として管理することも困難です。 そしてもちろん、このプロセスを可能な限り自動化することも該当します。  

この記事では、このシナリオの大まかなアプローチについて概説し、簡単な例を紹介します。

## <a name="background"></a>背景

C/C++ はクロスプラットフォーム言語と見なされていますが、ソース コードが実際にクロスプラットフォームになっているようにするために、十分な注意が必要です。つまり、すべてのターゲット コンパイラでサポートされている C/C++ のみを使用し、条件付きで含まれるプラットフォームまたはコンパイラに固有のコードがほとんどないか、あるいはまったくないようにします。

最終的には、コードはすべてのターゲット プラットフォームでコンパイルして正常に実行する必要があります。したがってこれは、ターゲット プラットフォーム (およびコンパイラ) 全体での共通事項に帰結します。 コンパイラ間のわずかな違いから問題が発生する可能性があるため、各ターゲット プラットフォームでの (可能であれば自動化による) 徹底的なテストがますます重要になります。  

## <a name="high-level-approach"></a>上位レベルのアプローチ

次の図は、C/C++ ソース コードをクロスプラットフォームの Xamarin ライブラリに変換し、それを NuGet 経由で共有して Xamarin.Forms アプリで使用するための、4 つのステージからなるアプローチを示しています。

![Xamarin で C/C++ を使用するための上位レベルのアプローチ](images/cpp-steps.jpg)

4 つのステージは以下のとおりです。

1. C/C++ ソース コードをプラットフォーム固有のネイティブ ライブラリにコンパイルする。
2. Visual Studio ソリューションでネイティブ ライブラリをラップする。
3. .NET ラッパー用の NuGet パッケージをパッキングおよびプッシュする。
4. NuGet パッケージを Xamarin アプリから使用する。

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>ステージ 1:C/C++ ソース コードをプラットフォーム固有のネイティブ ライブラリにコンパイルする

このステージの目標は、 C# ラッパーによって呼び出すことができるネイティブ ライブラリを作成することです。 これは、状況によっては関連性がない場合もあります。 この一般的なシナリオで使用できる多くのツールとプロセスについては、この記事では取り上げません。 重要な考慮事項は、C/C++ コードベースをあらゆるネイティブ ラッパー コードと同期させること、十分な単体テスト、およびビルドの自動化です。 

このチュートリアルのライブラリは、Visual Studio Code および付属のシェル スクリプトを使用して作成されました。 このチュートリアルの拡張バージョンについては、サンプルのこの部分について詳しく説明している [Mobile CAT GitHub リポジトリ](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin)に関する記事を参照してください。 この場合、ネイティブ ライブラリはサードパーティの依存関係として扱われますが、このステージはコンテキストのために示しています。

わかりやすくするために、このチュートリアルではアーキテクチャのサブセットのみを対象としています。 iOS では lipo ユーティリティを使用して、アーキテクチャ固有の個々のバイナリから 1 つの FAT バイナリを作成します。 Android では .so 拡張子の動的バイナリが使用され、iOS では .a 拡張子の静的 FAT バイナリが使用されます。 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>ステージ 2:Visual Studio ソリューションでネイティブ ライブラリをラップする

次のステージでは、ネイティブ ライブラリをラップして、.NET から簡単に使用できるようにします。 これは、4 つのプロジェクトを含む Visual Studio ソリューションで行います。 共有プロジェクトには、共通のコードが含まれています。 Xamarin.Android、Xamarin.iOS、および .NET Standard のそれぞれを対象とするプロジェクトでは、プラットフォームに依存しない方法でライブラリを参照できます。

このラッパーは、Paul Betts 氏が説明している "[bait and switch trick](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)" を使用します。 これは唯一の方法ではありませんが、ライブラリを簡単に参照できるようになり、使用する側のアプリケーション自体でプラットフォーム固有の実装を明示的に管理する必要がなくなります。 このテクニックでは基本的に、複数のターゲット (.NET Standard、Android、iOS) が同じ名前空間、アセンブリ名、およびクラス構造を共有するようにします。 NuGet は常にプラットフォーム固有のライブラリを優先して使用するため、実行時に .NET Standard バージョンが使用されることはありません。

この手順での作業の大部分は、P/Invoke を使用してネイティブ ライブラリ メソッドを呼び出し、基になるオブジェクトへの参照を管理することに焦点を当てます。 目標は、ライブラリの機能をコンシューマーに公開しながら、複雑なものを抽象化することです。 Xamarin.Forms の開発者は、アンマネージ ライブラリの内部動作に関する実用的な知識を持っている必要はありません。 一般的なマネージド C# ライブラリを使用しているかのように感じられるはずです。

最終的に、このステージの成果物は、ターゲットごとに 1 つの .NET ライブラリのセットと、次の手順でパッケージをビルドするために必要な情報を含む nuspec ドキュメントです。

**ステージ 3:.NET ラッパー用の NuGet パッケージをパッキングおよびプッシュする**

3 番目のステージでは、前の手順で作成したビルド成果物を使用して NuGet パッケージを作成します。 この手順の成果物は、Xamarin アプリから使用できる NuGet パッケージです。 このチュートリアルでは、NuGet フィードとして機能するローカル ディレクトリを使用します。 運用環境では、この手順でパブリックまたはプライベートの NuGet フィードにパッケージを公開する必要があり、この手順は完全に自動化される必要があります。

**ステージ 4:NuGet パッケージを Xamarin.Forms アプリから使用する**

最後の手順は、Xamarin.Forms アプリから NuGet パッケージを参照して使用することです。 これを行うには、前の手順で定義したフィードを使用するように Visual Studio で NuGet フィードを構成する必要があります。

フィードが構成されたら、クロスプラットフォームの Xamarin.Forms アプリの各プロジェクトからパッケージが参照される必要があります。 "bait-and-switch trick" は同一のインターフェイスを提供するため、1 つの場所で定義されたコードを使用してネイティブ ライブラリ機能を呼び出すことができます。

ソース コード リポジトリには、Azure DevOps にプライベート NuGet フィードを設定する方法と、そのフィードにパッケージをプッシュする方法に関する記事を含む、[参考資料の一覧](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)が含まれています。 この種類のフィードは、ローカル ディレクトリよりも多少設定に時間がかかりますが、チーム開発環境に適しています。

## <a name="walk-through"></a>チュートリアル

ここで説明する手順は **Visual Studio for Mac** に固有のものですが、その構造は **Visual Studio 2017** でも機能します。

### <a name="prerequisites"></a>必須コンポーネント

これに従うには、開発者には次のものが必要です。

- [NuGet コマンド ライン (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Visual Studio* *for Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> アプリを iPhone にデプロイするには、アクティブな [**Apple Developer アカウント**](https://developer.apple.com/)が必要です。

## <a name="creating-the-native-libraries-stage-1"></a>ネイティブ ライブラリの作成 (ステージ 1)

ネイティブ ライブラリの機能は、「[チュートリアル:スタティック ライブラリの作成と使用 (C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017)」の例に基づいています。

このチュートリアルでは、最初のステージであるネイティブ ライブラリのビルドをスキップします。これは、このシナリオではライブラリがサードパーティの依存関係として提供されているためです。 プリコンパイル済みネイティブ ライブラリは[サンプル コード](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin)に付属していますが、直接[ダウンロードする](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts)こともできます。

### <a name="working-with-the-native-library"></a>ネイティブ ライブラリの操作

元の *MathFuncsLib* のサンプルには、次の定義を持つ `MyMathFuncs` という 1 つのクラスが含まれています。

```cpp
namespace MathFuncs
{
    class MyMathFuncs
    {
    public:
        double Add(double a, double b);
        double Subtract(double a, double b);
        double Multiply(double a, double b);
        double Divide(double a, double b);
    };
}
```

追加のクラスでは、基礎となるネイティブ `MyMathFuncs` クラスを .NET コンシューマーが作成、破棄、および操作できるようにするラッパー関数を定義します。

```cpp
#include "MyMathFuncs.h"
using namespace MathFuncs;

extern "C" {
    MyMathFuncs* CreateMyMathFuncsClass();
    void DisposeMyMathFuncsClass(MyMathFuncs* ptr);
    double MyMathFuncsAdd(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsSubtract(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsMultiply(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsDivide(MyMathFuncs *ptr, double a, double b);
}
```

[Xamarin](https://visualstudio.microsoft.com/xamarin/) 側では、これらのラッパー関数が使用されます。

## <a name="wrapping-the-native-library-stage-2"></a>ネイティブ ライブラリのラップ (ステージ 2)

このステージでは、[前のセクション](#creating-the-native-libraries-stage-1)で説明した[プリコンパイル済みライブラリ](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts)が必要です。

### <a name="creating-the-visual-studio-solution"></a>Visual Studio ソリューションの作成

1. **Visual Studio for Mac** で、 **[新しいプロジェクト]** (*ウェルカム ページ*から) または **[新しいソリューション]** ( *[ファイル]* メニューから) をクリックします。
2. **[新しいプロジェクト]** ウィンドウで、 **[共有プロジェクト]** ( *[マルチプラットフォーム] > [ライブラリ]* から) を選択し、 **[次へ]** をクリックします。
3. 次のフィールドを更新し、 **[作成]** をクリックします。

    - **[プロジェクト名]:** MathFuncs.Shared  
    - **[ソリューション名]:** MathFuncs  
    - **[場所]:** 既定の保存場所を使用します (または別のものを選択します)   
    - **[ソリューション ディレクトリ内にプロジェクト ディレクトリを作成する]:** これをオンに設定します
4. **[ソリューション エクスプローラー]** で、**MathFuncs.Shared** プロジェクトをダブルクリックし、 **[メイン設定]** に移動します。
5. **.Shared** を **[既定の名前空間]** から削除して、**MathFuncs** のみが設定されるようにし、 **[OK]** をクリックします。
6. テンプレートによって作成された **MyClass.cs** を開き、クラスとファイル名の両方を **MyMathFuncsWrapper** に変更し、名前空間を **MathFuncs** に変更します。
7. ソリューション **MathFuncs** を **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[新しいプロジェクトの追加...]** を選択します。
8. **[新しいプロジェクト]** ウィンドウで、 **[.NET Standard ライブラリ]** ( *[マルチプラットフォーム] > [ライブラリ]* から) を選択し、 **[次へ]** をクリックします。
9. **[.NET Standard 2.0]** を選択し、 **[次へ]** を選択します。
10. 次のフィールドを更新し、 **[作成]** をクリックします。

    - **[プロジェクト名]:** MathFuncs.Standard  
    - **[場所]:** 共有プロジェクトと同じ保存場所を使用します   

11. **[ソリューション エクスプローラー]** で、**MathFuncs.Standard** プロジェクトをダブルクリックします。
12. **[メイン設定]** に移動し、 **[既定の名前空間]** を **[MathFuncs]** に更新します。
13. **[出力]** 設定に移動し、 **[アセンブリ名]** を **[MathFuncs]** に更新します。
14. **[コンパイラ]** 設定に移動し、 **[構成]** を **[リリース]** に変更し、 **[デバッグ情報]** を **[シンボルのみ]** に設定し、 **[OK]** をクリックします。
15. プロジェクトから **[Class1.cs]/[Getting Started]\(はじめに\)** を削除します (これらのいずれかがテンプレートの一部として含まれている場合)。
16. プロジェクトの **[依存関係]/[参照設定]** フォルダーを **Ctrl キーを押したままクリック**し、 **[参照の編集]** を選択します。
17. **[プロジェクト]** タブの **[MathFuncs.Shared]** を選択し、 **[OK]** をクリックします。
18. 次の構成を使用して、手順 7 から 17 を繰り返します (手順 9 は無視します) 。

    | **プロジェクト名**  | **テンプレート名**   | **新しいプロジェクト メニュー**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | クラス ライブラリ       | Android > ライブラリ      |
    | MathFuncs.iOS     | バインド ライブラリ     | iOS > ライブラリ          |

19. **[ソリューション エクスプローラー]** で、**MathFuncs.Android** プロジェクトをダブルクリックし、 **[コンパイラ]** 設定に移動します。

20. **[構成]** を **[デバッグ]** に設定して、 **[Define Symbols]\(シンボルを定義\)** を編集して **[Android;]** を含めます。

21. 同様に、 **[構成]** を **[リリース]** に変更して、 **[Define Symbols]\(シンボルを定義\)** を編集して **[Android;]** を含めます。

22. **MathFuncs.iOS** に対して手順 19 と 20 を繰り返します。どちらの場合も、 **[Define Symbols]\(シンボルを定義\)** を編集して **[Android;]** ではなく **[iOS;]** を含めます。

23. **[Release]** 構成でソリューションをビルドし (**Ctrl キー + COMMAND キー + B キー**) 、(それぞれのプロジェクトの bin フォルダーにある) 3 つのすべての出力アセンブリ (Android、iOS、.NET Standard) で同じ名前 **MathFuncs.dll** が共有されていることを確認します。

このステージでは、このソリューションには 3 つのターゲット (Android、iOS、.NET Standard 用に 1 つずつ) と、その 3 つのターゲットそれぞれが参照する共有プロジェクトがあります。 これらは、同じ既定の名前空間と出力アセンブリを同じ名前で使用するように構成する必要があります。 これは、前述の "bait and switch" アプローチに必要です。

### <a name="adding-the-native-libraries"></a>ネイティブ ライブラリの追加

ラッパー ソリューションにネイティブ ライブラリを追加するプロセスは、Android と iOS では多少異なります。

#### <a name="native-references-for-mathfuncsandroid"></a>MathFuncs.Android に対するネイティブ参照

1. **MathFuncs.Android** プロジェクトを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[新しいフォルダー]** を選択して **libs** という名前を付けます。

2. それぞれの **ABI** (アプリケーション バイナリ インターフェイス) について、**libs** フォルダーを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[新しいフォルダー]** を選択して、それぞれの **ABI** に関連した名前を付けます。 この場合、次のようになります。

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > より詳細な概要については、[NDK 開発者ガイド](https://developer.android.com/ndk/guides/)の「[アーキテクチャと CPU](https://developer.android.com/ndk/guides/arch)」のトピック、特に[アプリ パッケージでのネイティブ コード](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages)について説明されているセクションを参照してください。

3. フォルダー構造を確認します。  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. 次のマッピングに基づいて、対応する **.so** ライブラリをそれぞれの **ABI** フォルダーに追加します。

    **arm64-v8a:** libs/Android/arm64

    **armeabi-v7a:** libs/Android/arm  

    **x86:** libs/Android/x86

    **x86_64:** libs/Android/x86_64

    > [!NOTE]
    > ファイルを追加するには、それぞれの **ABI** を表すフォルダーを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[ファイルの追加...]** を選択します。 適切なライブラリを (**PrecompiledLibs** ディレクトリから) 選択し、 **[Open]\(開く\)** 、 **[OK]** の順にクリックし、既定のオプションの *[ファイルをディレクトリにコピーする]* のままにします。

5. 各 **.so** ファイルに対して **Ctrl キーを押したままクリック**し、 **[ビルド アクション]** メニューから **[EmbeddedNativeLibrary]** オプションを選択します。

これで、**libs** フォルダーは次のようになります。

```folders
- lib
    - arm64-v8a
        - libMathFuncs.so
    - armeabi-v7a
        - libMathFuncs.so
    - x86
        - libMathFuncs.so
    - x86_64
        - libMathFuncs.so
```

#### <a name="native-references-for-mathfuncsios"></a>MathFuncs.iOS に対するネイティブ参照

1. **MathFuncs.iOS** プロジェクトを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[ネイティブ参照の追加]** を選択します。 
2. **libMathFuncs.a** ライブラリを (**PrecompiledLibs** ディレクトリの libs/ios から) 選択し、 **[Open]\(開く\)** をクリックします 
3. **libMathFuncs** ファイル ( **[ネイティブ参照]** フォルダー内) を **Ctrl キーを押したままクリック**し、メニューから **[プロパティ]** オプションを選択します  
4. **[ネイティブ参照]** プロパティを構成して、それらが **[Properties]** Pad でチェックされる (チェックマーク アイコンが表示される) ようにします。

    - 強制読み込み
    - C++
    - スマート リンク

    > [!NOTE]
    > バインド ライブラリ プロジェクトの種類と[ネイティブ参照](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references)を一緒に使用すると、スタティック ライブラリが埋め込まれ、(NuGet パッケージでこれが含まれている場合であっても) これを参照する Xamarin.iOS アプリと自動的にリンクできるようになります。

5. **ApiDefinition.cs** を開き、テンプレート化されたコメント コードを削除 (`MathFuncs` 名前空間のみを残す) してから、**Structs.cs** に対して同じ手順を実行します 

    > [!NOTE]
    > バインド ライブラリ プロジェクトでは、ビルドするためにこれらのファイル (*ObjCBindingApiDefinition* と *ObjCBindingCoreSource* ビルド アクションを使用) が必要です。 しかしここでは、ネイティブ ライブラリを呼び出すコードを、これらのファイルの外部で、標準の P/Invoke を使用して Android と iOS の両方のライブラリ ターゲットで共有できるように記述します。

### <a name="writing-the-managed-library-code"></a>マネージド ライブラリ コードの記述

次に、ネイティブ ライブラリを呼び出す C# コードを記述します。 目標は、根底にある複雑さを隠すことです。 コンシューマーがネイティブ ライブラリの内部構造や P/Invoke の概念に関する実用的な知識を持たなくても済むようにします。  

#### <a name="creating-a-safehandle"></a>SafeHandle の作成

1. **MathFuncs.Shared** プロジェクトを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[ファイルの追加...]** を選択します。 
2. **[新しいファイル]** ウィンドウで **[空のクラス]** を選択し、**MyMathFuncsSafeHandle** という名前を指定して、 **[New]\(新規作成\)** をクリックします
3. **MyMathFuncsSafeHandle** クラスを実装します。

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > マネージド コードでアンマネージ リソースを操作するには、[SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) を使用することをお勧めします。 これにより、クリティカル ファイナライズとオブジェクトのライフサイクルに関連する多数の定型コードが抽象化されます。 このハンドルの所有者は、これを後で他の管理対象リソースと同様に扱うことができ、完全な [Dispose パターン](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)を実装する必要はありません。 

#### <a name="creating-the-internal-wrapper-class"></a>内部ラッパー クラスの作成

1. **MyMathFuncsWrapper.cs** を開き、それを内部静的クラスに変更します

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. 同じファイル内で、次の条件付きステートメントをクラスに追加します。

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > これにより、ライブラリのビルド対象が **Android** か **iOS** かに基づいて、**DllName** 定数値が設定されます。 これは、それぞれのプラットフォームで使用される異なる名前付け規則に対処するためだけでなく、この場合に使用されるライブラリの種類にも対処するためです。 Android はダイナミック ライブラリを使用するため、拡張子を含むファイル名が必要です。 iOS の場合はスタティック ライブラリを使用するため、' *__Internal*' が必要です。

3. **MyMathFuncsWrapper.cs** ファイルの先頭に **System.Runtime.InteropServices** への参照を追加します

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. **MyMathFuncs** クラスの作成と破棄を処理するラッパー メソッドを追加します。

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > .NET ランタイムにそのライブラリ内で呼び出す関数の名前を明示的に指示する **EntryPoint** とともに、定数 **DllName** を **DllImport** 属性に渡します。 技術的には、マネージド メソッド名がアンマネージ メソッド名と同一である場合、**EntryPoint** 値を指定する必要はありません。 指定されていない場合は、マネージド メソッド名が **EntryPoint** として代わりに使用されます。 ただし、明示的なものにすることをお勧めします。  

5. 次のコードを使用して、**MyMathFuncs** クラスを操作できるようにするラッパー メソッドを追加します。

    ```csharp
    [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
    internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
    internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
    internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
    internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
    ```

    > [!NOTE]
    > この例では、パラメーターに単純型を使用しています。 この場合、マーシャリングはビットごとのコピーであるため、追加の作業は必要ありません。 また、標準の **IntPtr**ではなく、**MyMathFuncsSafeHandle** クラスを使用することにも注意してください。 **IntPtr** は、マーシャリング プロセスの一部として、**SafeHandle** に自動的にマップされます。

6. 完成した **MyMathFuncsWrapper** クラスが次のように表示されることを確認します。

    ```csharp
    using System.Runtime.InteropServices;

    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
            #if Android
                const string DllName = "libMathFuncs.so";
            #else
                const string DllName = "__Internal";
            #endif

            [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
            internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

            [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
            internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);

            [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
            internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
            internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
            internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
            internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
        }
    }
    ```

#### <a name="completing-the-mymathfuncssafehandle-class"></a>MyMathFuncsSafeHandle クラスの完成

1. **MyMathFuncsSafeHandle** クラスを開き、**ReleaseHandle** メソッド内のプレースホルダー **TODO** コメントに移動します。

    ```csharp
    // TODO: Release the handle here
    ```

1. **TODO** 行を次のように置き換えます。

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(handle);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>MyMathFuncs クラスの記述

ラッパーが完成したので、アンマネージ C++ の MyMathFuncs オブジェクトへの参照を管理する MyMathFuncs クラスを作成します。  

1. **MathFuncs.Shared** プロジェクトを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[ファイルの追加...]** を選択します。 
2. **[新しいファイル]** ウィンドウで **[空のクラス]** を選択し、**MyMathFuncs** という名前を指定して、 **[New]\(新規作成\)** をクリックします
3. 次のメンバーを **MyMathFuncs** クラスに追加します。

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. クラスのコンストラクターを実装して、クラスがインスタンス化されるときにネイティブの **MyMathFuncs** オブジェクトへのハンドルを作成して格納するようにします。

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. 次のコードを使用して、**IDisposable** インターフェイスを実装します。

    ```csharp
    public class MyMathFuncs : IDisposable
    {
        ...

        protected virtual void Dispose(bool disposing)
        {
            if (handle != null && !handle.IsInvalid)
                handle.Dispose();
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        // ...
    }
    ```

6. **MyMathFuncsWrapper** クラスを使用して **MyMathFuncs** メソッドを実装し、格納したポインターを基底のアンマネージ オブジェクトに渡すことによって、内部で実際の作業を実行します。 コードは次のようになります。

    ```csharp
    public double Add(double a, double b)
    {
        return MyMathFuncsWrapper.Add(handle, a, b);
    }

    public double Subtract(double a, double b)
    {
        return MyMathFuncsWrapper.Subtract(handle, a, b);
    }

    public double Multiply(double a, double b)
    {
        return MyMathFuncsWrapper.Multiply(handle, a, b);
    }

    public double Divide(double a, double b)
    {
        return MyMathFuncsWrapper.Divide(handle, a, b);
    }
    ```

#### <a name="creating-the-nuspec"></a>nuspec の作成

NuGet を使用してライブラリをパッケージ化して配布するには、ソリューションに **nuspec** ファイルが必要です。 これにより、サポートされている各プラットフォームに対して組み込まれるアセンブリが特定されます。

1. **MathFuncs** ソリューションを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[Add Solution Folder]\(ソリューション フォルダーの追加\)** を選択して、**SolutionItems** という名前を付けます。
2. **SolutionItems** フォルダーを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[新しいファイル...]** を選択します。
3. **[新しいファイル]** ウィンドウで **[空の XML ファイル]** を選択し、**MathFuncs.nuspec** という名前を指定して、 **[New]\(新規作成\)** をクリックします。
4. **NuGet** コンシューマーに表示される基本的なパッケージ メタデータを使用して、**MathFuncs.nuspec** を更新します。 次に例を示します。

    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>
            <id>MathFuncs</id>
            <version>$version$</version>
            <authors>Microsoft Mobile Customer Advisory Team</authors>
            <description>Sample C++ Wrapper Library</description>
            <requireLicenseAcceptance>false</requireLicenseAcceptance>
            <copyright>Copyright 2018</copyright>
        </metadata>
    </package>
    ```

    > [!NOTE]
    > このマニフェストに使用されるスキーマの詳細については、[nuspec リファレンス](https://docs.microsoft.com/nuget/reference/nuspec) のドキュメントを参照してください。

5. `<package>` 要素の子として `<files>` 要素を追加し (`<metadata>` のすぐ下)、各ファイルを個別の `<file>` 要素で識別します。

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > パッケージがプロジェクトにインストールされ、同じ名前で複数のアセンブリが指定されている場合、NuGet は、特定のプラットフォームに最も特化したアセンブリを効率的に選択します。

6. **Android** アセンブリ用の `<file>` 要素を追加します。

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. **iOS** アセンブリ用の `<file>` 要素を追加します。

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. **netstandard2.0** アセンブリ用の `<file>` 要素を追加します。

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. **nuspec** マニフェストを確認します。

    ```xml
    <?xml version="1.0"?>
    <package>
    <metadata>
        <id>MathFuncs</id>
        <version>$version$</version>
        <authors>Microsoft Mobile Customer Advisory Team</authors>
        <description>Sample C++ Wrapper Library</description>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <copyright>Copyright 2018</copyright>
    </metadata>
    <files>

        <!-- Android -->
        <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
        <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
        
        <!-- iOS -->
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
        
        <!-- netstandard2.0 -->
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />

    </files>
    </package>
    ```

    > [!NOTE]
    > このファイルは**リリース** ビルドからのアセンブリ出力パスを指定します。そのため、その構成を使用してソリューションをビルドしてください。

この時点で、ソリューションには 3 つの .NET アセンブリと、サポートする **nuspec** マニフェストが含まれています。

## <a name="distributing-the-net-wrapper-with-nuget"></a>NuGet を使用した .NET ラッパーの配布

次の手順では、NuGet パッケージをパッケージ化して配布することで、それをアプリで簡単に使用でき、依存関係として管理できるようにします。 ラッピングと使用はすべて 1 つのソリューション内で行うことができますが、NuGet を使用してライブラリを配布することで分離が可能になり、これらのコードベースを個別に管理することができます。

### <a name="preparing-a-local-packages-directory"></a>ローカル パッケージ ディレクトリの準備

NuGet フィードの最も単純な形式は、ローカル ディレクトリです。

1. **Finder** で、使いやすいディレクトリに移動します。 たとえば、 **/Users** などです。
2. **[ファイル]** メニューから **[新規フォルダー]** を選択し、**local-nuget-feed** などのわかりやすい名前を指定します。

### <a name="creating-the-package"></a>パッケージの作成

1. **[ビルド構成]** を **[リリース]** に設定し、**COMMAND キー と B キー**を使用してビルドを実行します。
2. **[ターミナル]** を開き、**nuspec** ファイルが格納されているフォルダーにディレクトリを変更します。
3. **[ターミナル]** で、**nuspec** ファイル、**バージョン** (1.0.0 など)、および **OutputDirectory** ([前の手順](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed)で作成したフォルダー (つまり **local-nuget-feed**) を使用) を指定して **nuget pack** コマンドを実行します。 次に例を示します。

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **local-nuget-feed** ディレクトリに **MathFuncs.1.0.0.nupkg** が作成されていることを**確認します**。

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[省略可能] Azure DevOps でのプライベート NuGet フィードの使用

より堅牢な手法については [Azure DevOps での NuGet パッケージの使用](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package)に関する記事を参照してください。そこでは、プライベート フィードを作成し、(前の手順で生成した) パッケージをそのフィードにプッシュする方法が説明されています。

このワークフローは、たとえば [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) などを使用して、完全に自動化することをお勧めします。 詳細については、「[Azure Pipelines の使用を開始する](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts)」を参照してください。

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>.NET ラッパーを Xamarin.Forms アプリから使用する

このチュートリアルを完了するには、ローカル **NuGet** フィードに公開されたばかりのパッケージを使用する **Xamarin. Forms** アプリを作成します。

### <a name="creating-the-xamarinforms-project"></a>**Xamarin.Forms** プロジェクトの作成

1. **Visual Studio for Mac** の新しいインスタンスを開きます。 これは **[ターミナル]** から行うことができます。

    ```bash
    open -n -a "Visual Studio"
    ```

2. **Visual Studio for Mac** で、 **[新しいプロジェクト]** (*ウェルカム ページ*から) または **[新しいソリューション]** ( *[ファイル]* メニューから) をクリックします。
3. **[新しいプロジェクト]** ウィンドウで、 **[空白フォームのアプリ]** ( *[マルチプラットフォーム] > [アプリ]* から) を選択し、 **[次へ]** をクリックします。
4. 次のフィールドを更新し、 **[次へ]** をクリックします。

    - **[アプリ名]:** MathFuncsApp。
    - **[組織の識別子]:** たとえば、_com.{your_org}_ のように、逆引き名前空間を使用します。
    - **[ターゲット プラットフォーム]:** 既定値 (Android と iOS の両方のターゲット) を使用します。
    - **[共有コード]:** これを .NET Standard に設定します ("共有ライブラリ" ソリューションも可能ですが、このチュートリアルでは扱いません)。

5. 次のフィールドを更新し、 **[作成]** をクリックします。

    - **[プロジェクト名]:** MathFuncsApp。
    - **[ソリューション名]:** MathFuncsApp。  
    - **[場所]:** 既定の保存場所を使用します (または別のものを選択します)。

6. **ソリューション エクスプローラー**で、最初のテストのターゲット (**MathFuncsApp.Android** または **MathFuncs.iOS**) を**Ctrl キーを押したままクリック**し、 **[スタートアップ プロジェクトに設定]** を選択します。
7. 優先する **デバイス**または **シミュレーター**/**エミュレーター**を選択します。 
8. ソリューションを実行し (**COMMAND キー + RETURN キー**)、テンプレート化された **Xamarin.Forms** プロジェクトがビルドされ、正常に実行されることを検証します。 

    > [!NOTE]
    > **iOS** (具体的にはシミュレーター) では、ビルド/デプロイ時間が最も速くなる傾向があります。

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>NuGet 構成へのローカル NuGet フィードの追加

1. **Visual Studio** で、(**Visual Studio** メニューから) **[ユーザー設定]** を選択します。
2. **[NuGet]** セクションで **[ソース]** を選択し、 **[追加]** をクリックします。
3. 次のフィールドを更新し、 **[変換元の追加]** をクリックします。

    - **Name:** わかりやすい名前 (たとえば Local-Packages) を指定します。  
    - **[場所]:** [前の手順](#preparing-a-local-packages-directory)で作成した **local-nuget-feed** フォルダーを指定します。

    > [!NOTE]
    > この場合、 **[ユーザー名]** と **[パスワード]** を指定する必要はありません。 

4. **[OK]** をクリックします。

### <a name="referencing-the-package"></a>パッケージの参照

各プロジェクト (**MathFuncsApp**、**MathFuncsApp.Android**、**MathFuncsApp.iOS**) に対して、次の手順を繰り返します。

1. プロジェクトを **Ctrl キーを押したままクリック**し、 **[追加]** メニューから **[NuGet パッケージの追加...]** を選択します。
2. **MathFuncs** を検索します。 
3. パッケージの**バージョン**が **1.0.0** であること、およびその他の詳細が期待どおりに表示されること (たとえば、 **[タイトル]** と **[説明]** が *MathFuncs* および *Sample C++ Wrapper Librar* であること) を確認します。 
4. **MathFuncs** パッケージを選択し、 **[パッケージの追加]** をクリックします。

### <a name="using-the-library-functions"></a>ライブラリ関数の使用

各プロジェクトでの **MathFuncs** パッケージへの参照により、関数を C# コードで使用できるようになりました。

1. **MathFuncsApp** の共通の **Xamarin.Forms** プロジェクト (**MathFuncsApp.Android** と **MathFuncsApp.iOS** の両方によって参照されるもの) から **MainPage.xaml.cs** を開きます。
2. そのファイルの先頭に、**System.Diagnostics** および **MathFuncs** の **using** ステートメントを追加します。

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. `MainPage` クラスの先頭で `MyMathFuncs` クラスのインスタンスを宣言します。

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. `ContentPage` 基底クラスの `OnAppearing` および `OnDisappearing` メソッドをオーバーライドします。

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
    }
    ```

5. 前に宣言した `myMathFuncs` 変数を初期化するように `OnAppearing` メソッドを更新します。

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. `myMathFuncs` の `Dispose` メソッドを呼び出すように `OnDisappearing` メソッドを更新します。

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. 次のように、**TestMathFuncs** というプライベート メソッドを実装します。

    ```csharp
    private void TestMathFuncs()
    {
        var numberA = 1;
        var numberB = 2;

        // Test Add function
        var addResult = myMathFuncs.Add(numberA, numberB);

        // Test Subtract function
        var subtractResult = myMathFuncs.Subtract(numberA, numberB);

        // Test Multiply function
        var multiplyResult = myMathFuncs.Multiply(numberA, numberB);

        // Test Divide function
        var divideResult = myMathFuncs.Divide(numberA, numberB);

        // Output results
        Debug.WriteLine($"{numberA} + {numberB} = {addResult}");
        Debug.WriteLine($"{numberA} - {numberB} = {subtractResult}");
        Debug.WriteLine($"{numberA} * {numberB} = {multiplyResult}");
        Debug.WriteLine($"{numberA} / {numberB} = {divideResult}");
    }
    ```

8. 最後に、`OnAppearing` メソッドの最後で `TestMathFuncs` を呼び出します。

    ```csharp
    TestMathFuncs();
    ```

9. 各ターゲットプラット フォームでアプリを実行し、 **[アプリケーションの出力]** パッドの出力が次のようになっていることを確認します。

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Android でのテスト時に '*DLLNotFoundException*' が発生した場合、または iOS でビルド エラーが発生した場合は、使用しているデバイス/エミュレーター/シミュレーターの CPU アーキテクチャが、サポートするように選択したサブセットと互換性があることを必ず確認してください。 

## <a name="summary"></a>まとめ

この記事では、NuGet パッケージを介して配布される共通の .NET ラッパーを通じてネイティブ ライブラリを使用する Xamarin.Forms アプリを作成する方法について説明しました。 このチュートリアルに記載されている例は、この方法をより簡単に示すために、意図的に非常に単純化されています。 実際のアプリケーションでは、例外処理、コールバック、より複雑な型のマーシャリング、他の依存関係ライブラリとのリンクなどの複雑さに対処する必要があります。 重要な考慮事項は、 C++ コードの展開がラッパーおよびクライアント アプリケーションと調整および同期されるプロセスです。 このプロセスは、これらの懸念事項の一方または両方が 1 つのチームの責任であるかどうかによって異なる場合があります。 いずれにしても、自動化は大きなメリットです。 関連するダウンロードとともに、いくつかの重要な概念についての詳細を提供するリソースを以下に示します。 

### <a name="downloads"></a>ダウンロード

- [NuGet コマンド ライン (CLI) ツール](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>使用例

- [C++ による Hyperlapse クロスプラットフォーム モバイル開発](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++ および Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles サンプル ポート](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>関連項目

[この投稿の内容に関連する記事](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)