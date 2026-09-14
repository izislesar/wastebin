# wastebin selfhost notes

Single sqlite file, scratch image, ~10mb ram. Nothing autostarts.

Note: the image has no shell, so `docker exec` won't work. Logs only.

## up

```bash
docker compose -f selfhost/docker-compose.yml up -d
```

open http://localhost:8083.

## backup

```bash
docker compose -f selfhost/docker-compose.yml stop
docker run --rm -v wastebin-data:/data -v $(pwd)/backups:/b alpine \
  tar czf /b/wastebin-$(date +%F).tar.gz /data
docker compose -f selfhost/docker-compose.yml start
```

## update

```bash
docker compose -f selfhost/docker-compose.yml pull
docker compose -f selfhost/docker-compose.yml up -d
```

## k8s later

manifests live in `k8s/` (added separately): deployment + service + ingress
with a 128Mi limit — this thing OOMs never, but limits are habit.
