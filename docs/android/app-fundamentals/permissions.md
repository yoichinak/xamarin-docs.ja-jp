---
title: Xamarin. Android のアクセス許可
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: d3513dc086998306ae79855a2c4d8c1a3060ac47
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249707"
---
# <a name="permissions-in-xamarinandroid"></a>Xamarin. Android のアクセス許可

## <a name="overview"></a>概要

Android アプリケーションは独自のサンドボックスで実行され、セキュリティ上の理由から、デバイス上の特定のシステムリソースやハードウェアにアクセスすることはできません。 ユーザーは、これらのリソースを使用する前に、アプリに対するアクセス許可を明示的に付与する必要があります。 たとえば、アプリケーションは、ユーザーからの明示的なアクセス許可なしにデバイスの GPS にアクセスすることはできません。 アプリがアクセス許可`Java.Lang.SecurityException`なしに保護されたリソースにアクセスしようとすると、Android によってがスローされます。

アクセス許可は、アプリの開発時にアプリケーション開発者が**Androidmanifest .xml**で宣言します。 Android には、これらのアクセス許可に対するユーザーの同意を取得するための2つの異なるワークフローがあります。

- Android 5.1 (API レベル 22) 以下を対象とするアプリの場合、アプリのインストール時にアクセス許可要求が発生しました。 ユーザーがアクセス許可を付与しなかった場合、アプリはインストールされません。 アプリをインストールした後は、アプリをアンインストールしない限り、アクセス許可を取り消すことはできません。
- Android 6.0 (API レベル 23) 以降では、ユーザーはアクセス許可をより細かく制御できるようになりました。アプリがデバイスにインストールされている限り、アクセス許可の付与または取り消しを行うことができます。 このスクリーンショットは、Google Contacts アプリのアクセス許可の設定を示しています。 さまざまなアクセス許可の一覧が表示され、ユーザーはアクセス許可を有効または無効にすることができます。

![サンプルアクセス許可画面](permissions-images/01-permissions-check.png) 

Android アプリは、保護されたリソースへのアクセス許可があるかどうかを確認するために、実行時に確認する必要があります。 アプリにアクセス許可がない場合は、ユーザーがアクセス許可を付与するために、Android SDK によって提供される新しい Api を使用して要求を行う必要があります。 アクセス許可は、次の2つのカテゴリに分類されます。

- **通常のアクセス許可**&ndash;これらのアクセス許可は、ユーザーのセキュリティまたはプライバシーに対してセキュリティ上の危険を最小限に抑えます。 Android 6.0 では、インストール時に通常のアクセス許可が自動的に付与されます。 [通常のアクセス許可の完全な一覧](https://developer.android.com/guide/topics/permissions/normal-permissions.html)については、Android のドキュメントを参照してください。
- **危険なアクセス許可**&ndash;通常のアクセス許可とは異なり、危険なアクセス許可とは、ユーザーのセキュリティまたはプライバシーを保護するアクセス許可のことです。 これらは、ユーザーが明示的に付与する必要があります。 SMS メッセージの送信または受信は、危険なアクセス許可を必要とするアクションの一例です。

> [!IMPORTANT]
> 権限が属しているカテゴリは、時間の経過と共に変わる可能性があります。  "通常の" アクセス許可として分類されたアクセス許可は、将来の API レベルで危険なアクセス許可に昇格される可能性があります。

危険なアクセス許可は、さらに[_アクセス許可グループ_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)に分類されます。 アクセス許可グループには、論理的に関連するアクセス許可が保持されます。 ユーザーがアクセス許可グループの1つのメンバーにアクセス許可を付与すると、Android はそのグループのすべてのメンバーに対してアクセス許可を自動的に付与します。 たとえば、 [`STORAGE`](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE)アクセス許可グループには、と`WRITE_EXTERNAL_STORAGE` `READ_EXTERNAL_STORAGE`の両方のアクセス許可が保持されます。 ユーザーがへ`READ_EXTERNAL_STORAGE` `WRITE_EXTERNAL_STORAGE`のアクセス許可を付与した場合、アクセス許可は同時に自動的に付与されます。

1つ以上のアクセス許可を要求する前に、アプリケーションがアクセス許可を要求する前にアクセス許可を必要とする理由について、論理的な方法を提供することをお勧めします。 ユーザーが論理的根拠を理解すると、アプリはユーザーにアクセス許可を要求できます。 この原理を理解することで、アクセス許可を付与する必要がある場合にユーザーが情報に基づいた決定を行い、影響がない場合はその影響を理解することができます。 

アクセス許可の確認と要求のワークフロー全体は、_実行時のアクセス許可_のチェックとして知られており、次の図にまとめることができます。 

[![実行時のアクセス許可のチェックフローチャート](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android サポートライブラリには、以前のバージョンの Android に対するアクセス許可用の新しい Api の一部があります。 これらの移植 Api は、デバイス上の Android のバージョンを自動的にチェックするため、毎回 API レベルのチェックを実行する必要はありません。  

このドキュメントでは、Xamarin Android アプリケーションにアクセス許可を追加する方法と、Android 6.0 (API レベル 23) 以降を対象とするアプリで実行時のアクセス許可チェックを実行する方法について説明します。

> [!NOTE]
> ハードウェアのアクセス許可が、Google Play によるアプリのフィルター処理に影響を与える可能性があります。 たとえば、アプリにカメラのアクセス許可が必要な場合、Google Play は、カメラがインストールされていないデバイスの Google Play ストアにアプリを表示しません。

<a name="requirements" />

## <a name="requirements"></a>要件

Xamarin Android プロジェクトには、 [xamarin. android. Support. 互換](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)NuGet パッケージを含めることを強くお勧めします。 このパッケージは、アクセス許可固有の Api を以前のバージョンの Android にバックポートし、アプリが実行されている Android のバージョンを常に確認する必要がない1つの共通インターフェイスを提供します。

## <a name="requesting-system-permissions"></a>システムアクセス許可の要求

Android のアクセス許可を操作するための最初の手順は、Android マニフェストファイルでアクセス許可を宣言することです。 これは、アプリがターゲットとしている API レベルに関係なく実行する必要があります。

Android 6.0 以降を対象とするアプリでは、ユーザーが過去のある時点でアクセス許可を付与したため、アクセス許可が次回有効になることを想定できません。 Android 6.0 を対象とするアプリは、常にランタイムアクセス許可チェックを実行する必要があります。 Android 5.1 以降を対象とするアプリでは、実行時のアクセス許可チェックを実行する必要はありません。

> [!NOTE]
> アプリケーションは、必要なアクセス許可のみを要求します。

### <a name="declaring-permissions-in-the-manifest"></a>マニフェストでのアクセス許可の宣言

アクセス許可は、 `uses-permission`要素を使用して**androidmanifest .xml**に追加されます。 たとえば、アプリケーションがデバイスの位置を特定する場合、十分な場所のアクセス許可が必要です。 マニフェストには、次の2つの要素が追加されます。 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio に組み込まれているツールのサポートを使用して、アクセス許可を宣言できます。

1. **ソリューションエクスプローラー**で **[プロパティ]** をダブルクリックし、プロパティウィンドウの **[Android マニフェスト]** タブを選択します。

    [![[Android マニフェスト] タブで必要なアクセス許可](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. アプリケーションに androidmanifest .xml がまだない場合は、[いいえ] **をクリックします。次のように**クリックして追加します。

    [!["いいえ" (AndroidManifest .xml) メッセージ](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. **[必要なアクセス許可]** ボックスの一覧から、アプリケーションに必要なアクセス許可を選択して保存します。

    [![選択されたカメラのアクセス許可の例](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac に組み込まれているツールのサポートを使用して、アクセス許可を宣言できます。

1. **Solution Pad**でプロジェクトをダブルクリックし、[オプション] を選択 **> Android アプリケーション > ビルド**します。

    [![表示される必要なアクセス許可セクション](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. プロジェクトに**Androidmanifest .xml**がまだない場合は、 **[Android マニフェストの追加]** ボタンをクリックします。

    [![プロジェクトの Android マニフェストが見つかりません](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. **必要なアクセス**許可の一覧からアプリケーションで必要なアクセス許可を選択し、[ **OK]** をクリックします。

    [![選択されたカメラのアクセス許可の例](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin では、ビルド時に一部のアクセス許可がデバッグビルドに自動的に追加されます。 これにより、アプリケーションのデバッグが容易になります。 特に、と`INTERNET` `READ_EXTERNAL_STORAGE`の2つの注目すべきアクセス許可があります。 これらの自動的に設定されたアクセス許可は、 **[必要なアクセス許可]** の一覧で有効になっていません。 ただし、リリースビルドでは、**必要なアクセス許可**一覧に明示的に設定されているアクセス許可のみを使用します。 

Android 5.1 (API レベル 22) 以下を対象とするアプリの場合は、それ以上の作業は必要ありません。 Android 6.0 (API 23 レベル 23) 以降で実行されるアプリは、実行時のアクセス許可チェックを実行する方法について、次のセクションに進む必要があります。 

### <a name="runtime-permission-checks-in-android-60"></a>Android 6.0 でのランタイムアクセス許可の確認

(Android サポートライブラリで利用可能)メソッドを使用して、特定のアクセス許可が付与されているかどうかを確認します。`ContextCompat.CheckSelfPermission` このメソッドは、次[`Android.Content.PM.Permission`](xref:Android.Content.PM.Permission)の2つの値のいずれかを持つ列挙型を返します。

- **`Permission.Granted`** &ndash;指定された権限が許可されています。
- **`Permission.Denied`** &ndash;指定された権限が許可されていません。

次のコードスニペットは、アクティビティのカメラアクセス許可を確認する方法の例です。 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

ベストプラクティスとして、アプリケーションのアクセス許可が必要である理由をユーザーに通知し、アクセス許可を付与するための情報に基づいた決定を行うことができるようにすることをお勧めします。 この例として、写真を撮影して geo タグを付けるアプリがあります。 カメラのアクセス許可が必要であることは明らかですが、アプリがデバイスの場所も必要とする理由が明確でない場合もあります。 理論的には、場所のアクセス許可が望ましい理由と、カメラのアクセス許可が必要である理由をユーザーが理解できるように、メッセージを表示する必要があります。

`ActivityCompat.ShouldShowRequestPermissionRationale`メソッドを使用して、ユーザーに根拠を表示するかどうかを決定します。 このメソッドは、 `true`特定のアクセス許可の根拠を表示する必要がある場合にを返します。 このスクリーンショットは、アプリケーションによって表示される Snackbar の例を示しています。このバーは、アプリがデバイスの場所を知る必要がある理由を説明しています。

![場所の根拠](permissions-images/07-rationale-snackbar.png) 

ユーザーがアクセス許可を`ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)`付与する場合は、メソッドを呼び出す必要があります。 このメソッドには、次のパラメーターが必要です。

- **アクティビティ**&ndash;これは、アクセス許可を要求しているアクティビティで、Android によって結果が通知されます。
- **アクセス許可**&ndash;要求されているアクセス許可の一覧。
- **Requestcode**呼び出しに`RequestPermissions`対するアクセス許可要求の結果を照合するために使用さ&ndash;れる整数値。 この値は、ゼロより大きい値である必要があります。

このコードスニペットは、ここで説明した2つのメソッドの例です。 まず、アクセス許可の内容を表示するかどうかを確認します。 このような場合には、論理的なものとして Snackbar が表示されます。 ユーザーが Snackbar で **[OK]** をクリックすると、アプリはアクセス許可を要求します。 ユーザーがこの原理を受け入れない場合、アプリはアクセス許可の要求に進まないようにする必要があります。 この原理が示されていない場合、アクティビティはアクセス許可を要求します。

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission`ユーザーが既にアクセス許可を付与されている場合でも、を呼び出すことができます。 それ以降の呼び出しは必要ありませんが、ユーザーがアクセス許可を確認 (または取り消し) する機会を提供します。 が`RequestPermission`呼び出されると、コントロールはオペレーティングシステムに渡され、アクセス許可を受け入れるための UI が表示されます。  

![Permssion ダイアログ](permissions-images/08-location-permission-dialog.png)

ユーザーが終了すると、Android はコールバックメソッド () `OnRequestPermissionResult`を使用して結果をアクティビティに返します。 このメソッドは、アクティビティによって`ActivityCompat.IOnRequestPermissionsResultCallback`実装される必要があるインターフェイスの一部です。 このインターフェイスには、という`OnRequestPermissionsResult`1 つのメソッドがあります。これは Android によって呼び出され、ユーザーの選択のアクティビティを通知します。 ユーザーがアクセス許可を付与している場合、アプリは保護されたリソースを使用できます。 を実装`OnRequestPermissionResult`する方法の例を次に示します。 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permission_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  

## <a name="summary"></a>まとめ

このガイドでは、Android デバイスでアクセス許可を追加および確認する方法について説明しました。 以前の Android アプリ間でアクセス許可がどのように機能するか (API レベル < 23) と新しい Android アプリ (API レベル > 22) の違いについて説明します。 Android 6.0 で実行時のアクセス許可チェックを実行する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [通常のアクセス許可の一覧](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [ランタイムアクセス許可のサンプルアプリ](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin. Android でのアクセス許可の処理](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
