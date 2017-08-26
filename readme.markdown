
To use this, create a `runtime` folder under `base-runtime-container`, and copy the full Functions Runtime in there.

Then go in the `runtime` folder and build the runtime container:

    docker build -t davidebbo/azure-functions-runtime .

Now go in the `user-functions-container` folder, and copy the user functions into the app folder (it has a sample function). Then build the container (which insherits the runtime container):

    docker build -t davidebbo/functest .
    
Now to run it, run:

    docker run -p 8000:80 davidebbo/functest
