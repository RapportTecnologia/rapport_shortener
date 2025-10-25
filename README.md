# Rapport Shortener

## Visão geral
- **Propósito**: Plataforma completa de encurtamento e monitoramento de URLs utilizada pelos produtos Rapport.
- **Arquitetura DDD**: O backend em `rapport-shortener/backend/` organiza camadas `domain`, `application` e `infrastructure`, expondo uma API Express com Sequelize/MySQL. O frontend em `rapport-shortener/frontend/` é um painel React (Create React App) que consome essa API.
- **Integração automática**: O servidor backend (`src/index.ts`) inicializa o React Dev Server, permitindo desenvolvimento integrado (API + interface) com um único comando.

## Pré-requisitos
- **Node.js** >= 18 e **npm** >= 9 instalados no servidor.
- **MySQL/MariaDB** com mecanismo InnoDB disponível e credenciais válidas.
- Acesso à porta configurada em `PORT` (backend) e, opcionalmente, `REACT_APP_PORT` (frontend).

## Configuração do banco de dados
- **Schema**: utilize o script `rapport-shortener/backend/sql/sql_create.sql` para criar as tabelas `sites`, `sites_shortenes` e `sites_shortener_stats`.
- **Credenciais**: o backend lê as variáveis `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASS` e `PORT` do arquivo `.env` em `rapport-shortener/backend/` (mantenha um `.env.example` versionado).

## Configuração do frontend
- Crie um `.env` em `rapport-shortener/frontend/` com:
  - `REACT_APP_BACKEND_URL`: URL pública da API Express (ex.: `https://s.rapport.tec.br`).
  - `REACT_APP_PORT`: porta em que o React Dev Server escutará no modo desenvolvimento.
  - `PORT`: repita a porta para o servidor de desenvolvimento, se necessário pelo provedor de hospedagem.
  - Arquive um `.env.example` com os campos esperados para facilitar a replicação do ambiente.

## Instalação das dependências
1. **Backend**
   ```bash
   cd rapport-shortener/backend
   npm install
   ```
2. **Frontend**
   ```bash
   cd rapport-shortener/frontend
   npm install
   ```

## Execução em desenvolvimento
- **Comando único**:
  ```bash
  cd rapport-shortener/backend
  npm run dev
  ```
  - O backend será iniciado no `PORT` definido e, automaticamente, o React Dev Server executará `npm start` usando `src/loadFrontendEnv.ts` para repassar variáveis do `.env` do frontend.
- **Logs**: acompanhe o terminal. Ajuste o logger conforme os padrões da organização (Pino com agrupamento `[setor/tema]`).

## Build e execução em produção
- **Build do backend**:
  ```bash
  cd rapport-shortener/backend
  npm run build
  ```
- **Start em produção** (após build):
  ```bash
  node dist/index.js
  ```
  - O código compilado continua inicializando o servidor do frontend. Para hospedar o frontend como estático, execute `npm run build` em `rapport-shortener/frontend/` e sirva a pasta `build/` em um servidor HTTP dedicado.

## Fluxo sugerido para deploy
1. Configurar banco (script em `backend/sql/`).
2. Criar `.env` no backend com credenciais e porta.
3. Criar `.env` no frontend apontando para o domínio público da API.
4. Instalar dependências em `backend/` e `frontend/`.
5. Executar `npm run build` em ambos os módulos.
6. Publicar `dist/index.js` (API) em ambiente Node.js e servir o `frontend/build/` via Nginx ou similar.
7. Monitorar logs seguindo o padrão `[setor/tema] Mensagem` com emoticons e estatísticas relevantes.

## Contatos e suporte
- Pull requests e issues são bem-vindos no repositório. Ajustes específicos podem seguir a política já descrita em `backend/README.md`.

