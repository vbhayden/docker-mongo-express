## MongoDB + Express UI
Simplified MongoDB instance with the **[Express UI](https://hub.docker.com/_/mongo-express)**.

This repo contains a Docker Compose setup for standing up a MongoDB instance alongside the popular Mongo Express UI, with Expess proxied behind Nginx for SSL and the common ENV variables ready to be configured.  The scripts here assume an Ubuntu-like OS, but can be modified for others without much issue.

## Setup
Setup is pretty straightforward, we need to ensure that Docker is installed and then it handles the rest.

### tl;dr:
- `git clone https://github.com/vbhayden/docker-mongo-express`
- `cd docker-mongo-express`
- `sudo ./install-reqs.sh`
- `cp example.env .env`
- Populate the `.env` file (see below)
- `sudo ./init-ssl.sh <HOSTNAME>` (see below)
- `sudo docker-compose up -d --build`

### Setting the Config Values
The repo comes with an `example.env` file that you will want to (but really should) configure.  Each service has a few properties exposed by default, but you can refer to the Docker Hub docs for each to add more.

Service|Variable|Why am I setting this?
-|-|-
**Nginx**|`HOSTNAME`|the domain name of the machine used to host this stack, can be `localhost`
**MongoDB**|`MONGO_USERNAME`|Username for MongoDB
-|`MONGO_PASSWORD`|Password for MongoDB
**Mongo Express**|`UI_USERNAME`|Username for the Express UI
-|`UI_PASSWORD`|Password for the Express UI

### Setting up SSL
This stack uses Certbot and Nginx to handle SSL, including temporary self-signed certs to help the process along.  We run the `init-ssl.sh` script during setup to place these into a directory corresponding to Nginx's `HOSTNAME` variable.  

With that, the stack can start without issue but to get an actual SSL cert from Certbot:
```
sudo ./certbot/generate.sh <HOSTNAME>
sudo docker-compose restart nginx
```
