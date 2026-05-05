# Comando para criar uma pasta no usuário para guardar os containers criados

## Altere as linhas com os valores que desejar
| -e POSTGRES_PASSWORD=senhaSegura
| -e POSTGRES_USER=usuario

```bash
mkdir -p ~/docker/postgres_data
docker run -d \
  --name meu_postgres \
  -e POSTGRES_PASSWORD=senhaSegura \
  -e POSTGRES_USER=usuario \
  -e POSTGRES_DB=meubanco \
  -v ~/docker/postgres_data:/var/lib/postgresql \
  -p 5432:5432 \
  postgres:18
```
