# Docker Swarm Demo

**Open a terminal and ssh into the machine where you want to run your manager node.**

```docker swarm init --advertise-addr <manager_ip>```

example:
$ docker swarm init --advertise-addr 192.168.38.7
Swarm initialized: current node (dxn1zf6l61qsb1josjja83ngz) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

The --advertise-addr flag configures the manager node to publish its address as 192.168.38.7. The other nodes in the swarm must be able to access the manager at the IP address.

The output includes the commands to join new nodes to the swarm. Nodes will join as managers or workers depending on the value for the --token flag.


**Run docker info to view the current state of the swarm:**

```docker info```

**Run the docker node ls command to view information about nodes:**

```docker node ls```


## Set up a Docker registry

**Start the registry as a service on your swarm:**

```docker service create --name registry --publish published=5000,target=5000 registry:2```

Check its status with docker service ls:

```docker service ls```

## Test the app with Compose

**Bring up docker compose**

```docker-compose up -f docker-compose.yml up --build -d```

**Check that the app is running with**

```docker-compose ps```

**You can test the app with curl:**

```curl http://localhost:8000```

"Hello World! I have been seen 1 times."

**Bring the app down:**

```docker-compose down --volumes```

## Push the generated image to the registry

```docker-compose push```

## Deploy the stack to the swarm

**Create the stack with docker stack deploy:**

```docker stack deploy --compose-file docker-compose.yml stackdemo```

**Check that it’s running with docker stack services stackdemo:**

```docker stack services stackdemo```

**As before, you can test the app with curl:**

```curl http://localhost:8000```

"Hello World! I have been seen 1 times."

**Thanks to Docker’s built-in routing mesh, you can access any node in the swarm on port 8000 and get routed to the app:**

```curl http://address-of-other-node:8000```

"Hello World! I have been seen 2 times."

**Bring the stack down with**

```docker stack rm stackdemo```

**bring your Docker Engine out of swarm mode**

```docker swarm leave --force```
