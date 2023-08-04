# Setting up a MySQL Instance in Google CloudSQL

## Creating your instance
In the Google Cloud Console (GCP), click on `Create a Database`. Then select `Create Instance` and choose `MySQL`.
> You may be asked to enable APIs -- enable if asked

Fill in instance ID and password.

For `Database version`, select MySQL 8.0.

Choose `Cloud SQL Edition` Enterprise.

Select the `Sandbox` preset ($0.14 per hour).

For `Region`, select us-central (default).

For `Zonal Availability`, select single-zone.

Now choose `Customize your instance`.
> - Open `Data Protection` and uncheck `Enable Delete Protection`

Now you can create your instance -- this may take a few minutes.

## Authorizing your network to connect to your instance
Once your instance is up, select `Connections` from the GCP sidebar.
Select `Networking` and scroll down to `Add a network`.

Enter a name for your network. In the `Network` field, enter your IP address.
> This can be determined from https://whatismyipaddress.com/.
> Copy the IPv4 and paste it in the `Network` field

***!!*** At the end of your IP address, add `/32`. This ensures that only your network can connect to this instance. 

Click `save` to save your network. You are now ready to connect to your MySQL instance.

## Editing your instance from the CLI

In the command line, enter the following command:
```
mysql -u root -p -h <instance-IP-address>
```
You will then be prompted to enter the password you created earlier when creating your MySQL instance.

When you are inside the MySQL server, the existing databases should be the following:

> - information_schema
> - mysql
> - performance_schema
> - sys
>   
> This can also be confirmed by entering the command `show databases; `

To begin entering data, create a new database using: `create database <your-db-name>;`

To confirm your database was created, use: `show databases;`

Once your database has been created, enter: `use <your-db-name>;`

You can now begin creating tables in your database. Table creation can be confirmed using `show tables;`

## Troubleshooting 

***ERROR 2003 (HY000): Can't connect to MySQL server on '<your-host-IP>:3306' (110)***

Your server may not be running, so clients cannot connect to it. Check in the GCP SQL that your instance is running.

