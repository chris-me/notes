docker-compose.yml

```yaml
version: '3'

services:
  prisma:
    image: prismagraphql/prisma:1.20
    restart: unless-stopped
    ports:
      - "4466:4466"
    environment:
      PRISMA_CONFIG: |
        port: 4466
        databases:
          default:
            connector: postgres
            host: postgres
            port: 5432
            user: prisma
            password: prisma
            migrations: true
  postgres:
    image: postgres:10.6
    restart: unless-stopped
    environment:
      POSTGRES_USER: prisma
      POSTGRES_PASSWORD: prisma
    volumes:
      - postgres:/var/lib/postgresql/data
  pgadmin:
    image: dpage/pgadmin4
    restart: unless-stopped
    ports:
      - "10080:80"
    environment:
      - PGADMIN_DEFAULT_EMAIL=foo@bar.com
      - PGADMIN_DEFAULT_PASSWORD=foobar
    volumes:
      - pgadmin-data:/var/lib/pgadmin

volumes:
  postgres:
  pgadmin-data:
```
