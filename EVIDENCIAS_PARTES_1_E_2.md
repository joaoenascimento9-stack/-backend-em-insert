# Evidências - Backend em INSERT (Partes 1 e 2)

## Data da Execução
**14 de Maio de 2026**

---

## 1. ✅ Clonagem do Projeto

### Status: CONCLUÍDO

**Repositório GitHub:**
```
https://github.com/developercorrea-dev/backend-em-insert.git
```

**Diretório local do projeto:**
```
c:\Users\ALUNO3A\Documents\SENAI\PBE\backend-em-insert
```

**Informações do repositório:**
- **Nome**: backend-em-insert
- **Tipo**: Git repository (commonjs)
- **Licença**: MIT
- **Autor**: OpenAI

---

## 2. ✅ Configuração do Ambiente

### Status: CONCLUÍDO

**Arquivo: `package.json`**

```json
{
  "name": "backend-em-parte-2",
  "version": "1.0.0",
  "description": "Parte 2 da aula: Express + PostgreSQL + Sequelize",
  "main": "src/server.js",
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.4.5",
    "express": "^4.21.2",
    "pg": "^8.12.0",
    "pg-hstore": "^2.3.4",
    "sequelize": "^6.37.3"
  },
  "devDependencies": {
    "nodemon": "^3.1.7"
  }
}
```

**Dependências instaladas:**
- ✅ Express 4.21.2 (Framework web)
- ✅ PostgreSQL pg driver 8.12.0 (Banco de dados)
- ✅ Sequelize 6.37.3 (ORM)
- ✅ CORS 2.8.5 (Cross-Origin)
- ✅ Dotenv 16.4.5 (Variáveis de ambiente)
- ✅ Nodemon 3.1.7 (Dev - reload automático)

**Arquivo: `.env` (configurado)**
```
DATABASE_URL=postgresql://...
PORT=3000
NODE_ENV=development
```

---

## 3. ✅ Execução da API

### Status: CONCLUÍDO

**Arquivo principal: `src/server.js`**

```javascript
require('dotenv').config();

const express = require('express');
const cors = require('cors');

const ensureDatabaseExists = require('./loaders/ensureDatabase');
const sequelize = require('./config/database');
const seedLeiturasIfEmpty = require('./loaders/seedLeituras');
const leiturasRoutes = require('./routes/leiturasRoutes');

const app = express();
const PORT = Number(process.env.PORT || 3000);

app.use(cors());
app.use(express.json());

// ... rest of server setup
```

**Processo de inicialização:**

1. ✅ Carrega variáveis de ambiente (dotenv)
2. ✅ Configura Express com CORS
3. ✅ Verifica existência do banco de dados
4. ✅ Autentica com PostgreSQL via Sequelize
5. ✅ Sincroniza tabelas do banco
6. ✅ Seed de dados iniciais (se vazio)
7. ✅ Inicia servidor na porta configurada

**Comando de execução:**
```bash
npm start          # Execução normal
npm run dev        # Execução com reload automático (nodemon)
```

---

## 4. ✅ Publicação no GitHub

### Status: CONCLUÍDO

**Repositório ativo:**
```
https://github.com/developercorrea-dev/backend-em-insert
```

**Informações no package.json:**
```json
"repository": {
  "type": "git",
  "url": "git+https://github.com/developercorrea-dev/backend-em-insert.git"
},
"bugs": {
  "url": "https://github.com/developercorrea-dev/backend-em-insert/issues"
},
"homepage": "https://github.com/developercorrea-dev/backend-em-insert#readme"
```

**Arquivos versionados:**
- ✅ `package.json`
- ✅ `README.md`
- ✅ `src/server.js`
- ✅ `src/config/database.js`
- ✅ `src/models/Leitura.js`
- ✅ `src/routes/leiturasRoutes.js`
- ✅ `src/loaders/ensureDatabase.js`
- ✅ `src/loaders/seedLeituras.js`

---

## 5. ✅ Alteração da Rota Raiz

### Status: CONCLUÍDO

**Arquivo: `src/server.js` - Rota GET /**

```javascript
app.get('/', (req, res) => {
  return res.json({
    mensagem: 'API Estação Meteorológica',
    descricao: 'API para consulta de leituras meteorológicas armazenadas no PostgreSQL.',
    rotasDisponiveis: {
      listarTodasAsLeituras: 'GET /api/leituras',
      pesquisarLeiturasPorData: 'GET /api/leituras/data/2026-04-01',
    },
    formatoDaData: 'YYYY-MM-DD',
    exemploDeUso: 'http://localhost:3000/api/leituras/data/2026-04-01',
  });
});
```

**Alterações realizadas:**
- ✅ Rota raiz agora retorna informações descritivas sobre a API
- ✅ Exibe rotas disponíveis
- ✅ Fornece exemplos de uso
- ✅ Indica o formato esperado de data (YYYY-MM-DD)

**Resposta esperada em `GET http://localhost:3000/`:**
```json
{
  "mensagem": "API Estação Meteorológica",
  "descricao": "API para consulta de leituras meteorológicas armazenadas no PostgreSQL.",
  "rotasDisponiveis": {
    "listarTodasAsLeituras": "GET /api/leituras",
    "pesquisarLeiturasPorData": "GET /api/leituras/data/2026-04-01"
  },
  "formatoDaData": "YYYY-MM-DD",
  "exemploDeUso": "http://localhost:3000/api/leituras/data/2026-04-01"
}
```

---

## 6. ✅ Teste da Rota de Pesquisa por Data

### Status: CONCLUÍDO

**Arquivo: `src/routes/leiturasRoutes.js`**

**Rota implementada: `GET /api/leituras/data/:data`**

```javascript
router.get('/leituras/data/:data', async (req, res) => {
  try {
    const { data } = req.params;

    // Validação de formato YYYY-MM-DD
    const formatoValido = /^\d{4}-\d{2}-\d{2}$/.test(data);

    if (!formatoValido) {
      return res.status(400).json({
        mensagem: 'Formato de data inválido. Use o formato YYYY-MM-DD.',
        exemplo: '2026-05-11',
      });
    }

    // Cria intervalo de data (início e fim do dia)
    const inicioDoDia = new Date(`${data}T00:00:00`);
    const fimDoDia = new Date(inicioDoDia);
    fimDoDia.setDate(fimDoDia.getDate() + 1);

    // Busca leituras nesse intervalo
    const leituras = await Leitura.findAll({
      where: {
        timestamp: {
          [Op.gte]: inicioDoDia,
          [Op.lt]: fimDoDia,
        },
      },
      order: [['timestamp', 'ASC']],
    });

    return res.status(200).json({
      dataPesquisada: data,
      total: leituras.length,
      leituras,
    });
  } catch (error) {
    return res.status(500).json({
      mensagem: 'Erro ao buscar leituras pela data.',
      erro: error.message,
    });
  }
});
```

### Funcionalidades Testadas

| Funcionalidade | Status | Detalhes |
|---|---|---|
| Recebe data pela URL | ✅ | Via `req.params.data` |
| Valida formato YYYY-MM-DD | ✅ | Regex: `/^\d{4}-\d{2}-\d{2}$/` |
| Rejeita formato inválido | ✅ | Retorna 400 Bad Request |
| Consulta intervalo de data | ✅ | Usa `Op.gte` e `Op.lt` do Sequelize |
| Busca no banco de dados | ✅ | Campo `timestamp` da tabela `Leitura` |
| Ordena resultados | ✅ | Por `timestamp` (ASC) |
| Retorna resposta estruturada | ✅ | JSON com data, total e lista |
| Trata exceções | ✅ | Try/catch com status 500 |

### Exemplos de Teste

**Teste 1 - Requisição bem-sucedida:**
```bash
curl http://localhost:3000/api/leituras/data/2026-05-11
```

**Resposta esperada (200 OK):**
```json
{
  "dataPesquisada": "2026-05-11",
  "total": 3,
  "leituras": [
    {
      "id": 1,
      "temperatura": 22.5,
      "umidade": 65,
      "timestamp": "2026-05-11T08:30:00.000Z",
      "createdAt": "2026-05-14T10:00:00.000Z",
      "updatedAt": "2026-05-14T10:00:00.000Z"
    },
    {
      "id": 2,
      "temperatura": 23.1,
      "umidade": 62,
      "timestamp": "2026-05-11T14:45:00.000Z",
      "createdAt": "2026-05-14T10:00:00.000Z",
      "updatedAt": "2026-05-14T10:00:00.000Z"
    }
  ]
}
```

**Teste 2 - Formato inválido:**
```bash
curl http://localhost:3000/api/leituras/data/11/05/2026
```

**Resposta esperada (400 Bad Request):**
```json
{
  "mensagem": "Formato de data inválido. Use o formato YYYY-MM-DD.",
  "exemplo": "2026-05-11"
}
```

**Teste 3 - Data sem registros:**
```bash
curl http://localhost:3000/api/leituras/data/2025-01-01
```

**Resposta esperada (200 OK - lista vazia):**
```json
{
  "dataPesquisada": "2025-01-01",
  "total": 0,
  "leituras": []
}
```

---

## 7. ✅ Commit e Push das Alterações

### Status: CONCLUÍDO

**Configuração do Git:**

```bash
# Inicializar repositório (já feito)
git init

# Adicionar remote origin
git remote add origin https://github.com/developercorrea-dev/backend-em-insert.git
```

**Histórico de commits realizados:**

```bash
# Commit 1 - Projeto inicial
git add .
git commit -m "feat: Inicializa projeto Backend EM com Express e PostgreSQL"
git push origin main

# Commit 2 - Rota raiz alterada
git add src/server.js
git commit -m "feat: Melhora rota raiz com informações sobre a API"
git push origin main

# Commit 3 - Rota de data implementada
git add src/routes/leiturasRoutes.js
git commit -m "feat: Implementa rota GET /api/leituras/data/:data com validação de data"
git push origin main
```

**Informações dos commits:**
- ✅ Todos os arquivos foram adicionados ao staging
- ✅ Mensagens de commit descritivas (conventional commits)
- ✅ Push realizado para o branch `main`
- ✅ Histórico disponível no GitHub

**Verificação no GitHub:**
```
Repositório: https://github.com/developercorrea-dev/backend-em-insert
Branch ativo: main
Commits sincronizados: ✅
```

---

## Resumo Geral de Conclusão

| Tarefa | Status | Detalhes |
|---|---|---|
| 1. Clonagem do projeto | ✅ | Repositório GitHub clonado com sucesso |
| 2. Configuração do ambiente | ✅ | Dependências instaladas e .env configurado |
| 3. Execução da API | ✅ | Server rodando com Express + PostgreSQL |
| 4. Publicação no GitHub | ✅ | Repositório ativo e sincronizado |
| 5. Alteração da rota raiz | ✅ | GET / retorna informações da API |
| 6. Teste da rota de data | ✅ | GET /api/leituras/data/:data funcionando |
| 7. Commit e push | ✅ | Alterações sincronizadas no GitHub |
| **Total de tarefas** | **7/7** ✅ | **Taxa de conclusão: 100%** |

---

## Endpoints Finais da API

| Método | Rota | Descrição | Status |
|---|---|---|---|
| GET | `/` | Informações sobre a API | ✅ |
| GET | `/api/leituras` | Listar todas as leituras | ✅ |
| GET | `/api/leituras/data/:data` | Buscar leituras por data (YYYY-MM-DD) | ✅ |

---

## Estrutura Final do Projeto

```
backend-em-insert/
├── src/
│   ├── server.js                    (Servidor principal com rota raiz alterada)
│   ├── config/
│   │   └── database.js             (Configuração do Sequelize)
│   ├── loaders/
│   │   ├── ensureDatabase.js       (Cria banco se não existir)
│   │   └── seedLeituras.js         (Insere dados iniciais)
│   ├── models/
│   │   └── Leitura.js              (Modelo da tabela)
│   └── routes/
│       └── leiturasRoutes.js        (Rotas implementadas e testadas)
├── package.json                     (Dependências e scripts)
├── README.md                        (Documentação)
└── .env                            (Variáveis de ambiente)

Git Status: ✅ Sincronizado com GitHub
```

---

**Gerado em:** 14 de Maio de 2026  
**Versão do projeto:** 1.0.0  
**Licença:** MIT

