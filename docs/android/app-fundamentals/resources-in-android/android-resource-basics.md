---
title: Android リソースの基本
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: c248949024d0e13a24863368e88aa559fa496806
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755245"
---
# <a name="android-resource-basics"></a>Android リソースの基本

ほとんどすべての Android アプリケーションには、何らかのリソースが含まれています。少なくとも、多くの場合、XML ファイルの形式でユーザーインターフェイスのレイアウトを持っています。 Xamarin Android アプリケーションを最初に作成すると、既定のリソースは Xamarin. Android プロジェクトテンプレートによって設定されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![リソース ファイル](android-resource-basics-images/01-resource-files-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![リソース ファイル](android-resource-basics-images/01-resource-files-xs.png)

-----

既定のリソースを構成する5つのファイルが Resources フォルダーに作成されました。

- **Icon .png** &ndash;アプリケーションの既定のアイコン

- メインアプリケーションの既定のユーザーインターフェイスレイアウトファイル。 &ndash; Android では **.xml**ファイル拡張子が使用されていますが、Xamarin で**は .xml ファイル拡張子が使用**されていることに注意してください。

- **文字列 .xml** &ndash;アプリケーションのローカライズに役立つ文字列テーブル

- **リソース** &ndash;に関する情報は必要ありません。削除しても安全です。 ここでは、Resources フォルダーとその中のファイルについて概要を説明します。

- Resource.designer.cs&ndash;このファイルは、Xamarin Android によって自動的に生成されて管理され、各リソースに割り当てられた一意の ID を保持します。 これは、Java で記述された Android アプリケーションが持っているのと同じように、その目的は同じです。 これは、Xamarin Android ツールによって自動的に作成され、随時再生成されます。

## <a name="creating-and-accessing-resources"></a>リソースの作成とアクセス

リソースの作成は、該当するリソースの種類のディレクトリにファイルを追加するのと同じように簡単です。 次のスクリーンショットは、ドイツ語のロケールの文字列リソースがプロジェクトに追加されたことを示しています。 **文字列 .xml**がファイルに追加されると、Xamarin Android ツールによって**ビルドアクション**が**androidresource**に自動的に設定されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![文字列のビルドアクション。 xml を AndroidResource に設定します。](android-resource-basics-images/02-build-action-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![文字列のビルドアクション。 xml を AndroidResource に設定します。](android-resource-basics-images/02-build-action-xs.png)

-----

これにより、Xamarin Android ツールはリソースを適切にコンパイルして APK ファイルに埋め込むことができます。 何らかの理由で**ビルドアクション**が**Android リソース**に設定されていない場合、ファイルは apk から除外され、リソースの読み込みまたはアクセスが試行されると、実行時エラーが発生し、アプリケーションがクラッシュします。

また、Android ではリソース項目に対して小文字のファイル名のみがサポートされていますが、Xamarin. Android はやや厳格であることに注意してください。大文字と小文字の両方のファイル名がサポートされます。 イメージ名の規則としては、小文字とアンダースコアを区切り記号として使用します (たとえば、 **[\_イメージ\_名 .png**])。 ダッシュまたは空白文字が区切り記号として使用されている場合、リソース名は処理できないことに注意してください。

リソースがプロジェクトに追加されたら、アプリケーション&ndash;でプログラムを使用して (コード内で)、または XML ファイルからリソースを使用する方法が2つあります。

## <a name="referencing-resources-programmatically"></a>参照 (プログラムによるリソースの)

プログラムによってこれらのファイルにアクセスするには、一意のリソース ID が割り当てられます。 このリソース ID は、という`Resource`特殊なクラスで定義された整数で、 **Resource.designer.cs**ファイルにあり、次のようになります。

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

各リソース ID は、リソースの種類に対応する入れ子になったクラス内に含まれています。 たとえば、ファイル**アイコン .png**がプロジェクトに追加されたときに、Xamarin. Android は`Resource`クラスを更新して、という名前`Icon`の定数を持つという`Drawable`入れ子になったクラスを作成します。
これにより、コード内のファイル**アイコン .png**をとして`Resource.Drawable.Icon`参照できます。 クラス`Resource`に加えられた変更はすべて Xamarin Android によって上書きされるため、クラスは手動で編集しないでください。

プログラムによって (コード内で) リソースを参照する場合は、次の構文を使用する Resources クラス階層を介してアクセスできます。

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

- **PackageName**&ndash;リソースを提供するパッケージ。他のパッケージのリソースが使用されている場合にのみ必要です。

- **ResourceType**&ndash;これは、上記で説明したリソースクラス内にある入れ子になったリソースの種類です。

- **リソース名**&ndash;これは、リソースのファイル名 (拡張子なし)、または XML 要素内のリソースの android: name 属性の値です。

## <a name="referencing-resources-from-xml"></a>参照 (XML からリソースを)

XML ファイル内のリソースには、次の特殊な構文でアクセスします。

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

- **PackageName**&ndash;リソースを提供するパッケージ。他のパッケージのリソースが使用されている場合にのみ必要です。

- **ResourceType**&ndash;これは、リソースクラス内にある入れ子になったリソースの種類です。

- **リソース名**これは、リソースのファイル名 (ファイルの種類の拡張子を*除く*)、または`android:name` XML 要素内のリソースの属性の値です。 &ndash;

たとえば、次のようなレイアウトファイルの内容があり**ます。**

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

この例には[`ImageView`](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) 、"**フラグ**" という名前のリソースを必要とするがあります。 で`ImageView`は、 `src`属性がに`@drawable/flag`設定されています。 アクティビティが開始されると、Android は、**フラグ .png**という名前のファイル (たとえば、ファイル拡張子が**flag .jpg**などの別のイメージ形式 **) である**ことを確認し、そのファイルをに読み込んで`ImageView`表示します。
このアプリケーションを実行すると、次の図のようになります。

![ローカライズされた ImageView](android-resource-basics-images/03-localized-screenshot.png)
