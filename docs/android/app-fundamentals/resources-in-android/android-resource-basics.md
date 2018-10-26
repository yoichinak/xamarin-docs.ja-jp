---
title: Android リソースの基本
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: 2673021fae2f0a0b45761bf4ed619c92fb826b13
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110135"
---
# <a name="android-resource-basics"></a>Android リソースの基本

ほぼすべての Android アプリケーションは何らかのリソースには少なくとも多くの場合、XML ファイルの形式でユーザー インターフェイスのレイアウトがあります。 Xamarin.Android アプリケーションが作成されると、既定のリソースは、Xamarin.Android プロジェクト テンプレートでのセットアップです。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![リソース ファイル](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![リソース ファイル](android-resource-basics-images/01-resource-files-xs.png)
 
-----

既定のリソースを構成する 5 つのファイルは、リソース フォルダーに作成されました。

-  **Icon.png** &ndash;アプリケーションの既定のアイコン

-  **Main.axml** &ndash;アプリケーションの既定のユーザー インターフェイスのレイアウト ファイルです。 Android 間を使用して、 **.xml**ファイルの拡張機能は、Xamarin.Android で使用して、 **.axml**ファイル拡張子。

-  **Strings.xml** &ndash;アプリケーションのローカライズを支援ストリング テーブル

-  **AboutResources.txt** &ndash;これは必要ありませんし、安全に削除された可能性があります。 だけのリソース フォルダーとファイルが、高レベルな概要を提供します。

-  **Resource.designer.cs** &ndash;このファイルが自動的に生成され、Xamarin.Android と一意の保留によって管理される ID の各リソースに割り当てられます。 これは非常に似ています、Java で記述された Android アプリケーションを持つ R.java ファイルと同じ目的です。 Xamarin.Android ツールによって自動的に作成し、時間の経過に再生成されます。


## <a name="creating-and-accessing-resources"></a>作成して、リソースにアクセスします。

リソースの作成は、対象のリソースの種類のディレクトリにファイルを追加するだけです。 以下のスクリーン ショットは、ドイツ語のロケールの文字列リソースがプロジェクトに追加されたかを示しています。 ときに**Strings.xml** 、ファイルに追加された、**ビルド アクション**に自動的に設定された**AndroidResource** Xamarin.Android ツールによって。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![ビルド Strings.xml AndroidResource に設定のアクション](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![ビルド Strings.xml AndroidResource に設定のアクション](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

これにより、Xamarin.Android ツールを適切にコンパイルし、APK ファイルへのリソースを埋め込みます。 何らかの理由の場合、**ビルド アクション**に設定されていない**Android リソース**はファイル、APK から除外し、読み込みまたはリソースにアクセスしようとすると、実行時エラーになり、アプリケーションがクラッシュします。

または Android には、リソース アイテムのファイル名が小文字のみがサポートされます、Xamarin.Android では少し寛容; に注意してください。これは、大文字と小文字の両方のファイル名をサポートします。 イメージ名の規則は、区切り記号としてアンダー スコアと小文字を使用する (たとえば、**マイ\_イメージ\_name.png**)。 ダッシュやスペースを区切り記号として使用する場合にリソース名を処理できないことに注意してください。

アプリケーションで使用する 2 つの方法があるリソースがプロジェクトに追加されたら、&ndash;プログラムで (コード) 内や、XML ファイル。


## <a name="referencing-resources-programmatically"></a>リソースをプログラムで参照します。

一意のリソース ID を割り当てられているこれらのファイルをプログラムでアクセスするには このリソース ID と呼ばれる特殊なクラスで定義されている整数`Resource`、ファイル内に存在**Resource.designer.cs**、このようなものになります。

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

それぞれのリソース ID は、リソースの種類に対応する入れ子になったクラス内に含まれています。 たとえば、ファイル**Icon.png** Xamarin.Android の更新をプロジェクトに追加された、`Resource`という入れ子になったクラスを作成するクラス、`Drawable`定数内で名前付き`Icon`します。
これにより、ファイル**Icon.png**としてコードで参照される`Resource.Drawable.Icon`します。 `Resource`クラス手動で編集しないでとに加えた変更は、Xamarin.Android によって上書きされます。

プログラムで (コード) 内のリソースを参照するときに、次の構文を使用するリソースのクラス階層を使用してアクセスできます。

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash;リソースを提供する、のみ、パッケージが必要なときに使用されている他のパッケージからのリソース。

-  **ResourceType** &ndash;上記で説明したリソース クラス内にある入れ子になったリソースの種類します。

-  **リソース名** &ndash; (拡張子なし)、リソースのファイル名または XML 要素に含まれるリソースの android: name 属性の値になります。


## <a name="referencing-resources-from-xml"></a>XML からリソースを参照します。

XML ファイルにリソースを以下へのアクセス、特別な構文。

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash;リソースを提供する、のみ、パッケージが必要なときに使用されている他のパッケージからのリソース。

-  **ResourceType** &ndash;リソース クラス内にある入れ子になったリソースの種類します。

-  **リソース名**&ndash;これは、リソースのファイル名 (*せず*ファイルの種類の拡張機能) の値は、 `android:name` XML 要素に含まれるリソースの属性。

レイアウト ファイルの内容など**Main.axml**、次に示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

この例では、 [ `ImageView` ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview)という名前の描画可能なリソースが必要な**フラグ**します。 `ImageView`がその`src`属性に設定 **@drawable/flag**します。 Android は、ディレクトリ内になります、アクティビティの開始時に**リソース/ディスプレイ**という名前のファイルの**flag.png** (ファイルの拡張機能が、別のイメージ形式をこのようなにできる**flag.jpg**)そのファイルを読み込むし、表示、`ImageView`します。
このアプリケーションを実行すると、次の図のようになります。

![ローカライズされた ImageView](android-resource-basics-images/03-localized-screenshot.png)

