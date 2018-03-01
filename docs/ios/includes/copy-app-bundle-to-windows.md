
Podczas tworzenia aplikacji systemu iOS w programie Visual Studio i agent kompilacji Mac, nie zostanie skopiowany do komputera z systemem Windows pakietu App. Dodaje nowe narzędzia platformy Xamarin dla Visual Studio 7.4 `CopyAppBundle` właściwość, która umożliwia CI kompilacje do skopiowania .app pakietów do systemu Windows.

Aby używać tej funkcji, należy dodać`CopyAppBundle` .csproj chcesz zastosować tę funkcję do w grupie właściwości dla właściwości. Na przykład poniższy przykład przedstawia sposób skopiować do komputera z systemem Windows dla pakietu App **debugowania** kompilacji przeznaczonych dla **iPhoneSimulator**:

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

