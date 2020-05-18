# How to Docker React CSS Styling

## Based on three Robin Wieruch articles

- [How to install Docker on MacOS](https://www.robinwieruch.de/docker-macos)
- [How to Docker with create-react-app](https://www.robinwieruch.de/docker-create-react-app-development)
- [How to CSS Style in React](https://www.robinwieruch.de/react-css-styling)

## What's the plan

- Set up a [Create React App]() dev environment running locally in docker
- Hot reload dev our way through the CSS style article
- Commit everything to a Github repo we can work through, commit by commit

## Docker up

- Make sure you have docker installed
  Just follow the first article on how to install Docker (with Docker Machine instead of Docker Desktop).
  If you're on a Mac but you prefer Docker Desktop, that will work fine
  Also, no matter what system you're on, if you have docker running, we're good to go
- So in my case, I have docker-machine running, but not as a service, just ... there. 
  So I gear up VS Code in a directory, and do the following
  - See if docker-machine is running
  -
      ```bash
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

