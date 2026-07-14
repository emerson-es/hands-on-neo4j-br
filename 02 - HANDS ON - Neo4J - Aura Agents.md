# Criar um Aura Agent

Passos para criar um agente de IA (GraphRAG) sobre a instância Aura, usando os dados restaurados.

## 1. Criar o agente

1. No menu lateral, abra **Agents** (dentro de **Data Services**).
2. Clique em **Create Agent**. Há duas opções:

### Opção A — Create with AI (recomendado)

1. Selecione a instância (precisa estar **Running**).
2. Se o banco tiver embeddings vetoriais, marque **This instance contains vector embeddings** e escolha o provedor/modelo (ex.: OpenAI `text-embedding-3-small`).
3. Escreva um prompt descrevendo o caso de uso do agente: o que ele faz, exemplos de perguntas, e quando usar cada tipo de ferramenta (templates Cypher, busca por similaridade, Text2Cypher como último recurso).
   Copie o prompt abaixo:
```
  Você é um guia especialista na rede metroferroviária da Região Metropolitana de São Paulo. Responda perguntas de forma clara, correta e explicativa.
  Como por exemplo menor rota entre 2 estações. Todas as informações factuais devem ser obtidas exclusivamente da base de dados do grafo, e nunca da sua própria memória.
  Ajude os usuário a ajustar os nomes de estação e linhas caso necessário.
```
5. Clique em **Create** e aguarde alguns minutos — a IA lê o schema do grafo e gera nome, descrição, prompt de sistema e as ferramentas (tools) automaticamente.

### Opção B — Create from scratch

1. Selecione a instância alvo.
2. Preencha nome, descrição e (opcional) instruções de prompt — quanto mais detalhado, melhor o agente se comporta.
3. Adicione manualmente as ferramentas (ver seção abaixo).

## 3. Ferramentas disponíveis

- **Cypher Template**: query Cypher parametrizada e reutilizável. Ideal para perguntas previsíveis e lógica de negócio bem definida. Evite retornar embeddings ou nós/relações inteiros; limite a 10-50 linhas.
- **Similarity Search**: busca semântica via índice vetorial. Requer embeddings no banco. Pode encadear uma query Cypher para trazer dados conectados ao resultado.
- **Text2Cypher**: converte pergunta em linguagem natural em Cypher dinamicamente. Use só como último recurso, quando nenhuma outra ferramenta cobre o caso — deixe isso explícito na descrição da ferramenta.

## 4. Testar o agente

1. Na tela do agente, teste as seguintes perguntas: 
  
  ```
    Preciso ir do Campo Limpo pra Aeroporto-Guarulhos. Qual a melhor forma?
  ```

```
  Quais estações tem a Linha 17-Ouro e quais conexões ela tem?
```

3. Abra o painel de **Reasoning** para ver quais ferramentas foram chamadas, com quais parâmetros, e como o resultado foi montado.
4. Ajuste descrições das ferramentas/prompt se o agente escolher a ferramenta errada.
5. Clique em **Create agent** (ou **Update agent**) para salvar.


**Referências:**
- [Aura Agent — documentação oficial](https://neo4j.com/docs/aura/aura-agent/)
- [Tutorial passo a passo — Developer Guides](https://neo4j.com/developer/genai-ecosystem/aura-agent-getting-started/)

