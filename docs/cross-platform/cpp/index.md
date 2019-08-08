---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Xamarin で CC++ /ライブラリを使用する
description: Visual Studio for Mac を使用すると、Xamarin とC++ C#を使用して、クロスプラットフォームの C/コードを Android および iOS 用のモバイルアプリにビルドして統合できます。 この記事では、Xamarin アプリでプロジェクトをC++設定およびデバッグする方法について説明します。
author: mikeparker104
ms.author: miparker
ms.date: 12/17/2018
ms.openlocfilehash: a6c5e172fa9fe41e210f332d351adc307d0c7df3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648194"
---
# <a name="use-cc-libraries-with-xamarin"></a>Xamarin で CC++ /ライブラリを使用する

## <a name="overview"></a>概要

Xamarin を使用すると、開発者は Visual Studio でクロスプラットフォームのネイティブモバイルアプリを作成できます。 通常、 C#バインドは、既存のプラットフォームコンポーネントを開発者に公開するために使用されます。 ただし、Xamarin アプリが既存のコードベースを使用する必要がある場合もあります。 チームによっては、大規模で十分に最適化され、高度に最適化されたコードベースをにC#移植するための時間、予算、リソースがないことがあります。

[クロスC++プラットフォームモバイル開発のためのビジュアル](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development)を使用するC++とC# 、C/およびコードを同じソリューションの一部として構築できるため、統合されたデバッグエクスペリエンスを含む多くの利点が得られます。 Microsoft では、このC++方法で C/および Xamarin を使用して、 [hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw)や[Pix カメラ](https://www.microsoft.com/microsoftpix)などのアプリを提供しています。

ただし、場合によっては、既存の C/C++ツールとプロセスを保持し、ライブラリコードをアプリケーションから分離したままにして、サードパーティ製のコンポーネントと同様にライブラリを扱うことが必要になることがあります。 このような状況では、関連するメンバーをに公開するC#だけでなく、ライブラリを依存関係として管理することも困難です。 もちろん、このプロセスの多くを可能な限り自動化します。  

この記事では、このシナリオの大まかなアプローチについて概説し、簡単な例を紹介します。

## <a name="background"></a>背景

C/C++はクロスプラットフォーム言語と見なされますが、ソースコードが実際にクロスプラットフォームであることを確認するために細心の注意を払うC++必要があります。これは、すべてのターゲットコンパイラでサポートされている C/を使用し、条件付きで含まれるプラットフォームをほとんどまたはまったく含まない、コンパイラ固有のコード。

最終的には、すべてのターゲットプラットフォームでコードをコンパイルして正常に実行する必要があるため、対象となるプラットフォーム (およびコンパイラ) 全体で共通点が実現されます。 コンパイラ間の小さな違いによって問題が発生する可能性があるため、各ターゲットプラットフォームでの徹底的なテスト (可能であれば自動化) がますます重要になります。  

## <a name="high-level-approach"></a>高レベルのアプローチ

次の図は、C/C++ソースコードを NuGet で共有され、Xamarin. Forms アプリで使用されるクロスプラットフォーム Xamarin ライブラリに変換するために使用される4段階のアプローチを示しています。
 
![Xamarin で C/C++を使用するための高レベルのアプローチ](images/cpp-steps.jpg)

4つのステージは次のとおりです。

1. C/C++ソースコードをプラットフォーム固有のネイティブライブラリにコンパイルします。
2. Visual Studio ソリューションを使用してネイティブライブラリをラップする。
3. .NET ラッパー用の NuGet パッケージのパッキングとプッシュ。
4. Xamarin アプリから NuGet パッケージを使用する。

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>ステージ 1:C/C++ソースコードをプラットフォーム固有のネイティブライブラリにコンパイルする

この段階の目的は、 C#ラッパーによって呼び出すことができるネイティブライブラリを作成することです。 これは、状況によっては関連性がない場合もあります。 この一般的なシナリオでは、多くのツールとプロセスがこの記事の範囲を超えています。 重要な考慮事項は、CC++ /コードベースをネイティブラッパーコードと同期させ、十分な単体テストとビルドオートメーションを維持することです。 

チュートリアルのライブラリは、シェルスクリプトが付属している Visual Studio Code を使用して作成されました。 このチュートリアルの拡張バージョンについては、サンプルのこの部分について詳しく説明している[MOBILE CAT GitHub リポジトリ](https://github.com/xamarin/mobcat/blob/dev/samples/cpp_with_xamarin/)を参照してください。 ネイティブライブラリは、この場合はサードパーティの依存関係として扱われますが、この段階はコンテキストのために示されています。

わかりやすくするために、このチュートリアルではアーキテクチャのサブセットのみを対象としています。 IOS では、lipo ユーティリティを使用して、アーキテクチャ固有の個々のバイナリから1つの fat バイナリを作成します。 Android では、で動的バイナリが使用されます。したがって、iOS では、拡張子 a の静的 fat バイナリが使用されます。 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>ステージ 2:Visual Studio ソリューションを使用したネイティブライブラリのラップ

次の段階では、ネイティブライブラリをラップして、.NET から簡単に使用できるようにします。 これは、4つのプロジェクトを含む Visual Studio ソリューションを使用して行います。 共有プロジェクトには、共通のコードが含まれています。 Xamarin、Xamarin、iOS、および .NET Standard を対象とするプロジェクトでは、プラットフォームに依存しない方法でライブラリを参照できます。

このラッパーは、Paul Betts によって説明されている "[bait と switch トリック](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)" を使用します。 これは唯一の方法ではありませんが、ライブラリを簡単に参照できるようになるため、使用中のアプリケーション自体でプラットフォーム固有の実装を明示的に管理する必要がなくなります。 基本的に、ターゲット (.NET Standard、Android、iOS) が同じ名前空間、アセンブリ名、およびクラス構造を共有していることを確認します。 NuGet は常にプラットフォーム固有のライブラリを優先するため、.NET Standard のバージョンは実行時には使用されません。

この手順のほとんどの作業では、P/Invoke を使用してネイティブライブラリメソッドを呼び出し、基になるオブジェクトへの参照を管理する方法に焦点を当てます。 目標は、ライブラリの機能をコンシューマーに公開しながら、複雑さを抽象化することです。 Xamarin. フォーム開発者は、アンマネージライブラリの内部動作に関する実用的な知識を持っている必要はありません。 他のマネージC#ライブラリを使用しているように感じます。

このステージの出力は、最終的に、次の手順でパッケージをビルドするために必要な情報を含む nuspec ドキュメントと共に、ターゲットごとに1つの .NET ライブラリのセットになります。

**ステージ 3:.NET ラッパー用の NuGet パッケージのパッキングとプッシュ**

3番目のステージでは、前の手順で作成したビルドアーティファクトを使用して NuGet パッケージを作成します。 この手順の結果は、Xamarin アプリから使用できる NuGet パッケージです。 このチュートリアルでは、ローカルディレクトリを使用して、NuGet フィードとして機能します。 運用環境では、この手順でパッケージをパブリックまたはプライベートの NuGet フィードに発行する必要があり、完全に自動化されている必要があります。

**ステージ 4:Xamarin. Forms アプリからの NuGet パッケージの使用**

最後の手順は、Xamarin. Forms アプリから NuGet パッケージを参照して使用することです。 これには、前の手順で定義したフィードを使用するように Visual Studio で NuGet フィードを構成する必要があります。

フィードが構成されたら、クロスプラットフォームの Xamarin. Forms アプリの各プロジェクトからパッケージを参照する必要があります。 ' Bait とスイッチのトリック ' は同じインターフェイスを提供するので、1つの場所で定義されたコードを使用してネイティブライブラリの機能を呼び出すことができます。

ソースコードリポジトリには、Azure DevOps でプライベート NuGet フィードを設定する方法と、そのフィードにパッケージをプッシュする方法に関する記事を含む、[さらに参考](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)となる一覧が含まれています。 ローカルディレクトリよりも少し多くの設定を行う必要がありますが、この種類のフィードはチーム開発環境に適しています。

## <a name="walk-through"></a>チュートリアル

ここで説明する手順は**Visual Studio for Mac**に固有のものですが、構造は**Visual Studio 2017**でも動作します。

### <a name="prerequisites"></a>必須コンポーネント

開発者が従うためには、次のものが必要です。

- [NuGet コマンドライン (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Visual Studio* *Mac の場合*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> IPhone にアプリを展開するには、アクティブな[**Apple 開発者アカウント**](https://developer.apple.com/)が必要です。

## <a name="creating-the-native-libraries-stage-1"></a>ネイティブライブラリの作成 (ステージ 1)

ネイティブライブラリの機能は、チュートリアルの[例に基づいています。スタティックライブラリを作成して使用C++する](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017)()。

このチュートリアルでは、このシナリオではライブラリがサードパーティの依存関係として提供されているため、ネイティブライブラリを構築するための最初の段階をスキップします。 プリコンパイル済みネイティブライブラリは、[サンプルコード](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin)と共にインクルードされるか、直接[ダウンロード](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts)できます。

### <a name="working-with-the-native-library"></a>ネイティブライブラリの操作

元の*MathFuncsLib*の例には、次`MyMathFuncs`の定義を持つという1つのクラスが含まれています。

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

追加のクラスは、.net コンシューマーが基になるネイティブ`MyMathFuncs`クラスを作成、破棄、および操作できるようにするラッパー関数を定義します。

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

これらのラッパー関数は、 [Xamarin](https://visualstudio.microsoft.com/xamarin/) 側で使用されます。

## <a name="wrapping-the-native-library-stage-2"></a>ネイティブライブラリのラップ (ステージ 2)

この段階では、[前のセクション](##creating-the-native-libraries-stage-1)で説明した[プリコンパイル済みライブラリ](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts)が必要です。

### <a name="creating-the-visual-studio-solution"></a>Visual Studio ソリューションの作成

1. **Visual Studio for Mac**で、[**新しいプロジェクト**] ([*ようこそ] ページ*から) または [**新しいソリューション**] ([*ファイル*] メニューから) をクリックします。
2. [**新しいプロジェクト**] ウィンドウで、 **[共有プロジェクト**] を選択し (*マルチプラットフォーム > ライブラリ*内から)、[**次へ**] をクリックします。
3. 次のフィールドを更新し、[**作成**] をクリックします。

    - **プロジェクト名:** MathFuncs  
    - **ソリューション名:** MathFuncs  
    - **設置**既定の保存場所を使用する (または別のものを選択する)   
    - **ソリューションディレクトリ内にプロジェクトを作成します。** これをオンに設定します
4. **ソリューションエクスプローラー**で、 **MathFuncs**プロジェクトをダブルクリックし、**メイン設定**に移動します。
5. **を削除します。** **既定の名前空間**から共有して、 **MathFuncs** only に設定し、[ **OK]** をクリックします。
6. テンプレートによって作成された**MyClass.cs**を開き、クラスとファイル名の両方を**MyMathFuncsWrapper**に変更し、名前空間を**MathFuncs**に変更します。
7. ソリューション**MathFuncs**を制御し、[**追加**] メニューの [**新しいプロジェクトの追加**] を**クリック**します。
8. [**新しいプロジェクト**] ウィンドウで **.NET Standard ライブラリ**を選択し (*マルチプラットフォーム > ライブラリ*内から)、[**次へ**] をクリックします。
9. **.NET Standard 2.0**を選択し、[**次へ**] をクリックします。
10. 次のフィールドを更新し、[**作成**] をクリックします。

    - **プロジェクト名:** MathFuncs  
    - **設置**共有プロジェクトと同じ保存場所を使用する   

11. **ソリューションエクスプローラー**で、 **MathFuncs**プロジェクトをダブルクリックします。
12. **メイン設定**に移動し、**既定の名前空間**を**MathFuncs**に更新します。
13. **出力**の設定に移動し、[**アセンブリ名**] を**MathFuncs**に更新します。
14. **コンパイラ**の設定に移動し、**構成**を [**リリース**] に変更し、**デバッグ情報**を**シンボルのみ**に設定して、[ **OK**] をクリックします。
15. プロジェクトから開始された**Class1.cs/Getting**を削除します (これらのうちの1つがテンプレートの一部として含まれている場合)。
16. [プロジェクトの**依存関係/参照**] フォルダーを CTRL **+ クリック**し、[**参照の編集**] を選択します。
17. [**プロジェクト**] タブの [ **MathFuncs** ] を選択し、[ **OK**] をクリックします。
18. 次の構成を使用して、手順 7-17 (手順 9. を無視) を繰り返します。

    | **プロジェクト名**  | **テンプレート名**   | **[新しいプロジェクト] メニュー**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs | クラス ライブラリ       | Android > ライブラリ      |
    | MathFuncs     | バインドライブラリ     | iOS > ライブラリ          |

19. **ソリューションエクスプローラー**で、 **MathFuncs**プロジェクトをダブルクリックし、**コンパイラ**設定に移動します。

20. **構成**が [**デバッグ**] に設定されている状態で、[**シンボルの定義**] を編集して**Android;** を追加します。

21. **構成**を [**リリース**] に変更し、[**シンボルの定義**] を編集して**Android;** も含めます。

22. **MathFuncs**に対して手順19-20 を繰り返し、 **Android**の代わりに**Ios**を含めるように**シンボルを定義**します。どちらの場合も同様です。

23. **リリース**構成 (**CONTROL + COMMAND + B**) でソリューションをビルドし、3つすべての出力アセンブリ (Android、iOS、.NET Standard) が (それぞれのプロジェクトの bin フォルダー内に) 同じ名前の**MathFuncs**を共有していることを確認します。

この段階では、ソリューションには3つのターゲットがあります。1つは Android、iOS、.NET Standard のそれぞれ、もう1つは3つのターゲットのそれぞれによって参照される共有プロジェクトです。 同じ既定の名前空間と出力アセンブリを同じ名前で使用するように構成する必要があります。 これは、前述の「bait とスイッチ」アプローチに必要です。

### <a name="adding-the-native-libraries"></a>ネイティブライブラリの追加

ラッパーソリューションにネイティブライブラリを追加するプロセスは、Android と iOS で多少異なります。

#### <a name="native-references-for-mathfuncsandroid"></a>MathFuncs のネイティブリファレンス

1. **MathFuncs**プロジェクトを CTRL **+ クリック**し、[**追加**] メニューの [**新しいフォルダー** ] を選択し**ます。**

2. 各**abi** (アプリケーションバイナリインターフェイス) に対して、 **lib フォルダーを**ctrl **+ クリック**し、[**追加**] メニューの [**新しいフォルダー** ] を選択して、それぞれの**abi**の後に名前を付けます。 この場合、次のようになります。

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > より詳細な概要については、「 [NDK 開発者ガイド](https://developer.android.com/ndk/guides/)」の「[アーキテクチャと cpu](https://developer.android.com/ndk/guides/arch) 」を参照してください。特に、[アプリパッケージでのネイティブコード](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages)のアドレス指定に関するセクションを参照してください。

3. フォルダー構造を確認します。  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. 次のマッピングに基づいて、各**ABI**フォルダーに対応する**so**ライブラリを追加します。

    **arm64-arm64-v8a:** Lib/Android/arm64

    **armeabi-armeabi-v7a:** Lib/Android/arm  

    **x86:** Lib/Android/x86

    **x86_64:** Lib/Android/x86_64

    > [!NOTE]
    > ファイルを追加するには、それぞれの**ABI**を表すフォルダーを**ctrl + クリック**し、[**追加**] メニューの [**ファイルの追加...** ] をクリックします。 適切なライブラリ ( **PrecompiledLibs**ディレクトリから) を選択し、[**開く**] をクリックして、[ **OK** ] をクリックします。既定のオプションのままにして、*ファイルをディレクトリにコピー*します。

5. それぞれ**のファイルに**ついて、コントロールをクリックし、[**ビルドアクション**] メニューから **[** **EmbeddedNativeLibrary** ] オプションを選択します。

これで **、lib**フォルダーは次のように表示されます。

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

#### <a name="native-references-for-mathfuncsios"></a>MathFuncs のネイティブ参照

1. **MathFuncs**プロジェクトを CTRL **+ クリック**し、[**追加**] メニューの [**ネイティブ参照の追加**] を選択します。 
2. LibMathFuncs ライブラリを選択し ( **PrecompiledLibs**ディレクトリの [ライブラリ/ios] から)、[**開く**] をクリックし**ます。** 
3. **LibMathFuncs**ファイル (**ネイティブ参照**フォルダー内) を ctrl **+ クリック**し、メニューから [**プロパティ**] オプションを選択します。  
4. **プロパティ**パッドでチェックされるように**ネイティブ参照**プロパティを構成します (ティックアイコンが表示されます)。

    - 強制読み込み
    - 勧めC++
    - スマートリンク

    > [!NOTE]
    > [ネイティブ参照](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references)と共にバインドライブラリプロジェクトの種類を使用すると、スタティックライブラリが埋め込まれ、それを参照する Xamarin ios アプリ (NuGet パッケージに含まれている場合でも) に自動的にリンクされるようになります。

5. **ApiDefinition.cs**を開き、テンプレート化されたコメント付きコード`MathFuncs` (名前空間のみを残します) を削除し、 **Structs.cs**に対して同じ手順を実行します。 

    > [!NOTE]
    > バインドライブラリプロジェクトをビルドするには、( *Objcbindingapidefinition*および*Objcbindingapidefinition*ビルドアクションを使用して) これらのファイルが必要です。 ただし、標準の P/Invoke を使用して Android と iOS の両方のライブラリターゲット間で共有できるように、これらのファイルの外部でネイティブライブラリを呼び出すコードを記述します。

### <a name="writing-the-managed-library-code"></a>マネージライブラリコードの記述

次に、ネイティブC#ライブラリを呼び出すコードを記述します。 目標は、基になる複雑さをすべて隠すことです。 コンシューマーは、ネイティブライブラリの内部構造または P/Invoke 概念に関する実用的な知識を必要としません。  

#### <a name="creating-a-safehandle"></a>SafeHandle の作成

1. **MathFuncs**プロジェクトを CTRL **+ クリック**し、[**追加**] メニューの [**ファイルの追加...** ] を選択します。 
2. [**新しいファイル**] ウィンドウで [**空のクラス**] を選択し、 **MyMathFuncsSafeHandle**という名前を指定して、[**新規**] をクリックします。
3. **MyMathFuncsSafeHandle**クラスを実装します。

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
    > マネージコードでアンマネージリソースを操作するには、 [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2)を使用することをお勧めします。 これにより、クリティカル終了とオブジェクトのライフサイクルに関連する多数の定型コードが抽象化されます。 このハンドルの所有者は、その後、他のマネージリソースと同様に処理でき、完全に[破棄](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)可能なパターンを実装する必要はありません。 

#### <a name="creating-the-internal-wrapper-class"></a>内部ラッパークラスの作成

1. **MyMathFuncsWrapper.cs**を開き、内部の静的クラスに変更します。

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. 同じファイルで、次の条件付きステートメントをクラスに追加します。

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > これにより、ライブラリが**Android**または**iOS**用にビルドされているかどうかに基づいて、 **DllName**定数値が設定されます。 これは、それぞれのプラットフォームで使用されるさまざまな名前付け規則に対処するためのものであり、この場合に使用されるライブラリの種類も対象となります。 Android はダイナミックライブラリを使用しているため、拡張子を含むファイル名が必要です。 IOS では、スタティックライブラリを使用しているため、' *__ Internal*' が必要です。

3. **MyMathFuncsWrapper.cs**ファイルの先頭に InteropServices への参照を追加します **。**

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. **MyMathFuncs**クラスの作成と破棄を処理するラッパーメソッドを追加します。

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > この定数**DllName**を**DllImport**属性に渡し、 **EntryPoint**と共に、そのライブラリ内で呼び出す関数の名前を .net ランタイムに明示的に指示しています。 技術的には、マネージメソッド名がアンマネージドメソッド名と同一である場合、 **EntryPoint**値を指定する必要はありません。 指定されていない場合は、代わりに、マネージメソッド名が**EntryPoint**として使用されます。 ただし、明示的にすることをお勧めします。  

5. ラッパーメソッドを追加して、次のコードを使用して**MyMathFuncs**クラスを操作できるようにします。

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
    > この例では、パラメーターに単純型を使用しています。 この場合、マーシャリングはビットごとのコピーであるため、追加の作業は必要ありません。 また、標準の**IntPtr**の代わりに**MyMathFuncsSafeHandle**クラスを使用することにも注意してください。 **IntPtr**は、マーシャリングプロセスの一環として、 **SafeHandle**に自動的にマップされます。

6. 完成した**MyMathFuncsWrapper**クラスが次のように表示されることを確認します。

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

1. **MyMathFuncsSafeHandle**クラスを開き、 **releasehandle**メソッド内のプレースホルダー **TODO**コメントに移動します。

    ```csharp
    // TODO: Release the handle here
    ```

1. **TODO**行を置き換えます。

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>MyMathFuncs クラスの記述

ラッパーが完成したので、アンマネージC++ MyMathFuncs オブジェクトへの参照を管理する MyMathFuncs クラスを作成します。  

1. **MathFuncs**プロジェクトを CTRL **+ クリック**し、[**追加**] メニューの [**ファイルの追加...** ] を選択します。 
2. [**新しいファイル**] ウィンドウで [**空のクラス**] を選択し、 **MyMathFuncs**という名前を指定して、[**新規**] をクリックします。
3. **MyMathFuncs**クラスに次のメンバーを追加します。

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. クラスのコンストラクターを実装して、クラスがインスタンス化されるときにネイティブ**MyMathFuncs**オブジェクトへのハンドルを作成して格納します。

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. 次のコードを使用して、 **IDisposable**インターフェイスを実装します。

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

6. **MyMathFuncsWrapper**クラスを使用して**MyMathFuncs**メソッドを実装し、基になるアンマネージオブジェクトに格納されているポインターを渡して、実際の作業を内部で実行します。 コードは次のようになります。

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

#### <a name="creating-the-nuspec"></a>Nuspec の作成

NuGet を使用してライブラリをパッケージ化して配布するために、ソリューションには**nuspec**ファイルが必要です。 これにより、サポートされている各プラットフォームに対して生成されるアセンブリが特定されます。

1. ソリューション**MathFuncs**を CTRL **+ クリック**し、[**追加**] メニューの [**ソリューションフォルダーの追加**] を選択し**ます。**
2. [ **Solutionitems** ] フォルダーを**クリック**し、[**追加**] メニューの [**新しいファイル**] をクリックします。
3. [**新しいファイル**] ウィンドウで [**空の XML ファイル**] を選択し、「 **MathFuncs. nuspec** 」という名前を指定して、[**新規作成**] をクリックします。
4. **NuGet**コンシューマーに表示される基本的なパッケージメタデータを使用して、 **MathFuncs**を更新します。 例:

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
    > このマニフェストに使用されるスキーマの詳細については、 [nuspec のリファレンス](https://docs.microsoft.com/nuget/reference/nuspec)ドキュメントを参照してください。

5. 要素を`<files>` `<package>`要素の子として (直後`<metadata>`に) 追加し、各ファイルを個別`<file>`の要素で識別します。

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > パッケージがプロジェクトにインストールされ、同じ名前で複数のアセンブリが指定されている場合、NuGet は、特定のプラットフォームに最も固有なアセンブリを効果的に選択します。

6. Android アセンブリ`<file>`の要素を追加します。

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. IOS アセンブリ`<file>`の要素を追加します。

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. **Netstandard 2.0**アセンブリの要素を追加します。`<file>`

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. **Nuspec**マニフェストを確認します。

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
    > このファイルは、**リリース**ビルドからのアセンブリ出力パスを指定します。そのため、この構成を使用してソリューションをビルドしてください。

この時点で、ソリューションには3つの .NET アセンブリとサポートする**nuspec**マニフェストが含まれています。

## <a name="distributing-the-net-wrapper-with-nuget"></a>NuGet を使用した .NET ラッパーの配布

次の手順では、NuGet パッケージをパッケージ化して配布します。これにより、アプリで簡単に使用できるようになり、依存関係として管理される可能性があります。 ラッピングと使用はすべて1つのソリューション内で行うことができましたが、NuGet を使用してライブラリを分離することによって、これらのコードベースを個別に管理することができます。

### <a name="preparing-a-local-packages-directory"></a>ローカルパッケージディレクトリを準備する

NuGet フィードの最も単純な形式は、ローカルディレクトリです。

1. **Finder**で、便利なディレクトリに移動します。 たとえば、 **/Users**のようになります。
2. [**ファイル**] メニューの [**新しいフォルダー** ] をクリックし、「**ローカル-nuget フィード**」などのわかりやすい名前を指定します。

### <a name="creating-the-package"></a>パッケージの作成

1. **ビルド構成**を [**リリース**] に設定し、**コマンド + B**を使用してビルドを実行します。
2. **ターミナル**を開き、ディレクトリを**nuspec**ファイルが格納されているフォルダーに変更します。
3. **ターミナル**で、[前の手順](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed) **で作成したフォルダーを使用して、nuspec ファイル、バージョン (1.0.0 など)、および outputdirectory を指定する nuget pack コマンドを実行します。ローカル-nuget フィード**。 例:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **MathFuncs**が**ローカルの nuget フィード**ディレクトリに作成されていることを**確認**します。

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>OPTIONALAzure DevOps でのプライベート NuGet フィードの使用

より堅牢な手法については、「 [Azure DevOps での NuGet パッケージの概要](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package)」で説明されています。これは、プライベートフィードを作成して、(前の手順で生成された) パッケージをそのフィードにプッシュする方法を示しています。

このワークフローを完全に自動化するのが理想的です。たとえば、 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts)を使用します。 詳細については、「 [Azure Pipelines の概要](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts)」を参照してください。

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Xamarin. Forms アプリからの .NET ラッパーの使用

このチュートリアルを完了するには、ローカルの**NuGet**フィードに発行されたパッケージを使用するための**Xamarin. Forms**アプリを作成します。

### <a name="creating-the-xamarinforms-project"></a>**Xamarin. Forms**プロジェクトの作成

1. **Visual Studio for Mac**の新しいインスタンスを開きます。 これは、**ターミナル**から実行できます。

    ```bash
    open -n -a "Visual Studio"
    ```

2. **Visual Studio for Mac**で、[**新しいプロジェクト**] ([*ようこそ] ページ*から) または [**新しいソリューション**] ([*ファイル*] メニューから) をクリックします。
3. [**新しいプロジェクト**] ウィンドウで、 **[空のフォームアプリ**] を選択し (*マルチプラットフォーム > アプリ*内から)、[**次へ**] をクリックします。
4. 次のフィールドを更新し、[**次へ**] をクリックします。

    - **アプリ名:** MathFuncsApp.
    - **組織の識別子:** たとえば、" _com. {your_org}_ " のように、逆引き名前空間を使用します。
    - **ターゲットプラットフォーム:** 既定値 (Android と iOS の両方のターゲット) を使用します。
    - **共有コード:** これを .NET Standard に設定します ("共有ライブラリ" ソリューションが可能ですが、このチュートリアルでは扱いません)。

5. 次のフィールドを更新し、[**作成**] をクリックします。

    - **プロジェクト名:** MathFuncsApp.
    - **ソリューション名:** MathFuncsApp.  
    - **設置**既定の保存場所を使用するか、別の場所を選択します。

6. **ソリューションエクスプローラー**で、最初のテストのターゲット (**MathFuncsApp**または**MathFuncs**) を制御して**クリック**し、[**スタートアッププロジェクトに設定**] を選択します。
7. 優先される**デバイス**または**シミュレーター**/**エミュレーター**を選択します。 
8. ソリューション (**コマンド + RETURN**) を実行して、テンプレート化された**Xamarin. Forms**プロジェクトが正常にビルドされ実行されることを検証します。 

    > [!NOTE]
    > **iOS**(具体的にはシミュレーター) は、ビルド/デプロイ時間が最速になる傾向があります。

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>NuGet 構成にローカルの NuGet フィードを追加する

1. **Visual studio**で、[**ユーザー設定**] を選択します ( **visual studio**のメニューから)。
2. [ **NuGet** ] セクションの [**ソース**] を選択し、[**追加**] をクリックします。
3. 次のフィールドを更新し、[**ソースの追加**] をクリックします。

    - **Name:** わかりやすい名前 (たとえば、ローカルパッケージ) を指定します。  
    - **設置**[前の手順](#preparing-a-local-packages-directory)で作成した**ローカルの nuget フィード**フォルダーを指定します。

    > [!NOTE]
    > この場合、**ユーザー名**と**パスワード**を指定する必要はありません。 

4. **[OK]** をクリックします。

### <a name="referencing-the-package"></a>パッケージの参照

各プロジェクト (**MathFuncsApp**、 **MathFuncsApp**、および**MathFuncsApp**) に対して、次の手順を繰り返します。

1. プロジェクトを ctrl **+ クリック**し、[**追加**] メニューの [ **NuGet パッケージの追加**] を選択します。
2. **MathFuncs**を検索します。 
3. パッケージの**バージョン**が**1.0.0**であること、およびその他の詳細が予想どおりに表示されることを確認します。これには、**タイトル**と**説明**、つまり、 *MathFuncs*および*サンプルC++ラッパーライブラリ*が含まれます。 
4. **MathFuncs**パッケージを選択し、[**パッケージの追加**] をクリックします。

### <a name="using-the-library-functions"></a>ライブラリ関数の使用

これで、各プロジェクトの**MathFuncs**パッケージへの参照を使用して、 C#コードで関数を使用できるようになりました。

1. **MathFuncsApp** ( **MathFuncsApp**と**MathFuncsApp**の両方で参照されます) から**MainPage.xaml.cs**を開きます。
2. 次のように 、ファイルの先頭に**MathFuncs と**の**using**ステートメントを追加します。

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. クラスの最上位`MyMathFuncs` `MainPage`にあるクラスのインスタンスを宣言します。

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. 基底クラスの`OnDisappearing`メソッドとメソッドをオーバーライドします。`OnAppearing` `ContentPage`

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

5. 前の`OnAppearing`手順で宣言し`myMathFuncs`た変数を初期化するようにメソッドを更新します。

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. メソッドを更新して、 `Dispose`で`myMathFuncs`メソッドを呼び出します。 `OnDisappearing`

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. 次のように、 **TestMathFuncs**というプライベートメソッドを実装します。

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

8. 最後に、 `TestMathFuncs` `OnAppearing`メソッドの末尾でを呼び出します。

    ```csharp
    TestMathFuncs();
    ```

9. 各ターゲットプラットフォームでアプリを実行し、**アプリケーション出力**パッドの出力を次のように検証します。

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Android でのテスト時に '*system.dllnotfoundexception*' が発生した場合、または iOS でビルドエラーが発生した場合は、使用しているデバイス/エミュレーター/シミュレーターの CPU アーキテクチャが、サポートするように選択したサブセットと互換性があることを確認してください。 

## <a name="summary"></a>まとめ

この記事では、NuGet パッケージを介して配布される共通の .NET ラッパーを介してネイティブライブラリを使用する Xamarin. Forms アプリを作成する方法について説明しました。 このチュートリアルで示す例は、この方法をより簡単に説明するために、意図的に非常に単純なものです。 実際のアプリケーションでは、例外処理、コールバック、より複雑な型のマーシャリング、他の依存関係ライブラリとのリンクなどの複雑さに対処する必要があります。 重要な考慮事項は、 C++コードの進化がラッパーおよびクライアントアプリケーションと連携して同期されるプロセスです。 このプロセスは、これらの懸念事項の一方または両方が1つのチームの責任であるかどうかによって異なります。 どちらの方法でも、自動化は実際の利点です。 いくつかのリソースについては、関連するダウンロードと共に、いくつかの重要な概念についてさらに詳しく説明します。 

### <a name="downloads"></a>ダウンロード

- [NuGet コマンドライン (CLI) ツール](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>使用例

- [を使用したハイパープラットフォーム間のモバイル開発C++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++および Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono サンロサンゼルスサンプルポート](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>関連項目

[この投稿の内容に関連する記事](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
