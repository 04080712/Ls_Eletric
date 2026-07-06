Objetivo da Aula

Capacitar você a configurar a troca de dados via **P2P (Peer-to-Peer)** e, mais importante, realizar o tratamento analítico de falhas de rede. O foco é garantir que a lógica de controle seja resiliente, operando apenas com dados válidos e "frescos".

---

1. A Arquitetura P2P no XG5000

A comunicação P2P permite que o CLP atue como um cliente Modbus TCP, lendo (**READ**) e escrevendo (**WRITE**) dados em escravos na rede.

- **Blocos de Escrita (WRITE):** Utilizados para enviar comandos de giro e setpoints de frequência para o inversor.
- **Blocos de Leitura (READ):** Utilizados para obter o status real do motor (frequência atual, corrente, alarmes).

**Boa Prática Industrial:** Desacople a lógica de controle dos detalhes de hardware criando blocos de abstração para leitura e escrita, garantindo um **scan determinístico** onde os dados são atualizados em intervalos previsíveis.

---

2. Gestão Analítica de Status: As Flags NDR e ERR

A robustez de um sistema de comunicação não está no envio do dado, mas na verificação se ele chegou. O XG5000 gera automaticamente flags de diagnóstico para cada bloco P2P configurado.

- **Flag NDR (Normal Data Reception/Response):** Indica que a operação (leitura ou escrita) do bloco específico foi concluída com **sucesso**.
- **Flag ERR (Error):** Indica que houve uma **falha** na comunicação daquele bloco (ex: cabo desconectado, timeout ou IP inexistente).

**A Regra de Ouro da Integração:** Nunca processe um dado lido da rede sem antes validar o status de "saúde" (_health_) do bloco através da flag NDR.

---

3. Implementação de "Guarda de Dados" (Data Guarding)

Um erro comum é o CLP continuar executando o cálculo de sincronismo com um valor de velocidade "congelado" se a rede cair. Para evitar isso, utilizamos as flags como guardas lógicas.

**Exemplo de Lógica Profissional:**

```
// Apenas atualiza a variável interna se a leitura (Index 02) foi bem-sucedida
|   _P2P1_NDR02                                                              |
|-------| |------------------------------------[MOV D00101 INV01_FREQ_REAL]--| [191, Conversation History]

// Se houver erro na comunicação, ativa um alarme e para o sincronismo
|   _P2P1_ERR02                                                              |
|-------| |-------+------------------------------------[SET ALM_REDE_INV01]---|
                  |                                                          |
                  +------------------------------------[RST SIST_OK]---------| [23, 24, Conversation History]
```

---

4. Diagnóstico e Monitoramento Ativo

Para garantir a operação contínua, um programador profissional utiliza ferramentas de monitoramento ativo:

- **P2P Monitor:** Acesse `[Monitor] -> [System Monitoring]` para visualizar em tempo real quais blocos estão transmitindo e quais apresentam erro.
- **Watchdogs e Heartbeats:** Implemente sinais de "batida de coração" entre o CLP e os dispositivos externos para assegurar que todos os componentes estão comunicando-se ativamente.
- **Tratamento de Exceções:** Configure o sistema para um **estado seguro** (_safe state_) em caso de perda total de comunicação, normalmente desligando as saídas físicas para proteção de pessoal e equipamentos.

---

Exercício de Aula

1. No seu projeto, localize as flags `_P2P1_NDRxx` e `_P2P1_ERRxx` na janela **Variable/Comment** (aba View Flag).
2. Crie uma lógica de intertravamento onde o comando de giro para o Inversor 2 só é permitido se a flag **NDR** do bloco de leitura do Inversor 1 estiver pulsando regularmente.
3. Simule uma falha de comunicação (forçando o bit ERR via monitoração) e verifique se o sistema transita corretamente para o estado de erro.