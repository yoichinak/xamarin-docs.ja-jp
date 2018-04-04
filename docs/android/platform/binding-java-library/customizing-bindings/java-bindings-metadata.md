---
title: Java バインディング メタデータ
description: Xamarin.Android で c# コードでは、Java ネイティブ インターフェイス (JNI) で指定されている低レベルの詳細を抽象化するメカニズムのバインディングを介して Java ライブラリを呼び出します。 Xamarin.Android は、これらのバインディングを生成するツールを提供します。 このツールは、により、プロシージャは名前空間、およびメンバーの名前を変更するなどのメタデータを使用してバインディングを作成する方法、開発者のコントロールを付与します。 このドキュメントのメタデータの動作方法について説明します、属性をそのメタデータをまとめたものをサポートしているし、このメタデータの変更によるバインドの問題を解決する方法について説明します。
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 6dea13fcda43cad22b8bea9838bbcb23b97820c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="java-bindings-metadata"></a>Java バインディング メタデータ

_Xamarin.Android で c# コードでは、Java ネイティブ インターフェイス (JNI) で指定されている低レベルの詳細を抽象化するメカニズムのバインディングを介して Java ライブラリを呼び出します。Xamarin.Android は、これらのバインディングを生成するツールを提供します。このツールは、により、プロシージャは名前空間、およびメンバーの名前を変更するなどのメタデータを使用してバインディングを作成する方法、開発者のコントロールを付与します。このドキュメントのメタデータの動作方法について説明します、属性をそのメタデータをまとめたものをサポートしているし、このメタデータの変更によるバインドの問題を解決する方法について説明します。_


## <a name="overview"></a>概要

Xamarin.Android **Java バインディング ライブラリ**とも呼ばれるツールを利用して既存の Android ライブラリをバインドするために必要な作業の多くを自動化しようとする、_バインド ジェネレーター_です。 Java ライブラリをバインドするときに、Xamarin.Android は Java クラスを検査し、すべてのパッケージ、型、およびメンバーの一覧を生成すると、バインドします。 この Api の一覧に含まれている XML ファイルに格納されている **\{directory}\obj\Release\api.xml プロジェクト**の**リリース**ビルドで**\{プロジェクトdirectory}\obj\Debug\api.xml**の**デバッグ**を構築します。

![Obj/デバッグ フォルダーの api.xml ファイルの場所](java-bindings-metadata-images/java-bindings-metadata-01.png)

バインド ジェネレーターが使用する、 **api.xml**をガイドラインとして、必要な c# のラッパー クラスを生成するためのファイルです。 この XML ファイルの内容は、さまざまな Google の_Android のオープン ソース プロジェクト_形式です。
次のスニペットの内容の例は、 **api.xml**:

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

この例では**api.xml**でクラスを宣言、`android`という名前のパッケージ`Manifest`自身を拡張する、`java.lang.Object`です。

多くの場合、ヒューマン アシスタンスをバインド アセンブリのコンパイルを妨げている問題を解決するのにまたは、Java API を多く"のような .NET"と考える必要があります。 たとえば、.NET 名前空間に Java のパッケージ名を変更する、クラスやメソッドの戻り値の型を変更するために必要な場合があります。

これらの変更が変更することで達成できない**api.xml**直接です。
代わりに、変更は、Java バインド ライブラリ テンプレートによって提供される特殊な XML ファイルに記録されます。 バインド アセンブリを作成するときに、バインド ジェネレーターこれらのマッピング ファイルが反映される Xamarin.Android バインド アセンブリをコンパイルするときに

これらの XML マッピング ファイルは、「、**変換**プロジェクトのフォルダー。

-   **MetaData.xml** &ndash;生成されたバインディングの名前空間の変更など、最終的な API への変更を許可します。 

-   **EnumFields.xml** &ndash; Java 間のマッピングを含む`int`定数と c#`enums`です。 

-   **EnumMethods.xml** &ndash; Java からメソッドのパラメーターと戻り値の型に変更できます`int`c# 定数`enums`です。 

**MetaData.xml**など、バインドの汎用的な変更が許可されているファイルは、これらのファイルのインポート、最も。

-   名前空間、クラス、メソッド、またはフィールドの名前変更 .NET 規則に従っているためです。 

-   名前空間、クラス、メソッド、または不要なフィールドを削除しています。 

-   クラスを異なる名前空間に移動します。 

-   バインディングのデザインを作成する追加の他のサポート クラスでは、.NET framework のパターンに従います。 

説明に進むことができます**Metadata.xml**で詳しく説明します。


## <a name="metadataxml-transform-file"></a>Metadata.xml 変換ファイル

既に学びました、ファイルと**Metadata.xml**バインド ジェネレーターによってバインド アセンブリの作成に影響を与えるために使用します。
メタデータの形式を使用して[XPath](https://www.w3.org/TR/xpath/)構文とほとんど同じですが、 *GAPI メタデータ*」に記載[GAPI メタデータ](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)ガイドです。 この実装では、XPath 1.0 の完全な実装ではほとんどは、し、したがって 1.0 標準では、項目をサポートします。 このファイルは、変更、追加、非表示、または API ファイル内の任意の要素または属性を移動する強力な XPath が基づくメカニズムです。 すべてのメタデータ仕様の規則要素には、ルールの適用先となるノードを識別するパス属性が含まれます。 ルールは、次の順序で適用されます。

* **ノードの追加** &ndash; path 属性によって指定されたノードに子ノードを追加します。
* **attr** &ndash; path 属性により指定される要素の属性の値を設定します。
* **ノードを削除**&ndash;指定した XPath と一致するノードを削除します。

次の例に示します、 **Metadata.xml**ファイル。

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

次の一覧を示しますより一般的に使用される XPath 要素の一部が Java API の。

-   `interface` &ndash; Java インターフェイスを検索するために使用します。 例:`/interface[@name='AuthListener']`です。

-   `class` &ndash; クラスを検索するために使用します。 例:`/class[@name='MapView']`です。

-   `method` &ndash; Java クラスまたはインターフェイスでメソッドを検索するために使用します。 例:`/class[@name='MapView']/method[@name='setTitleSource']`です。

-   `parameter` &ndash; メソッドのパラメーターを識別します。 例。 `/parameter[@name='p0']`



### <a name="adding-types"></a>型の追加

`add-node`要素が新しいラッパー クラスを追加する Xamarin.Android バインディング プロジェクトに指示が**api.xml**です。 たとえば、次のスニペットは、コンス トラクターと 1 つのフィールドを持つクラスを作成するバインドのジェネレーターをダイレクトは。

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>型の削除

Java の型を無視してバインドできません Xamarin.Android バインディング ジェネレーターに指示することができます。 これは、追加することで、 `remove-node` XML 要素を**metadata.xml**ファイル。

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>メンバーの名前を変更します。

直接編集して実行するメンバーの名前を変更することはできません、 **api.xml** Xamarin.Android に元の Java ネイティブ インターフェイス (JNI) 名が必要とするためのファイルです。 したがって、`//class/@name`属性を変更することはできません以外の場合は、バインディングは機能しません。

型の名前を変更する場合を考えます`android.Manifest`です。
これを実現するには直接編集しようと可能性があります**api.xml**およびクラスの名前を変更する次のようにします。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

これにより、ラッパー クラスの次の c# コードを作成するバインド ジェネレーターが発生します。

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

ラッパー クラスの名前は通知`NewName`、元の Java 型はまだ`Manifest`します。 Xamarin.Android バインディング クラスの任意のメソッドにアクセスする可能性が不要になった`android.Manifest`; ラッパー クラスが存在しない Java 型にバインドします。

正しくラップの型 (またはメソッド) の管理対象の名前を変更する必要がある設定を`managedName`この例で示すように属性します。

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>名前を変更する`EventArg`ラッパー クラス

Xamarin.Android バインディング ジェネレーターを識別、`onXXX`の set アクセス操作子メソッド、_リスナー型_、c# イベントと`EventArgs`サブクラスが生成されます .NET をサポートするには、Java ベースのリスナーの API を flavouredパターン。 たとえば、次の Java クラスとメソッドについて考えてみます。

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android プレフィックスが削除されます`on`setter メソッドを代わりに使用して`2DSignNextManuever`の名前の基準となる、`EventArgs`サブクラスです。 サブクラスは、以下のようにというされます。

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

有効な c# クラス名ではないです。 この問題を修正するバインディングの作成者が使用する必要があります、`argsType`属性を有効な c# の名前を指定、`EventArgs`サブクラス。
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>サポートされる属性

次のセクションでは、Java Api に変換するための属性について説明します。

### <a name="argstype"></a>argsType

この属性が名前を付けるの setter メソッドに配置された、 `EventArg` Java リスナーをサポートするために生成されるサブクラスです。 さらに詳しく以下のセクションに記載されて[EventArg ラッパー クラスの名前を変更する](#Renaming_EventArg_Wrapper_Classes)このガイドで後でします。

### <a name="eventname"></a>eventName

イベントの名前を指定します。 空の場合は、イベントの生成を抑制します。
これは、セクション タイトルの詳細について、「 [EventArg ラッパー クラス名前を変更する](#Renaming_EventArg_Wrapper_Classes)です。

### <a name="managedname"></a>managedName

これを使用して、パッケージ、クラス、メソッド、またはパラメーターの名前を変更できます。 たとえば、Java クラスの名前を変更するために`MyClass`に`NewClassName`:

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

`managedType` メソッドの戻り値の型の変更を使用します。 一部の状況でバインド ジェネレーターが正しく推論されません、コンパイル時エラーの原因となる Java メソッドの戻り値の型。 このような状況で考えられる解決方法の 1 つを使用すると、メソッドの戻り値の型を変更します。

たとえば、バインディング ジェネレーターと信じている Java メソッド`de.neom.neoreadersdk.resolution.compareTo()`返す必要があります、 `int`、その結果は、エラー メッセージに**エラー CS0535: ' DE です。Neom.Neoreadersdk.Resolution' はインターフェイス メンバー 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' を実装していない**です。 次のスニペットは、生成される c# のメソッドの戻り値の型を変更する方法、`int`を`Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

メソッドの戻り値の型を変更します。 (として返す属性につながることが互換性のない変更 JNI シグネチャの変更) は、戻り値の属性変更されません。 次の例では、戻り値の型の`append`メソッドがから変更された`SpannableStringBuilder`に`IAppendable`(C# でサポートしていないこと共変の戻り値の型を思い出してください)。

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>難読化

Java ライブラリを難読化ツール Xamarin.Android バインディング ジェネレーターと c# のラッパー クラスを生成するには、その機能に影響します。 難読化されたクラスの特性を含める: * クラス名が含まれています、 **$**、つまり**$.class** * クラス名が完全セキュリティが侵害の小文字、つまり**a.class**

このスニペットは、C# の場合、「難読化されていない」型を生成する方法の例を示します。

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

この属性は、管理対象のプロパティの名前を変更するのには使用できます。

使用する特殊なケース`propertyName`がある場合、Java クラスのフィールドの getter メソッドのみが含まれます。 このような状況で、バインド ジェネレーターは、書き込み専用プロパティ、.NET で勧めできるものを作成します。 次のスニペットを設定して、.NET プロパティを「削除」する方法を示しています、`propertyName`に空の文字列。

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Setter メソッドと getter メソッドがバインド ジェネレーターによって作成されるまだことに注意してください。

### <a name="sender"></a>sender

メソッドのパラメーターにする必要がありますを指定します、`sender`メソッドがイベントにマップされるパラメーター。 値を指定できます`true`または`false`です。 例えば:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>参照可能範囲

この属性を使用して、クラス、メソッド、またはプロパティの表示設定を変更します。 たとえば、ある可能性がありますに昇格させるために必要な`protected`Java メソッドは、c# ラッパーを対応することができるように`public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml と EnumMethods.xml

Android ライブラリがライブラリのプロパティまたはメソッドに渡される状態を表す整数の定数を使用する場合があります。 多くの場合、これらの整数の定数を c# で列挙型にバインドすると便利です。 このマッピングを容易にするには、 **EnumFields.xml**と**EnumMethods.xml**バインド プロジェクト内のファイルです。 

### <a name="defining-an-enum-using-enumfieldsxml"></a>EnumFields.xml を使用して列挙型の定義

**EnumFields.xml**ファイルには、Java の間のマッピングが含まれています。`int`定数と c#`enums`です。 C# の列挙型のセット用に作成されているは、次の例を見てみましょう`int`定数。 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Java クラスを次に移動しました`SKRealReachSettings`と呼ばれる c# 列挙を定義および`SKMeasurementUnit`名前空間に`Skobbler.Ngx.Map.RealReach`です。 `field`エントリ Java 定数の名前を定義します (例`UNIT_SECOND`)、列挙型のエントリの名前 (例`Second`) と両方のエンティティによって表される整数値 (例`0`)。 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>EnumMethods.xml を使用して、Getter または Setter メソッドを定義します。

**EnumMethods.xml**ファイルを使用して Java からメソッドのパラメーターと戻り値の型を変更する`int`c# 定数`enums`です。 マップつまり、c# の列挙型の読み書き (で定義されている、 **EnumFields.xml**ファイル) java`int`定数`get`と`set`メソッドです。

指定された、`SKRealReachSettings`列挙型の定義上、次**EnumMethods.xml**ファイルでこの列挙型の getter と setter が定義します。

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

最初の`method`行は、Java の戻り値をマップ`getMeasurementUnit`メソッドを`SKMeasurementUnit`列挙型。 2 番目`method`行マップの最初のパラメーター、`setMeasurementUnit`同じ列挙型にします。

すべての変更を行うには、次のコードで使える Xamarin.Android を設定する、 `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>まとめ

この記事の内容が Xamarin.Android はメタデータを使用して、API の定義を変換する方法を説明した、 *Google* *AOSP 形式*です。 使用して後で可能な変更点に関する*Metadata.xml*メンバーの名前を変更するときに発生する制限を検査、および各属性を使用すべき状況を説明する、XML のサポートされている属性の一覧を表示します。



## <a name="related-links"></a>関連リンク

- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [GAPI メタデータ](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
