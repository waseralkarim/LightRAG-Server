**LightRAG to use Redis, PostgreSQL/Redis (Redis/PGDocStatusStorage), Neo4j, and Milvus** as your storage backends instead of the defaults.

## **Step 1: Install Dependencies for Storages**

Since you already have LightRAG in a virtual environment, activate it first:

```bash
cd ~/LightRAG
source venv/bin/activate
```

Then install the extra dependencies for the storage backends:

```bash
# Redis
pip install redis

# PostgreSQL (for PGDocStatusStorage)
pip install psycopg2-binary

# Neo4j
pip install neo4j

# Milvus (vector storage)
pip install pymilvus
```

> Make sure Redis, PostgreSQL, Neo4j, and Milvus servers are installed and running on your VM or accessible remotely.
> 

## **Install and Start Redis**

```bash
sudo apt update
sudo apt install redis-server -y

# Enable Redis to start on boot
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Test connection
redis-cli ping
# Should return: PONG
```

---

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

---

## **Install and Start Neo4j**

Neo4j can be installed via apt repository:

```bash
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo apt-key add -
echo 'deb https://debian.neo4j.com stable 4.4' | sudo tee /etc/apt/sources.list.d/neo4j.list
sudo apt update
sudo apt install neo4j -y

# Start and enable Neo4j
sudo systemctl enable neo4j
sudo systemctl start neo4j

# Set initial password
neo4j-admin set-initial-password your_neo4j_password
```

> Default Bolt port: 7687
> 

# Install Milvus in Docker

Milvus provides an installation script to install it as a docker container. The script is available in the [Milvus repository](https://raw.githubusercontent.com/milvus-io/milvus/master/scripts/standalone_embed.sh). To install Milvus in Docker, just run

```bash
# Download the installation script

curl -sfL https://raw.githubusercontent.com/milvus-io/milvus/master/scripts/standalone_embed.sh -o standalone_embed.sh

# Start the Docker container

bash standalone_embed.sh start
```

## **Configure `.env`**

Edit your `.env` file:

```bash
nano .env
```

Update the storage backend variables like this:

```
############################
### Storage selection
############################

# Use Redis for KV and Doc Status
LIGHTRAG_KV_STORAGE=RedisKVStorage
LIGHTRAG_DOC_STATUS_STORAGE=RedisDocStatusStorage

# Use Neo4j for Graph
LIGHTRAG_GRAPH_STORAGE=Neo4JStorage

# Use Milvus for Vector
LIGHTRAG_VECTOR_STORAGE=MilvusVectorDBStorage

############################
### Connection settings
############################

### PostgreSQL (not used for now, can leave default)
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=your_username
POSTGRES_PASSWORD=your_password
POSTGRES_DATABASE=your_database

### Neo4j Configuration
NEO4J_URI=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your_password
NEO4J_DATABASE=neo4j

### Redis
REDIS_URI=redis://localhost:6379
REDIS_SOCKET_TIMEOUT=30
REDIS_CONNECT_TIMEOUT=10
REDIS_MAX_CONNECTIONS=100
REDIS_RETRY_ATTEMPTS=3

### Milvus
MILVUS_URI=http://localhost:19530
MILVUS_DB_NAME=default

```
