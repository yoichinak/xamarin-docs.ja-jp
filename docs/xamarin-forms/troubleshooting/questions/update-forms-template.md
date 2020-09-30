---
title: Xamarin.Forms既定のテンプレートを新しい NuGet パッケージに更新することはできますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9dc5b8e63a35c3a0b797d0794af7a31aea4969c9
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555594"
---
# <a name="can-i-update-the-no-locxamarinforms-default-template-to-a-newer-nuget-package"></a>Xamarin.Forms既定のテンプレートを新しい NuGet パッケージに更新することはできますか。

このガイドでは、 Xamarin.Forms 例として .NET Standard ライブラリテンプレートを使用しますが、共有プロジェクトテンプレートでも同じ一般的な方法を使用でき Xamarin.Forms ます。 このガイドは、1.5.1.6471 から2.1.0.6529 に更新する例を基に記述されてい Xamarin.Forms ますが、同じ手順で他のバージョンを既定値として設定することもできます。

1. 元のテンプレートを `.zip` 次のものからコピーします。

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. を `.zip` 一時的な場所に解凍します。

3. 古いバージョンのパッケージのすべての出現箇所を、 Xamarin.Forms 使用する新しいバージョンに変更します。
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    例: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. メインの [複数プロジェクトのテンプレートファイル](/visualstudio/ide/how-to-create-multi-project-templates) () の "name" 要素を変更し `Xamarin.Forms.PCL.vstemplate` て、一意にしてください。 次に例を示します。

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. テンプレートフォルダー全体を再 zip します。 ファイルの元のファイル構造と一致していることを確認し `.zip` ます。 ファイルは、 `Xamarin.Forms.PCL.vstemplate` フォルダー内ではなく、ファイルの先頭に配置する必要があり `.zip` ます。

6. ユーザーごとの Visual Studio テンプレートフォルダーに "Mobile Apps" サブディレクトリを作成します。
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. 新しい zip 形式のテンプレートフォルダーを新しい "Mobile Apps" ディレクトリにコピーします。

8. 手順 3. のバージョンと一致する NuGet パッケージをダウンロードします。 たとえば、 [ https://nuget.org/api/v2/package/ Xamarin.Forms /2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (「」も参照 [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file) ) を実行し、Xamarin Visual Studio extensions フォルダーの適切なサブフォルダーにコピーします。
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`