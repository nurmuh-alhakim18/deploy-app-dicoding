# DEPLOYING PROFILE APP ON APP ENGINE

1. Open Cloud Shell
2. Clone project

   ```bash
   git clone -b profile-app https://github.com/nurmuh-alhakim18/deploy-app-dicoding.git
   ```

3. Change directory to where `app.yaml` file is located

   ```bash
   cd deploy-app-dicoding/profile-app
   ```

4. Deploy the app

   ```bash
   gcloud app deploy
   ```

5. Choose the region where App Engine app will be located, I choose number `8` (asia-southeast2)
6. Input `Y` to continue
7. Wait the app deployment for a while
8. If the process is completed, run this command to view the URL of app
   
   ```bash
   gcloud app browse
   ```

   Or you can go to App Engine's dashboard
   - On navigation menu, choose `App Engine` > `Dashboard`

9. Check if the app is working properly
10. If you don't use the service and want to stop it
    - On navigation menu, choose `App Engine` > `Settings`
    - Click `DISABLE APPLICATION`
    - Input your app ID
    - Click `DISABLE`