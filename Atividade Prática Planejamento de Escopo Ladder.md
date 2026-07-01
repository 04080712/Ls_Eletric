Comportamento da máquina: 
***Quero que imagine uma esteira que possa rodar para frente e para trás, tenha botão de emergência, IHM, Sinaleiro e inversor de Frequência.*** 


"Para inicio dos trabalhos é importante que seja feito como uma TABELA DE ESTADO"
## Tabela de Estados: 

| ID  | Estado               | Objetivo                                        |
| --- | -------------------- | ----------------------------------------------- |
| 0   | Desligada            | Painel sem alimentação ou CLP não inicializado. |
| 10  | Parada               | Máquina pronta para receber comandos.           |
| 20  | Partida Horário      | Preparando a partida no sentido horário.        |
| 30  | Rodando Horário      | Operação normal.                                |
| 40  | Partida Anti-horário | Preparando a partida no sentido anti-horário.   |
| 50  | Rodando Anti-horário | Operação normal no sentido inverso.             |
| 60  | Parada Controlada    | Desaceleração em andamento.                     |
| 90  | Alarme               | Falha detectada.                                |
| 99  | Emergência           | Botão de emergência acionado.                   |
## Diagrama de Estados




"O próximo passo é fazer a pergunta: O que deve acontecer neste estado."
### 10 - Parada:
Quando a máquina está Parada o que Deve acontecer neste estado?  saída 
- Inversor de Frequencia Parado
- Sinaleiro Verde e Amarelo acesos juntos
- Sinaleiro Vermelho apagado
- IHm mostrando #Maquina_Parada
- Aceita comando de partida
Note que em nenhuma dessas ações muda o estado da máquina. 

**Para cada mudança de estado é feito uma tabela de Transições**

| Estado Atual | Evento             | Condições | Próximo Estado       |
| ------------ | ------------------ | --------- | -------------------- |
| Parada       | Start Horário      |           | Partida horário      |
| Parada       | Start Anti-horário |           | Partida Anti-horário |
| Parada       | Emergência         |           | Emergência           |
| Parada       | Falha              |           | Alarme               |

### 20 - Partida Horário: 
No estado Partida horário o que deve acontecer?

- Deve ser dado Run no inversor de frequência(Rampa de Acc de 10s).
- Os Sinaleiros verde e amarelo piscando juntos.
- Mostrar alguma Mensagem na IHM(Esteira entrando em execução).
- Atualizar a IHM.
- Verificar status de inversor e emergência.

**Para cada mudança de estado é feito uma tabela de Transições** 

| Estado Atual    | Evento                          | Condições | Próximo Estado    |
| --------------- | ------------------------------- | --------- | ----------------- |
| Partida Horário | Frequência atingiu a referência |           | Rodando horário   |
| Partida Horário | Stop                            |           | Parada Controlada |
| Partida Horário | Emergência                      |           | Emergência        |
| Partida Horário | Falha                           |           | Alarme            |











