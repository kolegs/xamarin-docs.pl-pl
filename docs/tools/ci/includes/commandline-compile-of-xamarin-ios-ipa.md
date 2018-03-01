
Użycie poniższego polecenia, aby określić kompilację wersji rozwiązania **SOLUTION_FILE.sln** dla telefonów iPhone. Można ustawić, określając lokalizację IPA `IpaPackageDir` właściwości w wierszu polecenia:

 - Dla komputerów Mac za pomocą **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

**Xbuild** polecenia zwykle znajdują się w katalogu **/Library/Frameworks/Mono.framework/Commands**.

 - W systemie Windows, przy użyciu **msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**MSBUILD** nie zostanie automatycznie powiększona `$( )` wyrażenia przekazany za pomocą wiersza polecenia. Z tego powodu zaleca się użyj pełnej ścieżki, podczas ustawiania `IpaPackageDir` w wierszu polecenia.


Zobacz [informacje o wersji dla systemu iOS 9,8](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) więcej szczegółów dotyczących `IpaPackageDir` właściwości.
