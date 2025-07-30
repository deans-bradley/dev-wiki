To install Redis, you will require WSL2 (Windows subsystem for Linux), you can check is WSL is installed by running:

```shell
wsl --version
```

If you do not have WSL, you can follow this [guide](https://learn.microsoft.com/en-us/windows/wsl/install) provided by Microsoft.
### Install Redis

Add the repository to the `apt` index, update it, and then install:

```shell
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```

Lastly, start the Redis server like so:

```shell
sudo service redis-server start
```

### Connect to Redis

Once Redis is running, you can test it by running `redis-cli`:

```shell
redis-cli
```

Test the connection with the `ping` command:

```shell
127.0.0.1:6379> ping
PONG
```
