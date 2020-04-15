---
title: Android マニフェストの操作
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 1438c012608b367c21ebcc401c058b186b917f53
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027804"
---
# <a name="working-with-the-android-manifest"></a>Android マニフェストの操作

**AndroidManifest.xml** は、Android プラットフォームの強力なファイルであり、アプリケーションの機能と要件を Android 向けに記述することができます。 ただし、それを使用するのは簡単ではありません。 Xamarin.Android を使用すると、カスタム属性をクラスに追加することができ、追加した属性は独自のマニフェストを自動的に生成するために使用されるので、このような難しさを最小限に抑えることができます。 その目標は、ユーザーの 99% が **AndroidManifest.xml** を手作業で変更する必要がないようにすることです。 

**AndroidManifest.xml** はビルド プロセスの一部として生成され、**Properties/AndroidManifest.xml** 内で検出された XML が、カスタム属性から生成された XML とマージされます。 マージされた結果の **AndroidManifest.xml** は **obj** サブディレクトリに格納されます。たとえば、デバッグ ビルドの場合は **obj/Debug/android/AndroidManifest.xml** にあります。 マージ プロセスは簡単です。コード内のカスタム属性を使用して XML 要素が生成され、それらの要素が **AndroidManifest.xml** に "*挿入*" されます。 

## <a name="the-basics"></a>基本事項

コンパイル時に、[Activity](xref:Android.App.Activity) から派生した非 `abstract` クラスがアセンブリでスキャンされ、それらで [`[Activity]`](xref:Android.App.ActivityAttribute) 属性が宣言されます。 その後、これらのクラスと属性を使用してマニフェストがビルドされます。 次に例を示します。 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

この結果として、**AndroidManifest.xml** では何も生成されません。 `<activity/>` 要素を生成する場合は、[`[Activity]`](xref:Android.App.Activity) 
カスタム属性を使用する必要があります。 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

この例では、次の xml フラグメントが **AndroidManifest.xml** に追加されます。

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` 属性は `abstract` 型には影響しません。`abstract` 型は無視されます。

### <a name="activity-name"></a>活動名

Xamarin.Android 5.1 以降では、アクティビティの型名は、エクスポートされる型のアセンブリ修飾名の MD5SUM に基づいています。 これにより、2 つの異なるアセンブリから同じ完全修飾名を指定できるようになり、パッケージ化エラーは発生しません。 (Xamarin Android 5.1 より前は、アクティビティの既定の型名は、小文字にされた名前空間とクラス名から作成されていました)。 

この既定値をオーバーライドし、アクティビティの名前を明示的に指定する場合は、[`Name`](xref:Android.App.ActivityAttribute.Name) プロパティを使用します。 

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
> 名前を変更すると実行時に型参照の速度が低下する可能性があるため、`Name` プロパティは下位互換性のためにのみ使用する必要があります。 小文字の名前空間とクラス名に基づくアクティビティの既定の型名を想定したレガシ コードがある場合、互換性の維持に関するヒントについて「[Android の呼び出し可能なラッパーの名前付け](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)」を参照してください。 

### <a name="activity-title-bar"></a>アクティビティのタイトル バー

既定では、アプリケーションの実行時に Android によってタイトル バーが表示されます。 これに使用される値は [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label) です。 ほとんどの場合、この値はクラス名とは異なります。 タイトル バーでアプリのラベルを指定するには、[`Label`](xref:Android.App.ActivityAttribute.Label) プロパティを使用します。
次に例を示します。 

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

### <a name="launchable-from-application-chooser"></a>アプリケーションの選択から起動可能にする

既定では、アクティビティは Android のアプリケーション起動画面に表示されません。 これは、アプリケーションには多くのアクティビティが存在する可能性があり、そのすべてにアイコンを表示したくないためです。 アプリケーション起動画面から起動できるようにする必要があるものを指定するには、[`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher) プロパティを使用します。 次に例を示します。 

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

### <a name="activity-icon"></a>アクティビティ アイコン

既定では、システムによって提供される既定の起動アイコンがアクティビティに割り当てられます。 カスタム アイコンを使用するには、最初に **.png** を **Resources/drawable** に追加し、そのビルド アクションを **AndroidResource** に設定します。次に、[`Icon`](xref:Android.App.ActivityAttribute.Icon) プロパティを使用して、使用するアイコンを指定します。 次に例を示します。 

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

Android マニフェストにアクセス許可を追加すると (「[Android マニフェストにアクセス許可を追加する](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)」で説明されています)、これらのアクセス許可は **Properties/AndroidManifest.xml** に記録されます。 たとえば、`INTERNET` アクセス許可を設定した場合は、次の要素が **Properties/AndroidManifest.xml** に追加されます。 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

デバッグ ビルドでは、デバッグを容易にするために、いくつかのアクセス許可 (`INTERNET` や `READ_EXTERNAL_STORAGE` など) が自動的に設定されます。これらの設定は、生成される **obj/Debug/android/AndroidManifest.xml** でのみ設定され、 **[必要なアクセス許可]** の設定では有効として表示されません。 

たとえば、生成されたマニフェスト ファイル **obj/Debug/android/AndroidManifest.xml** を調べると、次のアクセス許可要素が追加されていることがわかる場合があります。 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

マニフェストのリリース ビルド バージョン (**obj/Debug/android/AndroidManifest.xml**) では、これらのアクセス許可は自動的には "*構成されません*"。 アプリをリリース ビルドに切り替えると、デバッグ ビルドでは使用できたアクセス許可が失われる場合は、そのアクセス許可を **[必要なアクセス許可]** の設定でアプリに対して明示的に設定していることを確認してください (Visual Studio for Mac では **[ビルド] > [Android アプリケーション]** 、Visual Studio では **[プロパティ] > [Android マニフェスト]** )。 

## <a name="advanced-features"></a>高度な機能

### <a name="intent-actions-and-features"></a>インテントのアクションと機能

Android マニフェストでは、アクティビティの機能を記述する方法が提供されています。 これは、[インテント](https://developer.android.com/guide/topics/manifest/intent-filter-element.html)と [`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
カスタム属性によって行われます。 アクティビティに適したアクションを指定するには [`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)
コンストラクターを使用し、どのカテゴリが適しているかを指定するには [`Categories`](xref:Android.App.IntentFilterAttribute.Categories)
プロパティを使用する方法を示します。 少なくとも 1 つのアクティビティを指定する必要があります (アクティビティがコンストラクターで提供されているのはこのためです)。 `[IntentFilter]` は複数回指定することができ、使用するたびに `<activity/>` 内に異なる `<intent-filter/>` 要素が作成されます。 次に例を示します。

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

Android マニフェストには、アプリケーション全体に対するプロパティを宣言する手段も用意されています。 これを行うには、`<application>` 要素とそれに対応する [Application](xref:Android.App.ApplicationAttribute) カスタム属性を使用します。 これらは、アクティビティごとの設定ではなく、アプリケーション全体 (アセンブリ全体) の設定であることにご注意ください。 通常は、アプリケーション全体に対して `<application>` プロパティを宣言してから、アクティビティごとにこれらの設定を (必要に応じて) オーバーライドします。 

たとえば、**AssemblyInfo.cs** に追加された次の `Application` 属性では、アプリケーションをデバッグできること、ユーザーが判読できる名前が **My App** であること、およびすべてのアクティビティの既定のテーマとして `Theme.Light` スタイルを使用していることが示されます。 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

この宣言により、**obj/Debug/android/AndroidManifest.xml** には次のような XML フラグメントが生成されます。

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

この例では、アプリ内のすべてのアクティビティが既定で `Theme.Light` スタイルに設定されます。 あるアクティビティのテーマを `Theme.Dialog` に設定すると、そのアクティビティだけで `Theme.Dialog` スタイルが使用され、アプリ内の他のすべてのアクティビティでは `<application>` 要素で設定されている `Theme.Light` スタイルが既定で使用されます。 

`<application>` 属性を構成する方法は `Application` 要素だけではありません。 代わりの方法として、**Properties/AndroidManifest.xml** の `<application>` 要素に属性を直接挿入することもできます。 これらの設定は、**obj/Debug/android/AndroidManifest.xml** に存在する最終的な `<application>` 要素にマージされます。 カスタム属性で指定したデータは、**Properties/AndroidManifest.xml** の内容によって常にオーバーライドされることにご注意ください。 

`<application>` 要素では、アプリケーション全体の多数の属性を構成できます。これらの設定の詳細については、[ApplicationAttribute](xref:Android.App.ApplicationAttribute) の[パブリック プロパティ](xref:Android.App.ApplicationAttribute)に関するセクションを参照してください。 

## <a name="list-of-custom-attributes"></a>カスタム属性の一覧

- [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute): [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML フラグメントが生成されます 
- [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute): [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html) XML フラグメントが生成されます 
- [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute): [/manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML フラグメントが生成されます 
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute): [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML フラグメントが生成されます 
- [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute): [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML フラグメントが生成されます 
- [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute): [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML フラグメントが生成されます 
- [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute): [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML フラグメントが生成されます 
- [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute): [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML フラグメントが生成されます 
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute): [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML フラグメントが生成されます 
- [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute): [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML フラグメントが生成されます 
- [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute): [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML フラグメントが生成されます 
- [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute): [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML フラグメントが生成されます 
- [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute): [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML フラグメントが生成されます 
- [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute): [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML フラグメントが生成されます
