# ps-qr-decode
Notes for decoding QR (and other barcode) images with PowerShell

Tested on Windows 10 21H2 (PowerShell 5.1) with ZXing.Net v0.16.8
ZXing.Net package: https://www.nuget.org/packages/ZXing.Net

##Setup
###If Visual Studio is not Installed or Visual Studio is installed without NuGet
If using PowerShell and Visual Studio NuGet package provider is not installed, perform the following steps to install/update NuGet and add nuget.org as a package provider.
1. Install/update NuGet package provider
  `Install-PackageProvider Nuget -Force -Verbose`
2. Check package sources for nuget.org
  Run `Get-PackageSource` and check for an entry with a location of `https://www.nuget.org/api/v2` 
3. If nuget.org is not listed as a package source in the output of step two, add it
  `Register-PackageSource -Name nuget.org -Location https://www.nuget.org/api/v2 -ProviderName NuGet`

###Install ZXing.Net package
1. Install ZXing.Net package
  `$Directory = ([xml]((Install-Package ZXing.Net -SkipDependencies -Force).SwidTagText)).SoftwareIdentity.Payload.Directory`
2. Build and print path to zxing.dll for .NET 4.0. You should take note of the path for future use.
  `$Path = $Directory.location + '\' + $Directory.name + '\lib\net40\zxing.dll'`
  `if (Test-Path $Path){"`nPath to .NET 4.0 library is in the `$Path variable and printed below. Take note of this file path:`n    $Path`n"} else {"`n.NET 4.0 library not found. Search the 'lib' directory of the package for the appropriate DLL:`n    $($Directory.location + '\' + $Directory.name)`n"}`

###Use ZXing.Net to decode barcode images
NOTE: You MUST set $Path to the location of the .dll before performing any of the following steps.
1. Add .NET class to PowerShell session
  `Add-Type -Path $Path`
2. Create BarcodeReader object
  `$BarcodeReader = [ZXing.BarcodeReader]::new()`
