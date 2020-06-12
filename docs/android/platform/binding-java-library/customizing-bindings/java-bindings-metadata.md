---
title: Java バインド メタデータ
description: Xamarin.Android の C# コードでは、バインドを使用して Java ライブラリを呼び出します。これは、Java ネイティブ インターフェイス (JNI) で指定されている下位レベルの詳細を抽象化するメカニズムです。 Xamarin.Android には、これらのバインドを生成するツールが用意されています。 このツールを使用すると、メタデータを使用してバインドを作成する方法を開発者が制御できます。これにより、名前空間の変更やメンバーの名前変更などの手順が可能になります。 このドキュメントでは、メタデータの動作、メタデータでサポートされる属性の概要、およびこのメタデータを変更してバインドの問題を解決する方法について説明します。
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 2a88888b2306589930ad6386fb69bbd3b48924b7
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571377"
---
# <a name="java-bindings-metadata"></a>Java バインド メタデータ

_Xamarin.Android の C# コードでは、バインドを使用して Java ライブラリを呼び出します。これは、Java ネイティブ インターフェイス (JNI) で指定されている下位レベルの詳細を抽象化するメカニズムです。Xamarin.Android には、これらのバインドを生成するツールが用意されています。このツールを使用すると、メタデータを使用してバインドを作成する方法を開発者が制御できます。これにより、名前空間の変更やメンバーの名前変更などの手順が可能になります。このドキュメントでは、メタデータの動作、メタデータでサポートされる属性の概要、およびこのメタデータを変更してバインドの問題を解決する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Android の **Java バインド ライブラリ**は、既存の Android ライブラリをバインドするために必要な作業の大部分を、"_バインド ジェネレーター_" と呼ばれるツールを使用して自動化しようします。 Java ライブラリをバインドすると、Xamarin.Android は Java クラスを検査し、バインドするすべてのパッケージ、型、およびメンバーの一覧を生成します。 この API の一覧は XML ファイルに格納されます。これは、**リリース** ビルドの場合は **\{project directory}\obj\Release\api.xml** にあり、**デバッグ** ビルドの場合は **\{project directory}\obj\Debug\api.xml** にあります。

![obj/Debug フォルダー内の api.xml ファイルの場所](java-bindings-metadata-images/java-bindings-metadata-01.png)

バインド ジェネレーターでは、必要な C# ラッパー クラスを生成するためのガイドラインとして、**api.xml** ファイルが使用されます。 この XML ファイルの内容は、Google の "_Android オープンソース プロジェクト_" 形式のバリエーションです。
以下のスニペットは、**api.xml** の内容の例です。

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

この例では、**api.xml** で、`java.lang.Object` を拡張する `Manifest` という名前の `android` パッケージ内のクラスを宣言しています。

多くの場合、Java API が ".NET と同様" に感じられるようにするために、またはバインド アセンブリがコンパイルされない問題を修正するために、人間の介入が必要です。 たとえば、Java パッケージ名を .NET 名前空間に変更したり、クラスの名前を変更したり、メソッドの戻り値の型を変更したりすることが必要になる場合があります。

これらの変更は、**api.xml** を直接変更しても実現されません。
代わりに、変更は、Java バインド ライブラリ テンプレートによって提供される特殊な XML ファイルに記録されます。 Xamarin.Android バインド アセンブリをコンパイルするときに、バインド ジェネレーターは、バインド アセンブリを作成するときにこれらのマッピング ファイルの影響を受けます。

これらの XML マッピング ファイルは、プロジェクトの **Transforms** フォルダーにある場合があります。

- **MetaData.xml** &ndash; 生成されたバインドの名前空間の変更など、最終的な API に変更を加えることができます。 

- **EnumFields.xml** &ndash; Java の `int` 定数と C# の `enums` の間のマッピングが含まれています。 

- **EnumMethods.xml** &ndash; メソッドのパラメーターと戻り値の型を Java の `int` 定数から C# の `enums` に変更できます。 

**MetaData.xml** ファイルは、バインドに対して次のような汎用的な変更を可能にするため、これらのファイルの中で最も重要です。

- 名前空間、クラス、メソッド、またはフィールドの名前を変更して、.NET の規則に従うようにする。 

- 不要な名前空間、クラス、メソッド、またはフィールドを削除する。 

- クラスを別の名前空間に移動する。 

- バインドの設計が .NET のフレームワーク パターンに従うように追加のサポート クラスを追加する。 

**Metadata.xml** について詳しく説明します。

## <a name="metadataxml-transform-file"></a>Metadata.xml 変換ファイル

既に説明したように、ファイル **Metadata.xml** はバインド ジェネレーターによって使用され、バインド アセンブリの作成に影響を及ぼします。
メタデータ形式では [XPath](https://www.w3.org/TR/xpath/) 構文が使用され、[GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) ガイドで説明されている *GAPI メタデータ*とほぼ同じです。 この実装は、XPath 1.0 のほぼ完全な実装であるため、1.0 標準の項目をサポートします。 このファイルは、API ファイル内の要素または属性を変更、追加、非表示、または移動するための強力な XPath ベースのメカニズムです。 メタデータ仕様のすべてのルール要素には、ルールが適用されるノードを識別するパス属性が含まれています。 ルールは以下の順序で適用されます。

- **add-node** &ndash; path 属性で指定されたノードに子ノードを追加します。
- **attr** &ndash; path 属性で指定された要素の属性の値を設定します。
- **remove-node** &ndash; 指定した XPath に一致するノードを削除します。

次に、**Metadata.xml** ファイルの例を示します。

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

次に、Java API の一般的に使用される XPath 要素の一部を示します。

- `interface` &ndash; Java インターフェイスを検索するために使用されます。 例を示します: `/interface[@name='AuthListener']`。

- `class` &ndash; クラスを検索するために使用されます。 例を示します: `/class[@name='MapView']`。

- `method` &ndash; Java クラスまたはインターフェイスのメソッドを検索するために使用されます。 例を示します: `/class[@name='MapView']/method[@name='setTitleSource']`。

- `parameter` &ndash; メソッドのパラメーターを指定します。 例: `/parameter[@name='p0']`

### <a name="adding-types"></a>型の追加

`add-node` 要素は、**api.xml** に新しいラッパー クラスを追加するように Xamarin.Android バインド プロジェクトに指示します。 たとえば、次のスニペットは、コンストラクターと 1 つのフィールドを持つクラスを作成するようにバインド ジェネレーターに指示します。

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>型の削除

Java の型を無視してバインドしないように、Xamarin.Android のバインド ジェネレーターに指示することができます。 これを行うには、`remove-node` XML 要素を **metadata.xml** ファイルに追加します。

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>メンバー名の変更

**api.xml** ファイルを直接編集しても、メンバーの名前を変更することはできません。Xamarin.Android では、元の Java ネイティブ インターフェイス (JNI) 名が必要になるためです。 したがって、`//class/@name` 属性を変更することはできません。変更すると、バインドは機能しません。

`android.Manifest` 型の名前を変更する場合を考えてみます。
これを実現するために、**api.xml** を直接編集し、次のようにクラスの名前を変更するとします。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

これにより、ラッパー クラス用の次の C# コードがバインド ジェネレーターによって作成されます。

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

ラッパー クラスの名前が `NewName` に変更されたことに注意してください。元の Java 型は `Manifest` のままです。 Xamarin.Android バインド クラスで `android.Manifest` 上のメソッドにアクセスできなくなりました。ラッパー クラスが存在しない Java 型にバインドされているためです。

ラップされた型 (またはメソッド) のマネージド名を適切に変更するには、次の例に示すように `managedName` 属性を設定する必要があります。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes"></a>

#### <a name="renaming-eventarg-wrapper-classes"></a>`EventArg` ラッパー クラスの名前を変更する

Xamarin.Android バインド ジェネレーターが "_リスナー型_" に対して `onXXX` セッター メソッドを識別すると、Java ベースのリスナー パターン用の .NET 形式の API をサポートするために、C# イベントと `EventArgs` サブクラスが生成されます。 例として、次の Java クラスとメソッドを考えます。

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android では、セッター メソッドからプレフィックス `on` が削除され、代わりに `EventArgs` サブクラスの名前の基準として `2DSignNextManuever` が使用されます。 サブクラスには次のような名前が付けられます。

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

これは、有効な C# クラス名ではありません。 この問題を解決するには、バインド作成者は `argsType` 属性を使用し、`EventArgs` サブクラスに有効な C# の名前を指定する必要があります。

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>サポート対象属性

以下のセクションでは、Java API を変換するためのいくつかの属性について説明します。

### <a name="argstype"></a>argsType

この属性は、Java リスナーをサポートするために生成される `EventArg` サブクラスに名前を指定するセッター メソッドに配置されます。 詳細については、このガイドで後述する「[EventArg ラッパー クラスの名前を変更する](#Renaming_EventArg_Wrapper_Classes)」を参照してください。

### <a name="eventname"></a>eventName

イベントの名前を指定します。 空の場合、イベントの生成が抑制されます。
詳細については、「[EventArg ラッパー クラスの名前を変更する](#Renaming_EventArg_Wrapper_Classes)」を参照してください。

### <a name="managedname"></a>managedName

これは、パッケージ、クラス、メソッド、またはパラメーターの名前を変更するために使用されます。 たとえば、Java クラスの名前を `MyClass` から `NewClassName` に変更するには、次のようにします。

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

次の例は、メソッド `java.lang.object.toString` の名前を `Java.Lang.Object.NewManagedName` に変更するための XPath 式を示しています。

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` は、メソッドの戻り値の型を変更するために使用されます。 場合によっては、バインド ジェネレーターが Java メソッドの戻り値の型を誤って推論し、コンパイル時エラーが発生することがあります。 このような状況で考えられる解決策の 1 つは、メソッドの戻り値の型を変更することです。

たとえば、バインド ジェネレーターによって、Java メソッド `de.neom.neoreadersdk.resolution.compareTo()` は `int` を返し、パラメーターとして `Object` を受け取ると推論されたとします。この場合、次のエラー メッセージが発生します: **エラー CS0535:'DE.Neom.Neoreadersdk.Resolution' はインターフェイス メンバー 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' を実装しません**。 次のスニペットは、生成された C# メソッドの最初のパラメーターの型を `DE.Neom.Neoreadersdk.Resolution` から `Java.Lang.Object` に変更する方法を示しています。 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

メソッドの戻り値の型を変更します。 これによって、戻り属性が変更されることはありません (戻り属性を変更すると、JNI 署名に互換性のない変更が発生する可能性があるためです)。 次の例では、`append` メソッドの戻り値の型が `SpannableStringBuilder` から `IAppendable` に変更されています (C# では、共変の戻り値の型がサポートされていません)。

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obfuscated

Java ライブラリを難読化するツールは、Xamarin.Android のバインド ジェネレーターおよび C# ラッパー クラスを生成する機能に干渉する可能性があります。 難読化したクラスの特性は次のとおりです。 

- クラス名に **$** が含まれ、**a$.class** のようになります
- クラス名は、**a.class** のように小文字の場合は完全に危険にさらされています。

このスニペットは、"難読化されていない" C# 型を生成する方法の例です。

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

この属性は、マネージド プロパティの名前を変更するために使用できます。

`propertyName` を使用する特殊なケースとして、Java クラスにフィールドのゲッター メソッドのみが含まれている場合があります。 このような状況では、バインド ジェネレーターは書き込み専用のプロパティを作成する必要がありますが、これは .NET では推奨されません。 次のスニペットは、`propertyName` を空の文字列に設定することによって .NET プロパティを "削除" する方法を示しています。

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

セッターおよびゲッター メソッドは、バインド ジェネレーターによって引き続き作成されることに注意してください。

### <a name="sender"></a>sender

メソッドがイベントにマップされるときに、メソッドのどのパラメーターを `sender` パラメーターにするかを指定します。 値は `true` または `false` です。 次に例を示します。

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>参照可能範囲

この属性は、クラス、メソッド、またはプロパティの可視性を変更するために使用されます。 たとえば、対応する C# ラッパーが `public` になるように、`protected` Java メソッドを昇格する必要がある場合があります。

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml および EnumMethods.xml

Android ライブラリでは、整数定数を使用して、ライブラリのプロパティまたはメソッドに渡される状態を表す場合があります。 多くの場合、これらの整数定数をの C# の列挙型にバインドすると便利です。 このマッピングを容易にするには、バインド プロジェクトで **EnumFields.xml** と **EnumMethods.xml**ファイルを使用します。 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields.xml を使用した列挙型の定義

**EnumFields.xml** ファイルには、Java の `int` 定数と C# の `enums` の間のマッピングが含まれています。 次に、一連の `int` 定数に対して作成される C# の列挙型の例を見てみましょう。 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

ここでは、Java クラス `SKRealReachSettings` を使用し、名前空間 `Skobbler.Ngx.Map.RealReach` で `SKMeasurementUnit` という名前の C# 列挙型を定義しました。 `field` エントリには、Java 定数の名前 (例: `UNIT_SECOND`)、列挙エントリの名前 (例: `Second`)、および両方のエンティティによって表される整数値 (例: `0`) が定義されています。 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods.xml を使用したゲッター/セッター メソッドの定義

**EnumMethods.xml** ファイルでは、メソッドのパラメーターと戻り値の型を Java の `int` 定数から C# の `enums` に変更できます。 言い換えると、C# 列挙型 (**EnumFields.xml** ファイルで定義されている) の読み取りと書き込みを、Java の `int` 定数の `get` と `set` メソッドにマップします。

上で定義されている `SKRealReachSettings` 列挙型を使用すると、次の **EnumMethods.xml** ファイルにより、この列挙型のゲッター/セッターが定義されます。

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

最初の `method` 行は、Java の `getMeasurementUnit` メソッドの戻り値を `SKMeasurementUnit` 列挙型にマップします。 2 番目の `method` 行は、`setMeasurementUnit` の最初のパラメーターを同じ列挙型にマップします。

これらの変更をすべて設定すると、Xamarin.Android で次のコードを使用して、`MeasurementUnit` を設定できます。 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>まとめ

この記事では、Xamarin.Android でメタデータを使用して、API 定義を "*Google* *AOSP 形式*" から変換する方法について説明しました。 *Metadata.xml* を使用して可能な変更について説明した後は、メンバーの名前を変更するときに発生する制限を確認し、サポートされている XML 属性の一覧と、各属性を使用するタイミングを示しました。

## <a name="related-links"></a>関連リンク

- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
