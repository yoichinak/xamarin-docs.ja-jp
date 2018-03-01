---
title: "指紋認証ガイダンス"
ms.topic: article
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20defea58ec0c09becd5a3cec1961806cb0acc1d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="fingerprint-authentication-guidance"></a>指紋認証ガイダンス

## <a name="fingerprint-authentication-guidance"></a>指紋認証ガイダンス

概念は、これまで見てきたし、Android 6.0 を囲む Api 指紋認証、指紋 Api を使用するための一般的なアドバイスについて説明します。

1. **Android のサポート ライブラリ v4 互換性 Api を使用して**&ndash;これをコードから API チェックを削除することで、アプリケーション コードを簡略化し、最も可能性のデバイスを対象とするアプリケーションを許可します。
2. **代替手段指紋認証を提供**&ndash;指紋認証は、アプリケーションのユーザーを認証するための優れた、クイック方法、ただし、それを想定できないことには常に作業したり利用可能にします。 か可能性こと指紋スキャナーが失敗する可能性があります、レンズはおそらくダーティで、ユーザーに指紋認証を使用するデバイスを構成ことがない可能性があります、指紋が不足しているのでがなった。 ユーザーがアプリケーションを使用して、指紋認証を使用する希望可能性がありますしないこともできます。 これらの理由から、Android アプリケーションは、ユーザー名やパスワードなどの代替の認証プロセスを提供する必要があります。
3. **Google の指紋アイコンを使用して**&ndash;すべてのアプリケーションが Google から提供される同じ指紋アイコンを使用する必要があります。 標準的なアイコンを使用できるようになります Android ユーザー アプリで指紋認証が使用される場所を認識します。 
    
    ![Android の指紋アイコン](summary-images/ic-fp-40px.png)
    
4. **ユーザーに通知する**&ndash;アプリケーションは、いくつかの種類の指紋スキャナーがアクティブであるユーザーに通知を表示する必要がありますとタッチまたはスワイプを待機します。 

## <a name="summary"></a>まとめ

指紋認証は、アプリ内購入など、機密性の高い機能と対話するユーザーに容易にユーザーをすばやく検証 Xamarin.Android アプリケーションを許可する優れた方法です。 このガイドでは、概念と API が Xamarin.Android アプリケーションでの Android 6.0 指紋を組み込むために必要なコードについて説明します。

API の自体には、指紋を説明した最初`FingerprintManager`(および`FingerprintManagerCompat`)。 について説明しましたが、どのように`FingerprintManager.AuthenticationCallbacks`抽象クラスは、アプリケーションによって拡張し、指紋ハードウェアと、アプリケーション自体の間の媒介として使用する必要があります。 Java を使用して指紋スキャナーの結果の整合性を確認する方法について説明しましたし、`Cipher`オブジェクト。 最後に、触れたもう少しデバイスで指紋を登録する方法を説明しを使用して、テストで**adb**をエミュレーターで指紋スワイプをシミュレートします。 

完了していない場合かを確認、[サンプル アプリケーション](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)このガイドに付属しています。 [指紋ダイアログ サンプル](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)Xamarin.Android に Java から移植されたし、指紋認証 Android アプリケーションを追加する方法の別の例を提供します。



## <a name="related-links"></a>関連リンク

- [指紋ガイド サンプル アプリ](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [指紋ダイアログのサンプル](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [指紋アイコン](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
