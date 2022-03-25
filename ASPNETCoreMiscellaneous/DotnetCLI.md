# .NET CLI

> According to Microsoft's official documentation, the .NET command-line interface (CLI) is a cross-platform tool for developing, building, running, and publishing .NET applications.

## .NET CLI commands

- ### **dotnet run**

Run source code without any explicit compile or launch commands.

Run the project in the current directory:

```console
>> dotnet run
```

Run the project from the path specified:

```console
>> dotnet run --project <projectPath>
```

- ### **dotnet restore**

Use NuGet to restore dependencies as well as project-specific tools that are specified in the project file.

Restore the dependencies and tools for the project in the current directory:

```console
>> dotnet restore
```

Restore the dependencies and tools for the project in the given path:

```console
>> dotnet restore ./API/API.csproj
```