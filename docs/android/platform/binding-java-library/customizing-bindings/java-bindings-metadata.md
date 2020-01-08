---
title: Java バインド メタデータ
description: C#Xamarin のコードは、バインドを使用して Java ライブラリを呼び出します。これは、Java ネイティブインターフェイス (JNI) で指定されている下位レベルの詳細を抽象化するメカニズムです。 Xamarin Android には、これらのバインドを生成するツールが用意されています。 このツールを使用すると、メタデータを使用してバインディングを作成する方法を開発者が制御できます。これにより、名前空間の変更やメンバーの名前変更などの手順が可能になります。 このドキュメントでは、メタデータの動作、メタデータでサポートされる属性の概要、およびこのメタデータを変更してバインディングの問題を解決する方法について説明します。
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 25a5d79084f7caa78eec4011c047bd19a63ef748
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487790"
---
# <a name="java-bindings-metadata"></a>Java バインド メタデータ

_C#Xamarin のコードは、バインドを使用して Java ライブラリを呼び出します。これは、Java ネイティブインターフェイス (JNI) で指定されている下位レベルの詳細を抽象化するメカニズムです。Xamarin Android には、これらのバインドを生成するツールが用意されています。このツールを使用すると、メタデータを使用してバインディングを作成する方法を開発者が制御できます。これにより、名前空間の変更やメンバーの名前変更などの手順が可能になります。このドキュメントでは、メタデータの動作、メタデータでサポートされる属性の概要、およびこのメタデータを変更してバインディングの問題を解決する方法について説明します。_

## <a name="overview"></a>の概要

Xamarin.Android **Java バインディング ライブラリ**とも呼ばれるツールのヘルプで既存の Android ライブラリをバインドするために必要な作業の多くが自動化しようとする、_バインディング ジェネレーター_します。 Java ライブラリをバインドするときに、Xamarin.Android は Java のクラスを検査し、すべてのパッケージ、型、およびメンバーの一覧を生成すると、バインドします。 含まれる XML ファイルにこの API の一覧が格納されている **\{プロジェクト directory}\obj\Release\api.xml** の **リリース** ビルドで **\{プロジェクトdirectory}\obj\Debug\api.xml** の **デバッグ** を構築します。

![Obj/Debug フォルダー内の api .xml ファイルの場所](java-bindings-metadata-images/java-bindings-metadata-01.png)

バインドジェネレーターは、必要なC#ラッパークラスを生成するためのガイドラインとして、 **api .xml**ファイルを使用します。 この XML ファイルの内容は、Google の_Android オープンソースプロジェクト_形式のバリエーションです。
次のスニペットは、 **api .xml**の内容の例です。

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

この例では、 **api .xml**は `java.lang.Object`を拡張する `Manifest` という名前の `android` パッケージ内のクラスを宣言します。

多くの場合、Java API の ".NET と同様" の感覚を持つように、またはバインディングアセンブリがコンパイルされない原因となる問題を修正するために、人間の支援が必要です。 たとえば、Java パッケージ名を .NET 名前空間に変更したり、クラスの名前を変更したり、メソッドの戻り値の型を変更したりすることが必要になる場合があります。

これらの変更は、 **api .xml**を直接変更しても実現できません。
代わりに、変更は、Java バインドライブラリテンプレートによって提供される特殊な XML ファイルに記録されます。 Xamarin Android バインドアセンブリをコンパイルするときに、バインドアセンブリを作成するときに、これらのマッピングファイルによってバインドジェネレーターが影響を受けるようになります。

これらの XML マッピングファイルは、プロジェクトの **[変換]** フォルダーにあります。

- **Xml** &ndash; を使用すると、生成されたバインディングの名前空間を変更するなど、最終的な API に変更を加えることができます。 

- **Enumfields .xml** &ndash; には、Java `int` 定数とC# `enums` 間のマッピングが含まれています。 

- **Enummethods .xml** &ndash; を使用すると、Java `int` 定数から `enums` にC#メソッドのパラメーターと戻り値の型を変更できます。 

**メタデータの .xml**ファイルは、次のような一般的な目的の変更を可能にするため、これらのファイルを最も多くインポートします。

- 名前空間、クラス、メソッド、またはフィールドの名前を変更して、.NET の規則に従うようにします。 

- 不要な名前空間、クラス、メソッド、またはフィールドの削除。 

- クラスを別の名前空間に移動します。 

- バインディングの設計を行うためのサポートクラスを追加して、.NET framework パターンに従います。 

**メタデータ**の詳細については、「」を参照してください。

## <a name="metadataxml-transform-file"></a>Metadata .xml 変換ファイル

既に学習したように、ファイル**メタデータ .xml**はバインドジェネレーターによって使用され、バインディングアセンブリの作成に影響を及ぼします。
メタデータ形式は[XPath](https://www.w3.org/TR/xpath/)構文を使用し、「 [gapi メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)ガイド」で説明されている*gapi メタデータ*とほぼ同じです。 この実装は、ほとんどの場合、XPath 1.0 の完全な実装であるため、1.0 標準の項目をサポートします。 このファイルは、API ファイル内の要素または属性を変更、追加、非表示、または移動するための強力な XPath ベースの機構です。 メタデータ仕様のすべてのルール要素には、ルールが適用されるノードを識別するパス属性が含まれています。 規則は次の順序で適用されます。

- **add-node**&ndash;パス属性で指定されたノードに子ノードを追加します。
- **attr** &ndash;、path 属性によって指定された要素の属性の値を設定します。
- **remove-node**&ndash;指定された XPath と一致するノードを削除します。

次に、**メタデータの .xml**ファイルの例を示します。

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

- `interface` &ndash; Java インターフェイスを見つけるために使用されます。 例を示します: `/interface[@name='AuthListener']`。

- クラスを検索するために使用 &ndash; `class`。 例を示します: `/class[@name='MapView']`。

- `method` &ndash; Java クラスまたはインターフェイスのメソッドを検索するために使用されます。 例を示します: `/class[@name='MapView']/method[@name='setTitleSource']`。

- メソッドのパラメーターを識別 &ndash; `parameter` ます。 例: `/parameter[@name='p0']`

### <a name="adding-types"></a>型の追加

`add-node` 要素は、新しいラッパークラスを**api .xml**に追加するように Xamarin Android プロジェクトに指示します。 たとえば、次のスニペットは、コンストラクターと1つのフィールドを持つクラスを作成するようにバインドジェネレーターに指示します。

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>型の削除

Java の種類を無視してバインドしないように、Xamarin の Android バインドジェネレーターに指示することができます。 これを行うには、`remove-node` XML 要素を**メタデータ .xml**ファイルに追加します。

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>メンバー名の変更

Xamarin Android には元の Java ネイティブインターフェイス (JNI) 名が必要なため、 **api .xml**ファイルを直接編集しても、メンバーの名前を変更することはできません。 したがって、`//class/@name` 属性を変更することはできません。の場合、バインドは機能しません。

`android.Manifest`型の名前を変更する場合を考えてみます。
これを実現するには、 **api .xml**を直接編集し、次のようにクラスの名前を変更します。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

これにより、次C#のラッパークラスのコードがバインドジェネレーターによって作成されます。

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

ラッパークラスの名前が `NewName`に変更されたことに注意してください。元の Java の種類はまだ `Manifest`。 `android.Manifest`上のメソッドにアクセスするために、Xamarin. Android バインドクラスを使用できなくなりました。ラッパークラスは、存在しない Java 型にバインドされています。

ラップされた型 (またはメソッド) のマネージ名を適切に変更するには、次の例に示すように `managedName` 属性を設定する必要があります。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>`EventArg` ラッパークラスの名前変更

Flavoured バインドジェネレーターが_リスナー型_に対して `onXXX` setter メソッドを識別すると、Java C#ベースのリスナーパターン用の .net API をサポートするために、event サブクラスと `EventArgs` サブクラスが生成されます。 例として、次の Java クラスとメソッドについて考えてみます。

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin Android は setter メソッドからプレフィックス `on` を削除し、代わりに `EventArgs` サブクラスの名前の基準として `2DSignNextManuever` を使用します。 サブクラスには次のような名前が付けられます。

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

これは、有効C#なクラス名ではありません。 この問題を解決するには、バインド作成者は `argsType` 属性を使用しC# 、`EventArgs` サブクラスに有効な名前を指定する必要があります。

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>サポートされる属性

次のセクションでは、Java API を変換するための属性について説明します。

### <a name="argstype"></a>argsType

この属性は、Java リスナーをサポートするために生成される `EventArg` サブクラスに名前を指定する setter メソッドに配置されます。 詳細については、このガイドで後述する「 [EventArg ラッパークラスの名前変更](#Renaming_EventArg_Wrapper_Classes)」を参照してください。

### <a name="eventname"></a>eventName

イベントの名前を指定します。 空の場合は、イベントの生成を抑制します。
詳細については、セクション「 [EventArg ラッパークラスの名前変更](#Renaming_EventArg_Wrapper_Classes)」を参照してください。

### <a name="managedname"></a>managedName

これは、パッケージ、クラス、メソッド、またはパラメーターの名前を変更するために使用されます。 たとえば、Java クラスの名前を `MyClass` に変更するには `NewClassName`次のようにします。

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

次の例は、メソッド `java.lang.object.toString` の名前を `Java.Lang.Object.NewManagedName`に変更するための XPath 式を示しています。

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

メソッドの戻り値の型を変更するには、`managedType` を使用します。 場合によっては、バインドジェネレーターが Java メソッドの戻り値の型を誤って推論し、コンパイル時エラーが発生することがあります。 このような状況で考えられる解決策の1つは、メソッドの戻り値の型を変更することです。

たとえば、バインドジェネレーターは、Java メソッド `de.neom.neoreadersdk.resolution.compareTo()` が `int` を返し、パラメーターとして `Object` を受け取る必要があることを認識します。これにより、エラーメッセージ**CS0535: ' DE が返されます。Neom. Neoreadersdk ' は、インターフェイスメンバー ' Java. Lang.ini.... オブジェクト ' を実装していません**。 次のスニペットは、生成されたC#メソッドの最初のパラメーターの型を `DE.Neom.Neoreadersdk.Resolution` から `Java.Lang.Object`に変更する方法を示しています。 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

メソッドの戻り値の型を変更します。 これによって、return 属性が変更されることはありません (戻り属性を変更すると、JNI 署名に互換性のない変更が発生する可能性があります)。 次の例では、`append` メソッドの戻り値の型が `SpannableStringBuilder` から `IAppendable` に変更されC#ています (では、共変の戻り値の型がサポートされていません)。

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>づらい

Java ライブラリを難読化するツールは、Xamarin の Android バインドジェネレーターとラッパークラスを生成C#する機能に干渉する可能性があります。 難読化したクラスの特性は次のとおりです。 

- クラス名には、 **$** 、つまり **$. クラス**が含まれています。
- クラス名は、小文字、つまり **. クラス**で完全に侵害されます。

このスニペットは、"難読化されていない" C#型を生成する方法の例です。

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

この属性は、マネージプロパティの名前を変更するために使用できます。

`propertyName` を使用する特殊なケースとして、Java クラスにフィールドの getter メソッドのみが含まれている場合があります。 このような状況では、バインドジェネレーターは書き込み専用のプロパティを作成します。これは .NET では推奨されません。 次のスニペットは、`propertyName` を空の文字列に設定することによって .NET プロパティを "削除" する方法を示しています。

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Setter メソッドと getter メソッドは、バインドジェネレーターによって引き続き作成されることに注意してください。

### <a name="sender"></a>sender

メソッドがイベントにマップされるときに、メソッドのパラメーターを `sender` パラメーターとして指定する必要があるかどうかを指定します。 値は `true` または `false` です。 例:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>参照可能範囲

この属性は、クラス、メソッド、またはプロパティの可視性を変更するために使用されます。 たとえば、対応C#するラッパーが `public`されるように、`protected` Java メソッドを昇格する必要がある場合があります。

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields .xml および Enumfields .xml

Android ライブラリが整数定数を使用して、ライブラリのプロパティまたはメソッドに渡される状態を表す場合があります。 多くの場合、これらの整数定数をのC#列挙型にバインドすると便利です。 このマッピングを容易にするには、バインドプロジェクトで**Enumfields** .xml ファイルと**enumfields .xml**ファイルを使用します。 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields .xml を使用した列挙型の定義

**Enumfields .xml**ファイルには、Java `int` 定数とC# `enums`間のマッピングが含まれています。 次に、一連の `int` 定数にC#対して作成される列挙型の例を見てみましょう。 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

ここでは、Java クラス `SKRealReachSettings` を取得し、 C#名前空間 `Skobbler.Ngx.Map.RealReach`で `SKMeasurementUnit` と呼ばれる列挙型を定義しました。 `field` エントリは、Java 定数 (例 `UNIT_SECOND`) の名前、列挙エントリの名前 (例 `Second`)、および両方のエンティティによって表される整数値 (例 `0`) を定義します。 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods を使用した Getter メソッドと Setter メソッドの定義

**Enummethods .xml**ファイルを使用すると、メソッドのパラメーターと戻り値の型をC# Java `int` 定数から `enums`に変更できます。 言い換えると、列挙**型 (enumfields .xml**ファイルでC#定義されている) の読み取りと書き込みを Java `int` 定数 `get` および `set` メソッドにマップします。

上で定義された `SKRealReachSettings` 列挙型の場合、次の**enummethods**ファイルはこの列挙型の getter/setter を定義します。

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

最初の `method` 行は、Java `getMeasurementUnit` メソッドの戻り値を `SKMeasurementUnit` 列挙型にマップします。 2番目の `method` 行は、`setMeasurementUnit` の最初のパラメーターを同じ列挙型にマップします。

これらの変更をすべて適用したら、Xamarin. Android の次のコードを使用して、`MeasurementUnit`を設定できます。 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>要約

この記事では、Xamarin Android がメタデータを使用して、API 定義を*Google*のデータ*形式*から変換する方法について説明しました。 *メタデータ*を使用して可能な変更について説明した後、メンバーの名前を変更するときに発生する制限を調べ、各属性を使用するタイミングについて説明します。

## <a name="related-links"></a>関連リンク

- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
