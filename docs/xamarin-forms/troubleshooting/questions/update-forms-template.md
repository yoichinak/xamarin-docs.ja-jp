---
title: 最新の NuGet パッケージに Xamarin.Forms の既定のテンプレートを更新できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fc479b4b0651e3312b855673730be21c2076d833
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>最新の NuGet パッケージに Xamarin.Forms の既定のテンプレートを更新できますか。

このガイドは、例として、Xamarin.Forms .NET 標準のライブラリ テンプレートを使用しますが、同じ一般的な方法は、Xamarin.Forms 共有プロジェクト テンプレートのでも機能します。 このガイドは、Xamarin.Forms 1.5.1.6471 に 2.1.0.6529 からの更新の例で記述されたが、同じ手順は、既定値としてその他のバージョンを設定することです。

1.  元のテンプレートをコピー`.zip`から。

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  解凍、`.zip`一時的な場所にします。

3.  使用するには、新しいバージョンには、すべてのフォームのパッケージの古いバージョンの出現回数を変更します。
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    例: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  メインの"name"要素を変更[複数プロジェクトのテンプレート ファイル](http://msdn.microsoft.com/library/ms185308.aspx)(`Xamarin.Forms.PCL.vstemplate`) 一意になるようにします。 例えば:
    > <Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>

5.  再度、全体のテンプレート フォルダーを圧縮します。 元のファイル構造と一致することを確認、`.zip`ファイル。 `Xamarin.Forms.PCL.vstemplate`ファイルがの上部にする必要があります、`.zip`任意のフォルダー内ではなく、ファイルです。

6.  ユーザーごとの Visual Studio のテンプレート フォルダーで"Mobile Apps"サブディレクトリを作成します。
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  新しい"Mobile Apps"ディレクトリに新しい zip 形式のテンプレート フォルダーをコピーします。

8.  手順 3 からバージョンと一致する NuGet パッケージをダウンロードします。 たとえば、 [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (も参照してください[ http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file))、し、適切な Visual Studio の Xamarin 拡張機能フォルダーのサブフォルダーにコピーします。
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
