Objetivo da Aula

Dominar o ambiente de depuração do **XG5000**, utilizando pontos de parada (**Breakpoints**) e execução controlada por ciclos (**Scan Break**) para diagnosticar falhas lógicas que ocorrem rápido demais para o monitoramento convencional.

---

1. Requisitos para o Modo Debug

Diferente do monitoramento comum, o modo Debug exige controle total sobre o ciclo de varredura da CPU.

- **Conexão:** O CLP deve estar online e o programa no PC deve ser idêntico ao do CLP.
- **Modo da CPU:** A depuração **não está disponível** se o CLP estiver em modo RUN. Você deve alternar para o modo **Debug** via menu `[Online] -> [Change Mode] -> [Debug]` ou `[Debug] -> [Start/Stop Debugging]`.
- **Aviso de Segurança:** Durante o Debug, o ciclo de varredura é interrompido. Isso significa que as saídas físicas podem ser mantidas no último estado ou desligadas, dependendo da sua configuração de parâmetros básicos discutida no Módulo 1.

---

2. Gestão de Breakpoints (Pontos de Parada)

Um **Breakpoint** é uma marcação que instrui a CPU a interromper a execução exatamente naquela linha de código.

- **Como inserir:** Posicione o cursor na etapa desejada e use `[Debug] -> [Set/Remove Breakpoints]`. Um círculo vermelho aparecerá ao lado da instrução.
- **Limite:** O XG5000 permite registrar até **62 Breakpoints** simultâneos no CLP.
- **Lista de Breakpoints:** No menu `[Debug] -> [Breakpoints List]`, você pode habilitar, desabilitar ou saltar diretamente para a localização de qualquer ponto de parada no projeto.

---

3. Execução Passo a Passo (Stepping)

Assim que a CPU atinge um Breakpoint, ela pausa. Para avançar, você utiliza os comandos de "Passo":

1. **Step Into (F11):** Executa a próxima instrução. Se a linha for um **CALL**, o depurador entrará dentro da sub-rotina para você analisar a lógica interna.
2. **Step Over (F10):** Executa a próxima linha, mas **não entra** em sub-rotinas. Ele executa o bloco CALL inteiro e para na linha seguinte.
3. **Step Out (Shift+F11):** Se você estiver dentro de uma sub-rotina, este comando termina a execução dela e volta para o programa principal.
4. **Go (F5):** Libera a execução até que o **próximo** Breakpoint seja atingido.

**Exemplo Didático:** Se o seu cálculo de sincronismo PID está gerando um valor estranho, coloque um Breakpoint na instrução PID. Use o **Step Into** para ver como os ganhos Kp e Ti estão afetando o registrador de saída (MV) em tempo real.

---

4. Breakpoints Condicionais: Device e Scan

Para erros intermitentes, parar em uma linha fixa não é suficiente. O XG5000 oferece condições avançadas:

**A. Device Break (Parada por Valor)**

- **O que faz:** Para a execução quando um dispositivo atinge um valor específico ou é escrito/lido.
- **Exemplo:** Você pode configurar para a CPU parar apenas quando a variável `INV01_ERRO` for igual a `1`.

**B. Scan Break (Parada por Ciclos)**

- **O que faz:** Executa o programa por um número exato de ciclos de varredura e depois para.
- **Exemplo:** Útil para analisar o que acontece nos primeiros 10 ciclos após a partida da máquina. Configure o **Scan Count** para 10 e execute o **Go**.

---

Exercício Prático de Depuração

1. **Habilite o Modo Debug:** Coloque seu simulador ou CLP real em modo Debug.
2. **Defina um Breakpoint:** Escolha a linha que realiza o cálculo do erro de sincronismo (`SUB D101 D111 D500`).
3. **Analise a Transição:**
    - Mude manualmente o valor de `D101` via **Device Monitoring**.
    - Use o comando **Go** para ver a CPU parar na sua linha marcada.
    - Observe o valor de `D500` ser atualizado apenas após você dar o comando de passo.
4. **Teste o Scan Break:** Configure um Scan Break de 5 ciclos e observe a mensagem do sistema confirmando a parada após a contagem.