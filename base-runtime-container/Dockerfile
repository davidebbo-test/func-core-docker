FROM davidebbo/core-and-node
WORKDIR /app
COPY FunctionsRuntime FunctionsRuntime
ENV AzureWebJobsScriptRoot=/home/site/wwwroot
ENTRYPOINT ["dotnet", "FunctionsRuntime/Microsoft.Azure.WebJobs.Script.WebHost.dll"]
