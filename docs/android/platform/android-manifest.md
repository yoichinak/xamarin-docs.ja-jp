---
title: Android マニフェストを伴う作業
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 18817063900437baa625d8572f0ae28fec77be1e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-android-manifest"></a>Android マニフェストを伴う作業


## <a name="overview"></a>概要

**AndroidManifest.xml**機能と Android アプリケーションの要件を記述できる Android プラットフォームの強力なファイルです。 ただしでの作業は簡単ではありません。 Xamarin.Android するマニフェストを自動的に生成し、使用される、クラスにカスタム属性を追加することによってこの問題を最小限に抑えるに役立ちます。 目標は、ユーザーの 99% を手動で変更する必要はありません、 **AndroidManifest.xml**です。 

**AndroidManifest.xml**ビルド プロセスと内の XML の一部として生成される**Properties/AndroidManifest.xml**カスタム属性から生成される XML のトピックとマージされます。 結合の結果として得られる**AndroidManifest.xml**に存在する、 **obj**サブディレクトリ; に配置されているなど、 **obj/Debug/android/AndroidManifest.xml**デバッグ ビルドの. マージ プロセスは単純な: コード内でカスタム属性を使用して、XML 要素を生成することと*挿入*にそれらの要素**AndroidManifest.xml**です。 



## <a name="the-basics"></a>基本事項

ため、コンパイル時にアセンブリがスキャンされる以外`abstract`から派生したクラス[アクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)いて、 [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/)に属性が宣言されています。 使用して、これらのクラスと属性、マニフェストをビルドします。 次に例を示します。 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

これは、結果で生成されても、何も**AndroidManifest.xml**です。 場合は、`<activity/>`を生成する要素を使用する必要があります、 [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute)カスタム属性。 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

この例では、次の xml フラグメントに追加する**AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]`属性も何も起こりません`abstract`型です。`abstract`型は無視されます。



### <a name="activity-name"></a>活動名

Xamarin.Android 5.1 から始まり、アクティビティの型名はエクスポートされる型のアセンブリ修飾名の md5 チェックサムに基づいています。 これにより、2 つのアセンブリから指定して、パッケージのエラーを取得できませんに同じ完全修飾名です。 (Xamarin.Android 5.1 の前に、アクティビティの既定値の型名が作成、小文字に変換した名前空間とクラス名から)。 

この既定の動作をオーバーライドし、使用して、アクティビティの名前を明示的に指定する場合、 [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)プロパティ。 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

この例では、次の xml フラグメントが生成されます。

```xml
<activity android:name="awesome.demo.activity" />
```

*注*: 使用する必要があります、`Name`旧バージョンと互換性のため、名前を変更するように対してのみプロパティは、実行時に型の検索速度が低下します。 小文字に変換した名前空間に基づく活動の既定値の型名とクラス名を参照してください。 必要とするレガシ コードがあれば[Android 呼び出し可能ラッパーの名前付け](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming)ヒントについては、互換性を維持することにします。 


### <a name="activity-title-bar"></a>活動のタイトル バー

既定では、Android ことで、アプリケーションのタイトル バーを実行するとします。 このために使用する値は[ `/manifest/application/activity/@android:label`](http://developer.android.com/guide/topics/manifest/activity-element.html#label)です。 ほとんどの場合、この値は、クラス名と異なります。 タイトル バーで、アプリのラベルを指定するには、使用、 [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/)プロパティです。
例えば: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

この例では、次の xml フラグメントが生成されます。

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>アプリケーションの選択 ウィンドウから起動可能です

既定では、アクティビティは表示されません [Android のアプリケーション ランチャー] 画面。 これは、存在する可能性は多くのアクティビティで、アプリケーションごとのアイコンをたくないためです。 どちらかをアプリケーション起動プログラムから起動可能にする必要がありますを指定する、 [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/)プロパティです。 例えば: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

この例では、次の xml フラグメントが生成されます。

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>[アクティビティ] アイコン

既定では、システムによって提供される既定ランチャー アイコンに、アクティビティが与えられます。 カスタム アイコンを使用するには、まず追加、 **.png**に**リソース/描画**、そのビルド アクションに設定**AndroidResource**を使用して、 [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/)プロパティを使用するアイコンを指定します。 例えば: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

この例では、次の xml フラグメントが生成されます。

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>アクセス許可

Android マニフェストへのアクセス許可を追加すると (」の説明に従って[Android マニフェストへのアクセス許可を追加](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/))、これらのアクセス許可に記録される**Properties/AndroidManifest.xml**です。 たとえば、設定した場合、 `INTERNET` 、アクセス許可に、次の要素が追加**Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

デバッグ ビルドは自動的にデバッグを容易にするいくつかのアクセス許可を設定 (など`INTERNET`と`READ_EXTERNAL_STORAGE`)&ndash;これらの設定を設定だけで、生成された**obj/Debug/android/AndroidManifest.xml**ではありません有効になっているように、**アクセス許可を必要な**設定します。 

生成されたマニフェスト ファイルを確認する場合など、 **obj/Debug/android/AndroidManifest.xml**、アクセス許可の要素を追加する、次を参照してください可能性があります。 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

リリース ビルド マニフェストのバージョン (で**obj/Debug/android/AndroidManifest.xml**)、これらの権限は*いない*自動的に構成されています。 リリース ビルドへの切り替えはデバッグ ビルドで使用可能なアクセス許可を失ったにアプリで発生する場合にこのアクセス許可を明示的に設定することを確認してください、**アクセス許可を必要な**(参照、アプリの設定**ビルド > Android アプリケーション**Mac; 用の Visual Studio で、次を参照してください。**プロパティ > Android マニフェスト**Visual Studio で)。 




## <a name="advanced-features"></a>高度な機能


### <a name="intent-actions-and-features"></a>インテントの動作と機能

Android マニフェストでは、アクティビティの機能を記述する方法を提供します。 使用してこれを行う[インテント](http://developer.android.com/guide/topics/manifest/intent-filter-element.html)と[ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)カスタム属性です。 アクティビティの適切なアクションを指定できます、 [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/)コンス トラクター、およびカテゴリが適切な[ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/)プロパティです。 少なくとも 1 つのアクティビティには、(そのため、コンス トラクターで指定されるアクティビティ) を指定する必要があります。 `[IntentFilter]` 複数回、され、別の結果は各を使用して提供されます`<intent-filter/>`内の要素、`<activity/>`です。 例えば:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

この例では、次の xml フラグメントが生成されます。

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Application 要素

Android マニフェストでは、アプリケーション全体のプロパティを宣言するための手段も提供します。 使用してこれを行う、`<application>`要素と、対応する、[アプリケーション](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/)カスタム属性です。 これらの活動の設定ではなく、アプリケーション全体 (アセンブリ全体に適用) 設定があることに注意してください。 宣言する通常、`<application>`アプリケーション全体のプロパティをオーバーライドしてこれらの設定 (必要に応じて)、アクティビティごとに、します。 

たとえば、次`Application`属性が追加**AssemblyInfo.cs**アプリケーションをデバッグできること、そのユーザーが判読できる名前を示すために**My App**、こと、および使用、 `Theme.Light`のすべてのアクティビティの既定のテーマとスタイル。 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

この宣言により、次の XML フラグメントを生成する**obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
この例では、アプリのすべての活動は既定で、`Theme.Light`スタイル。 設定すると、アクティビティのテーマ`Theme.Dialog`、アクティビティを使用する、`Theme.Dialog`スタイルのアプリ内の他のすべてのアクティビティは既定値中に、`Theme.Light`で設定されているスタイル、`<application>`要素。 

`Application`要素は、構成する唯一の方法ではありません`<application>`属性。 直接属性を挿入する代わりに、`<application>`要素の**Properties/AndroidManifest.xml**です。 これらの設定が最後にマージされます`<application>`要素内にある**obj/Debug/android/AndroidManifest.xml**です。 なおの内容**Properties/AndroidManifest.xml**カスタム属性によって提供されるデータを常にオーバーライドします。 

構成できる多くのアプリケーション全体属性がある、`<application>`要素ですこれらの設定に関する詳細については、次を参照してください。、[パブリック プロパティ](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties)のセクション[ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>カスタム属性の一覧

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : 生成、 [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML フラグメント 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : 生成、 [/マニフェスト/アプリケーション](http://developer.android.com/guide/topics/manifest/application-element.html)XML フラグメント 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : 生成、[インストルメンテーションマニフェスト/](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML フラグメント 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : 生成、 [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML フラグメント 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : 生成、 [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML フラグメント 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : 生成、 [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML フラグメント 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : 生成、 [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML フラグメント 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : 生成、 [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML フラグメント 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : 生成、 [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML フラグメント 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : 生成、 [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML フラグメント 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : 生成、 [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML フラグメント 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : Generates a  [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML fragment 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : Generates a  [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML fragment 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : Generates a  [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML fragment

