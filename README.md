## Sample how to create postgresql database

### Create folders for tablespace

```shell script
mkdir -p /pgdata/pg_tblspc/<name>_ts_idx
mkdir -p /pgdata/pg_tblspc/<name>_ts_data
chown -R postgres:postgres /pgdata/pg_tblspc/<name>_ts_idx
chown -R postgres:postgres /pgdata/pg_tblspc/<name>_ts_data
chmod 700 /pgdata/pg_tblspc/<name>_ts_idx
chmod 700 /pgdata/pg_tblspc/<name>_ts_data
``` 

### Create database objects

```sql
--- Create database
create database <database_name>;
 
--- Create users, schemas, tablespace
create user <user_name> with encrypted password 'password';
create schema <schema_name>;
create schema ext;
grant connect on database <db_name> to <user_name>;
grant all on schema <schema_name> to <user_name>;
alter user <user_name> VALID UNTIL 'infinity';
grant usage on schema <schema_name> to <user_name>;
alter default privileges in schema <schema_name> grant ALL on tables to <user_name>;
grant select on all sequences in schema <schema_name> to <user_name>;
grant all privileges on all tables in schema <schema_name> to <user_name>;
grant select on all tables in schema <schema_name> to <user_name>;

--- Create tablespace
create tablespace <name>_ts_data owner <schema_name> location '/pgdata/pg_tblspc/<name>_ts_data';
create tablespace <name>_ts_idx owner <schema_name> location '/pgdata/pg_tblspc/<name>_ts_idx';

--- Grant permissions
grant usage on schema ext to <user_name>;

--- Create extensions
create extension "uuid-ossp" schema ext;
create extension pgcrypto schema ext;
create extension pg_cron schema ext; 
```
if files for tablespaces were already created you need this also:
```sql
grant all on tablespace <tablespace_name> to <user_name>;
```
