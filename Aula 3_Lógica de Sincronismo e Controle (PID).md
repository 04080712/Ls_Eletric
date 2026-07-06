Objetivo da Aula

Implementar o controle de sincronismo utilizando a instrução **PID**, configurando os parâmetros de ganho de forma analítica e utilizando o **PID Monitor** para validar a estabilidade da malha em tempo real.

---

1. A Malha de Controle PID no XG5000

Diferente da compensação proporcional simples, o PID (Proporcional, Integral e Derivativo) ajusta o comando do motor baseando-se no erro acumulado e na velocidade da variação, eliminando o "erro de regime permanente" [Conversation History].

- **PV (Process Value):** No nosso caso, é a frequência real lida do Inversor 1 via Modbus (armazenada em `D101`).
- **SV (Set Value):** É o objetivo de sincronismo (ex: a frequência alvo que o escravo deve atingir para igualar o mestre).
- **MV (Manipulated Value):** É a saída calculada pelo CLP que será enviada para o comando de frequência do Inversor 2 (`D10`).

---

2. Configuração e Monitoramento Visual (PID Monitor)

O XG5000 oferece um ambiente dedicado para evitar que você precise ajustar ganhos "no escuro" dentro do código Ladder.

**A. Acessando o PID Monitor**

Vá ao menu `[Monitor] -> [PID Monitor]`. O software suporta até 256 loops divididos em 8 blocos.

1. **Block/Loop Information:** Selecione o loop desejado (ex: LOOP00) para abrir a janela de monitoramento.
2. **Trend Graph:** Visualize em tempo real as curvas de **PV**, **SV** e **MV**. Isso é vital para identificar oscilações ou respostas lentas do motor.

**B. Ajuste de Parâmetros (Tuning)**

Na janela **Setting View**, você define os ganhos:

- **Kp (Ganho Proporcional):** Força da correção inicial.
- **Ti (Tempo Integral):** Elimina o erro residual ao longo do tempo.
- **Td (Tempo Derivativo):** Amortece a resposta para evitar que o motor "passe do ponto" (overshoot).
- **Nota Profissional:** No modo de monitoramento, você só pode alterar esses valores através da função **Change Current Value**.

---

3. Integração Analítica: Rampas e Sincronismo

Para um sincronismo profissional, a consistência na gestão de tempos e rampas é crucial.

- **Padronização de Rampas:** Defina as acelerações e desacelerações como _setpoints_ editáveis (ex: `D2` e `D3`), garantindo que o comportamento do sistema seja previsível e seguro.
- **Controle de "Saúde" (Health Check):** Cada bloco de controle PID deve ter indicadores de status. Utilize as flags de comunicação P2P (como `_P2P1_ERR02`) para travar o cálculo PID caso a leitura da frequência real do mestre falhe, evitando correções baseadas em dados congelados [Conversation History].

---

4. Diagnóstico Avançado com Data Trace

Se a malha PID apresentar instabilidades rápidas que o monitor visual não captura, utilize o **Data Trace**.

- O Data Trace coleta dados diretamente da memória do CLP em cada ciclo de _scan_, permitindo uma análise muito mais precisa do que o monitoramento de tendência comum.
- Você pode definir gatilhos (**Triggers**) para iniciar a captura de dados exatamente quando o erro de sincronismo ultrapassar um limite aceitável.

---

Exercício de Implementação Profissional

1. **Configure o Bloco PID:** No XG5000, associe o Loop 00 à leitura de `D101` (PV) e à saída `D10` (MV).
2. **Abra o PID Monitor:** Com o CLP em RUN, inicie o gráfico de tendência e aplique um degrau de velocidade no mestre.
3. **Sintonize a Malha:** Ajuste o ganho Proporcional até que o escravo responda rapidamente, e então adicione o Integral para zerar a diferença de velocidade entre os dois motores.
4. **Valide com Data Trace:** Realize uma captura de 1000 amostras durante a aceleração e verifique se o erro de sincronismo permanece dentro da tolerância de projeto.