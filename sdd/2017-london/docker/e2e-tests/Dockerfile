# escape=`

FROM sixeyed/msbuild:netfx-4.5.2-10.0.14393.1198 AS builder

WORKDIR C:\src\ProductLaunch.EndToEndTests
COPY src\ProductLaunch\ProductLaunch.EndToEndTests\packages.config .
RUN nuget restore packages.config -PackagesDirectory ..\packages

COPY src\ProductLaunch C:\src
RUN msbuild ProductLaunch.EndToEndTests.csproj /p:OutputPath=C:\out\tests\EndToEndTests

# test runner
FROM sixeyed/nunit:3.6.1-10.0.14393.1198
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Set-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord

WORKDIR /e2e-tests
CMD nunit3-console ProductLaunch.EndToEndTests.dll

COPY --from=builder C:\out\tests\EndToEndTests .