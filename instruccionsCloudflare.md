# Cloudflare

## Exposar un port local a internet (a través de cloudflare tunnel)

Mitjançant

```bash
cloudflared tunnel --protocol http2 --url http://localhost:4000
```

Això obrirà el port 4000 del que estigui funcionant al local cap a internet.<br />
En aquest cas, Elixir.
