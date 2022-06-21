### This repo was created for the FIRST Threat-Informed Defense workshop. Please follow these steps to ensure a successful install. 

## 1. Required Software
### Git
We recommend [GitHub Desktop](https://desktop.github.com/) for simplicity, but any git client will work
### NodeJS
You can download NodeJS [here](https://nodejs.org/en/download/)
### Angular CLI
`npm install -g @angular/cli`
### Docker Desktop
[Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/)  
[Docker Desktop for Mac](https://docs.docker.com/desktop/mac/install/)
### Python
Download Python [here](https://www.python.org/downloads/)
### PIP
Mac - `$ python -m ensurepip --upgrade`  
Windows - `py -m ensurepip --upgrade`

## 2. Required Repos
### Workbench
The Workbench repos need to be under the same parent directory.  
**Workbench Frontend** - 
```
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-frontend.git
```
**Workbench API** - 
```
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api.git
```
**Workbench Collection Manager** - 
```
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-collection-manager.git
```  

**To eliminate timeouts, replace `attack-workbench-frontend/nginx/nginx.conf` with this updated [.conf file](nginx.conf)**  
**To make the Workbench database persistent, replace `attack-workbench-frontend/docker-compose.yml` with this [updated file](docker-compose.yml)**

### Navigator

**Navigator Repo** - 
```
git clone https://github.com/mitre-attack/attack-navigator.git
```

To point Navigator at your workbench, replace `attack-navigator/nav-app/src/assests/config.json` with this [updated file](config.json)  

### Website
**Website Repo** - 
```
git clone https://github.com/mitre-attack/attack-website.git
```
To point Website at your local instance of Workbench, replace `attack-website/modules/site_config.py` with this [updated file](site_config.py)  

## 3. Workbench
### Build docker images
**YOU MUST START THE DOCKER DAEMON BEFORE RUNNING DOCKER COMPOSE**  
You can do this by starting docker desktop.  

1. Navigate to the `attack-workbench-frontend` directory (containing the `docker-compose.yml` file)
2. Run the command:
```shell
docker-compose up
```

This command will build all of the necessary Docker images and run the corresponding Docker containers.

### Access Docker instance

With the docker-compose running you can access the ATT&CK Workbench application by visiting the URL `localhost` in your browser.

## 4. Navigator
### Build the server
1. Navigate to the `/attack-navigator/nav-app`
2. Run `npm install`

### Serve application on local machine

1. Run `ng serve` within `/attack-navigator/nav-app`
2. Browse to `localhost:4200` in browser

## 5. Website

### Install requirements

1. Create a virtual environment: 
    - macOS and Linux: `python3 -m venv env`
    - Windows: `py -m venv env`
2. Activate the virtual environment: 
    - macOS and Linux: `source env/bin/activate`
    - Windows: `env/Scripts/activate.bat`
3. Install requirement packages: `pip3 install -r requirements.txt`
### Build and serve the local site

1. Update `navigator_link` field within `modules/site-config.py` to point to local navigator instance. This will link the website with your custom Navigator
```shell
navigator_link = "../attack-navigator/nav-app"
```
2. Update local ATT&CK data:   
   `python3 update-attack.py`  
   _Note: `update-attack.py`, has many optional command line arguments which affect the behavior of the build. Run `python3 update-attack.py -h` for a list of arguments and an explanation of their functionality.  
3. Serve the html to `localhost:8000`: 
    1. `cd output`
    2. `python3 -m pelican.server`

### (Optional) Installing, building, and serving the site via Docker 

1. Build the docker image:
  ``` 
docker build -t workshop_website .
2. Run a docker container:
```
docker run --name attack_website -d -p 8888:80 workshop_website
```