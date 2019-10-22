---
ms.openlocfilehash: 31c8d1b781648f2d9c69062c52e71478fc7a496b
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "69529127"
---

次のコマンドラインを使用して、iPhone のソリューション**SOLUTION_FILE**のリリースビルドを指定します。 IPA の場所は、コマンドラインで `IpaPackageDir` プロパティを指定することによって設定できます。

- Mac では、次のように**xbuild**を使用します。

  ```
  xbuild /p:Configuration="Release" \ 
         /p:Platform="iPhone" \ 
         /p:IpaPackageDir="$HOME/Builds" \
         /t:Build MyProject.sln
  ```

**Xbuild**コマンドは、通常、directory **/Library/framework/コマンド**で見つかります。

- Windows の場合、 **msbuild**を使用します。

  ```
  msbuild /p:Configuration="Release" 
          /p:Platform="iPhone" 
          /p:IpaPackageDir="%USERPROFILE%\Builds" 
          /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
          /t:Build MyProject.sln
  ```

**msbuild**では、コマンドラインによって渡された `$( )` 式が自動的に展開されることはありません。 このため、コマンドラインで `IpaPackageDir` を設定するときは、完全なパスを使用することをお勧めします。

@No__t_1 プロパティの詳細については、「 [iOS 9.8 のリリースノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location)」を参照してください。
