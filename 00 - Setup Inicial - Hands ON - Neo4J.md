# Setup do Neo4j Aura

Passos para criar uma instância gratuita no Neo4j Aura e restaurar o dump deste repositório.

## 1. Criar conta free no Neo4j Aura

1. Acesse [console.neo4j.io](https://console.neo4j.io) e clique em **Sign up**.
2. Cadastre-se com email/Google/GitHub.
3. Em **Create instance**, escolha o plano **AuraDB Free**.
4. Dê um nome à instância e clique em **Create**.
5. Guarde o usuário (`neo4j`) e a senha exibidos na tela — eles não aparecem novamente.
6. Aguarde a instância ficar com status **Running**.

## 2. Restaurar o dump na instância

O arquivo de dump está em [`dump/neo4j.dump`](./dump/neo4j.dump) neste repositório (ajuste o caminho se o nome/local for diferente).

1. Clone este repositório e localize o arquivo `.dump`.
2. No [console.neo4j.io](https://console.neo4j.io), abra o card da instância criada.
3. Clique na aba **Restore from backup file**.
4. Arraste o arquivo `.dump` (ou clique para selecioná-lo).
5. Confirme a restauração. **Atenção:** isso sobrescreve os dados atuais da instância.
6. Aguarde a conclusão — o status volta para **Running** quando terminar.

## 3. Conectar

Use a URI `neo4j+s://<instance-id>.databases.neo4j.io`, usuário `neo4j` e a senha da instância para conectar via [Neo4j Browser](https://console.neo4j.io) ou qualquer driver oficial.

---

**Referência:** [Backup, export, restore e upload — Neo4j Aura](https://neo4j.com/docs/aura/managing-instances/backup-restore-export/)
