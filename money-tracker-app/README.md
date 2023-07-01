# DEPLOYING FULLSTACK MONEY TRACKER APP USING APP ENGINE AND CLOUD SQL

## CLONE PROJECT
1. Open Cloud Shell
2. Clone Front-End
   
   ```bash
   git clone -b money-tracker https://github.com/dicodingacademy/a133-gcp-labs.git money-tracker
   ```

3. Clone Backend-End
   
   ```bash
   git clone -b money-tracker-api https://github.com/dicodingacademy/a133-gcp-labs.git money-tracker-api
   ```

## DEPLOYING BACK-END
1. Create Cloud Storage Bucket service account
    - On navigation menu, choose `IAM & Admin` > `Service Accounts`
    - Click `CREATE SERVICE ACCOUNT`
    - Input `image-upload` on Service account name field
      or the name you want, but it should define its functions
    - Click `CREATE AND CONTINUE`
    - For its role, choose `Cloud Storage` > `Storage Object Creator`
    - Click `Continue`
    - Click `Done`
2. Saving service account key
    - Click the service account email you created
    - Go to `KEYS` tab
    - Click `ADD KEY` > `Create new key`
    - Choose `JSON` as key type
    - Click `Create`
3. Set up service account to project
    - Open service account key you have downloaded
    - Open editor on Cloud Shell by clicking `Open Editor`
    - Open `servieaccountkey.json`
    - Change the `project_id`, `private_key_id`, `private_key` based on your own key
4. Create Buckets
    - On navigation menu, choose `Cloud Storage` > `Buckets`
    - Click `CREATE`
    - Input the configurations of bucket
      My configurations:
      - Name: (Create your own because it should be unique)
      - Location: `Region` > `asia-southeast2 (Jakarta)`
      - Storage class: Standard
      - Unchecked public access
      - Access control: Uniform
      - Protection tools: None
    - Click `CREATE`
5. Set Cloud Storage to project
    - On Editor, open `modules` > `imgUpload.js`
    - Change `projectId` and `bucketName` based on yours
6. Create Cloud SQL Instance
    - On navigation menu, choose `SQL`
    - Click `CREATE INSTANCE`
    - Choose your database engine, I chose MySQL
    - If you have not enabled the API, click `ENABLE API`
    - Input the configurations of SQL instance
      My Configurations:
      - Instance ID: money-tracker-db
      - Password: (Set your own)
      - Database version: MySQL 8.0
      - Configuration: Production
      - Region: asia-southeast2 (Jakarta)
      - Zonal availability: Multiple zones (Highly available)
    - Click `CREATE INSTANCE`
    - Wait for a while since it takes minutes
7. Create SQL table
    - On navigation menu, choose `API & Services` > `Library`
    - Search for `Cloud SQL Admin API` and click it
    - Click `Enable`
    - Wait for the system to enable it for few minutes
    - Connect to instance from Cloud Shell

      ```bash
      gcloud sql connect your-db-name --user=root --quiet
      ```

    - Wait for a while and input your password
    - Create and use database on your SQL instance

     ```bash
     CREATE DATABASE db;
     ```

     ```bash
     USE db;
     ```

    - Create table on your db
      - On editor, open `create_table.sql`
      - Copy the all lines
      - Back to your terminal that are connected to SQL instance and paste them
      - If you want to check the datas in table

        ```bash
        SELECT * FROM records;
        ```

8. Set the db to public
    - On your SQL instance page, choose `Connections`
    - Go to `NETWORKING` tab
    - Click `ADD A NETWORK`
    - Input the data:
      - Name: (Optional)
      - Network: 0.0.0.0/0
    - Click `Save`
9. Set SQL instance to project
    - On editor, open `routes` > `record.js`
    - Change `host`, `database`, and `password` based on yours
10. Deploy back-end app
    - On editor, open `app.yaml`
    - Change the service from `backend` to `default`
    - Back to terminal
    - Change directory to where the yaml file is

      ```bash
      cd money-tracker-api
      ```

    - Deploy the app

      ```bash
      gcloud app deploy
      ```
    
    - Choose the region where App Engine app will be located, I choose number `8` (asia-southeast2)
    - Input `Y` to continue and wait for a while
    - Back to `app.yaml`
    - Change the service from `default` to `backend`
    - Redeploy the app

      ```bash
      gcloud app deploy
      ```
    
    - Input `Y` to continue to update the service and wait for it
    - On navigation menu, choose `App Engine` > `Services`
    - Click `backend` to check if the app is working
    - `Response Success!` should be shown on the page