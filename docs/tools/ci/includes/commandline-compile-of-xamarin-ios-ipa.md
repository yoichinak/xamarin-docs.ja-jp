
以下のコマンドラインは、iPhone 用の **SOLUTION_FILE.sln** というソリューションのリリースビルドを指定するものです。 IPAの場所は、コマンドラインで `IpaPackageDir` プロパティを指定することで設定できます。

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
