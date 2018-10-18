FROM uilianries/conanmsvcbase

RUN Invoke-WebRequest "https://download.microsoft.com/download/9/B/B/9BB1309E-1A8F-4A47-A6C5-ECF76672A3B3/BuildTools_Full.exe" -OutFile "$env:TEMP\BuildTools_Full.exe" -UseBasicParsing
RUN & "$env:TEMP\BuildTools_Full.exe" /Silent /Full
RUN Remove-Item -Path "$env:TEMP\BuildTools_Full.exe" -Force

RUN Install-WindowsFeature NET-Framework-45-ASPNET ; \
    Install-WindowsFeature Web-Asp-Net45

WORKDIR "C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0"

RUN Invoke-WebRequest "https://dist.nuget.org/win-x86-commandline/latest/nuget.exe" -OutFile "C:\windows\nuget.exe" -UseBasicParsing
RUN & "C:\windows\nuget.exe" Install MSBuild.Microsoft.VisualStudio.Web.targets -Version 12.0.4
RUN mv 'C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0\MSBuild.Microsoft.VisualStudio.Web.targets.12.0.4\tools\VSToolsPath\*' 'C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0'

RUN Get-ChildItem -Path 'C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0' -Recurse; \
    Get-ChildItem -Path 'C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v12.0\MSBuild.Microsoft.VisualStudio.Web.targets.12.0.4' -Recurse

RUN $OldPath = (Get-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name Path).Path
RUN $NewPath = OldPath + ';C:\\Program Files (x86)\\MSBuild\\12.0\\Bin'
RUN Set-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name Path -Value $NewPath
RUN $ActualPath = (Get-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name Path).Path
RUN Write-Host "$ActualPath"

WORKDIR "C:/Users/ContainerAdministrator"
ENTRYPOINT ["C:\\Program Files (x86)\\MSBuild\\12.0\\Bin\\msbuild.exe"]
