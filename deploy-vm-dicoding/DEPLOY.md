# DEPLOYING NODEJS APP ON GOOGLE COMPUTE ENGINE

## CREATE FIREWALL FROM CONSOLE OR CLOUD SHELL

### CONSOLE
1. Choose VPC Network > Firewall
2. Click `CREATE FIREWALL RULE`
3. Input the configurations as below:
    - Name: app-server-firewall
    - Targets: Specified target tags
    - Target tags: web-server
    - Source filter: IPv4 ranges
    - Source IPv4 ranges: 0.0.0.0/0
    - Protocols and port: 
        - Specified protocols and ports
        - Check TCP
        - Input 5000
4. Click `Create`

### CLOUD SHELL
1. Open Cloud Shell
2. Run this command

   ```bash
   gcloud compute --project=deploy-vm-dicoding firewall-rules create app-server-firewall --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:5000 --source-ranges=0.0.0.0/0 --target-tags=web-server
   ```

## CREATE COMPUTE ENGINE
1. Choose Compute Engine > VM instances
2. Click `CREATE INSTANCE`
3. Input the configurations as the below:
    - Name: web-server
    - Region:	asia-southeast2 (Jakarta)
    - Zone:	asia-southeast2-a
    - Machine: type	e2-micro (2 vCPU, 1 GB memory)
    - Boot disk: 
        - Type: New balanced persistent disk
        - Size: 10 GB
        - Image: Ubuntu 20.04 LTS
    - Click `Advanced options` > `Networking`
        - Input `web-server` on Network tags section
    - Click `Create`

## CONFIGURE COMPUTE ENGINE INSTANCE
1. On VM instances page, click `SSH` beside web-server instances
2. Authorize the SSH if prompted
3. Run this command to check if git is available

   ```bash
   git --version
   ```

4. If not available, run this

   ```bash
   sudo apt-get install git
   ```

5. Clone the repository by
   ```bash
   git clone https://github.com/nurmuh-alhakim18/notes-app-back-end.git
   ```

6. Change directory by
   
   ```bash
   cd notes-app-back-end 
   ```