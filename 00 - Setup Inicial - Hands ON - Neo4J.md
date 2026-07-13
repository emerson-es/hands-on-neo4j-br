# Setup do Neo4j Aura

Passos para criar uma instância gratuita no Neo4j Aura e restaurar o backup deste repositório.

## 1. Criar conta free no Neo4j Aura

1. Acesse [console.neo4j.io](https://console.neo4j.io) e clique em **Sign up**.
2. Cadastre-se com email/Google/GitHub.
3. Em **Create instance**, escolha o plano **AuraDB Free**.
4. Dê um nome à instância e clique em **Create**.
5. Guarde o usuário (`neo4j`) e a senha exibidos na tela — eles não aparecem novamente.
6. Aguarde a instância ficar com status **Running**.

## 2. Restaurar o dump na instância

O arquivo de dump (neo4j-2026-07-13T15-55-41-10e73bf9.backup) está em https://github.com/emerson-es/hands-on-neo4j-br/blob/main/neo4j-2026-07-13T15-55-41-10e73bf9.backup neste repositório .

1. Faça o download para sua maquina .
2. No [console.neo4j.io](https://console.neo4j.io), abra o card da instância criada.
3. Clique na aba **Restore from backup file**.
4. Arraste o arquivo `.backup` (ou clique para selecioná-lo).
5. Confirme a restauração. **Atenção:** isso sobrescreve os dados atuais da instância.
6. Aguarde a conclusão — o status volta para **Running** quando terminar.

---
**Referência:** [Backup, export, restore e upload — Neo4j Aura](https://neo4j.com/docs/aura/managing-instances/backup-restore-export/)
