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

O arquivo de dump que vamos utlizar no hands on [Backup_Neo4J](https://github.com/emerson-es/hands-on-neo4j-br/blob/main/neo4j-2026-07-13T20-40-01-10e73bf9.backup) - Faça o Download.

1. Faça o download do Dump acima para sua maquina .
2. No [console.neo4j.io](https://console.neo4j.io), abra o card da instância criada.
3. Clique na aba **Restore from backup file**.
   <img width="1861" height="423" alt="restore" src="https://github.com/user-attachments/assets/d6d60ef3-cf7e-4436-a263-37170be642ab" />
4. Arraste o arquivo `.backup` (ou clique para selecioná-lo).
   <img width="684" height="429" alt="restore 2" src="https://github.com/user-attachments/assets/d1267891-406a-4ae4-8ce0-b393c9240031" />

7. Confirme a restauração. **Atenção:** isso sobrescreve os dados atuais da instância.
8. Aguarde a conclusão — o status volta para **Running** quando terminar.
   <img width="1699" height="289" alt="restoe 3" src="https://github.com/user-attachments/assets/26ff3d7e-c372-4514-8a99-2619ef6aea81" />


---
**Referência:** [Backup, export, restore e upload — Neo4j Aura](https://neo4j.com/docs/aura/managing-instances/backup-restore-export/)
