---
title: Java バインド メタデータ
description: C#Xamarin.Android でのコードでは、バインドは、Java ネイティブ インターフェイス (JNI) で指定された低レベルの詳細を抽象化するメカニズムを Java ライブラリを呼び出します。 Xamarin.Android では、これらのバインドを生成するツールを提供します。 このツールには、開発者のコントロールにより、プロシージャは名前空間を変更して、メンバーの名前を変更するなどのメタデータを使用して、バインディングを作成する方法ことができます。 このドキュメントのメタデータの動作方法について説明します、メタデータを属性をまとめたものをサポートしているし、このメタデータの変更によるバインドの問題を解決する方法について説明します。
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: ce9bf0293b846299cc7cd06773ce936f725715fa
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669896"
---
# <a name="java-bindings-metadata"></a>Java バインド メタデータ

_C#Xamarin.Android でのコードでは、バインドは、Java ネイティブ インターフェイス (JNI) で指定された低レベルの詳細を抽象化するメカニズムを Java ライブラリを呼び出します。Xamarin.Android では、これらのバインドを生成するツールを提供します。このツールには、開発者のコントロールにより、プロシージャは名前空間を変更して、メンバーの名前を変更するなどのメタデータを使用して、バインディングを作成する方法ことができます。このドキュメントのメタデータの動作方法について説明します、メタデータを属性をまとめたものをサポートしているし、このメタデータの変更によるバインドの問題を解決する方法について説明します。_


## <a name="overview"></a>概要

Xamarin.Android **Java バインディング ライブラリ**とも呼ばれるツールのヘルプで既存の Android ライブラリをバインドするために必要な作業の多くが自動化しようとする、_バインディング ジェネレーター_します。 Java ライブラリをバインドするときに、Xamarin.Android は Java のクラスを検査し、すべてのパッケージ、型、およびメンバーの一覧を生成すると、バインドします。 含まれる XML ファイルにこの Api の一覧が格納されている **\{プロジェクト directory}\obj\Release\api.xml** の **リリース** ビルドで **\{プロジェクトdirectory}\obj\Debug\api.xml** の **デバッグ** を構築します。

![Obj/デバッグ フォルダーで api.xml ファイルの場所](java-bindings-metadata-images/java-bindings-metadata-01.png)

バインディング ジェネレーターを使用して、 **api.xml**をガイドラインとして、必要なを生成するためのファイルC#ラッパー クラス。 この XML ファイルの内容は、Google のバリエーション_Android オープン ソース プロジェクト_形式。
内容の例を次のスニペットに示します**api.xml**:

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

この例で**api.xml**でクラスを宣言して、`android`という名前のパッケージ`Manifest`まで、`java.lang.Object`します。

多くの場合、人間のアシスタンスは、以上"のような .NET"と思われる Java API を作成するかをコンパイルからバインド アセンブリを妨げる問題を修正する必要があります。 たとえば、.NET 名前空間への Java のパッケージ名の変更、クラスやメソッドの戻り値の型を変更するために必要な場合があります。

これらの変更が変更することで達成できない**api.xml**直接します。
代わりに、変更は、Java バインド ライブラリ テンプレートによって提供される特別な XML ファイルに記録されます。 バインドのアセンブリを作成するときにバインディング ジェネレーターこれらのマッピング ファイルが反映される Xamarin.Android バインド アセンブリをコンパイルするときに

これらの XML マッピング ファイルが存在する可能性があります、**変換**プロジェクトのフォルダー。

-   **MetaData.xml** &ndash;生成されたバインディングの名前空間を変更するなど、最終的な API に対する変更を実行できます。 

-   **EnumFields.xml** &ndash; Java の間のマッピングを含む`int`定数とC#`enums`します。 

-   **EnumMethods.xml** &ndash;メソッドのパラメーターと戻り値の型を Java から変更を許可する`int`に定数をC#`enums`します。 

**MetaData.xml**など、バインドに汎用的な変更が許可されている、ファイルがこれらのファイルのインポート、最も。

-   .NET の規則に従うように名前空間、クラス、メソッド、またはフィールドの名前変更します。 

-   名前空間、クラス、メソッド、または必要のないフィールドを削除しています。 

-   クラスは、異なる名前空間に移動します。 

-   バインディングのデザインに追加するその他のサポート クラスでは、.NET framework のパターンに従います。 

説明に進むことができます。 **Metadata.xml**で詳しく説明します。


## <a name="metadataxml-transform-file"></a>Metadata.xml 変換ファイル

既に学習した、ファイルと**Metadata.xml**バインド アセンブリの作成に影響するバインディング ジェネレーターによって使用されます。
メタデータ形式を使用して[XPath](https://www.w3.org/TR/xpath/)構文とほぼ同一である、 *GAPI メタデータ*で説明されている[GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)ガイド。 この実装では、XPath 1.0 のほぼ完全な実装ですし、したがって 1.0 標準で項目をサポートします。 このファイルは、変更、追加、非表示にする、または API ファイルで任意の要素または属性に移動する強力な XPath ベースのメカニズムです。 すべてのメタデータの仕様の規則要素には、ルールが適用されるノードを識別するパス属性が含まれます。 ルールは、次の順序で適用されます。

* **ノードの追加**&ndash;パス属性で指定されたノードに子ノードを追加します。
* **attr** &ndash;パス属性で指定された要素の属性の値を設定します。
* **ノードを削除**&ndash;指定された XPath と一致するノードを削除します。

例を次に、 **Metadata.xml**ファイル。

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

Java api の次の一般的に使用される XPath 要素のいくつか示します。

-   `interface` &ndash; Java インターフェイスを検索するために使用します。 例:`/interface[@name='AuthListener']`します。

-   `class` &ndash; クラスを特定するために使用します。 例:`/class[@name='MapView']`します。

-   `method` &ndash; Java のクラスまたはインターフェイスのメソッドを検索するために使用します。 例:`/class[@name='MapView']/method[@name='setTitleSource']`します。

-   `parameter` &ndash; メソッドのパラメーターを特定します。 例。 `/parameter[@name='p0']`



### <a name="adding-types"></a>型を追加します。

`add-node`要素を新しいラッパー クラスを追加する Xamarin.Android バインド プロジェクト教えてくれる**api.xml**します。 たとえば、次のスニペットでは、コンス トラクターと 1 つのフィールドを持つクラスを作成するバインド ジェネレーターを指示します。

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>型の削除

Java 型を無視し、それをバインドする Xamarin.Android バインド ジェネレーターに指示することになります。 これは、追加することで、 `remove-node` XML 要素を**metadata.xml**ファイル。

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>メンバーの名前を変更します。

直接編集をメンバーの名前を変更することはできませんが、 **api.xml** Xamarin.Android が元の Java ネイティブ インターフェイス (JNI) 名を必要とするためのファイルします。 そのため、`//class/@name`属性は変更できません。 以外である場合、バインドは機能しません。

型の名前を変更する場合を考えてみます`android.Manifest`します。
これを実現する可能性がありますしようを直接編集**api.xml**とクラスの名前を変更するようになります。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

これは、結果、次を作成するバインディング ジェネレーターC#ラッパー クラスのコード。

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

ラッパー クラスの名前は通知`NewName`、元の Java 型は、まだ`Manifest`します。 Xamarin.Android のバインディング クラスのすべてのメソッドにアクセスすることができなくなった`android.Manifest`; ラッパー クラスが存在しない Java 型にバインドします。

適切にラップされた型 (またはメソッド) の管理対象の名前を変更するには設定するために必要な`managedName`この例で示すように属性します。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>名前を変更する`EventArg`ラッパー クラス

Xamarin.Android バインド ジェネレーターを識別すると、`onXXX`の setter メソッドを_リスナーの種類_、C#イベントと`EventArgs`サブクラスが生成されます、.NET をサポートするには、Java ベースの API を flavouredリスナーのパターン。 たとえば、次の Java クラスとメソッドを検討してください。

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android は、プレフィックスをドロップ`on`set アクセス操作子メソッドから、代わりに使用`2DSignNextManuever`の名前を基準として、`EventArgs`サブクラスです。 サブクラスは、以下のように名前は。

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

これは、法律ではありませんC#クラス名。 この問題を修正するバインディングの作成者が使用する必要があります、`argsType`属性し、有効な提供C#の名前、`EventArgs`サブクラスです。
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>サポートされている属性

次のセクションでは、Java Api を変換するための属性について説明します。

### <a name="argstype"></a>argsType

この属性は名前を付けるの setter メソッドに配置されます、 `EventArg` Java リスナーをサポートするために生成されるサブクラスです。 これはセクションで以下の詳細に説明されている[EventArg ラッパー クラスの名前を変更する](#Renaming_EventArg_Wrapper_Classes)このガイドで後でします。

### <a name="eventname"></a>eventName

イベントの名前を指定します。 空の場合、イベントの生成を抑制します。
セクション タイトルにさらに詳しく記載されてこの[EventArg ラッパー クラスの名前を変更する](#Renaming_EventArg_Wrapper_Classes)します。

### <a name="managedname"></a>managedName

これを使用して、パッケージ、クラス、メソッド、またはパラメーターの名前を変更できます。 たとえば、Java クラスの名前を変更する`MyClass`に`NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

次の例は、メソッドの名前を変更する XPath 式を示しています`java.lang.object.toString`に`Java.Lang.Object.NewManagedName`:。

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` メソッドの戻り値の型の変更に使用されます。 状況によってはバインディング ジェネレーターが正しく推測、コンパイル時エラーになります、Java のメソッドの戻り値の型。 このような状況でソリューションの 1 つでは、メソッドの戻り値の型を変更します。

たとえば、バインディング ジェネレーターは Java メソッド`de.neom.neoreadersdk.resolution.compareTo()`返す必要があります、 `int`、その結果は、エラー メッセージに**エラー CS0535:' DE します。Neom.Neoreadersdk.Resolution' はインターフェイス メンバー 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' を実装していない**します。 次のスニペットは、生成された戻り値の型を変更する方法を示しますC#からメソッド、`int`を`Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

メソッドの戻り値の型を変更します。 (属性には互換性のない変更で JNI の署名により返される変更) としては、戻り値の属性変更されません。 次の例では、戻り値の型の`append`メソッドがから変更され`SpannableStringBuilder`に`IAppendable`(することを思い出してくださいC#は共変の戻り値の型をサポートしていません)。

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>難読化

Java ライブラリを難読化ツールは、Xamarin.Android バインド ジェネレーターおよび生成するには、その機能を妨げる可能性がC#ラッパー クラス。 難読化されたクラスの特性が含まれます * クラス名が含まれています、 **$**、つまり **$.class** * クラス名がつまり侵害の小文字の場合は、完全 **a.class**

このスニペットを生成する方法の例は、「難読化されていない」、C#型。

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

この属性は、管理対象のプロパティの名前を変更するのには使用できます。

使用する特殊なケース`propertyName`Java クラスのフィールドの getter メソッドのみがある状況が含まれます。 このような状況で、バインド ジェネレーターは、書き込み専用プロパティ、.NET で勧めするものを作成します。 次のスニペットを設定して、.NET プロパティを「削除」する方法を示しています、`propertyName`に空の文字列。

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

まだバインディング ジェネレーターによって、setter と getter メソッドを作成することに注意してください。

### <a name="sender"></a>sender

メソッドのパラメーターにする必要がありますを指定します、`sender`パラメーター、メソッドがイベントにマップされている場合。 値を指定できます`true`または`false`します。 例えば:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>参照可能範囲

この属性は、クラス、メソッド、またはプロパティの可視性の変更に使用されます。 たとえば、ある可能性がありますに昇格するために必要な`protected`その it の対応するための Java メソッドC#ラッパーが`public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml と EnumMethods.xml

Android ライブラリが、ライブラリのプロパティまたはメソッドに渡される状態を表す整数の定数を使用する場合があります。 多くの場合、これらの整数定数で列挙型にバインドすると便利ですC#します。 このマッピングを容易にするには使用、 **EnumFields.xml**と**EnumMethods.xml**バインド プロジェクト内のファイル。 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields.xml を使用して列挙型の定義

**EnumFields.xml**ファイルには、Java の間のマッピングが含まれています。`int`定数とC#`enums`します。 次の例を見てみましょう、C#列挙型のセットの作成中`int`定数。 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

ここで、Java クラスを思い出させて`SKRealReachSettings`定義されていると、C#列挙型と呼ばれる`SKMeasurementUnit`名前空間に`Skobbler.Ngx.Map.RealReach`します。 `field`エントリ Java 定数の名前を定義します (例`UNIT_SECOND`)、列挙型のエントリの名前 (例`Second`) と両方のエンティティによって表される整数値 (例`0`)。 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods.xml を使用して、Getter と Setter メソッドを定義します。

**EnumMethods.xml**メソッドのパラメーターと戻り値の型を変更する Java からファイルを使用できます`int`に定数をC#`enums`します。 マップつまり、読み取りと書き込みのC#列挙型 (で定義されている、 **EnumFields.xml**ファイル) Java から`int`定数`get`と`set`メソッド。

指定された、`SKRealReachSettings`列挙型の定義上、次**EnumMethods.xml**ファイルはこの列挙型の getter と setter を定義します。

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

最初の`method`行は、Java の戻り値をマップ`getMeasurementUnit`メソッドを`SKMeasurementUnit`列挙型。 2 番目の`method`行の最初のパラメーターのマップ、`setMeasurementUnit`同じ列挙型にします。

すべての変更を行うには、することができますを使用して、次のコード Xamarin.Android で設定する、 `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>まとめ

この記事では、Xamarin.Android でメタデータを使用して、API 定義を変換する方法について説明、 *Google* *AOSP 形式*します。 可能な変更の説明の後を使用して*Metadata.xml*メンバーの名前を変更するときに発生する制限を検査、および各属性を使用するときに説明する、XML のサポートされている属性の一覧を表示します。



## <a name="related-links"></a>関連リンク

- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [GAPI メタデータ](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
