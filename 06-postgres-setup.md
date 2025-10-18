## **Install and Start PostgreSQL (Optional if you don’t want Redis)**

```bash
sudo apt install postgresql postgresql-contrib -y

# Start and enable PostgreSQL
sudo systemctl enable postgresql
sudo systemctl start postgresql

# Switch to postgres user
sudo -i -u postgres

# Open psql shell
psql

# Create database and user for LightRAG
CREATE DATABASE lightrag;
CREATE USER devops WITH PASSWORD 'your_pg_password';
GRANT ALL PRIVILEGES ON DATABASE lightrag TO devops;
\q

exit
```

> Replace your_pg_password with a strong password.
>
