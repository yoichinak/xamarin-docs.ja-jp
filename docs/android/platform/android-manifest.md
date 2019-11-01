---
title: Android マニフェストの操作
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 1438c012608b367c21ebcc401c058b186b917f53
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027804"
---
# <a name="working-with-the-android-manifest"></a>Android マニフェストの操作

**Androidmanifest .xml**は、android プラットフォームの強力なファイルで、アプリケーションの機能と要件を android に記述することができます。 ただし、それを使用するのは簡単ではありません。 カスタム属性をクラスに追加することで、このような問題を最小限に抑えることができます。これは、マニフェストを自動的に生成するために使用されます。 目標は、ユーザーの99% が**Androidmanifest .xml**を手動で変更する必要がないことです。 

**Androidmanifest .xml**はビルドプロセスの一部として生成され、 **Properties/androidmanifest** XML 内にある xml は、カスタム属性から生成された xml とマージされます。 結果として得られるマージされた**Androidmanifest .xml**は、 **obj**サブディレクトリに存在します。たとえば、デバッグビルドの場合、 **obj/debug/android/AndroidManifest**に存在します。 マージプロセスは簡単です。コード内でカスタム属性を使用して XML 要素を生成し、それらの要素を**Androidmanifest .xml**に*挿入*します。 

## <a name="the-basics"></a>基本事項

コンパイル時に、[アクティビティ](xref:Android.App.Activity)から派生し、 [`[Activity]`](xref:Android.App.ActivityAttribute)属性が宣言されている非`abstract` クラスのアセンブリがスキャンされます。 次に、これらのクラスと属性を使用してマニフェストをビルドします。 次に例を示します。 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

この結果、 **Androidmanifest .xml**に何も生成されません。 `<activity/>` 要素を生成する場合は、を使用する必要があり[`[Activity]`](xref:Android.App.Activity) 
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

この例では、次の xml フラグメントを**Androidmanifest .xml**に追加します。

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` 属性は `abstract` 型には影響しません。`abstract` 型は無視されます。

### <a name="activity-name"></a>活動名

Xamarin Android 5.1 以降では、アクティビティの型名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づいています。 これにより、2つの異なるアセンブリから同じ完全修飾名を指定できるようになり、パッケージ化エラーは発生しません。 (Xamarin Android 5.1 より前では、アクティビティの既定の型名は、小文字の名前空間とクラス名から作成されていました)。 

この既定値をオーバーライドして、アクティビティの名前を明示的に指定する場合は、 [`Name`](xref:Android.App.ActivityAttribute.Name)プロパティを使用します。 

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

> [!NOTE]
> `Name` プロパティは、旧バージョンとの互換性のためにのみ使用してください。このような名前を変更すると、実行時に型参照の速度が低下する可能性があります。 アクティビティの既定の型名を小文字の名前空間とクラス名に基づいて要求するレガシコードがある場合は、「 [Android 呼び出し可能ラッパーの名前付け](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)」で互換性の維持に関するヒントを参照してください。 

### <a name="activity-title-bar"></a>アクティビティのタイトルバー

既定では、Android は実行時にアプリケーションにタイトルバーを提供します。 これに使用される値は[`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label)です。 ほとんどの場合、この値はクラス名とは異なります。 タイトルバーにアプリのラベルを指定するには、 [`Label`](xref:Android.App.ActivityAttribute.Label)プロパティを使用します。
(例: 

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

### <a name="launchable-from-application-chooser"></a>アプリケーションの選択からの起動可能な

既定では、アクティビティは Android のアプリケーションランチャー画面に表示されません。 これは、アプリケーション内に多くのアクティビティが存在する可能性があり、1つのアイコンを必要としないためです。 アプリケーションランチャーから起動可能なするものを指定するには、 [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher)プロパティを使用します。 (例: 

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

### <a name="activity-icon"></a>アクティビティアイコン

既定では、システムによって提供される既定のランチャーアイコンがアクティビティに与えられます。 カスタムアイコンを使用するには、まず、 **.png**を**リソース/** 作成元に追加し、そのビルドアクションを**androidresource**に設定します。次に、 [`Icon`](xref:Android.App.ActivityAttribute.Icon)プロパティを使用して、使用するアイコンを指定します。 (例: 

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

Android マニフェストにアクセス許可を追加すると (「 [Android マニフェストにアクセス許可を追加](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)する」の説明を参照)、これらのアクセス許可は**Properties/AndroidManifest .xml**に記録されます。 たとえば、`INTERNET` のアクセス許可を設定すると、次の要素が**Properties/AndroidManifest**に追加されます。 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

デバッグビルドでは、デバッグを容易にするために (`INTERNET` や `READ_EXTERNAL_STORAGE`などの &ndash;) いくつかのアクセス許可が自動的に設定されます。これらの設定は、生成される**obj/debug/android/AndroidManifest .xml**でのみ設定され、必要に応じて有効として表示されません。 **アクセス許可**の設定。 

たとえば、生成されたマニフェストファイルを**obj/Debug/android/AndroidManifest**で調べると、次のようなアクセス許可要素が追加されている可能性があります。 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

マニフェストのリリースビルドバージョン ( **obj/Debug/android/AndroidManifest**) では、これらのアクセス許可は自動的には構成され*ません*。 リリースビルドへの切り替えによって、デバッグビルドで使用できるアクセス許可がアプリに付与されていない場合は、アプリに対して**必要なアクセス**許可の設定でこのアクセス許可を明示的に設定していることを確認してください (「 **build > Android」を参照してください)。** Visual Studio for Mac 内のアプリケーション「Visual Studio での**Android マニフェスト > プロパティ**」を参照してください)。 

## <a name="advanced-features"></a>高度な機能

### <a name="intent-actions-and-features"></a>インテントアクションと機能

Android マニフェストには、アクティビティの機能を記述する方法が用意されています。 これは[インテント](https://developer.android.com/guide/topics/manifest/intent-filter-element.html)と[`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)によって行われます。
カスタム属性。 [`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)を使用して、アクティビティに適したアクションを指定できます。
コンストラクターと、 [`Categories`](xref:Android.App.IntentFilterAttribute.Categories)に適したカテゴリ
プロパティを使用する方法を示します。 少なくとも1つのアクティビティを指定する必要があります (これは、アクティビティがコンストラクターで提供される理由です)。 `[IntentFilter]` は複数回指定でき、各使用は `<activity/>`内で個別の `<intent-filter/>` 要素になります。 (例:

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

Android マニフェストには、アプリケーション全体のプロパティを宣言する手段も用意されています。 これを行うには、`<application>` 要素と、対応する[アプリケーション](xref:Android.App.ApplicationAttribute)カスタム属性を使用します。 これらは、アクティビティごとの設定ではなく、アプリケーション全体 (アセンブリ全体) の設定であることに注意してください。 通常は、アプリケーション全体の `<application>` プロパティを宣言してから、アクティビティごとにこれらの設定 (必要に応じて) をオーバーライドします。 

たとえば、次の `Application` 属性は、アプリケーションをデバッグできること、ユーザーが判読できる名前が**アプリ**であること、およびすべてのアクティビティの既定のテーマとして `Theme.Light` スタイルを使用することを示すために**AssemblyInfo.cs**に追加されます。 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

この宣言により、次の XML フラグメントが**obj/Debug/android/AndroidManifest**に生成されます。

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

この例では、アプリ内のすべてのアクティビティが既定で `Theme.Light` スタイルに設定されています。 アクティビティのテーマを `Theme.Dialog`に設定すると、そのアクティビティのみが `Theme.Dialog` スタイルを使用しますが、アプリ内の他のすべてのアクティビティは既定で `<application>` 要素で設定されている `Theme.Light` スタイルになります。 

`Application` 要素は、`<application>` 属性を構成する唯一の方法ではありません。 または、 **Properties/AndroidManifest .xml**の `<application>` 要素に属性を直接挿入することもできます。 これらの設定は、 **obj/Debug/android/AndroidManifest**に存在する最後の `<application>` 要素にマージされます。 **Properties/AndroidManifest .xml**の内容は、カスタム属性によって提供されるデータを常にオーバーライドすることに注意してください。 

`<application>` 要素で構成できるアプリケーション全体の属性は多数あります。これらの設定の詳細については、「 [Applicationattribute](xref:Android.App.ApplicationAttribute)」の「[パブリックプロパティ](xref:Android.App.ApplicationAttribute)」セクションを参照してください。 

## <a name="list-of-custom-attributes"></a>カスタム属性の一覧

- [Android. ActivityAttribute](xref:Android.App.ActivityAttribute) : [/Manifest/applicationxml](https://developer.android.com/guide/topics/manifest/activity-element.html)フラグメントを生成します。 
- [Android. ApplicationAttribute](xref:Android.App.ApplicationAttribute) : [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html) XML フラグメントを生成します。 
- [InstrumentationAttribute](xref:Android.App.InstrumentationAttribute) : によって、 [/manifestxml](https://developer.android.com/guide/topics/manifest/instrumentation-element.html)フラグメントが生成されます。 
- [Android. IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) :[フィルター](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML フラグメントを生成します 
- [Android. App.config 属性](xref:Android.App.MetaDataAttribute):[データ](https://developer.android.com/guide/topics/manifest/meta-data-element.html)XML フラグメントを生成します。 
- [PermissionAttribute](xref:Android.App.PermissionAttribute) :[アクセス許可](https://developer.android.com/guide/topics/manifest/permission-element.html)の XML フラグメントを生成します。 
- [Android. Get-help Groupattribute](xref:Android.App.PermissionGroupAttribute) :[グループ](https://developer.android.com/guide/topics/manifest/permission-group-element.html)XML フラグメントを生成します 
- [Android. Get-help Treeattribute](xref:Android.App.PermissionTreeAttribute) :[ツリー](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML フラグメントを生成します。 
- [Android. App. ServiceAttribute](xref:Android.App.ServiceAttribute) : [/Manifest/applicationxml](https://developer.android.com/guide/topics/manifest/service-element.html)フラグメントを生成します。 
- [Android. Applicationlibraryattribute](xref:Android.App.UsesLibraryAttribute) : [/Manifest/applicationxml](https://developer.android.com/guide/topics/manifest/uses-library-element.html)フラグメントを生成します。 
- [UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute) :[アクセス許可](https://developer.android.com/guide/topics/manifest/uses-permission-element.html)XML フラグメントを生成します。 
- [BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute) : [/manifest/Application/レシーバー](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML フラグメントを生成します。 
- [Android. ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute) : [/Manifest/applicationxml](https://developer.android.com/guide/topics/manifest/provider-element.html)フラグメントを生成します。 
- [GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute) : [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML フラグメントを生成します
