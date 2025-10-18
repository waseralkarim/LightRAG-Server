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

### Edit Redis config: `sudo vim /etc/redis/redis.conf`

```jsx
requirepass redispass

bind 0.0.0.0

protected-mode no
```
