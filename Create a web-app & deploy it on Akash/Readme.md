This hands-on experience will introduce you to containerization with Docker and deployment on a decentralized cloud platform like Akash Network. Very little knowledge of programming is required to follow along.

## MISSION 🎯
Create a basic web-app in Python & deploy it on Akash
1. Prerequisites

To help you in completing this mission, you need to download:
- Python
- Docker Desktop
- Visual Studio Code
Now is the time to create an account or login to Docker Desktop before proceeding to step 2.

2. Creating the environment
Run the following commands in a terminal to create a directory for the project and to open it in Visual Studio Code:
```
mkdir akash_web_app
cd akash_web_app
code
```.

3. Creating & testing the web-app

Create a Python script app.py and paste the following code to create a basic web-app that listens on port 8080:

```
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

4. Containerizing, testing & uploading your application

Create a Dockerfile named Dockerfile (note: there should be no file extension) with the following content:

FROM python:3.8-alpine
COPY . /
RUN pip install aiohttp
CMD ["python", "app.py"]

Build a Docker image from the Dockerfile by running the following command. Note that the username should be the same as your username in Docker Desktop.

docker build -t username/akash_web_app:0.0.1 .

If the build command worked without errors, you can try running the image locally and it should work the same as our Python application worked locally. Note that this command is running in detached mode and will not show logs and must be closed from within Docker Desktop.

docker run -d -p 8080:8080 username/akash_web_app:0.0.1

If everything works, you can upload the Docker image to Docker Hub:

docker push username/akash_web_app:0.0.1

5. Deploying to Akash

By using any deployment of your liking (e.g. Akash Console), you can deploy your application by pasting the following SDL. The only modification you need to do is change USERNAME/AKASH_WEB_APP:0.0.1 to the username, image name, and version of your own. Some providers supports https://, although this web app requires you to make sure that you open it with http://.

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

Resources

Akash Website

Akash Discord

Akash Console
