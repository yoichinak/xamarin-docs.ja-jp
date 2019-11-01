---
title: Xamarin.Forms の既定のテンプレートを新しい NuGet パッケージに更新することはできますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 4a628deb3e6f9282d49d71ac694506c3a0616ee9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73005390"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Xamarin.Forms の既定のテンプレートを新しい NuGet パッケージに更新することはできますか。

このガイドでは、例として Xamarin Forms .NET Standard ライブラリテンプレートを使用しますが、同じ一般的な方法は、Xamarin. Forms 共有プロジェクトテンプレートでも機能します。 このガイドは、1.5.1.6471 から2.1.0.6529 に更新する例を基に記述されていますが、同じ手順で他のバージョンを既定値として設定することもできます。

1. 元のテンプレート `.zip` を次のものからコピーします。

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. `.zip` を一時的な場所に解凍します。

3. 以前のバージョンの Xamarin. Forms パッケージのすべての出現箇所を、使用する新しいバージョンに変更します。
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    例: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. メインの[マルチプロジェクトテンプレートファイル](https://msdn.microsoft.com/library/ms185308.aspx)(`Xamarin.Forms.PCL.vstemplate`) の "name" 要素を変更して、一意にしてください。 (例:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. テンプレートフォルダー全体を再 zip します。 `.zip` ファイルの元のファイル構造と一致していることを確認します。 `Xamarin.Forms.PCL.vstemplate` ファイルは、フォルダー内ではなく、`.zip` ファイルの先頭に配置する必要があります。

6. ユーザーごとの Visual Studio テンプレートフォルダーに "Mobile Apps" サブディレクトリを作成します。
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. 新しい zip 形式のテンプレートフォルダーを新しい "Mobile Apps" ディレクトリにコピーします。

8. 手順 3. のバージョンと一致する NuGet パッケージをダウンロードします。 たとえば、 [https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (「 [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)」も参照)、Xamarin Visual Studio extensions フォルダーの適切なサブフォルダーにコピーします。
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
