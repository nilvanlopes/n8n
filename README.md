# n8n com Docker Swarm

Stack do [n8n](https://n8n.io/) para este ambiente Docker Swarm, com PostgreSQL, Redis e publicação via Traefik.

## Pré-requisitos

- Docker com Swarm ativo
- Rede `traefik-public`
- Rede `n8n`
- Traefik já publicado no cluster
- Um domínio apontando para o Traefik

## Configuração

1. Crie o arquivo de ambiente:

   ```bash
   cp n8n/.env.example n8n/.env
   ```

2. Edite `n8n/.env` e ajuste pelo menos:

- `N8N_HOST`
- `N8N_EDITOR_BASE_URL`
- `WEBHOOK_URL`
- `POSTGRES_PASSWORD`
- `N8N_ENCRYPTION_KEY`

3. Faça o deploy:

   ```bash
   make deploy-n8n
   ```

## Observações

- A stack roda em `queue mode`, com `n8n-main` e `n8n-worker`.
- `N8N_ENCRYPTION_KEY` precisa ser estável. Se mudar depois, credenciais já salvas podem parar de funcionar.
- `WEBHOOK_URL` deve usar a URL pública final do serviço.
- O `n8n-main` é exposto via Traefik na porta interna `5678`.

## Acesso

Depois do deploy, a interface ficará disponível em `https://<N8N_HOST>`.
