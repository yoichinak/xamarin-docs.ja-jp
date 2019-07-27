---
ms.openlocfilehash: e383bbccd4e76be8a208f5680e5cf21e45a0dbc3
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511945"
---

以下のコマンドラインは、iPhone 用の **SOLUTION_FILE.sln** というソリューションのリリースビルドを指定するものです。 IPA の場所を設定するには、コマンドライン`IpaPackageDir`でプロパティを指定します。

- Mac における **xbuild** の使用:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

**Xbuild** コマンドは、通常 **/Library/Frameworks/Mono.framework/Commands** ディレクトリにあります。

- Windows における **msbuild** の使用:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**msbuild** は、コマンドラインによって渡される `$( )` 式を、自動的に展開しません。 このため、コマンドラインで `IpaPackageDir` を設定する場合は、フルパスを使うことをお勧めします。

`IpaPackageDir` プロパティについてのより詳しい情報は、[iOS 9.8 のリリースノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location) を参照してください。
