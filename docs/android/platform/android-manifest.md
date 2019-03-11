---
title: Android マニフェストの使用
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 5e354f8271257ab21a855bdf5d576ce3062fadc7
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668245"
---
# <a name="working-with-the-android-manifest"></a>Android マニフェストの使用


## <a name="overview"></a>概要

**AndroidManifest.xml**は、Android プラットフォーム機能と android アプリケーションの要件について説明するために強力なファイルです。 ただし、扱うことは簡単ではありません。 Xamarin.Android は、マニフェストを自動的に生成するを使用してクラスをカスタム属性を追加することによってこの問題を最小限に抑えるには役立ちます。 目標は、ユーザーの 99% が手動で変更する必要があることはありません**AndroidManifest.xml**します。 

**AndroidManifest.xml**は、ビルド プロセスと内にある XML の一部として生成**Properties/AndroidManifest.xml**カスタム属性から生成される XML に結合されます。 マージ結果**AndroidManifest.xml**に存在する、 **obj**サブディレクトリ; に存在するなど、 **obj/Debug/android/AndroidManifest.xml**デバッグ ビルドには. マージ プロセスは簡単です: コード内のカスタム属性を使用して、XML 要素の生成と*挿入*それらの要素に**AndroidManifest.xml**します。 



## <a name="the-basics"></a>基本事項

ため、コンパイル時にアセンブリがスキャンされる以外`abstract`から派生するクラス[アクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)いて、 [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/)属性で宣言されています。 使用して、これらのクラスと属性、マニフェストをビルドします。 次に例を示します。 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

これは、結果内で生成される何も、 **AndroidManifest.xml**します。 場合は、 `<activity/>` 、生成される要素を使用する必要がある、 [`[Activity]`](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) 
カスタム属性: 

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

`[Activity]`属性も何も起こりません`abstract`型です。`abstract`種類は無視されます。



### <a name="activity-name"></a>活動名

Xamarin.Android 5.1 以降では、アクティビティの型名がエクスポートされる型のアセンブリ修飾名の md5 チェックサムに基づいています。 これにより、2 つの異なるアセンブリから指定して、パッケージのエラーを取得できませんに同じ完全修飾名です。 (Xamarin.Android 5.1 では、前に、アクティビティの既定の型名が作成、小文字の名前空間とクラス名から)。 

この既定をオーバーライドし、使用して、アクティビティの名前を明示的に指定する場合、 [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)プロパティ。 

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

*注*: 使用する必要があります、`Name`プロパティは下位互換性の理由から、その名前を変更するには実行時に型の照合の速度が低下します。 小文字の名前空間に基づくアクティビティの既定の型名とクラス名では、参照が必要とするレガシ コードがあれば[Android 呼び出し可能ラッパー名前付け](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming)互換性の維持に関するヒント。 


### <a name="activity-title-bar"></a>アクティビティのタイトル バー

既定では、Android ことで、アプリケーションのタイトル バーを実行した場合。 このために使用する値は[ `/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label)します。 ほとんどの場合、この値は、クラス名と異なります。 タイトル バーに、アプリのラベルを指定するには、使用、 [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/)プロパティ。
例: 

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


### <a name="launchable-from-application-chooser"></a>アプリケーションの選択 ダイアログ ボックスから起動可能

既定では、アクティビティは表示されませんで Android のアプリケーションの起動画面。 これは、存在する可能性は多くのアクティビティで、アプリケーションでは、1 つにつき、アイコンが必要ないためです。 アプリケーション起動プログラムから起動可能なものがありますを指定するには、使用、 [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/)プロパティ。 例: 

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



### <a name="activity-icon"></a>アクティビティのアイコン

既定では、システムによって提供される既定のランチャー アイコン、アクティビティが指定されます。 カスタム アイコンを使用するには、まず追加、 **.png**に**リソース/drawable**、ビルド アクションを設定**AndroidResource**を使用して、 [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/)プロパティを使用するアイコンを指定します。 例: 

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

Android マニフェストにアクセス許可を追加すると (」の説明に従って[Android マニフェストへのアクセス許可を追加](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest))、これらのアクセス許可に記録される**Properties/AndroidManifest.xml**します。 たとえば、設定した場合、 `INTERNET` 、アクセス許可に次の要素が追加**Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

デバッグ ビルドが容易にデバッグする一部のアクセス許可を自動的に設定 (など`INTERNET`と`READ_EXTERNAL_STORAGE`)&ndash;これらの設定を設定のみに生成された**obj/Debug/android/AndroidManifest.xml**はできません有効になっていると表示される、**アクセス許可が必要な**設定します。 

生成されたマニフェスト ファイルを確認する場合など、 **obj/Debug/android/AndroidManifest.xml**次の追加の permission 要素を参照してください可能性があります。 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

リリースでのビルド マニフェストのバージョン (で**obj/Debug/android/AndroidManifest.xml**)、これらの権限は*いない*自動的に構成されています。 リリース ビルドに切り替えると、アプリをデバッグ ビルドで使用可能なアクセス許可を失ったで発生する場合このアクセス許可を明示的に設定することを確認、**アクセス許可が必要な**(参照アプリの設定**ビルド > Android アプリケーション**Visual studio for Mac は、次を参照してください。**プロパティ > Android マニフェスト**Visual Studio で)。 




## <a name="advanced-features"></a>高度な機能


### <a name="intent-actions-and-features"></a>インテント アクションと機能

Android マニフェストでは、アクティビティの機能を記述するための手段を提供します。 使用してこれには[インテント](https://developer.android.com/guide/topics/manifest/intent-filter-element.html)と [`[IntentFilter]`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 
カスタム属性です。 アクティビティの適切なアクションを指定することができます、 [`IntentFilter`](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) 
コンス トラクター、および対象のカテゴリは適切な [`Categories`](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) 
プロパティを使用する方法を示します。 少なくとも 1 つのアクティビティには、(これは、コンス トラクターでアクティビティが提供されているためにです) を提供する必要があります。 `[IntentFilter]` 複数回、および使用するたびの個別の結果に用意できる`<intent-filter/>`内の要素、`<activity/>`します。 例えば:

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

Android マニフェストでは、アプリケーション全体のプロパティを宣言することもできます。 使用してこれには、`<application>`要素と、対応する、[アプリケーション](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/)カスタム属性。 アクティビティごとの設定ではなく、アプリケーション全体 (アセンブリ全体) の設定できることに注意してください。 通常を宣言する`<application>`全体のアプリケーションのプロパティをオーバーライドしてこれらの設定 (必要に応じて)、アクティビティごとに、します。 

たとえば、次`Application`属性が追加されます**AssemblyInfo.cs**をアプリケーションをデバッグできること、そのユーザーが判読できる名前が示す**My App**、し、を使用して`Theme.Light`としてすべてのアクティビティの既定のテーマのスタイル。 

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
この例で、アプリのすべての活動は既定になります、`Theme.Light`スタイル。 アクティビティのテーマを設定した場合`Theme.Dialog`、アクティビティを使用するだけ、`Theme.Dialog`スタイルの既定値は、アプリの他のすべてのアクティビティ中に、`Theme.Light`スタイルで設定されている、`<application>`要素。 

`Application`要素は、構成する唯一の方法ではありません`<application>`属性。 直接属性を挿入する代わりに、`<application>`要素の**Properties/AndroidManifest.xml**します。 これらの設定が最後にマージされる`<application>`要素内にある**obj/Debug/android/AndroidManifest.xml**します。 注意の内容**Properties/AndroidManifest.xml**カスタム属性によって提供されるデータを常にオーバーライドします。 

構成できる多くのアプリケーション全体の属性がある、`<application>`要素ですこれらの設定の詳細については、次を参照してください。、[パブリック プロパティ](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties)の[ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>カスタム属性の一覧

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) :生成、 [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML フラグメント 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) :生成、[マニフェスト/アプリケーション](https://developer.android.com/guide/topics/manifest/application-element.html)XML フラグメント 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) :生成、[インストルメンテーションマニフェスト/](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML フラグメント 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) :生成、 [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML フラグメント 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) :生成、 [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML フラグメント 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) :生成、 [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML フラグメント 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) :生成、 [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML フラグメント 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) :生成、 [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML フラグメント 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) :生成、 [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML フラグメント 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) :生成、 [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML フラグメント 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) :生成、 [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML フラグメント 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) :生成、 [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML フラグメント 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) :生成、 [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML フラグメント 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) :生成、 [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML フラグメント

