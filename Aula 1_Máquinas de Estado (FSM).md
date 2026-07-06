Objetivo da Aula

Aprender a modelar e codificar processos complexos de forma que o CLP execute apenas uma etapa por vez, facilitando o rastreio de falhas e garantindo um comportamento determinístico da máquina.

---

1. O Conceito Analítico de FSM

Uma máquina de estado é um modelo comportamental que descreve o ciclo de vida de um processo através de **estados** distintos e **transições** condicionadas.

- **Estado:** Uma condição específica onde a máquina realiza uma ação ou espera por um evento (ex: "Avançando Cilindro", "Aguardando Peça").
- **Evento/Transição:** O gatilho (sensor, botão ou timer) que permite a mudança de um estado para o próximo.
- **Guarda:** Uma expressão booleana que deve ser verdadeira para que a transição ocorra.

**Boa Prática Moore vs. Mealy:** Em automação, a **Máquina de Moore** (onde as saídas dependem apenas do estado atual) é preferida pela sua estabilidade e facilidade de depuração em relação à Máquina de Mealy.

---

2. Metodologia de Desenvolvimento (Passo a Passo)

Para um desenvolvimento analítico profissional, siga esta sequência antes de abrir o editor Ladder:

1. **Análise do Processo:** Identifique todos os movimentos e fases de espera.
2. **Diagrama de Estados:** Desenhe um fluxograma circular com retângulos arredondados para os estados e setas para as transições.
3. **Numeração de Estados:** Atribua IDs numéricos (ex: 0, 10, 20) ou bits de memória (M1, M2...).
4. **Mapeamento de Ações:** Defina o que cada estado faz fisicamente (quais saídas ativa).

---

3. Implementação Prática no XG5000 (Método de Registradores)

A arquitetura profissional no XG5000 divide o programa em três regiões lógicas distintas:

**Região 1: Inicialização (First Scan)**

Define qual será o estado de "repouso" assim que o CLP entra em RUN.

- Use a flag especial **_ON** (ou F99) ou o contato de **First Scan** (F90) para dar o `MOV` inicial ou `SET` no estado 0.

**Região 2: Lógica de Transição**

Aqui você define como a máquina evolui.

```
// Exemplo: Se estiver no ESTADO 10 e o sensor P1 ativar, vá para ESTADO 20
|   [= D500 10]   P0001 (SENSOR)                                            |
|-------| |-----------| |-------------------------[MOV 20 D500]-----------| [Conversation History]
```

**Vantagem Analítica:** O uso de um registrador único (ex: `D500`) garante que **apenas um estado esteja ativo**, eliminando conflitos de lógica onde dois comandos opostos tentam atuar ao mesmo tempo.

**Região 3: Região de Saída (Output Mapping)**

Nesta região, você associa os estados às bobinas físicas.

```
// O motor só liga se a máquina estiver no Estado 10 OU Estado 30
|   [= D500 10]                                          P0020 (MOTOR)     |
|-------| |-------+------------------------------------------( )---------|
|                |                                                        |
|   [= D500 30]  |                                                        |
|-------| |-------+                                                        | [17, 18]
```

---

4. Boas Práticas Industrial e Segurança

- **Nomenclatura Descritiva:** Não nomeie estados como "Estado 1". Use nomes como `EST_CARGA_PECA` ou `EST_FURACAO_ATIVA`.
- **Tratamento de Erros:** Crie um estado de **ALERTA** ou **FALHA**. Se um sensor não atuar em um determinado tempo (Timer), mova o registrador de estado para o ID de erro e desligue as saídas.
- **Intertravamento Global:** Utilize o bit de emergência discutido no módulo anterior como condição de guarda para todas as transições de movimento [23, Conversation History].
- **Instrução END:** Nunca esqueça que todo programa sequencial deve terminar com a instrução `END` para fechar o ciclo de varredura.

---

Exercício de Aula

1. Modele um semáforo de duas vias usando o **Diagrama de Estados**.
2. Implemente a lógica no XG5000 utilizando o método de registradores (MOV) no registrador `D1000`.
3. Utilize o **Variable Monitoring** para forçar as transições e observar o valor de `D1000` mudando conforme a sequência