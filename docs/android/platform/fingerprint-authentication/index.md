---
title: "指紋認証"
description: "このガイドでは、Xamarin.Android アプリケーションに、Android 6.0 で導入された、指紋認証を追加する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 79f5f81e11f62359c3b951500d4ab5cbd63fb507
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="fingerprint-authentication"></a>指紋認証

_このガイドでは、Xamarin.Android アプリケーションに、Android 6.0 で導入された、指紋認証を追加する方法について説明します。_


## <a name="fingerprint-authentication-overview"></a>指紋認証の概要

Android デバイスで指紋スキャナーの到着時は、ユーザー認証の代わりに、従来のユーザー名/パスワード メソッドを使用してアプリケーションを提供します。 ユーザーの認証に指紋を使用できるようになります、ユーザー名とパスワードよりも小さい関与するレベルはセキュリティを組み込むアプリケーションです。

FingerprintManager Api 指紋スキャナーにデバイスを対象に実行している API level 23 (Android 6.0) またはそれ以降。 Api に含まれて、`Android.Hardware.Fingerprints`名前空間。 Android のサポート ライブラリ v4 は、指紋の古いバージョンの Android 用の Api のバージョンを提供します。 互換性 Api 内にある、`Android.Support.v4.Hardware.Fingerprint`名前空間、経由で配布されて、 [Xamarin.Android.Support.v4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)です。

[FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (とそのサポート ライブラリの対応する[FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) は、ハードウェアのスキャン、指紋を使用するための主要なクラスです。 このクラスは、ハードウェア自体との対話を管理するシステム レベルのサービスを Android SDK でラッパーです。 指紋スキャナーを起動およびスキャナーからのフィードバックに応答を行います。 このクラスは、3 つだけのメンバーと非常に簡単なインターフェイスには。

* **`Authenticate`** &ndash; このメソッドは、ハードウェアのスキャナーを初期化し、ユーザーの指紋をスキャンするを待って、バック グラウンドで、サービスの開始。
* **`EnrolledFingerprints`** &ndash; このプロパティは返します`true`ユーザーが 1 つまたは複数の指紋をデバイスに登録されている場合。
* **`HardwareDetected`** &ndash; このプロパティは、デバイスが指紋をスキャンをサポートしているかどうかに使用されます。

`FingerprintManager.Authenticate`メソッドは、指紋スキャナーを開始する Android アプリケーションによって使用されます。 次のスニペットは、サポート ライブラリの互換性の Api を使用してを起動する方法の例を示します。

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

このガイドには、使用する方法を検討、`FingerprintManager`指紋認証での Android アプリケーションを強化するための Api です。 インスタンスを作成し、作成する方法に適用されます、`CryptoObject`指紋スキャナーからの結果を保護するためにします。 サブクラスをアプリケーションがどのように検証`FingerprintManager.AuthenticationCallback`指紋スキャナーからのフィードバックに応答しています。 最後に、後ほどお見せ、Android デバイスまたはエミュレーターにフィンガー プリントを登録する方法と使用方法**adb**指紋スキャンをシミュレートします。

## <a name="requirements"></a>必要条件

指紋認証には、Android 6.0 (API level 23) が必要です。 または高い、指紋スキャナーを持つデバイス。 

指紋は、認証されている各ユーザーのデバイスで登録されている必要があります。 これには、パスワード、PIN、スワイプ パターンまたは顔認識を使用する画面のロックを設定が含まれます。 一部の Android エミュレーターで指紋認証機能をシミュレートすることができます。  これら 2 つのトピックの詳細についてを参照してください、[フィンガー プリントを登録する](enrolling-fingerprint.md)セクションです。 






## <a name="related-links"></a>関連リンク

- [指紋ガイド サンプル アプリ](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [指紋ダイアログのサンプル](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [実行時にアクセス許可を要求します。](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [フィンガー プリントおよび支払いの API (ビデオ)](https://youtu.be/VOn7VrTRlA4)
