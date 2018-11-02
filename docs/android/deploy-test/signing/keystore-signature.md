---
title: キーストアの署名の検索
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d32b2a20fee6b2bb007ee620e0ae4203e950bb98
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112465"
---
# <a name="finding-your-keystores-signature"></a>キーストアの署名の検索

Xamarin.Android アプリの MD5 または SHA1 署名は、APK の署名に使用された **.keystore** ファイルに依存します。 通常、デバッグ ビルドではリリース ビルドとは異なる **.keystore** ファイルが使用されます。

## <a name="for-debug--non-custom-signed-builds"></a>デバッグ/非カスタム署名付きビルドの場合

Xamarin.Android は同じ **debug.keystore** ファイルですべてのデバッグ ビルドに署名します。 このファイルは Xamarin.Android が最初にインストールされたときに生成されます。以下の手順で、既定の Xamarin.Android **debug.keystore** ファイルの MD5 または SHA1 署名を探すプロセスについて説明します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

アプリの署名に利用された Xamarin **debug.keystore** ファイルを見つけます。 既定では、Xamarin.Android アプリケーションのデバッグ バージョンの署名に利用されたキーストアは次の場所にあります。

**C:\\Users\\*ユーザー名*\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

キーストアに関する情報は、JDK から `keytool.exe` コマンドを実行して取得できます。 このツールは通常、次の場所にあります。

**C:\\Program Files (x86)\\Java\\jdk*VERSION*\\bin\\keytool.exe**

**keytool.exe** を含むディレクトリを `PATH` 環境変数に追加します。
**コマンド プロンプト**を開き、次のコマンドで `keytool.exe` を実行します。

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

実行すると、**keytool.exe** は次のテキストを出力します。 **MD5:** ラベルと **SHA1:** ラベルで、それぞれの署名が確認されます。

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

アプリの署名に利用された Xamarin **debug.keystore** ファイルを見つけます。 既定では、Xamarin.Android アプリケーションのデバッグ バージョンの署名に利用されたキーストアは次の場所にあります。

**~/.local/share/Xamarin/Mono for Android/debug.keystore**


キーストアに関する情報は、JDK から **keytool** コマンドを実行して取得できます。 このツールは通常、次の場所にあります。

**/System/Library/Java/JavaVirtualMachines/*VERSION*.jdk/Contents/Home/bin/keytool**

**keytool** を含むディレクトリを **PATH** 環境変数に追加します。
**ターミナル**を開き、次のコマンドを使用して **keytool** を実行します。

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

実行すると、**keytool** は次のテキストを出力します。 **MD5:** ラベルと **SHA1:** ラベルで、それぞれの署名が確認されます。

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## <a name="for-release--custom-signed-builds"></a>リリース/カスタム署名付きビルドの場合

カスタム **.keystore** ファイルで署名されたリリース ビルドのプロセスは上と同じですが、リリース **.keystore** ファイルが Xamarin.Android で使用される **debug.keystore** ファイルに代わります。 リリース キーストア ファイルの作成時のキーストア パスワードとエイリアス名を独自の値に変更します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio の **[配布]** ウィザードで Xamarin.Android アプリに署名すると、キーストアは次の場所に置かれます。

**C:\\Users\\*ユーザー名*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\*別名*\\*別名*.keystore**

たとえば、「[新しい証明書の作成](~/android/deploy-test/signing/index.md#newcertvs)」の手順で新しい署名キーを作成した場合、キーストアは次の場所に置かれます。

**C:\\Users\\*ユーザー名*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\chimp\\chimp.keystore**

Xamarin.Android アプリに署名する方法については、「[Android アプリケーション パッケージに署名する](~/android/deploy-test/signing/index.md)」を参照してください。


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac の **[署名と配布...]** ウィザードでアプリに署名すると、結果のキーストアは次の場所に置かれます。

**~/Library/Developer/Xamarin/Keystore/*alias*/*alias*.keystore**

たとえば、「[新しい証明書の作成](~/android/deploy-test/signing/index.md#newcertxs)」の手順で新しい署名キーを作成した場合、キーストアは次の場所に置かれます。

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**

Xamarin.Android アプリに署名する方法については、「[Android アプリケーション パッケージに署名する](~/android/deploy-test/signing/index.md)」を参照してください。


-----
