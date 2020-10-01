---
title: 指紋認証のガイダンス
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: aaa67972afeffd10c038a145a5703e917647b0fb
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453871"
---
# <a name="fingerprint-authentication-guidance"></a>指紋認証のガイダンス

## <a name="fingerprint-authentication-guidance"></a>指紋認証のガイダンス

Android 6.0 の指紋認証に関する概念と API がわかったので、次は Fingerprint API の使用に関する一般的なアドバイスについて説明します。

1. **Android Support Library v4 Compatibility API 使用する** &ndash; コードで API をチェックする必要がないのでアプリケーションのコードが簡素化され、アプリケーションで可能な限り多くのデバイスを対象にできます。
2. **指紋認証の代替手段を提供する** &ndash; 指紋認証は、アプリケーションでユーザーをすばやく認証できる優れた方法ですが、常に動作していたり使用可能であるとは限りません。 指紋スキャナーが故障していたり、レンズが汚れていたり、ユーザーが指紋認証を使用するようにデバイスを構成していなかったり、指紋が失われていたりする可能性があります。 また、ユーザーがアプリケーションで指紋認証を使用することを望まない場合もあります。 このような理由から、Android アプリケーションでは、ユーザー名とパスワードのような、代替認証プロセスを用意する必要があります。
3. **Google の指紋アイコンを使用する** &ndash; すべてのアプリケーションでは、Google によって提供される同じ指紋アイコンを使用する必要があります。 標準のアイコンを使用すると、Android ユーザーが、アプリ内で指紋認証が使用されている場所を簡単に認識できるようになります。 
    
    ![Android の指紋アイコン](summary-images/ic-fp-40px.png)
    
4. **ユーザーに通知する** &ndash; アプリケーションでは、指紋スキャナーがアクティブになっていて、タッチまたはスワイプを待っていることを、ユーザーに通知する必要があります。 

## <a name="summary"></a>まとめ

指紋認証は、Xamarin.Android アプリケーションでユーザーを迅速に検証し、ユーザーがアプリ内購入などの重要な機能を簡単に操作できるようにするための、優れた方法です。 このガイドでは、Android 6.0 Fingerprint API を Xamarin.Android アプリケーションに組み込むために必要な概念とコードについて説明しました。

最初に、Fingerprint API 自体である `FingerprintManager` (および `FingerprintManagerCompat`) について説明しました。 `FingerprintManager.AuthenticationCallbacks` 抽象クラスをアプリケーションで拡張し、指紋ハードウェアとアプリケーション自体の仲介役として使用する方法について説明しました。 次に、Java の `Cipher` オブジェクトを使用して、指紋スキャナーの結果の整合性を確認する方法について説明しました。 最後に、デバイスに指紋を登録し、**adb** を使用してエミュレーターで指紋のスワイプをシミュレートする方法を示すことで、テストについて少し説明しました。 

まだ行っていない場合は、このガイドに付属している[サンプル アプリケーション](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)を参照してください。 [Fingerprint Dialog Sample](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) は Java から Xamarin.Android に移植されており、Android アプリケーションに指紋認証を追加する方法についてのもう 1 つの例が提供されます。

## <a name="related-links"></a>関連リンク

- [指紋ガイド サンプル アプリ](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Fingerprint Dialog Sample](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [指紋アイコン](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)