# sharedredis
Docker Redis Shared Volume

Requirements:
- Docker
- docker-compose

Steps:

1) Configure port, network, user and password as you wish. Default port is 6380.
2) `docker-compose up -d`

By using the default docker-compose.yml of this proyect, your containers will connect to the 6379 port using the shared network.
If you want to connect outside docker (e.g, sqlpro or pgAdmin), you can do it by connecting through localhost at port 6380.

Now, on your local docker postgres, you can set your connection to be sharedpg. In your proyect's docker-compose.yml, you can set the network to bridge so it can connect to this postgres server, like this:

```
networks:
  shared:
    driver: bridge
    external: true

services:
  your-service:
    ...
    networks:
      - shared

volumes:
  sharedredis:
    external: true
```

If you're using Rails, here's an example of how my redis initializer looks like:

```
url = ENV['REDIS_URL'] || Rails.application.secrets.rails_redis_url

Redis.current = Redis.new(
  url: url
)

```

You can autostart your container whenever your OS boots up, running this command:
`docker start sharedredis`
