# Docker NS1 Dynamic DNS
This updates DNS records in NS1 with the current IP (from [ipify.org](https://www.ipify.org)) every 5 minutes. The script runs under `cron` inside a lightweight Alpine-based Docker container.

[NS1](https://ns1.com) is a DNS provider that offers a generous free plan (500k queries/month, 50 records) and an API.

## Usage
### docker run
```
docker run -d \
    -e DOMAINS=example.com=www.example.com,blog.example.com;example.org=www.example.org,anotherblog.example.org \
    -e FREQUENCY=5 \
    rzen/ns1-dynamic-dns:latest
```

### docker-compose
```yaml
services:
  dynamic-dns:
    image: rzen/ns1-dynamic-dns:latest
    environment:
      - NS1_API_KEY=${NS1_API_KEY}  # defined in .env file
      - DOMAINS=example.com=www.example.com,blog.example.com;example.org=www.example.org,anotherblog.example.org
      - FREQUENCY=5
    restart: always
```

### custom frequency
You can change the value of the `FREQUENCY` environment variable to make the script run every `$FREQUENCY` minutes. The default is every 5 minutes.

### testing
To test the script, run it through `docker run` and append `/dynamic-dns.py`. This will run the script once, then kill the container. Example:

```
docker run --rm rzen/ns1-dynamic-dns:latest /dynamic-dns.py
```

## Configuration

Config file is not required in this fork.

API key is provided in the `NS1_API_KEY` environment variable.

Domains and hosts are provided in the `DOMAINS` environment variable.
