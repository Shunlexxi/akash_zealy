Create a directory 

```
mkdir akash_web_app
```

go to / change directory
`cd akash_web_app`

Open/Create a new code file 
code .

Paste this code in the app.py file
    from aiohttp import web
    async def hello(request):
        return web.Response(text="Hello Akash!")

    app = web.Application()
    app.add_routes([web.get('/', hello)])

    web.run_app(app)

We need to install the aiohttp dependency, which we can do by running the following command:

pip install aiohttp


Now, try starting the web server locally:
    python app.py

Build a Docker image from the Dockerfile by running the following command. 
Note that the username should be the same as your username in Docker Desktop.
    docker build -t shunlexxi/akash_web_app:0.0.1 .


If the build command worked without errors, you can try running the image locally and it should work 
the same as our Python application worked locally. Note that this command is running in detached mode 
and will not show logs and must be closed from within Docker Desktop.
    docker run -d -p 8080:8080 shunlexxi/akash_web_app:0.0.1

If everything works, you can upload the Docker image to Docker Hub:
    docker push shunlexxi/akash_web_app:0.0.1

Deploying to Akash

By using any deployment of your liking (e.g. Akash Console), 
you can deploy your application by pasting the following SDL. 
The only modification you need to do is change USERNAME/AKASH_WEB_APP:0.0.1 to the username, 
image name, and version of your own. Some providers supports https://, although this web app 
requires you to make sure that you open it with http://.
    ---
version: "2.0"

services:
  ourapp:
    image: username/akash_web_app:0.0.1
    expose:
      - port: 8080
        as: 8080
        to:
          - global: true

profiles:
  compute:
    ourapp:
      resources:
        cpu:
          units: 1.0
        memory:
          size: 512Mi
        storage:
          size: 512Mi
  placement:
    akash:
      pricing:
        ourapp:
          denom: uakt
          amount: 10000

deployment:
  ourapp:
    akash:
      profile: ourapp
      count: 1


  