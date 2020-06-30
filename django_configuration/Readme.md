# How to configure Django server PostgreSQL database
First, run a PostgreSQL prompt:
```
sudo -u postgres psql
```

Now, create a database for your project:
```
CREATE DATABASE myproject;
```

Next, create a database user for our project. Make sure to select a secure password:
```
CREATE USER myprojectuser WITH ENCRYPTED PASSWORD 'password';
```

Next, we are setting configurations, the are all recommendations from the Django project:
```
ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';
ALTER ROLE myprojectuser SET timezone TO 'UTC';
```

Now, we can give our new user access to administer our new database:
```
GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
```

Reference: [How To Set Up Django with Postgres, Nginx, and Gunicorn on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-20-04)