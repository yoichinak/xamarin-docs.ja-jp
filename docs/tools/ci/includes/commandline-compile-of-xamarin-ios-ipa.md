---
ms.openlocfilehash: 31c8d1b781648f2d9c69062c52e71478fc7a496b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529127"
---

以下のコマンドラインは、iPhone 用の **SOLUTION_FILE.sln** というソリューションのリリースビルドを指定するものです。 IPA の場所を設定するには、コマンドライン`IpaPackageDir`でプロパティを指定します。

- Mac における **xbuild** の使用:

  ```
  xbuild /p:Configuration="Release" \ 
         /p:Platform="iPhone" \ 
         /p:IpaPackageDir="$HOME/Builds" \
         /t:Build MyProject.sln
  ```

**Xbuild** コマンドは、通常 **/Library/Frameworks/Mono.framework/Commands** ディレクトリにあります。

- Windows における **msbuild** の使用:

  ```
  msbuild /p:Configuration="Release" 
          /p:Platform="iPhone" 
          /p:IpaPackageDir="%USERPROFILE%\Builds" 
          /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
          /t:Build MyProject.sln
  ```

**msbuild** は、コマンドラインによって渡される `$( )` 式を、自動的に展開しません。 このため、コマンドラインで `IpaPackageDir` を設定する場合は、フルパスを使うことをお勧めします。

`IpaPackageDir` プロパティについてのより詳しい情報は、[iOS 9.8 のリリースノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location) を参照してください。
