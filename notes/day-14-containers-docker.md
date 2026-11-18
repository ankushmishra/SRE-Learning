# Day 14 - Containers and Docker

## Session goal

Package the Day 8 Flask app as a Docker image, run it with a local port mapping,
verify the app, stop the container, and recreate it from the same image.

Suggested time: 90-120 minutes.

## 1. Fundamentals

A container image is a packaged filesystem and configuration used to start
containers. A container is a running instance of an image. A Dockerfile records
the build steps. `docker build` creates an image; `docker run` creates and
starts a container. Port publishing maps a host port to a container port.

Containers share the host kernel, unlike full virtual machines that run a
separate guest operating system. Containers start quickly and package an
application's userspace dependencies, but they still need resource limits,
security controls, persistent-storage decisions, and monitoring.

## 2. Practical lab

Use the personal lab app from Day 8. Keep secrets out of the image and repository.
Use a pinned dependency version in `requirements.txt` for a repeatable learning
build. A local development server is acceptable for this exercise only.

### 2.1 Add a Dockerfile

From the directory containing `app.py` and `requirements.txt`, create:

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
RUN useradd --create-home appuser
USER appuser
EXPOSE 5000
CMD ["flask", "--app", "app", "run", "--host=0.0.0.0", "--port=5000"]
```

The Flask process must listen on `0.0.0.0` inside the container so the
container network can reach it. Add a `.dockerignore` so local virtual
environments, Git metadata, and caches are not copied into the image:

```text
.venv
__pycache__
.git
*.pyc
```

If your Flask module or application object has another name, adjust the
`--app` value. The sample uses a floating base tag for readability; for a
repeatable or production build, choose and maintain a deliberate base-image
version/digest and dependency lock.

### 2.2 Build and run locally

```bash
docker build -t sre-flask-day14 .
docker run -d --name sre-flask-day14 -p 127.0.0.1:5000:5000 sre-flask-day14
docker ps
curl -i http://127.0.0.1:5000/health
```

The host binding `127.0.0.1` keeps this learning app reachable only from the
same machine. If the app has no `/health` route, use `/` or the route you added
on Day 8. `EXPOSE` documents a container port; `-p` publishes it on the host.

Read logs and stop the container:

```bash
docker logs sre-flask-day14
docker stop sre-flask-day14
docker rm sre-flask-day14
```

Run it again from the same image and verify the route again:

```bash
docker run -d --name sre-flask-day14 -p 127.0.0.1:5000:5000 sre-flask-day14
curl -i http://127.0.0.1:5000/health
docker stop sre-flask-day14
```

Leave this stopped container in place for the Day 15 failure drill. Remove it
later with `docker rm sre-flask-day14` when you are finished with both labs.

## 3. Review and Day 14 notes

1. What is the difference between an image and a container?
2. Why must the app listen on `0.0.0.0` inside the container?
3. What does `-p 127.0.0.1:5000:5000` map?
4. What would a virtual machine include that a container shares with its host?
5. What would you change before using this image beyond a lab?

- Image tag:
- Container run command:
- Route response:
- Rebuild/relaunch result:
- Three things learned:
  1.
  2.
  3.
- Confusing command / resolution:

## Done when

- [ ] The image builds from the Flask app.
- [ ] The app responds through the local host port mapping.
- [ ] I stopped and restarted a container from the image.
- [ ] I can describe image, container, and VM differences.

## Further reading

- [Docker: writing a Dockerfile](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)
- [Docker: build images](https://docs.docker.com/get-started/docker-concepts/building-images/)

