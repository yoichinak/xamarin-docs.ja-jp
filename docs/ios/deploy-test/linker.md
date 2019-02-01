---
title: Xamarin.iOS アプリをリンクする
description: このドキュメントでは Xamarin.iOS リンカーについて説明します。このリンカーは、Xamarin.iOS アプリケーションから未使用のコードを削除してサイズを縮小するために使用します。
ms.prod: xamarin
ms.assetid: 3A4B2178-F264-0E93-16D1-8C63C940B2F9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/24/2017
ms.openlocfilehash: 04f92988d4d367abd5e6e864d4450aee2e6c1df2
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233797"
---
# <a name="linking-xamarinios-apps"></a>Xamarin.iOS アプリをリンクする

アプリケーションをビルドするとき、Visual Studio for Mac または Visual Studio は **mtouch** という名前のツールを呼び出します。このツールには、マネージド コードのリンカーが含まれています。 アプリケーションで利用されない機能をクラス ライブラリから削除するために使用されます。 これが目標としているのは、アプリケーションのサイズを小さくし、必要なデータだけで出荷することです。

リンカーではスタティック分析を利用し、アプリケーションが選択するさまざまなコード パスを判断します。 検出可能なものが何も削除されないように各アセンブリのあらゆる細部まで分析するため、少し重くなります。 デバッグ中のビルド時間を早めるために、シミュレーター ビルドでは既定で有効になっていません。 ただし、アプリケーションが小さくなるため、AOT コンパイルとデバイスへのアップロードが速くなります。*デバイス (リリース) ビルド*ではすべて、既定でリンカーが使用されます。

リンカーは静的なツールであり、リフレクション経由で呼び出される、あるいは動的にインスタンス化されるインクルードの種類およびメソッドの場合、設定できません。 この制約を回避するために、いくつかのオプションが存在します。

<a name="Linker_Behavior" />

## <a name="linker-behavior"></a>リンカーの動作

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

リンク プロセスは、**[プロジェクト オプション]** のリンカーの動作ドロップダウンからカスタマイズできます。 これにアクセスするには、iOS プロジェクトをダブルクリックし、**[iOS ビルド]、[リンカーのオプション]** の順に選択します。下の画像をご覧ください。

[![](linker-images/image1.png "リンカーのオプション")](linker-images/image1.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

リンク プロセスは、Visual Studio の **[プロジェクトのプロパティ]** のリンカーの動作ドロップダウンからカスタマイズできます。

次の手順で行います。

1. **ソリューション エクスプローラー**で**プロジェクト名**を右クリックし、**[プロパティ]** を選択します。

    ![](linker-images/linking01w.png "ソリューション エクスプローラーでプロジェクト名を右クリックし、[プロパティ] を選択します")
2. **[プロジェクトのプロパティ]** で **[iOS ビルド]** を選択します。

    ![](linker-images/linking02w.png "[iOS ビルド] を選択します")
3. 下の指示に従って操作し、リンク オプションを変更します。

-----

3 つの主要なオプションは次のとおりです。


### <a name="dont-link"></a>リンクしない

リンクを無効にすると、アセンブリは変更されません。 お使いの IDE の対象が iOS シミュレーターのとき、パフォーマンス上の理由から、これが既定の設定となります。 デバイス ビルドの場合、これは、アプリケーションの実行を妨げるバグがリンカーに含まれるときの解決策としてのみ利用してください。 アプリケーションが *-nolink* のみで動作する場合、[バグ レポート](https://github.com/xamarin/xamarin-macios/issues/new)を提出してください。

これは、コマンドライン ツールの mtouch を使用するときの *-nolink* オプションに相当します。

<a name="Link_SDK_assemblies_only" />

### <a name="link-sdk-assemblies-only"></a>SDK アセンブリのみをリンクする

このモードでは、リンカーはアセンブリに変更を加えません。アプリケーションで利用されないものを全部削除することで、SDK アセンブリ (Xamarin.iOS と共に出荷されるものなど) のサイズを減らします。 これは、お使いの IDE の対象が iOS デバイスのときの既定の設定です。

これは、コードを変更する必要がなく、最も単純な選択肢です。 すべてをリンクする場合との違いは、このモードではリンカーが最適化のいくつかを実行できないことです。そのため、すべてをリンクするための仕事量と最終的なアプリケーション サイズの折り合いになります。

これは、コマンドライン ツールの mtouch を使用するときの *-linksdk* オプションに相当します。

<a name="Link_all_assemblies" />

### <a name="link-all-assemblies"></a>すべてのアセンブリをリンクする

すべてをリンクするとき、リンカーはあらゆる最適化を活用し、アプリケーションを可能な限り小さくします。 ユーザー コードが変更されるので、リンカーのスタティック分析で検出できないようにコードで機能が利用されるときはコードが中断されることがあります。 そのような場合、たとえば、Web サービス、リフレクション、シリアル化では、すべてをリンクするために、アプリケーションにいくつかの調整が必要になることがあります。

これは、コマンドライン ツールの **mtouch** を使用するときの *-linkall* オプションに相当します。

<a name="Controlling_the_Linker" />

## <a name="controlling-the-linker"></a>リンカーを制御する

リンカーを使用すると、動的に、あるいは間接的に呼び出したことがあるコードが削除されることもあります。 そのような状況に対処するために、アクションを細かく制御するための機能やオプションがいくつかリンカーに用意されています。

<a name="Preserving_Code" />

### <a name="preserving-code"></a>コードの維持

リンカーを使用すると、System.Reflection.MemberInfo.Invoke を利用したか、`[Export]` 属性でメソッドを Objective-C にエクスポートし、その後セレクターを手動で呼び出した際に動的に呼び出した可能性があるコードが削除されることがあります。

そのような場合、クラスレベルかメンバーレベルで `[Xamarin.iOS.Foundation.Preserve]` 属性を適用することで、クラス全体を使用するか、個々のメンバーを保持するようにリンカーに指示できます。 アプリケーションで静的にリンクされないメンバーはすべて、削除の対象となります。 この属性はそのため、静的に参照されないが、アプリケーションで必要となるメンバーに印を付けるために利用されます。

たとえば、型を動的にインスタンス化する場合、型の既定のコンストラクターを保存することもできます。 XML シリアル化を利用する場合、型のプロパティを保持することもできます。

この属性は、ある型のすべてのメンバーに適用するか、型自体に適用できます。 型全体を保持する場合、その型で構文 `[Preserve
(AllMembers = true)]` を利用できます。

含まれている型が維持された場合にのみ、特定のメンバーを維持したいことがあります。 その場合、`[Preserve (Conditional=true)]` を使用します。

クロス プラットフォームのポータブル クラス ライブラリ (PCL) をビルドするときなど、Xamarin ライブラリへの依存を希望しない場合でもこの属性を使用できます。

これを行うには、PreserveAttribute クラスを宣言し、それをコードで使用します。次のようになります。

```csharp
public sealed class PreserveAttribute : System.Attribute {
    public bool AllMembers;
    public bool Conditional;
}
```

どの名前空間にこれが定義されるかは問題ではありません。リンカーは型の名前でこの属性を探します。

 <a name="Skipping_Assemblies" />

### <a name="skipping-assemblies"></a>アセンブリをスキップする

リンカー プロセスから除外するアセンブリを指定し、他のアセンブリを通常どおりリンクできます。 このような処理は、一部のアセンブリで `[Preserve]` を利用できない場合に (サードパーティ コードなど)、あるいはバグの一時的な回避策として便利です。

これは、コマンドライン ツールの mtouch を使用するときの `--linkskip` オプションに相当します。

**[すべてのアセンブリをリンクする]** オプションを使用するとき、アセンブリ全体をスキップするようにリンカーに通知する場合、最上位アセンブリの**追加 mtouch 引数**オプションに次を入れます。

```csharp
--linkskip=NameOfAssemblyToSkipWithoutFileExtension
```

リンカーに複数のアセンブリをスキップさせる場合、複数の `linkskip` 引数を含めます。

```csharp
--linkskip=NameOfFirstAssembly --linkskip=NameOfSecondAssembly
```

このオプションを使用するユーザー インターフェイスはありませんが、Visual Studio for Mac の [プロジェクト オプション] ダイアログまたは Visual Studio プロジェクトの [プロパティ] ウィンドウの**追加 mtouch 引数**テキスト フィールドに表示できます。 (例: *--linkskip=mscorlib* は mscorlib.dll をリンクしませんが、ソリューションの他のアセンブリをリンクします)。

<a name="Disabling_Link_Away" />

### <a name="disabling-link-away"></a>"リンク削除" を無効にする

リンカーは、未対応や禁止されているものなど、デバイスで使用される可能性が非常に少ないコードを削除します。 まれに、(動作しているかどうかに関係なく) このようなコードにアプリケーションまたはライブラリが依存することもあります。 Xamarin.iOS 5.0.1 以降、リンカーにこの最適化をスキップするように指示することができます。

これは、コマンドライン ツールの mtouch を使用するときの *-nolinkaway* オプションに相当します。

このオプションを使用するユーザー インターフェイスはありませんが、Visual Studio for Mac の [プロジェクト オプション] ダイアログまたは Visual Studio プロジェクトの [プロパティ] ウィンドウの**追加 mtouch 引数**テキスト フィールドに表示できます。 (例: *--nolinkaway* は余分なコード (約 100 kb) を削除しません)。

### <a name="marking-your-assembly-as-linker-ready"></a>アセンブリをリンカー対応に設定する

ユーザーは SDK アセンブリのリンクだけを選択し、コードにはいかなるリンクも行わないように選択できます。  つまり、Xamarin のコア SDK に含まれないサードパーティ ライブラリはリンクされないことを意味します。

これは通常、ユーザーが自分のコードに `[Preserve]` 属性を手動で追加することを望まない場合に選択されます。  副作用は、サードパーティ ライブラリがリンクされないということです。サードパーティ ライブラリがリンカーにとって使い勝手がよいかどうかはわからないため、一般的にこれは初期設定として適切です。

プロジェクトにライブラリがある場合、あるいは再利用可能なライブラリを開発するとき、アセンブリをリンク可能としてリンカーに処理させる場合、必要な作業は次のようにアセンブリレベルの属性 [`LinkerSafe`](xref:Foundation.LinkerSafeAttribute) を追加することだけです。

```csharp
[assembly:LinkerSafe]
```

このためにライブラリが Xamarin ライブラリを参照する必要は実際にはありません。  たとえば、他のプラットフォームで実行されるポータブル クラス ライブラリをビルドする場合でも `LinkerSafe` 属性を使用できます。
Xamarin リンカーは `LinkerSafe` 属性を実際の型ではなく、名前で探します。  つまり、次のように自分でこのコードを記述しても動作します。

```csharp
[assembly:LinkerSafe]
// ... assembly attribute should be at top, before source
class LinkerSafeAttribute : System.Attribute {}
```

## <a name="custom-linker-configuration"></a>カスタム リンカーの構成

[リンカー構成ファイルを作成する方法](~/cross-platform/deploy-test/linker.md)に従って操作してください。


## <a name="related-links"></a>関連リンク

- [カスタム リンカーの構成](~/cross-platform/deploy-test/linker.md)
- [Mac でのリンク](~/mac/deploy-test/linker.md)
- [Android でのリンク](~/android/deploy-test/linker.md)
