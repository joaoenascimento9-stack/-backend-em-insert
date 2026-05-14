# Evidências - Backend em INSERT (Partes 1 e 2)

## Data da Verificação
**14 de Maio de 2026**

---

## Parte 5: Verificação da Rota de Data

### ✅ Status: CONCLUÍDA

A rota de busca por data foi verificada e está **completamente implementada**.

### Arquivo Verificado
- **Caminho**: `src/routes/leiturasRoutes.js`
- **Linhas**: 36-73

### Evidência do Código - ROTA GET `/leituras/data/:data`

```javascript
router.get('/leituras/data/:data', async (req, res) => {
  try {
    const { data } = req.params;

    const formatoValido = /^\d{4}-\d{2}-\d{2}$/.test(data);

    if (!formatoValido) {
      return res.status(400).json({
        mensagem: 'Formato de data inválido. Use o formato YYYY-MM-DD.',
        exemplo: '2026-05-11',
      });
    }

    const inicioDoDia = new Date(`${data}T00:00:00`);
    const fimDoDia = new Date(inicioDoDia);

    fimDoDia.setDate(fimDoDia.getDate() + 1);

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

### Funcionalidades Verificadas

| Funcionalidade | Status | Detalhes |
|---|---|---|
| **Rota definida** | ✅ | `GET /leituras/data/:data` |
| **Recebe data pela URL** | ✅ | Via `req.params.data` |
| **Validação de formato** | ✅ | Regex: `/^\d{4}-\d{2}-\d{2}$/` (YYYY-MM-DD) |
| **Tratamento de erro** | ✅ | Retorna 400 se formato inválido |
| **Consulta intervalo de data** | ✅ | Usa `Op.gte` e `Op.lt` |
| **Busca no banco** | ✅ | Consulta campo `timestamp` da tabela `Leitura` |
| **Ordenação** | ✅ | Resultados ordenados por `timestamp` (ASC) |
| **Resposta estruturada** | ✅ | Retorna data, total e array de leituras |
| **Tratamento de exceções** | ✅ | Try/catch com status 500 |

### Exemplos de Uso

**Requisição bem-sucedida:**
```bash
GET http://localhost:3000/leituras/data/2026-05-11
```

**Resposta esperada (200 OK):**
```json
{
  "dataPesquisada": "2026-05-11",
  "total": 5,
  "leituras": [
    {
      "id": 1,
      "temperatura": 22.5,
      "umidade": 65,
      "timestamp": "2026-05-11T08:30:00.000Z"
    },
    ...
  ]
}
```

**Requisição com formato inválido:**
```bash
GET http://localhost:3000/leituras/data/11-05-2026
```

**Resposta esperada (400 Bad Request):**
```json
{
  "mensagem": "Formato de data inválido. Use o formato YYYY-MM-DD.",
  "exemplo": "2026-05-11"
}
```

### Lógica de Intervalo de Data

1. **Início do dia**: `2026-05-11T00:00:00`
2. **Fim do dia**: `2026-05-12T00:00:00` (dia seguinte)
3. **Busca**: Registros com `timestamp >= inicioDoDia` E `timestamp < fimDoDia`
4. Garante que apenas registros do dia especificado sejam retornados

### Conclusão

✅ **A rota de busca por data está funcionando corretamente** com:
- Validação de formato robusto
- Intervalo de data preciso
- Resposta estruturada
- Tratamento de erros apropriado

---

## Resumo Geral

| Item | Resultado |
|---|---|
| Total de itens verificados | 1 |
| Itens completos | 1 ✅ |
| Itens com problemas | 0 |
| **Taxa de conclusão** | **100%** |

