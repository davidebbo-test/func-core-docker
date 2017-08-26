FROM microsoft/aspnetcore
WORKDIR /app
COPY runtime runtime
ENV AzureWebJobsScriptRoot=/app/app
ENTRYPOINT ["dotnet", "runtime/Microsoft.Azure.WebJobs.Script.WebHost.dll"]

