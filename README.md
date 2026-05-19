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
- O MCP nativo do n8n usa o mesmo host público. Para o MCP instance-level, use `https://<N8N_HOST>/mcp-server/http`.
- Workflows não ficam expostos ao MCP automaticamente: habilite `Settings > Instance-level MCP` e depois marque cada workflow como `Available in MCP`.
- O MCP Server Trigger por workflow usa rotas `/mcp*`. Esta stack tem uma única réplica `n8n-main`, então as conexões SSE/HTTP streamable ficam concentradas em uma instância.

## MCP

Para conectar um cliente MCP por token:

1. Acesse `https://<N8N_HOST>`.
2. Vá em `Settings > Instance-level MCP`.
3. Ative o MCP da instância.
4. Em `Connection details`, gere/copiei o token de acesso.
5. Configure o cliente com:

   ```toml
   [mcp_servers.n8n_mcp]
   url = "https://<N8N_HOST>/mcp-server/http"
   http_headers = { "authorization" = "Bearer <N8N_MCP_TOKEN>" }
   ```

Com o domínio atual do `.env`, a URL fica:

```text
https://n8n.nilvanlopes.com/mcp-server/http
```

## Fred local

Quando a API do Fred estiver exposta localmente via Traefik, o endpoint usado pelos workflows deve ser:

```text
http://fred-api:8000/messages/process
```

O n8n entra na rede `traefik-local`, então pode chamar o serviço pelo alias interno `fred-api` diretamente.

## Acesso

Depois do deploy, a interface ficará disponível em `https://<N8N_HOST>`.
