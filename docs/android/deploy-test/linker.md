---
title: Android でのリンク
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2018
ms.openlocfilehash: e5f494c2f41500b660bf333e7c63f0120536f52a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753841"
---
# <a name="linking-on-android"></a>Android でのリンク

Xamarin.Android アプリケーションは、"*リンカー*" を使ってアプリケーションのサイズを小さくします。 リンカーは、アプリケーションのスタティック分析を利用して、実際に使われているアセンブリ、型、メンバーを判別します。 その後、リンカーは "*ガベージ コレクター*" のように動作して、参照されているアセンブリ、型、メンバーのクロージャ全体が見つかるまで、参照されているアセンブリ、型、メンバーを継続的に検索します。 そして、このクロージャの外部にあるものはすべて "*破棄*" されます。

たとえば、[Hello, Android](https://docs.microsoft.com/samples/xamarin/monodroid-samples/hellom4a) の例を次に示します。

|構成|1.2.0 のサイズ|4.0.1 のサイズ|
|---|---|---|
|リンクなしのリリース:|14.0 MB|16.0 MB|
|リンクありのリリース:|4.2 MB|2.9 MB|

リンクの結果、1.2.0 ではパッケージのサイズが元の (リンクされていない) パッケージの 30% に、4.0.1 では 18% になります。

## <a name="control"></a>Control

リンクは "*スタティック分析*" に基づいています。 したがって、ランタイム環境に依存するものはすべて検出されません。

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```

### <a name="linker-behavior"></a>リンカーの動作

リンカーを制御する主要なメカニズムは、 **[プロジェクト オプション]** ダイアログ ボックスの **[リンカーの動作]** (Visual Studio では *[リンク]* ) ドロップダウンです。 次の 3 つのオプションがあります。

1. **リンクしない** (Visual Studio では "*なし*")
1. **SDK アセンブリのみをリンクする** ("*SDK アセンブリのみ*")
1. **すべてのアセンブリをリンクする** ("*SDK およびユーザー アセンブリ*")

**[リンクしない]** オプションは、リンカーをオフにします。上の "リンクなしのリリース" アプリケーション サイズの例では、この動作を使いました。 このオプションは、実行時の障害をトラブルシューティングする場合、およびリンカーが応答しているかどうかを確認する場合に役立ちます。 通常、運用環境のビルドにはこの設定は推奨されません。

**[SDK アセンブリのみをリンクする]** オプションは、[Xamarin.Android に付属するアセンブリ](~/cross-platform/internals/available-assemblies.md)のみをリンクします。
他のすべてのアセンブリ (コードなど) はリンクされません。

**[すべてのアセンブリをリンクする]** オプションはすべてのアセンブリをリンクし、静的参照が存在しない場合はユーザーのコードも削除される可能性があることを意味します。

上の例は、 *[リンクしない]* オプションと *[SDK アセンブリのみをリンクする]* オプションでは動作しますが、 *[すべてのアセンブリをリンクする]* 動作では失敗して、次のようなエラーが発生します。

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```

### <a name="preserving-code"></a>コードの維持

残しておきたいコードがリンカーによって削除される場合があります。 次に例を示します。

- `System.Reflection.MemberInfo.Invoke` を使って直接呼び出しているコードがある場合。

- 型を動的にインスタンス化する場合、型の既定のコンストラクターを維持したい場合があります。

- XML シリアル化を利用する場合は、型のプロパティを保持することができます。

このような場合は、[Android.Runtime.Preserve](xref:Android.Runtime.PreserveAttribute) 属性を使うことができます。 アプリケーションによって静的にリンクされていないすべてのメンバーは削除の対象になるため、この属性を使って、静的に参照されていないが、アプリケーションにとって必要であるメンバーをマークすることができます。 この属性は、ある型のすべてのメンバー、または型自体に適用できます。

次の例では、この属性を使って `Example` クラスのコンストラクターを維持しています。

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

型全体を維持する場合は、次の属性構文を使うことができます。

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

たとえば、次のコード フラグメントでは、`Example` クラス全体が XML シリアル化に対して維持されています。

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

含まれている型が維持された場合にのみ、特定のメンバーを維持したいことがあります。 そのような場合は、次の属性構文を使います。

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Xamarin ライブラリに依存したくない場合 (クロス プラットフォームのポータブル クラス ライブラリ (PCL) を作成している場合など) でも、`Android.Runtime.Preserve` 属性を使うことができます。 そのためには、次のように `Android.Runtime` 名前空間内で `PreserveAttribute` クラスを宣言します。

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```

上記の例で、`Preserve` 属性は `Android.Runtime` 名前空間で宣言されています。ただし、リンカーは型名でこの属性を検索するため、`Preserve` 属性はどの名前空間でも使用することができます。

### <a name="falseflag"></a>falseflag

[Preserve] 属性を使うことができない場合は、型が使われていることをリンカーに判断させながら、実行時には実行されないようなコード ブロックを提供すると役に立つことがよくあります。 この手法を使うには次のようにします。

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```

### <a name="linkskip"></a>linkskip

[AndroidLinkSkip MSBuild プロパティ](~/android/deploy-test/building-apps/build-process.md)を使うことにより、ユーザー提供のアセンブリのセットがまったくリンクされないように指定しながら、他のユーザー アセンブリを "*SDK アセンブリのみをリンクする*" 動作でスキップさせることができます。

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```

### <a name="linkdescription"></a>LinkDescription

[カスタム リンカー構成ファイル](~/cross-platform/deploy-test/linker.md)を含むことができるファイルに対して、[`@(LinkDescription)`](~/android/deploy-test/building-apps/build-process.md)
**ビルド アクション**が使われる場合があります。
ファイルの SSDL セクションに次の  要素を追加します。 維持する必要がある `internal` または `private` メンバーを維持するには、カスタム リンカー構成ファイルが必要な場合があります。

### <a name="custom-attributes"></a>カスタム属性

アセンブリがリンクされるとき、次のカスタム属性の型はすべてのメンバーから削除されます。

- System.ObsoleteAttribute
- System.MonoDocumentationNoteAttribute
- System.MonoExtensionAttribute
- System.MonoInternalNoteAttribute
- System.MonoLimitationAttribute
- System.MonoNotSupportedAttribute
- System.MonoTODOAttribute
- System.Xml.MonoFIXAttribute

アセンブリがリンクされるとき、次のカスタム属性の型はリリース ビルドのすべてのメンバーから削除されます。

- System.Diagnostics.DebuggableAttribute
- System.Diagnostics.DebuggerBrowsableAttribute
- System.Diagnostics.DebuggerDisplayAttribute
- System.Diagnostics.DebuggerHiddenAttribute
- System.Diagnostics.DebuggerNonUserCodeAttribute
- System.Diagnostics.DebuggerStepperBoundaryAttribute
- System.Diagnostics.DebuggerStepThroughAttribute
- System.Diagnostics.DebuggerTypeProxyAttribute
- System.Diagnostics.DebuggerVisualizerAttribute

## <a name="related-links"></a>関連リンク

- [カスタム リンカーの構成](~/cross-platform/deploy-test/linker.md)
- [iOS でのリンク](~/ios/deploy-test/linker.md)
