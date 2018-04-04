---
title: Android のリソースの基礎
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: f6be1001e5d3455a94e677f1bb5dc52ca574b873
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="android-resource-basics"></a>Android のリソースの基礎

ほぼすべての Android アプリケーションは何らかのリソースのことです。少なくとも多くの場合、XML ファイルの形式でユーザー インターフェイスのレイアウトがあります。 Xamarin.Android アプリケーションが最初に作成されるときに既定のリソースは Xamarin.Android プロジェクト テンプレートによってセットアップです。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![リソース ファイル](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![リソース ファイル](android-resource-basics-images/01-resource-files-xs.png)
 
-----

既定のリソースを構成する 5 つのファイルは、リソース フォルダーに作成されました。

-  **Icon.png** &ndash;アプリケーションの既定のアイコン

-  **[Main.axml]** &ndash;アプリケーションの既定のユーザー インターフェイスのレイアウト ファイルです。 注 Android 中に使用する、 **.xml**ファイル拡張子 Xamarin.Android を使用して、 **.axml**ファイル拡張子。

-  **Strings.xml** &ndash;アプリケーションのローカライズを支援ストリング テーブル

-  **AboutResources.txt** &ndash;これが不要と安全に削除された可能性があります。 だけ、リソース フォルダーおよび内のファイルの高レベルな概要を提供します。

-  **Resource.designer.cs** &ndash;このファイルが自動的に生成され、Xamarin.Android、およびを保持しますが、一意で保持されている ID の各リソースに割り当てられます。 これは非常に似ています、Java で記述された Android アプリケーションの必要があります R.java ファイルと同じ目的です。 Xamarin.Android ツールによって自動的に作成し、時間の経過に再生成されます。


## <a name="creating-and-accessing-resources"></a>作成して、リソースへのアクセス

リソースを作成することは、対象のリソースの種類のディレクトリにファイルを追加するように単純です。 以下のスクリーン ショットは、ドイツ語のロケールの文字列リソースがプロジェクトに追加されたを示しています。 ときに**Strings.xml** 、ファイルに追加された、**ビルド アクション**に自動的に設定された**AndroidResource** Xamarin.Android ツールで。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![ビルド設定 AndroidResource Strings.xml のアクション](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![ビルド設定 AndroidResource Strings.xml のアクション](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

これによりを正しくコンパイルおよび APK ファイルへのリソースを埋め込む Xamarin.Android ツールです。 何らかの理由の場合、**ビルド アクション**に設定されていない**Android リソース**APK からのファイルの除外し、および読み込みまたはリソースにアクセスしようとすると、実行時エラーが発生し、アプリケーションがクラッシュします。

また、することが重要 Android には、リソース アイテムのファイル名を小文字のみがサポート、Xamarin.Android はもう少しタイプセーフです。大文字と小文字の両方のファイル名をサポートします。 イメージ名の規則は区切り記号としてアンダー スコアと小文字を使用する (たとえば、 **my\_イメージ\_name.png**)。 ダッシュやスペースを区切り記号として使用する場合にリソース名を処理できないことに注意してください。

アプリケーションで使用する 2 つの方法があるリソースをプロジェクトに追加すると、&ndash;プログラム (コード) 内や、XML ファイルです。


## <a name="referencing-resources-programmatically"></a>リソースをプログラムで参照します。

これらのファイルをプログラムでアクセスするには、が割り当てられている一意のリソース id。 このリソース ID と呼ばれる特殊なクラスで定義されている整数`Resource`、ファイルに存在する**Resource.designer.cs**、し、次のようになります。

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

それぞれのリソース ID は、リソースの種類に対応する入れ子になったクラス内に含まれています。 たとえばときに、ファイル**Icon.png** Xamarin.Android 更新をプロジェクトに追加された、`Resource`クラス、入れ子になったクラスを作成すると呼ばれる`Drawable`定数の内部名前付き`Icon`します。
これにより、ファイル**Icon.png**としてコードで参照される`Resource.Drawable.Icon`です。 `Resource`クラス手動で編集しないでとそれに加えられた変更を加えた Xamarin.Android によって上書きされます。

プログラムで (コード) 内のリソースを参照するときに、次の構文を使用するリソースのクラスの階層構造を使用してアクセスできます。

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash;リソースを提供しているし、は、パッケージが必要なときにその他のパッケージからのリソースが使用されています。

-  **ResourceType** &ndash;が上記で説明したリソース クラス内で入れ子になったリソースの種類です。

-  **リソース名** &ndash; (拡張子なし)、リソースのファイル名または XML 要素に含まれるリソースの android: 名前属性の値。


## <a name="referencing-resources-from-xml"></a>XML からリソースを参照します。

次の XML ファイルにリソースへのアクセス、特別な構文。

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash;リソースを提供しているし、は、パッケージが必要なときにその他のパッケージからのリソースが使用されています。

-  **ResourceType** &ndash;リソース クラス内にある入れ子になったリソースの種類です。

-  **リソース名**&ndash;これは、リソースのファイル名 (*せず*ファイル タイプの拡張機能) の値は、 `android:name` XML 要素に含まれるリソースの属性です。

たとえば、ファイルの内容、レイアウト、 **Main.axml**、次に示します。

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

この例では、 [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview)ドロウアブルという名前のリソースを必要とする**フラグ**です。 `ImageView`がその`src`属性に設定 **@drawable/flag**です。 アクティビティの開始、Android は、ディレクトリ内なります**リソース/ディスプレイ**という名前のファイルの**flag.png** (ファイル拡張子は、別のイメージ形式と同様にしてでした**flag.jpg**)そのファイルを読み込んで表示で、`ImageView`です。
このアプリケーションを実行すると、次の図のようになります。

![ローカライズされた ImageView](android-resource-basics-images/03-localized-screenshot.png)

