Instructions to build the images on a Windows box using the Linux shell:

### Prerequisites

- Windows Linux bash shell
- In Linux shell, install the latest Node 8.x in the Linux shell: https://askubuntu.com/a/548776/639573
- In Linux shell, install dotnet 2.0: https://www.microsoft.com/net/download/linux
- Install Docker for Windows

### Enlisting and building the function runtime on Linux

- Clone https://github.com/Azure/azure-webjobs-sdk-script.git into a folder windows can access (e.g. `/mnt/d/code/GitHub/azure-webjobs-sdk-script-linux`)
- Check out the Core branch
- Run `dotnet publish WebJobs.Script.sln`

### Getting the correct grpc native binary

The default build only gets 32 bit binaries, and we need the 64 bit (to match the Linux Node)

- From azure-webjobs-sdk-script repo root, `cd src/WebJobs.Script.WebHost/bin/Debug/netcoreapp2.0/publish/workers/node/grpc`
- `npm install -g node-pre-gyp`
- `node-pre-gyp install`

### Building the docker images

Now, we leave the Linux shell and use Docker for Windows (because Linux bash on Windows can't run Docker).

#### Building the base image with just Core and Node (no Functions stuff yet)

From this repo's root, go into `base-container` and run

    docker build -t core-and-node .

#### Building the base Functions Runtime image (no user function files)

- From this repo's root, go into `base-runtime-container`
- Create a junction to the runtime you created earlier. Yes, this needs to be made easier, maybe using a git submodule. e.g. (adjust for your Functions repo) `junction FunctionsRuntime D:\code\GitHub\azure-webjobs-sdk-script-linux\src\WebJobs.Script.WebHost\bin\Debug\netcoreapp2.0\publish`
- `docker build -t azure-functions-runtime .`

### Building a test image with some function files, and running it

- From this repo's root, go into `user-functions-container`
- `docker build -t functest .`
- `docker run -p 8000:80 functest`
- Test the C# function: http://localhost:8000/api/HttpTriggerCSharp?name=David
- Test the Node function: http://localhost:8000/api/HttpTriggerJS?name=David

### Running the test on Azure Container Instances (ACI)

- `docker tag functest davidebbo/functest`
- Push the test image to docker hub (replace with your account): `docker push davidebbo/functest`
- Install the latest Azure CLI v2 (i.e. `az`)
- `az group create --name FuncACI --location eastus`
- `az container create --name funccore --image davidebbo/functest --memory 0.8 --resource-group FuncACI --ip-address public`
- Run `az container show --name  funccore --resource-group FuncACI` until you see `"provisioningState": "Succeeded"`
- Get the IP address and hit it in the browser
- Now append the same test paths as above, e.g. http://52.170.33.146/api/HttpTriggerCSharp?name=David