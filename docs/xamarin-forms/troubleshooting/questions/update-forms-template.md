---
title: Xamarin.Forms の既定のテンプレートを新しい NuGet パッケージに更新することはできますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 0c0b5e04bafc748b48ea007162cfd7277cc23752
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528345"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Xamarin.Forms の既定のテンプレートを新しい NuGet パッケージに更新することはできますか。

このガイドでは、例として Xamarin Forms .NET Standard ライブラリテンプレートを使用しますが、同じ一般的な方法は、Xamarin. Forms 共有プロジェクトテンプレートでも機能します。 このガイドは、1.5.1.6471 から2.1.0.6529 に更新する例を基に記述されていますが、同じ手順で他のバージョンを既定値として設定することもできます。

1. 元のテンプレート`.zip`を次のものからコピーします。

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. を一時的な場所に解凍します。`.zip`

3. 以前のバージョンの Xamarin. Forms パッケージのすべての出現箇所を、使用する新しいバージョンに変更します。
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    よう`<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. メインの[複数プロジェクトのテンプレートファイル](https://msdn.microsoft.com/library/ms185308.aspx)(`Xamarin.Forms.PCL.vstemplate`) の "name" 要素を変更して、一意にしてください。 例えば:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. テンプレートフォルダー全体を再 zip します。 `.zip`ファイルの元のファイル構造と一致していることを確認します。 ファイルは、フォルダー内ではなく、 `.zip`ファイルの先頭に配置する必要があります。 `Xamarin.Forms.PCL.vstemplate`

6. ユーザーごとの Visual Studio テンプレートフォルダーに "Mobile Apps" サブディレクトリを作成します。
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. 新しい zip 形式のテンプレートフォルダーを新しい "Mobile Apps" ディレクトリにコピーします。

8. 手順 3. のバージョンと一致する NuGet パッケージをダウンロードします。 たとえば、 [http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (「」も[https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)参照) を実行し、Xamarin Visual Studio extensions フォルダーの適切なサブフォルダーにコピーします。
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
