
ソリューションのリリース ビルドを指定する次のコマンド ライン**SOLUTION_FILE.sln** iPhone 用です。 指定して、IPA の場所を設定することができます、`IpaPackageDir`コマンドラインでのプロパティ。

 - Mac でを使用して**xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

**Xbuild**コマンドは、ディレクトリでよく見られる**/Library/Frameworks/Mono.framework/Commands**です。

 - Windows を使用して**msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**msbuild**は自動的に展開されません`$( )`コマンドラインによって渡される式です。 この理由からお勧めを設定する場合は、完全なパスを使用する、`IpaPackageDir`コマンドライン。


参照してください、 [iOS 9.8 のリリース ノート](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location)の詳細については、`IpaPackageDir`プロパティです。
