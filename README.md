# signalr-container-issue

### Describe the bug
Azure Signalr Service running on ASP.NET Core within a container fails to connect with 401

### To Reproduce

Sammple code and instructions to reporduce: https://github.com/urbanantics/signalr-container-issue

The code in this repo was created based on the Microsoft Azure Signalr quickstart https://docs.microsoft.com/en-us/azure/azure-signalr/signalr-quickstart-dotnet-core

Ensure you have docker installed

Note: To keep thigs simple, this example uses http website connection, I have tried setting up the website to accept https connections but still get the same error

1. Find and replace the following with an actual connection string from Azure Signalr service:

```
"ConnectionString": "<signalr service connection string>"
```

2. Compile and run the code using:

```
dotnet run
```

- visit http://localhost:5000
- you will see the standard quickstart chat app

3. Now create and run the chat app from inside a docker container with a volume pointing the same folder:

```
docker run -it `
    -p 8000:5000 `
    -w /src `
    -v "${PWD}:/src/" `
    --entrypoint /bin/bash mcr.microsoft.com/dotnet/sdk:5.0-buster-slim 

dotnet run
```
- visit http://localhost:8000
- you will see again the standard quickstart chat app
- however the chat app does not work, from the logs you see the following errors

### Exceptions (if any)

fail: Microsoft.Azure.SignalR.ServiceConnection[2]
      Failed to connect to '(Primary)https://<your-signalr-hub>.service.signalr.net', will retry after the back off period. Error detail: The server returned status code '401' when status code '101' was expected.. The server returned status code '401' when status code '101' was expected.. Id: f02b28df-566d-4dac-908f-8489750e8a76

### Further technical details

<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    <RootNamespace>chat_app</RootNamespace>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.SignalR" Version="1.8.1" />
  </ItemGroup>

</Project>