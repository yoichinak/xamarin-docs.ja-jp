---
ms.openlocfilehash: 05f1017f8c4b306996d3e8e165511ff9062a1026
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61047859"
---

以下のコマンドラインは、iPhone 用の **SOLUTION_FILE.sln** というソリューションのリリースビルドを指定するものです。 指定することで、IPA の場所を設定することができます、`IpaPackageDir`コマンドラインでのプロパティ。

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


`IpaPackageDir` プロパティについてのより詳しい情報は、[iOS 9.8 のリリースノート](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) を参照してください。
