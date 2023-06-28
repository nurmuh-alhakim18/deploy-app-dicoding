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
4. Click `Create`

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

5. Clone the repository

   ```bash
   git clone https://github.com/nurmuh-alhakim18/notes-app-back-end.git
   ```

6. Change directory
   
   ```bash
   cd notes-app-back-end 
   ```

7. Install NVM

  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
  ```

8. Exit the SSH and open it again
9. Install NVM

   ```bash
   nvm install v14.17.6
   ```
10. Change directory
   
    ```bash
    cd notes-app-back-end 
    ```

11. Install modules

   ```bash
   npm install
   ```

12. Run server

    ```bash
    npm run start-prod
    ```

13. Back to VM instance page and copy `External IP`

14. Check if the server is accessible

    ```bash
    External IP:5000
    ```
15. Open `http://notesapp-v1.dicodingacademy.com/`
16. Click `Change URL`
17. Input your URL (step 14)
18. Try and use the application, check if it is working properly or not

## INSTALLING NODEJS PROCESS MANAGER (OPTIONAL)
Process manager is used to monitor web servers on Compute Engine instances.
1. On SSH
   
   ```bash
   npm install -g pm2
   ```

2. Change directory
   
   ```bash
   cd notes-app-back-end 
   ```

3. Run this command

   ```bash
   pm2 start npm --name "notes-api" -- run "start-prod"
   ```

4. You can restart the process by

   ```bash
   pm2 restart notes-api
   ```

5. You can also stop and start it by   

   ```bash
   pm2 stop notes-api
   ```

   ```bash
   pm2 start notes-api
   ```