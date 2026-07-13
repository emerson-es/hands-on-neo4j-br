# Cypher Hands-On: Malha Metroferroviária de SP

**Schema usado:** `(:Estacao)-[:PERTENCE_A {ordem}]->(:Linha)`, `(:Estacao)-[:CONECTA {linha, distancia_km, tempo_min}]->(:Estacao)`, `(:Estacao)-[:PROXIMO_A_POI]->(:PontoDeInteresse)`.

---

## Nível 1 — Básico

### 1.1 Estações de uma linha, em ordem

```cypher
MATCH (e:Estacao)-[r:PERTENCE_A]->(l:Linha {nome: "Linha 3-Vermelha"})
RETURN e.nome AS estacao, r.ordem AS ordem
ORDER BY r.ordem;
```
**Técnica:** `MATCH` com filtro de propriedade direto no nó (`{nome: "..."}`), seguido de `ORDER BY` numa propriedade do relacionamento (`r.ordem`), não do nó. É o padrão mais comum: navegar uma aresta e ordenar pelo atributo que descreve essa navegação.

### 1.2 Quais linhas passam por uma estação

```cypher
MATCH (e:Estacao {nome: "Se"})-[:PERTENCE_A]->(l:Linha)
RETURN e.nome AS estacao, collect(l.nome) AS linhas;
```
**Técnica:** `collect()` agrega múltiplos nós relacionados numa lista só. Aqui identifica visualmente uma estação-hub (Sé aparece com 2 linhas) sem precisar de `COUNT` — a lista já entrega a resposta.

---

## Nível 2 — Intermediário

### 2.1 Menor caminho entre duas estações

```cypher
MATCH (origem:Estacao {nome: "Tucuruvi"}), (destino:Estacao {nome: "Jabaquara"})
MATCH caminho = shortestPath((origem)-[:CONECTA*..60]-(destino))
RETURN [n IN nodes(caminho) | n.nome] AS estacoes, length(caminho) AS trechos;
```
**Técnica:** `shortestPath()` sobre um padrão de comprimento variável (`*..60` = até 60 saltos). `nodes(caminho)` extrai os nós do caminho encontrado; `length(caminho)` conta os relacionamentos. Testado ao vivo: entre Tucuruvi e Jabaquara o algoritmo encontra uma rota de 20 trechos passando por 4 linhas diferentes (L1→L11→L10→L2→L5) — **mais curta em saltos do que ficar numa linha só**. Prova prática de que "menor caminho" ≠ "caminho mais óbvio".

### 2.2 Ranking de estações-hub

```cypher
MATCH (e:Estacao)-[:PERTENCE_A]->(l:Linha)
WITH e, count(DISTINCT l) AS n_linhas
WHERE n_linhas > 1
RETURN e.nome AS estacao, n_linhas
ORDER BY n_linhas DESC, estacao
LIMIT 5;
```
**Técnica:** `WITH` funciona como um pipeline — fecha a agregação (`count(DISTINCT l)`) antes de continuar a query, permitindo um `WHERE` **depois** de agregar (equivalente a `HAVING` em SQL). Resultado real: Brás lidera com 4 linhas convergindo.

---

## Nível 3 — Avançado

### 3.1 Rota mais rápida ponderada por tempo (Dijkstra)

```cypher
MATCH (origem:Estacao {nome: "Aeroporto-Guarulhos"}), (destino:Estacao {nome: "Congonhas"})
CALL apoc.algo.dijkstra(origem, destino, "CONECTA>|CONECTA<", "tempo_min")
YIELD path, weight
RETURN [n IN nodes(path) | n.nome] AS estacoes, weight AS tempo_total_min;
```
**Técnica:** `CALL ... YIELD` invoca um procedimento do plugin APOC — sai do Cypher puro e usa uma implementação de Dijkstra pronta. `"CONECTA>|CONECTA<"` diz para percorrer o relacionamento nos dois sentidos; `"tempo_min"` é a propriedade usada como peso. Diferença chave do nível 2: aqui o caminho considera o **custo acumulado**, não o número de saltos. Testado ao vivo: 18 estações, 59 minutos, cruzando 5 linhas diferentes.

### 3.2 Simulação "e se essa estação fechar?"

```cypher
MATCH (origem:Estacao {nome: "Jabaquara"}), (destino:Estacao {nome: "Palmeiras-Barra Funda"})
MATCH caminho_hoje = shortestPath((origem)-[:CONECTA*..60]-(destino))
WITH origem, destino, length(caminho_hoje) AS trechos_hoje
OPTIONAL MATCH caminho_sem_luz = shortestPath((origem)-[:CONECTA*..60]-(destino))
  WHERE NONE(n IN nodes(caminho_sem_luz) WHERE n.nome = "Luz")
RETURN trechos_hoje,
       CASE WHEN caminho_sem_luz IS NULL THEN "SEM ROTA POSSIVEL"
            ELSE length(caminho_sem_luz) END AS trechos_sem_luz;
```
**Técnica:** três coisas combinadas — `WHERE NONE(n IN nodes(caminho) WHERE ...)` filtra nós de dentro do próprio caminho (exclui a estação "fechada" da busca, sem apagar nada do banco); `OPTIONAL MATCH` garante que a query não quebra quando não existe mais rota alguma; `CASE WHEN` trata esse "sem rota" no retorno. Testado ao vivo: fechar Luz faz a rota Jabaquara→Palmeiras-Barra Funda ir de 16 para 17 trechos (desvia por Sé). Essa é a técnica por trás de qualquer simulação de resiliência de rede sem precisar de plugin de grafo.

---

## Resumo das técnicas por nível

| Nível | Técnicas |
|---|---|
| Básico | filtro de propriedade, `ORDER BY`, `collect()` |
| Intermediário | `shortestPath()`, caminho de comprimento variável, `WITH` como pipeline de agregação, filtro pós-agregação |
| Avançado | procedimento externo (APOC/Dijkstra) com peso customizado, exclusão de nó via `WHERE NONE()`, `OPTIONAL MATCH` + `CASE` para simulação sem mutar dados |
