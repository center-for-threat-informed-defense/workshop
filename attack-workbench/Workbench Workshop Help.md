# Workbench Workshop Help
## Download repos - 
**All repos must be under the same parent directory**  
WB front-end - `git clone https://github.com/center-for-threat-informed-defense attack-workbench-frontend.git`  
WB collection manager - `git clone https://github.com/center-for-threat-informed-defense/attack-workbench-collection-manager.git`  
WB REST API - `git clone https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api.git`  
Navigator - `git clone https://github.com/mitre-attack/attack-navigator.git`  
Attack Website - `git clone https://github.com/mitre-attack/attack-website.git`  


## Install software
NodeJS - https://nodejs.org/en/download/  
AngularCLI - `npm install -g @angular/cli`  
Docker - https://docs.docker.com/get-docker/  

## Config files
// add Workbench to Navigator  
edit  `attack-navigator/nav-app/src/assets/config.json` to add workbench just beneath versions
```
{
            "name": "ATT&CK Workbench Data",
            "version": "11",
            "domains": [
                {
                    "name": "Enterprise",
                    "identifier": "enterprise-attack",
                    "data": [
                        "http://localhost/api/stix-bundles/?domain=enterprise-attack"
                    ]
                },
                {
                    "name": "Mobile",
                    "identifier": "mobile-attack",
                    "data": [
                        "http://localhost/api/stix-bundles/?domain=mobile-attack"
                    ]
                },
                {
                    "name": "ICS",
                    "identifier": "ics-attack",
                    "data": [
                        "http://localhost/api/stix-bundles/?domain=ics-attack"
                    ]
                }
            ]
        },
```

// add timeout to nginx config to prevent frontend from timing out when downloading ATT&CK repo  
edit `attack-workbench-frontend/nginx/nginx.conf` to extend default timeout  
insert just beneath `server {`
``` 
proxy_connect_timeout 600;
proxy_send_timeout 600;
proxy_read_timeout 600;
send_timeout 600;
```

// make workbench database persistent  
edit `attack-workbench-frontend/docker-compose.yml` to include persistent volume  
replace `mongodb` section 
```
mongodb:
    container_name: attack-workbench-database
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - workspace-data:/data/db
      
volumes:
  workspace-data:
