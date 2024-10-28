# Google Cloud SQL proxy buildpack

This heroku buildpack adds the Google Cloud SQL proxy to your app to enable
connections to SQL instances in the GCP.

## Prerequisite

- Google service account with the Cloud SQL/Cloud SQL client role
- JSON key for the service account
- Google Cloud SQL instance set up with a postgresql user able to connect
  to the database

## Install

Add the proxy to your buildpacks. It's important that this buildpack should
not be the last one in the list, as that's used by heroku to determine your
startup processes. (--index=1)

     heroku buildpacks:add --index=1 https://github.com/qawolf/heroku-buildpack-cloud-sql-proxy

Add the **base64 encoded** GCP JSON credentials as `CLOUD_SQL_CREDENTIALS`
env variable to you app.

Set the instance the proxy should connect to with the `CLOUD_SQL_INSTANCE` env
variable.

Format: `<gcp_project>:<gcp_region>:<instance_name>`

Set the connection string for your DB library to
`postgres://<username>:<password>@localhost:5432/<database-name>`

Start the proxy before your app boots by adding the run script to your `Procfile`.

For example:

```
web: bin/run_cloud_sql_proxy && npm start
```
