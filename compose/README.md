# Compose

One folder per service, each with its own `docker-compose.yml` and an `.env.example` holding placeholder values.

```markdown
compose/
├── pihole/
├── jellyfin/
└── homepage/
```

Copy `.env.example` to `.env` on the server and fill in real values. `.env` is git-ignored.
