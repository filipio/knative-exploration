# Python HTTP Function

The boilerplate function code can be found in [`func.py`](./func.py). This function will respond
to incoming HTTP GET and POST requests.

## Setup

There already exists a sample [hello-python knative image](https://hub.docker.com/r/notnew77/hello-python) that can be used. If you want to use it, skip instructions below. If you want to create image from scratch, follow instructions:

1. Login to dockerhub using  
   `docker login`
2. Build knative function image with appropriate registry  
   `kn func build --registry <dockerhub-username>`
3. Push image to dockerhub public repositroy  
   `docker push <dockerhub-username>/hello-python`

## Endpoints

Running this function will expose three endpoints.

- `/` The endpoint for your function - `GET`, `POST`
- `/health/readiness` The endpoint for a readiness health check - `GET`
- `/health/liveness` The endpoint for a liveness health check - `GET`

## Testing

This function project includes a [unit test](./test_func.py). Update this
as you add business logic to your function in order to test its behavior.  
Note that you need to install dependencies from `requirements.txt` to test functions.

```console
python test_func.py
```

After deploying function to kubernetes cluster [`README.md`](../../kubernetes/README.md)), you can check `test.http` file to see how to access endpoints.
