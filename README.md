
## Starting up Elastic and OTEL collector

```
# start elastic first
docker compose up -d

# generate elastic service token
docker exec -it es bin/elasticsearch-service-tokens create elastic/kibana kibana-token

# export generated token 
export KIBANA_TOKEN='INSERT_TOKEN'

# run compose again to start kibana with the token
docker compose up -d
```

## Viewing Traces in Kibana

1. Open [http://localhost:5601](http://localhost:5601).

   * Username: `elastic`
   * Password: `changeme`

2. Go to **Kibana → Observability → APM**.

3. Send a test trace with `otel-cli`:

```bash
docker run --rm --network host ghcr.io/equinix-labs/otel-cli:latest \
  span \
    --service apm-test \
    --name "docker-test-span" \
    --kind client \
    --protocol grpc \
    --endpoint http://localhost:4317 --verbose
```

4. You should now see the trace in Kibana if it's sent successfully.
