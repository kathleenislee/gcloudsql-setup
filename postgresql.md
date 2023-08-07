# Setting up a Postgresql Instance in Google CloudSQL

## Creating your instance
In the Google Cloud Console (GCP), click on `Create a Database`. Then select `Create Instance` and choose `Postgresql`.
> You may be asked to enable APIs -- enable if asked

Fill in instance ID and password.

For `Database version`, select `PostgreSQL 15`.

Choose `Cloud SQL Edition` Enterprise.

Select the `Sandbox` preset ($0.14 per hour).

For `Region`, select us-central (default).

For `Zonal Availability`, select single-zone.

Now choose `Customize your instance`.
> Open `Data Protection` and uncheck `Enable Delete Protection`
> 
> Open `Flags` and set the following flags to `on`
> - `cloudsql.logical_decoding`
> - `cloudsql.enable_pglogical`

Now you can create your instance -- this may take a few minutes.

## Authorizing your network to connect to your instance
Once your instance is up, select `Connections` from the GCP sidebar.
Select `Networking` and scroll down to `Add a network`.

Enter a name for your network. In the `Network` field, enter your IP address.
> This can be determined from https://whatismyipaddress.com/.
> Copy the IPv4 and paste it in the `Network` field

**!!** At the end of your IP address, add `/32`. This ensures that only your network can connect to this instance. 

Click `save` to save your network. You are now ready to connect to your MySQL instance.

## Editing your instance from the CLI

In the command line, enter the following command:
```
psql -U <User-name> -h <host-IP-address>
```
To connect directly to a specific database, the following command can be used:
```
psql -U <User-name> -d <database-name> -h <host-IP-address>
```

> User-name can be found under `Users` in the GCP -- there should be a default user called `postgres` for a Postgresql instance
> - note: `postgres` is a superuser. Additional users can be created in the `Users` menu in the GCP
>   
> Database name can be found under `Databases` in the GCP -- there should be a default database called `postgres`
> 
> Host IP address can be found as the instance's Public IP Address

Once you are in the psql environment, you can display all databases using `\l`. 
To exit the display, press `q`.

To connect to/use any of the specific databases, enter `\c <database-name`.

If you would like to create a new database, this can be performed using `create database <database-name>;`.

You can now begin creating tables in your database. Table creation can be confirmed using the command `\dt`.

## TROUBLESHOOTING

***psql: error: connection server at "IP-address", port 5432 failed: Connection timed out***

Your server may not be running, so clients cannot connect to it. Check in the GCP that your instance is running.
