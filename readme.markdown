

    docker build -t davidebbo/azure-functions-runtime .
    docker build -t davidebbo/functest .
    docker run -p 8000:80 davidebbo/functest
