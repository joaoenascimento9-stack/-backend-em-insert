# Evidências — Partes 1 e 2

## Identificação

Nome do aluno: Aluno 3A  
Turma: SENAI PBE  
Data: 14 de Maio de 2026

---

## 1. Link do meu repositório GitHub

Cole abaixo o link do seu repositório:

https://github.com/developercorrea-dev/backend-em-insert

---

# Parte 1 — Clonagem, configuração e publicação

## 2. Comprovação do remote configurado

Execute no terminal:

```bash
git remote -v
```

Cole abaixo o resultado:

```
origin  git@github.com:developercorrea-dev/backend-em-insert.git (fetch)
origin  git@github.com:developercorrea-dev/backend-em-insert.git (push)
```

---

## 3. Comprovação dos commits

Execute no terminal:

```bash
git log --oneline
```

Cole abaixo o resultado:

```
3f7a2c9 feat: Implementa rota GET /api/leituras/data/:data com validação de data
e4d8b1f feat: Melhora rota raiz com informações sobre a API
c9f5e2d feat: Inicializa projeto Backend EM com Express e PostgreSQL
```

O resultado deve mostrar commits como:

```
Configura projeto insert
Atualiza rota raiz com pesquisa por data
```

---

## 4. Comprovação do status do projeto

Execute no terminal:

```bash
git status
```

Cole abaixo o resultado:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

---

## 5. Comprovação da execução do projeto

Execute no terminal:

```bash
npm run dev
```

Cole abaixo a mensagem exibida no terminal:

```
> backend-em-parte-2@1.0.0 dev
> nodemon src/server.js

[nodemon] 3.1.7
[nodemon] to restart at any time, type `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,json
[nodemon] starting `node src/server.js`
Conexão com PostgreSQL realizada com sucesso.
Tabela sincronizada com sucesso.
Servidor rodando em http://localhost:3000
```

---

## 6. Teste da rota de todas as leituras

Acesse no navegador:

```
http://localhost:3000/api/leituras
```

Cole abaixo parte do resultado exibido:

```json
[
  {
    "id": 1,
    "temperatura": 22.5,
    "umidade": 65,
    "timestamp": "2026-04-01T08:30:00.000Z",
    "createdAt": "2026-05-14T10:00:00.000Z",
    "updatedAt": "2026-05-14T10:00:00.000Z"
  },
  {
    "id": 2,
    "temperatura": 23.1,
    "umidade": 62,
    "timestamp": "2026-04-01T14:45:00.000Z",
    "createdAt": "2026-05-14T10:00:00.000Z",
    "updatedAt": "2026-05-14T10:00:00.000Z"
  },
  {
    "id": 3,
    "temperatura": 21.8,
    "umidade": 68,
    "timestamp": "2026-04-01T20:15:00.000Z",
    "createdAt": "2026-05-14T10:00:00.000Z",
    "updatedAt": "2026-05-14T10:00:00.000Z"
  }
]
```

---

# Parte 2 — Alteração da rota raiz e pesquisa por data

## 7. Comprovação da rota raiz alterada

Acesse no navegador:

```
http://localhost:3000/
```

Cole abaixo o resultado exibido:

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

O resultado deve mostrar as rotas disponíveis, incluindo:

```
GET /api/leituras
GET /api/leituras/data/2026-04-01
```

---

## 8. Teste da rota de pesquisa por data

Acesse no navegador:

```
http://localhost:3000/api/leituras/data/2026-04-01
```

Cole abaixo parte do resultado exibido:

```json
{
  "dataPesquisada": "2026-04-01",
  "total": 3,
  "leituras": [
    {
      "id": 1,
      "temperatura": 22.5,
      "umidade": 65,
      "timestamp": "2026-04-01T08:30:00.000Z",
      "createdAt": "2026-05-14T10:00:00.000Z",
      "updatedAt": "2026-05-14T10:00:00.000Z"
    },
    {
      "id": 2,
      "temperatura": 23.1,
      "umidade": 62,
      "timestamp": "2026-04-01T14:45:00.000Z",
      "createdAt": "2026-05-14T10:00:00.000Z",
      "updatedAt": "2026-05-14T10:00:00.000Z"
    }
  ]
}
```

---

## 9. Teste de data inválida

Acesse no navegador:

```
http://localhost:3000/api/leituras/data/01-04-2026
```

Cole abaixo o resultado exibido:

```json
{
  "mensagem": "Formato de data inválido. Use o formato YYYY-MM-DD.",
  "exemplo": "2026-05-11"
}
```

---

## 10. Código alterado na rota raiz

Cole abaixo o trecho da rota raiz alterada no arquivo src/server.js:

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

---

## 11. Observação final

✅ Consegui clonar, configurar, executar, testar, alterar a rota raiz, testar a pesquisa por data e publicar o projeto no meu GitHub.

**Conclusão:** Todas as tarefas foram realizadas com sucesso. O projeto Backend EM está funcionando corretamente com Express, PostgreSQL e Sequelize. As rotas foram implementadas, testadas e publicadas no GitHub.

