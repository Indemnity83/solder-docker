# Docker stack for Solder

A Docker environment utilizing docker-compose to facilitate running Solder. The only pre-requisites are Git and Docker. Great for both local and production deployment.

## Getting started

### Requirements
  * Git
  * Docker >= 1.13.0
    
### Install Solder Docker

Installing Solder Docker is easy, and it involves a few simple steps.
Get started by opening a terminal/command prompt window. The process will be similar to installing non-dockerized Solder.

```
git clone https://github.com/Indemnity83/solder-docker.git
cd solder-docker
```

Then, you can run ```docker-compose -f docker-dev-compose.yml run --rm composer composer install --no-dev```. That will install composer dependencies for Solder. It might take a while, depending on your internet speed.

```docker-compose -f docker-dev-compose.yml run --rm npm npm install --only=production``` will install Node dependencies for Solder. It might take a while, depending on your computer's processing power and internet speed.


```docker-compose -f docker-dev-compose.yml run --rm npm npm run production``` will generate files necessary for Solder to run.



```docker-compose run --rm php php artisan solder:install``` is the final step. It requires user input.
Before proceeding, you should probably wait 15-30 seconds first. It will help prevent a "Connection refused" error later.
You will get asked a few questions.

```App URL [http://localhost]:```
Here, you should enter the URL to your web server. If you're hosting it on a domain, it'd be http://mydomain.com or https://mydomain.com
If you're hosting locally, just press enter. The option inside the brackets is the default and it will fit your needs in that case.

```Database name [solder]:```
Press enter.

```Database host [localhost]:```
Type in ```db```.

```Database port [3306]:```
Press enter.

```Database user:```
Type in ```solder```.

```Database password ("null" for no password):```
Type in```solder```.
NOTE: The characters you're typing won't appear on screen. Don't worry, you're typing them in, but they're not displayed for security reasons.

```Do you want to migrate the database? (yes/no) [yes]:```
Press enter.

```Do you want to generate new API keys? (yes/no) [yes]:```
Press enter.

### Starting and Stopping Solder Docker

Once you have completed all the installation steps you can startup the containers all at once by running the `up` command (the `-d` flag will run the servers in the background).

```
docker-compose up -d
```

Stopping all the containers is as easy as calling `stop`

```
docker-compose stop
```

### Upgrading Solder

NOTE: Shut down your containers with ```docker-compose stop``` before proceeding.

The solder codebase is configured as a submodule of solder-docker. So updating to the latest commit of solder requires doing a submodule pull, then re-running some installation steps.

```
git submodule update
docker-compose -f docker-dev-compose.yml run --rm composer composer install --no-dev
docker-compose -f docker-dev-compose.yml run --rm npm npm install --only=production

docker-compose -f docker-dev-compose.yml run --rm npm npm run production
docker-compose run --rm php php artisan migrate --force
```

You can start up your containers again.
