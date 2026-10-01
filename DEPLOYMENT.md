# VM deployment

## Requirements

- Docker Engine
- Docker Compose v2
- Nginx or another reverse proxy forwarding to local port 3040

## Start the application

```bash
cp .env.example .env
docker compose up -d --build
```

The container binds only to `127.0.0.1:3040`, so it is not directly exposed to
the internet. Open the configured HTTPS domain and create the first CMS
administrator at `/admin`.

The named volume `eastafricaexplore_data` preserves the catalogue, administrator
account, enquiries, bookings, and affiliate-click records across container updates.

## Update from Git

```bash
git pull
docker compose up -d --build
```

## Back up the JSON data

```bash
docker run --rm \
  -v eastafricaexplore_data:/data \
  -v "$PWD":/backup \
  alpine tar -czf /backup/eastafricaexplore-data.tar.gz -C /data .
```

For a public client demo, place the container behind HTTPS. Do not expose the CMS
over plain HTTP on the public internet.
