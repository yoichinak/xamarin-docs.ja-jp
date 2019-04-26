---
title: 指紋認証のガイダンス
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 535eabe07cb4f4d36e6a6f918b5717efcc99185d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023561"
---
# <a name="fingerprint-authentication-guidance"></a>指紋認証のガイダンス

## <a name="fingerprint-authentication-guidance"></a>指紋認証のガイダンス

概念きましたし、Android 6.0 を囲む Api 指紋認証、指紋 Api を使用するための一般的なアドバイスについて説明します。

1. **Android サポート ライブラリ v4 互換性 Api を使用して** &ndash; API チェックをコードから削除することで、アプリケーション コードを簡略化され、最も可能性のデバイスを対象とするアプリケーションが許可されます。
2. **指紋認証に代わる手段を提供**&ndash;指紋認証がユーザーを認証するアプリケーションの優れた、簡単な方法で、ただし、その想定できない、または常に作業ができることです。 いる指紋スキャナーが失敗する可能性があります、レンズかもしれませんダーティ、ユーザーに指紋認証を使用するデバイスを構成ことがない可能性があります、指紋のため不明になったことができます。 ユーザーがアプリケーションで指紋認証を使用するに行わないこともできます。 これらの理由から、Android アプリケーションは、ユーザー名とパスワードなど、代替の認証プロセスを提供する必要があります。
3. **Google の指紋アイコンを使用して**&ndash;すべてのアプリケーションが Google から提供される同じ指紋アイコンを使用する必要があります。 標準的なアイコンの使用を簡単にアプリで指紋認証が使用されていることを認識する Android ユーザー向け。 
    
    ![Android の指紋アイコン](summary-images/ic-fp-40px.png)
    
4. **ユーザーに通知する**&ndash;アプリケーションは、ある種の指紋スキャナーがアクティブであるユーザーに通知を表示する必要があり、タッチまたはスワイプを待機しています。 

## <a name="summary"></a>まとめ

指紋認証は、ユーザー、アプリ内購入など、機密性の高い機能との対話をユーザーに、簡単に迅速に確認の Xamarin.Android アプリケーションを許可する優れた方法です。 このガイドでは、概念と API は、Xamarin.Android アプリケーションでの Android 6.0 フィンガー プリントを組み込むために必要なコードについて説明します。

API の自体、指紋を説明した最初`FingerprintManager`(と`FingerprintManagerCompat`)。 調べる方法、`FingerprintManager.AuthenticationCallbacks`抽象クラスは、アプリケーションによって拡張および指紋ハードウェアとアプリケーション自体の間の仲介役として使用する必要があります。 Java を使用して指紋スキャナーの結果の整合性を検証する方法について確認しましたし`Cipher`オブジェクト。 最後に、説明したビットを使用して、デバイスで指紋を登録する方法を説明するテスト**adb**をエミュレーターで指紋スワイプをシミュレートします。 

まだ行っていない、する必要がありますを調べて、[サンプル アプリケーション](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)このガイドに付属します。 [指紋ダイアログ サンプル](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)Java から Xamarin.Android に移植されていますが、Android アプリケーションに指紋認証を追加する方法の別の例を提供します。



## <a name="related-links"></a>関連リンク

- [指紋のガイド サンプル アプリ](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [指紋ダイアログのサンプル](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [指紋アイコン](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
