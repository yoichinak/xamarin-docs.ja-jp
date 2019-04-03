---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Xamarin を使用した C と C++ のライブラリを使用します。
description: Visual Studio for Mac ができますをビルドし、iOS、Android 向けモバイル アプリにクロス プラットフォームの C/C++ コードを統合する Xamarin を使用して、C#します。 この記事を設定して、Xamarin アプリで C++ プロジェクトをデバッグする方法について説明します。
author: mikeparker104
ms.author: miparker
ms.date: 12/17/2018
ms.openlocfilehash: a235a24d544e938d4bf29e6569564aface2f6972
ms.sourcegitcommit: 1c2565c372207bfa257cadac2a2d23d4f90b0cea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58866385"
---
# <a name="use-cc-libraries-with-xamarin"></a>Xamarin を使用した C と C++ のライブラリを使用します。

## <a name="overview"></a>概要

Xamarin では、Visual Studio を使用したクロス プラットフォーム ネイティブ モバイル アプリを作成することができます。 一般に、C#バインドを使用する開発者に既存のプラットフォーム コンポーネントを公開します。 ただし、既存を使用する Xamarin アプリの必要性がコードベース場合もあります。 場合がありますチームだけがないコードベースを時間、予算、またはリソースを十分にテストされた、大幅に最適化された大規模なポートをC#します。

[クロス プラットフォーム モバイル開発のための visual C](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development)により、C と C++ とC#統合されたデバッグ操作を含む多くの利点を提供する、同じソリューションの一部としてビルドするコードをします。 Microsoft はなど、アプリを配布するには、この方法で C と C++ と Xamarin を使用が[Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw)と[Pix カメラ](https://www.microsoft.com/microsoftpix)します。

ただし、場合によってはあるは、要望 (要件) を保持する既存の C と C++ のツールとライブラリ コードがサード パーティ製コンポーネントのような場合と同様に、ライブラリを扱う方法、アプリケーションから分離するために、インプレース プロセスです。 このような状況で課題がないだけを公開するのに関連するメンバーC#が依存関係としてライブラリを管理します。 もちろん、できるだけこのプロセスの自動化とは、します。  

この投稿でこのシナリオでの高レベルの方法について説明し、簡単な例について説明します。

## <a name="background"></a>背景

C/C++ はクロスプラット フォーム対応の言語と見なされますが、ソース コードが実際にクロス プラットフォーム、C/C++ を使用したのみ対象のすべてのコンパイラでサポートされているし、ほとんどまたはまったくの条件が含まれているプラットフォームを含むであることを確認する細心を実行する必要がありますかコンパイラ固有のコード。

最終的にコードはコンパイルされ、したがってこれを要約する共通の対象となるプラットフォーム (とコンパイラ) の間ですべてのターゲット プラットフォームで正常に実行する必要があります。 微妙な違いのコンパイラからこの問題が生じるし、ように徹底的なテスト (できれば自動) 各ターゲット プラットフォームがますます重要にします。  

## <a name="high-level-approach"></a>高レベルの方法

次の図は、クロス プラットフォーム Xamarin ライブラリは NuGet 経由で共有し、Xamarin.Forms アプリで使用される、C/C++ ソース コードに変換するために使用する 4 段階のアプローチを表します。
 

![Xamarin を C と C++ を使用するための高レベルの方法](images/cpp-steps.jpg)

4 つの段階は次のとおりです。

1.  プラットフォーム固有のネイティブ ライブラリに C と C++ のソース コードをコンパイルします。
2.  Visual Studio ソリューションを含むネイティブ ライブラリをラップします。
3.  梱包し、.NET ラッパー用の NuGet パッケージをプッシュします。
4.  Xamarin アプリから NuGet パッケージを使用します。

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>第 1 段階:プラットフォーム固有のネイティブ ライブラリに C と C++ ソース コードのコンパイル

このステージの目標は、呼び出すことができるネイティブ ライブラリを作成する、C#ラッパー。 これは、状況に応じて関連することができない可能性がありますもかまいません。 多くのツールとプロセスにこの一般的なシナリオに移動することができますは、この記事の範囲を超えています。 重要な考慮事項は、C と C++ のコードベースを任意のネイティブ ラッパー コード、十分な単体テストとの同期を管理することでビルドの自動化。 

付随するシェル スクリプトを使用して Visual Studio Code を使用して、チュートリアルでは、ライブラリが作成されています。 このチュートリアルの拡張バージョンが記載されて、 [Mobile CAT GitHub リポジトリ](https://github.com/xamarin/mobcat/blob/dev/samples/cppwithxamarin/README.md)さらに詳しくサンプルのこの部分をについて説明します。 ネイティブ ライブラリ中として扱われます、サードパーティの依存関係ここでが、コンテキストのこのステージを示します。


わかりやすくするため、このチュートリアルには、アーキテクチャのサブセットのみが対象とします。 Ios の場合は、個々 のアーキテクチャに固有のバイナリから 1 つの fat バイナリを作成するのに lipo ユーティリティを使用します。 Android では、動的なバイナリは .so 拡張機能を使用し、iOS では、静的 fat バイナリは .a 拡張機能を使用します。 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>第 2 段階:Visual Studio ソリューションを含むネイティブ ライブラリをラップします。

次のステージでは、.NET からそれらが簡単に使用できるように、ネイティブ ライブラリをラップします。 これは、4 つのプロジェクトで Visual Studio ソリューションで実行されます。 共有プロジェクトには、一般的なコードが含まれています。 対象とする Xamarin.Android、Xamarin.iOS、および .NET Standard の各プロジェクトでは、プラットフォームに依存しない方法で参照されるライブラリを使用できます。

ラッパーの使用の[おとりのトリック](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)、' Paul Betts によって記述します。 これは唯一の方法ではありませんが、簡単にライブラリを参照して、コンシューマー側アプリケーション自体内でプラットフォーム固有の実装を明示的に管理する必要を回避できます。 トリックには、(.NET Standard では、Android、iOS) のターゲットが同じ名前空間、アセンブリ名、およびクラスの構造を共有する本質的にすることです。 以降、NuGet は、プラットフォーム固有のライブラリを優先して常に、.NET Standard のバージョンは実行時に使用されません。

この手順では作業のほとんど P/invoke を使用してネイティブ ライブラリのメソッドを呼び出すし、基になるオブジェクトへの参照の管理に焦点を当てます。 目標は、任意の複雑さを抽象化中に、コンシューマーに、ライブラリの機能を公開します。 Xamarin.Forms の開発者は、アンマネージ ライブラリの内部動作の実務知識を持つ必要はありません。 その他の管理を使用しているように感じがする必要がありますC#ライブラリ。

最終的には、このステージの出力は、一連の .NET ライブラリは、次の手順でパッケージをビルドするために必要な情報を格納する nuspec ドキュメントと共に、ターゲットごとに 1 つです。

**第 3 段階:パッキングと .NET ラッパー用の NuGet パッケージをプッシュします。**

3 番目のステージは、前の手順からのビルド成果物を使用して NuGet パッケージを作成しています。 この手順の結果は、Xamarin アプリから使用できる NuGet パッケージです。 チュートリアルでは、NuGet フィードとして使用するローカル ディレクトリを使用します。 運用環境では、この手順は、パブリックにパッケージを発行する必要があります。 またはプライベート NuGet フィードし完全に自動化する必要があります。

**段階 4:Xamarin.Forms アプリから NuGet パッケージを使う**

最後の手順では、参照および Xamarin.Forms アプリから NuGet パッケージを使用します。 これは、前の手順で定義されているフィードを使用する Visual Studio でのフィード、NuGet の構成が必要です。

フィードを構成すると、パッケージは、クロスプラット フォーム-Xamarin.Forms アプリ内の各プロジェクトから参照できる必要があります。 'Bait とスイッチのトリック' は、ネイティブ ライブラリの機能は、1 つの場所で定義されているコードを使用して呼び出すことができますので、同一のインターフェイスを提供します。

ソース コード リポジトリには、[参考資料の一覧](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)プライベート NuGet フィードでは、Azure DevOps を設定する方法とそのフィードにパッケージをプッシュする方法に関する記事が含まれます。 ローカル ディレクトリよりも少しセットアップ時間が必要とする、この種類のフィードはチーム開発環境をことをお勧めします。

## <a name="walk-through"></a>チュートリアルについて

固有の手順に従って**Visual Studio for Mac**、構造体は**Visual Studio 2017**もします。

### <a name="prerequisites"></a>必須コンポーネント

作業を進めるにするには、開発者は必要があります。

-   [NuGet コマンドライン (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

-   [*Visual Studio* *for Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> アクティブな[ **Apple 開発者アカウント**](https://developer.apple.com/)が iphone アプリを展開するために必要です。

## <a name="creating-the-native-libraries-stage-1"></a>ネイティブ ライブラリ (ステージ 1) の作成

ネイティブ ライブラリの機能は、例からに基づいて[チュートリアル。スタティック ライブラリ (C++) の作成と](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017)します。

このチュートリアルでは、このシナリオでは、サードパーティの依存関係として、ライブラリが提供されるため、ネイティブ ライブラリを構築して、最初のステージをスキップします。 プリコンパイル済みのネイティブ ライブラリに含まれる、[サンプル コード](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin)することもできます[ダウンロード](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts)直接します。

### <a name="working-with-the-native-library"></a>ネイティブ ライブラリの操作

元の*MathFuncsLib*例には次の定義の MyMathFuncs と呼ばれる 1 つのクラスが含まれています。 

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

追加のクラスは、.NET コンシューマーを作成、破棄、および基になるネイティブの MyMathFuncs クラスとの対話を許可するラッパー関数を定義します。

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

これらのラッパー関数で使用されているがなります、 [Xamarin](https://visualstudio.microsoft.com/xamarin/) 側です。

## <a name="wrapping-the-native-library-stage-2"></a>折り返しのネイティブ ライブラリ (ステージ 2)

このステージが必要です、[ライブラリをプリコンパイル済み](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts)で説明されている、[前のセクション](https://docs.microsoft.com/xamarin/cross-platform/cpp/index)します。

### <a name="creating-the-visual-studio-solution"></a>Visual Studio ソリューションを作成します。

1. **Visual Studio for Mac**、 をクリックして**新しいプロジェクト**(から、*ウェルカム ページ*) または**新しいソリューション**(から、 *ファイル*メニュー)。
2. **新しいプロジェクト**ウィンドウで、選択**共有プロジェクト**(内から*マルチプラット フォーム > ライブラリ*) 順にクリックします**次**します。
3. 次のフィールドを更新し、クリックして**作成**:

    - **プロジェクト名:** MathFuncs.Shared  
    - **ソリューション名:** MathFuncs  
    - **場所:** 既定値を使用して、保存場所 (または別の方法を選択)   
    - **ソリューション ディレクトリ内のプロジェクトを作成します。** チェックするように設定します。
4. **ソリューション エクスプ ローラー**をダブルクリックして、 **MathFuncs.Shared**プロジェクトに移動して**メイン設定**します。
5. 削除**します。共有**から、 **Namespace の既定の**に設定されているため**MathFuncs**のみ、 をクリックし、 **OK**します。
6. 開いている**MyClass.cs** (テンプレートによって作成された) クラスとファイル名の両方の名前を変更**MyMathFuncsWrapper**名前空間を変更および**MathFuncs**。
7. **コントロール + クリック**ソリューションに**MathFuncs**を選択し、**新しいプロジェクトの追加.** から、**追加**メニュー。
8. **新しいプロジェクト**ウィンドウで、選択 **.NET Standard Library** (内から*マルチプラット フォーム > ライブラリ*) 順にクリックします**次**します。
9. 選択 **.NET Standard 2.0**  をクリックし、**次**します。
10. 次のフィールドを更新し、クリックして**作成**:

    - **プロジェクト名:** MathFuncs.Standard  
    - **場所:** 同じ保存場所として、共有プロジェクトを使用して、   

11. **ソリューション エクスプ ローラー**をダブルクリックして、 **MathFuncs.Standard**プロジェクト。
12. 移動します**メイン設定**、更新し、**既定 Namespace**に**MathFuncs**します。
13. 移動し、**出力**の設定を更新し、**アセンブリ名**に**MathFuncs**します。
14. 移動します、**コンパイラ**設定、変更、**構成**に**リリース**設定**デバッグ情報**に**シンボルのみ** をクリックし、 **OK**します。
15. 削除**Class1.cs/Getting 開始**プロジェクト (次のいずれかがされている場合、テンプレートの一部として含まれています)。
16. **コントロール + をクリックします**プロジェクト**依存関係、参照**フォルダーを選択し、**参照の編集**。
17. 選択**MathFuncs.Shared**から、**プロジェクト** タブの  をクリックし、 **OK**します。
18. 手順 7-17 (手順 9 無視します)、次の構成を使用します。

    | **プロジェクト名**  | **テンプレート名**   | **新しいプロジェクト メニュー**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | クラス ライブラリ       | Android > ライブラリ      |
    | MathFuncs.iOS     | ライブラリのバインド     | iOS > ライブラリ          |

19. **ソリューション エクスプ ローラー**をダブルクリックして、 **MathFuncs.Android**プロジェクトに移動し、**コンパイラ**設定します。

20. **構成**に設定**デバッグ**、編集**シンボルの定義**に含める**Android;** します。

21. 変更、**構成**に**リリース**、編集し、**シンボルの定義**も含める**Android;** します。

22. 手順 19 ~ 20 の**MathFuncs.iOS**編集、**シンボルの定義**に含める**iOS;** の代わりに**Android;** どちらの場合もします。

23. ソリューションをビルド**リリース**構成 (**コントロール コマンド + B**) および検証 (それぞれのプロジェクトの bin フォルダー) 内のすべての 3 つの出力アセンブリ (iOS、Android、.NET Standard) が同じで共有します。名前**MathFuncs.dll**します。

この段階では、Android、iOS と .NET Standard、および 3 つのターゲットの各によって参照される共有プロジェクトにそれぞれ 1 つ、3 つのターゲットがソリューションに必要です。 これらは、同じ名前で同じ既定の名前空間と出力アセンブリを使用する構成する必要があります。 これは、前に説明した 'おとり' アプローチの必要があります。

### <a name="adding-the-native-libraries"></a>ネイティブ ライブラリを追加します。

ラッパーのソリューションへのネイティブ ライブラリの追加のプロセスは、Android および iOS の間で若干異なります。

#### <a name="native-references-for-mathfuncsandroid"></a>MathFuncs.Android のネイティブ参照

1. **コントロール + クリック**で、 **MathFuncs.Android**プロジェクトを選択し、**新しいフォルダー**から、**追加** メニューの名前を付け、 **libs**.

2. 各**ABI** (アプリケーション バイナリ インターフェイス)、**コントロール + をクリックします**で、 **libs**フォルダーを選択し、**新しいフォルダー** から**追加**メニューで、その後、名前付けそれぞれ**ABI**します。 この場合、次のようになります。

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > さらに詳しい概要については、次を参照してください、[アーキテクチャと Cpu](https://developer.android.com/ndk/guides/arch)トピックから、 [NDK 開発者ガイド](https://developer.android.com/ndk/guides/)、アドレス指定に関するセクションでは具体的には[アプリ パッケージ内のネイティブ コード](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages).

3. フォルダー構造を確認します。  

    ```
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. 対応する追加 **.so**を各ライブラリ、 **ABI**フォルダーは、次のマッピングに基づきます。

    **arm64 v8a:** libs/Android/arm64

    **armeabi v7a:** libs/Android/arm  

    **x86:** libs/Android/x86

    **x86_64:** x86_64/ライブラリ/Android

    > [!NOTE]
    > ファイルを追加する**コントロール + をクリック**、それぞれを表すフォルダーで**ABI**を選択し、**ファイルを追加しています.** から、**追加**メニュー。 適切なライブラリを選択 (から、 **PrecompiledLibs**ディレクトリ) をクリックし、**オープン**順にクリックします**ok**に既定のオプションのまま*コピー、ファイルをディレクトリに*します。

5. 各、 **.so**ファイル、**コントロール + をクリック**選択し、 **EmbeddedNativeLibrary**オプションを**ビルド アクション**メニュー。

これで、 **libs**フォルダーが次のように表示されます。

```bash
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

#### <a name="native-references-for-mathfuncsios"></a>MathFuncs.iOS のネイティブ参照

1. **コントロール + クリック**上、 **MathFuncs.iOS**プロジェクトを選択し、**ネイティブ参照の追加**から、**追加**メニュー。 
2. 選択、 **libMathFuncs.a**ライブラリ (libs/ios 下から、 **PrecompiledLibs**ディレクトリ) をクリックし、**開く** 
3. **コントロール + をクリックします**上、 **libMathFuncs**ファイル (内で、**ネイティブ参照**フォルダーを選択し、**プロパティ**メニューのオプションを  
4. 構成、**ネイティブ参照**プロパティ (チェック マーク アイコンを表示) チェックするために、**プロパティ**パッド。
        
    - 強制読み込み
    - C++ には
    - スマート リンク 

    > [!NOTE]
    > と共にバインド ライブラリ プロジェクトの種類を使用して、[ネイティブ参照](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references)スタティック ライブラリを埋め込むし、(NuGet パッケージに含まれる) 場合でも、それを参照する Xamarin.iOS アプリに自動的にリンクするため、使用できます。 

5. 開いている**ApiDefinition.cs**、コメント付きコードをテンプレートを削除しています (だけ、 **MathFuncs**名前空間) の同じ手順を実行し、 **Structs.cs** 

    > [!NOTE]
    > バインディング ライブラリ プロジェクトには、これらのファイルが必要です (で、 *ObjCBindingApiDefinition*と*ObjCBindingCoreSource*ビルド アクション) を構築するためにします。 ただし、標準的な P/invoke を使用して Android と iOS の両方のライブラリ ターゲット間で共有できる方法でこれらのファイルの外部で、ネイティブ ライブラリを呼び出す、コードを記述します。

### <a name="writing-the-managed-library-code"></a>マネージ ライブラリ コードの記述

ここで、書き込み、C#をネイティブ ライブラリを呼び出すコードです。 目標は、基になる複雑な非表示にします。 コンシューマーでは、ネイティブ ライブラリ内部のまたは P/invoke の概念のすべての知識は必要ありません。  

#### <a name="creating-a-safehandle"></a>SafeHandle を作成します。

1. **コントロール + クリック**上、 **MathFuncs.Shared**プロジェクトを選択し、**ファイルを追加しています.** から、**追加**メニュー。 
2. 選択**空のクラス**から、**新しいファイル**ウィンドウで、名前を付けます**MyMathFuncsSafeHandle**  をクリックし、**新規**
3. 実装、 **MyMathFuncsSafeHandle**クラス。

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => this.handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > A [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2)はマネージ コードでアンマネージ リソースを使用することをお勧めします。 すぐに抽象化、多くの重要な終了処理とオブジェクトのライフ サイクルに関連する定型コードを入力します。 このハンドルの所有者、その後扱うことができますように、その他のマネージ リソースと完全には実装する必要はありません[Disposable パターン](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)します。 

#### <a name="creating-the-internal-wrapper-class"></a>内部ラッパー クラスを作成します。

1. 開いている**MyMathFuncsWrapper.cs**、内部の静的クラスに変更すること

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. 同じファイルでは、クラスに次の条件付きステートメントを追加します。

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > これにより設定、 **DllName**定数値がのライブラリがビルドされているかどうかに基づいて**Android**または**iOS**します。 これは、それぞれ各プラットフォームもここで使用されているライブラリの種類で使用される別の名前付け規則に対処します。 Android では、ダイナミック ライブラリを使用して、これで、ファイル名拡張子を含む必要があります。 Ios、'*__Internal*' がスタティック ライブラリを使用しているために必要です。

3. 参照を追加**System.Runtime.InteropServices**の上部にある、 **MyMathFuncsWrapper.cs**ファイル

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. ラッパーの作成と破棄を処理するメソッドを追加、 **MyMathFuncs**クラス。

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > 定数で渡される**DllName**を**DllImport**属性と共に、 **EntryPoint**明示的に 3/2/2006 .NET ランタイムに呼び出される関数の名前そのライブラリ。 技術的には、私たちを提供する必要はありません、 **EntryPoint**以外の場合、管理対象のメソッド名が管理されていないものと同じ値です。 マネージ メソッドの名前として使用が 1 つが指定されていない場合、 **EntryPoint**代わりにします。 ただし、明示的に指定することをお勧めします。  

5. 使用する操作を有効にするラッパー メソッドを追加、 **MyMathFuncs**クラスの次のコードを使用します。

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
    > この例では、パラメーターの単純型を使用しています。 ビットごとのコピーにはマーシャ リングであるためここでは不要に追加の作業です。 また、使用、 **MyMathFuncsSafeHandle**標準ではなくクラス**IntPtr**します。 **IntPtr**に自動的にマップされます、 **SafeHandle**マーシャ リング プロセスの一部として。

6. いることを確認、完成した**MyMathFuncsWrapper**クラスとして表示されます以下。

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

#### <a name="completing-the-mymathfuncssafehandle-class"></a>MyMathFuncsSafeHandle クラスの完了
1. 開く、 **MyMathFuncsSafeHandle**クラスは、プレース ホルダーに移動**TODO**内でコメント、 **ReleaseHandle**メソッド。
    ```csharp
    // TODO: Release the handle here
    ```
2. 置換、 **TODO**行。

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>MyMathFuncs クラスを記述

ラッパーが完了したら、これでは、アンマネージの C++ MyMathFuncs オブジェクトへの参照を管理する MyMathFuncs クラスを作成します。  

1. **コントロール + クリック**上、 **MathFuncs.Shared**プロジェクトを選択し、**ファイルを追加しています.** から、**追加**メニュー。 
2. 選択**空のクラス**から、**新しいファイル**ウィンドウで、名前を付けます**MyMathFuncs**  をクリックし、**新規**
3. 次のメンバーを追加、 **MyMathFuncs**クラス。

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. クラスのコンス トラクターを実装して、作成し、ネイティブのハンドルを保存するように**MyMathFuncs**オブジェクトのクラスがインスタンス化時に。

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. 実装、 **IDisposable**インターフェイスの次のコードを使用します。

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

6. 実装、 **MyMathFuncs**メソッドを使用して、 **MyMathFuncsWrapper**を基になるアンマネージ オブジェクトを格納したが、ポインターを渡すことによって内部的には、実際の作業を実行するクラス。 コードは、次のようにする必要があります。

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

#### <a name="creating-the-nuspec"></a>Nuspec を作成します。

ライブラリをパッケージ化され、NuGet 経由で配布するために、ソリューションが必要な**nuspec**ファイル。 これにより、サポートされているプラットフォームごとに生成されたアセンブリの対象は、識別します。

1.  **コントロール + をクリックします**ソリューション**MathFuncs**を選択し、**ソリューション フォルダーの追加**から、**追加** メニューの名前を付け、 **SolutionItems**.
2.  **コントロール + をクリックします**上、 **SolutionItems**フォルダーを選択し、**新しいファイル.** から、**追加**メニュー。
3.  選択**空の XML ファイル**から、**新しいファイル**ウィンドウで、名前を付けます**MathFuncs.nuspec**順にクリックします**新規**します。
4.  Update **MathFuncs.nuspec**に表示する基本的なパッケージ メタデータ、 **NuGet**コンシューマー。 例:


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
    >  参照してください、 [nuspec リファレンス](https://docs.microsoft.com/nuget/reference/nuspec)の詳細については、このマニフェストで使用されるスキーマのドキュメント。

5. 追加、`<files>`要素の子として、`<package>`要素 (真下`<metadata>`)、個別に各ファイルを識別する`<file>`要素。

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->        

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > パッケージがプロジェクトにインストールされている場合とが同じ名前で指定された複数のアセンブリがある場合は、NuGet は効果的に特定のプラットフォームに最も固有のアセンブリを選択します。

6. 追加、`<file>`の要素、 **Android**アセンブリ。

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. 追加、`<file>`の要素、 **iOS**アセンブリ。

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. 追加、`<file>`の要素、 **netstandard2.0**アセンブリ。

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. 確認、 **nuspec**マニフェストします。

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
    > このファイルからアセンブリの出力パスを指定します、**リリース**ビルドは、あるため、その構成を使用してソリューションを構築することを確認します。

3 つの .NET アセンブリおよびサポートするこの時点では、ソリューションに含まれる**nuspec**マニフェストします。

## <a name="distributing-the-net-wrapper-with-nuget"></a>NuGet の .NET ラッパーを配布します。

次の手順では、パッケージ化するために可能性がありますように簡単に、アプリによって使用される、依存関係として管理されている NuGet パッケージを配布します。 折り返しと消費量がすべて実行する 1 つのソリューション内を分離し、有効で NuGet ツールを使用してライブラリを配布するこれらの管理をコードベースしない個別にします。

### <a name="preparing-a-local-packages-directory"></a>ローカル パッケージ ディレクトリの準備

NuGet フィードの最も単純な形式では、ローカル ディレクトリを示します。

1.  **Finder**、便利なディレクトリに移動します。 たとえば、 **"/users"** します。
2.  選択**新しいフォルダー**から、**ファイル**などのわかりやすい名前を提供するメニュー**ローカル nuget フィード**します。

### <a name="creating-the-package"></a>パッケージを作成します。

1.  設定、**ビルド構成**に**リリース**を使用してビルドを実行および**コマンド + B**します。
2.  開いている**ターミナル**を含むフォルダーにディレクトリを変更し、 **nuspec**ファイル。
3.  **ターミナル**、実行、 **nuget パック**コマンドを指定する、 **nuspec**ファイル、**バージョン**(例: 1.0.0) と、 **OutputDirectory**で作成したフォルダーを使用して、[前の手順](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed)、つまり**ローカル nuget フィード**します。 例:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **確認**を**MathFuncs.1.0.0.nupkg**内で作成された、**ローカル nuget フィード**ディレクトリ。

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[省略可能]Azure DevOps での NuGet フィードのプライベートを使用します。

堅牢な手法については、 [Azure DevOps の NuGet パッケージの概要](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package)、そのフィードにプライベート フィードを作成し、(前の手順で生成された) パッケージをプッシュする方法を示しています。

このワークフローを完全に例を使用して自動化することをお勧め[Azure パイプライン](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts)します。 詳細については、次を参照してください。 [Azure パイプラインの概要](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts)します。

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Xamarin.Forms アプリから .NET ラッパーの使用
このチュートリアルを完了するには作成、 **Xamarin.Forms**だけパッケージを使用するアプリケーションをローカルに発行**NuGet**フィードします。

### <a name="creating-the-xamarinforms-project"></a>作成、 **Xamarin.Forms**プロジェクト

1. 新しいインスタンスを開く**Visual Studio for Mac**します。 これから実行できます**ターミナル**:

    ```bash
    open -n -a "Visual Studio"
    ```

2. **Visual Studio for Mac**、 をクリックして**新しいプロジェクト**(から、*ウェルカム ページ*) または**新しいソリューション**(から、 *ファイル*メニュー)。
3. **新しいプロジェクト**ウィンドウで、選択**空白フォームのアプリ**(内から*マルチプラット フォーム > アプリ*) 順にクリックします**次**します。
4. 次のフィールドを更新し、クリックして**次**:

    - **アプリ名:** MathFuncsApp します。
    - **組織の識別子:** たとえば、逆引き名前空間を使用して_com. {your_org}_ します。
    - **ターゲット プラットフォーム:** 既定 (Android および iOS の両方のターゲット) を使用します。
    - **共有コード:** これは、.NET Standard (「共有ライブラリ」ソリューションが可能ですが、このチュートリアルの対象外) に設定します。

5. 次のフィールドを更新し、クリックして**作成**:

    - **プロジェクト名:** MathFuncsApp します。
    - **ソリューション名:** MathFuncsApp します。  
    - **場所:** 既定値を使用して、保存場所 (または別の方法を選択)。

6. **ソリューション エクスプ ローラー**、**コントロール + クリック**ターゲット上 (**MathFuncsApp.Android**または**MathFuncs.iOS**) 初期テストで、選択**スタートアップ プロジェクトとして設定**します。
7. 選択、優先**デバイス**または**シミュレーター**/**エミュレーター**します。 
8. ソリューションの実行 (**コマンド + 戻り**) ことを検証する、テンプレート化された**Xamarin.Forms**プロジェクトをビルドおよび実行できます。 

    > [!NOTE]
    > **iOS** (具体的には、シミュレーター) は、最も高速なビルド/配置時間がある傾向があります。

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>フィード、NuGet の構成をローカルの NuGet を追加します。

1. **Visual Studio**、選択**設定**(から、 **Visual Studio**メニュー)。
2. 選択**ソース**の下にある、 **NuGet**セクションで、クリック**追加**します。
3. 次のフィールドを更新し、クリックして**ソースの追加**:

    - **名:** たとえば、ローカル パッケージのわかりやすい名前を提供します。  
    - **場所:** 指定、**ローカル nuget フィード**で作成したフォルダー、[前の手順](#preparing-a-local-packages-directory)します。

    > [!NOTE]
    > ここで指定する必要はありません、 **Username**と**パスワード**します。 

4. **[OK]** をクリックします。

### <a name="referencing-the-package"></a>パッケージの参照

各プロジェクトに対して、次の手順を繰り返します (**MathFuncsApp**、 **MathFuncsApp.Android**、および**MathFuncsApp.iOS**)。

1. **コントロール + クリック**、プロジェクトをクリックし**NuGet パッケージを追加しています.** から、**追加**メニュー。
2. 検索**MathFuncs**します。 
3. 確認、**バージョン**のパッケージは**1.0.0**よう期待どおりに、その他の詳細が表示されます、**タイトル**と**説明**、つまり、 *MathFuncs*と*サンプル C++ ラッパー ライブラリ*します。 
4. 選択、 **MathFuncs**をパッケージ化し、クリックして**パッケージの追加**します。

### <a name="using-the-library-functions"></a>ライブラリ関数を使用します。

ここを参照する、 **MathFuncs**関数は、プロジェクトの各パッケージが利用できる、C#コード。

1.  開いている**MainPage.xaml.cs**内から、 **MathFuncsApp**共通**Xamarin.Forms**プロジェクト (両方で参照されている**MathFuncsApp.Android**と**MathFuncsApp.iOS**)。
2.  追加**を使用して**ステートメント**System.Diagnostics**と**MathFuncs**ファイルの上部にあります。

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. インスタンスを宣言、`MyMathFuncs`の上部にあるクラス、`MainPage`クラス。

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. 上書き、`OnAppearing`と`OnDisappearing`からメソッド、`ContentPage`基本クラス。

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

5. 更新プログラム、`OnAppearing`を初期化するメソッド、`myMathFuncs`以前に宣言された変数。

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. 更新プログラム、`OnDisappearing`メソッドを呼び出す、`Dispose`メソッド`myMathFuncs`:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. というプライベート メソッドを実装**TestMathFuncs**次のようにします。

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

8. 最後に、呼び出す`TestMathFuncs`の最後に、`OnAppearing`メソッド。

    ```csharp
    TestMathFuncs();
    ```

9. 各ターゲット プラットフォームでアプリを実行し、出力の検証、**アプリケーション出力**パッドが次のように表示されます。

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > 発生した場合、'*DLLNotFoundException*' Android、または ios では、ビルド エラーをテストするを使用する、デバイス/エミュレーターまたはシミュレーターの CPU アーキテクチャを選択したサブセットと互換性があることを確認してくださいサポート。 

## <a name="summary"></a>まとめ

この記事では、NuGet パッケージを使用して分散の一般的な .NET ラッパーを通じてネイティブ ライブラリを使用して Xamarin.Forms アプリを作成する方法について説明します。 このチュートリアルで提供される例は意図的にする方法をより簡単に示しています。 非常に単純です。 実際のアプリケーションは、例外処理などの複雑なコールバック、複雑な型のマーシャ リングやその他の依存関係のライブラリとリンクを処理する必要があります。 重要な考慮事項は、C++ コードの進化を調整し、ラッパーおよびクライアント アプリケーションと同期するプロセスです。 このプロセスは、一方または両方のような問題が 1 つのチームの責任がどうかによって異なる場合があります。 どちらの方法でも、オートメーションは、実際のメリットです。 いくつかの関連するダウンロードとキーの概念に関する参考資料を提供する一部のリソースを以下に示します。 

### <a name="downloads"></a>ダウンロード

- [NuGet コマンドライン (CLI) ツール](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>使用例

- [C++ による Hyperlapse のクロス プラットフォーム モバイル開発](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++ と Xamarin)](https://blog.xamarin.com/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles サンプル ポート](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)

### <a name="further-reading"></a>関連項目

[この投稿の内容に関連する記事](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
