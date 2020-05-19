# Docker for Create React App development with hot reloading

## Ref

- [Create React App. Getting Started](https://create-react-app.dev/docs/getting-started/)
- [How to install Docker on MacOS](https://www.robinwieruch.de/docker-macos)
- [How to Docker with create-react-app](https://www.robinwieruch.de/docker-create-react-app-development)
  - [How to Docker with React (other kind of React app, i.e. with Webpack)](https://www.robinwieruch.de/docker-react-development)
- and last but not least
  we've added hot reloading via suggestions in [Michael Herman's Dockerizing a React App](https://mherman.org/blog/dockerizing-a-react-app/)

## What's the plan

- Set up a [Create React App]() dev environment running locally in docker
- Hot reload our way through any local dev tasks we have at hand

## Docker up

- Make sure you have docker installed
  Just follow the first article on how to install Docker (with Docker Machine instead of Docker Desktop).
  If you're on a Mac but you prefer Docker Desktop, that will work fine
  Also, no matter what system you're on, if you have docker running, we're good to go
- So in my case, I have docker-machine running, but not as a service, just ... there.
  So I gear up VS Code in a directory, and do the following
  - See if docker-machine is running
  - ```bash
    % docker-machine ip
    Error getting IP address: Host is not running
    % docker-machine start
    Starting "default"...
    (default) Check network to re-create if needed...
    (default) Waiting for an IP...
    Machine "default" was started.
    Waiting for SSH to be available...
    Detecting the provisioner...
    Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
    victorkane@Victors-MacBook-Air how to docker-with-css-styles-in-cra % docker-machine env
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://192.168.99.101:2376"
    export DOCKER_CERT_PATH="/Users/victorkane/.docker/machine/machines/default"
    export DOCKER_MACHINE_NAME="default"
    # Run this command to configure your shell:
    # eval $(docker-machine env)
    % eval $(docker-machine env)
    % docker-machine ip default
    192.168.99.101
    ```

## Whip up a CRA app

- Create a repo and run `create-react-app`
  We're going to run `create-react-app` in an unconventional manner
  by first creating a directory with our own README
  and initializing a git repo,
  instead of creating the whole app in a subdirectory
- ```bash
  git init
  npx create-react-app .
  mv README.md README.cra.md
  mv README.old.md README.md
  git add --all
  git commit -m "initial commit"
  ```
- We cleaned up and made our initial commit
  (since CRA created its own README and shunned ours over to a different name)
- We run the default app and make sure it's running ok
  by pointing our browser at port 3000
  `http://localhost:3000` if using Docker Desktop
  `http://<docker-machine ip>:3000` if using Docker Machine
- ```bash
  docker-machine ip

  npm start
  You can now view how-to-docker-with-css-styles-in-cra in the browser

  Local:            http://localhost:3000
  On Your Network:  http://n.n.n.n:3000
  ```

- We successfully point our browser at
  http://localhost:3000/

## Docker up!

- Now let's dockerize everything.
- Including hot reloading!
- We create `Dockerfile`
- We create .dockerignore
- We do `rm -rf node_modules` since we won't be running locally,
  but in the docker container
- Now build in the current directory and run
- ```bash
  docker build -t work:cra .
  Successfully built 53d3ebb0de6e
  Successfully tagged work:cra
  docker run -it --rm -v ${PWD}:/app -v /app/node_modules -p 3001:3000 -e CHOKIDAR_USEPOLLING=true work:cra
  ```
- Point your browser at http://localdev:3001/
  where `localdev` is the ip returned by `docker-machine ip`

## Test hot reloading

- When you execute the docker command,
  a container is executed based on the image we built
- ```bash
  docker ps
  CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
  177be52c5868        work:cra            "docker-entrypoint.s…"   8 minutes ago       Up 8 minutes        0.0.0.0:3001->3000/tcp   keen_haslett
  ```
- Change something in `src/App.js` and the browser should hot load with the changes

## Use the image for whatever you want!!!
