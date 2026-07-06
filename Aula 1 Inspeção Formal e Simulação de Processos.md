Objetivo da Aula

Aprender a utilizar as ferramentas de auditoria nativas do **XG5000** para erradicar erros de sintaxe e lógica, e dominar o **XG-SIM** para validar intertravamentos de segurança e cenários de falha em um ambiente virtual seguro.

---

1. Inspeção Formal com o "Check Program"

Antes de qualquer teste funcional, um programador profissional realiza uma auditoria estática do código. No XG5000, isso é feito através da ferramenta **Check Program**, acessível em `[View] -> [Check Program]`.

**O que o software inspeciona?**

- **Erros de Lógica (Logic Error):** Identifica conexões Ladder incompletas ou o perigoso **Erro L0100 (Curto-circuito)**, onde a lógica tenta interligar trilhos de energia sem contatos intermediários.
- **Erros de Gramática (Grammar Error):** Verifica se instruções de aplicação complexas (como CALL/SBRT ou FOR/NEXT) estão com a sintaxe correta e se a instrução **END** obrigatória está presente ao final do scan.
- **Bobinas Duplicadas (Duplicated Coil):** Esta é a verificação mais vital. O software varre todo o projeto para garantir que o mesmo endereço de saída (ex: `P00021`) não esteja sendo escrito em dois lugares diferentes, o que causaria conflitos lógicos destrutivos.

**Exemplo Didático:** Imagine que você criou uma lógica para ligar um motor no "Programa_Manual" e esqueceu que já havia uma bobina para o mesmo motor no "Programa_Automatico". Ao executar o **Check Program**, o XG5000 abrirá uma janela de mensagens listando o nome do programa, a linha e a coluna de ambas as ocorrências, permitindo que você corrija o conflito antes mesmo de ligar a máquina.

---

2. Validação Virtual com XG-SIM

O **XG-SIM** é um PLC virtual baseado em Windows que permite executar seu programa sem a necessidade de hardware físico. Ele é o seu "gêmeo digital" para testes de estresse.

**Como iniciar a simulação:**

1. Com o projeto aberto, vá em `[Tools] -> [Start Simulator]`.
2. O software entrará em modo **Online** e exibirá o status de **Run** na barra de ferramentas.
3. Utilize a janela de **System Monitoring** para visualizar o estado dos módulos virtuais configurados.

**Simulação de I/O e Variáveis:**

Para testar a lógica, você não precisa de sensores reais. Você pode:

- Dar um duplo clique em um contato no Ladder e forçar seu estado para **ON** ou **OFF**.
- Utilizar a função **Device Monitoring** para alterar valores de registradores (como simulando uma temperatura subindo ou a frequência de um inversor).

---

3. Validando Intertravamentos e Cenários de Falha

A simulação profissional não serve apenas para ver se a máquina "liga", mas para garantir que ela "pare" quando algo der errado.

**Cenário de Teste: Intertravamento de Segurança**

Utilizando o exemplo de uma **Máquina de Furação Automática**:

- **Lógica:** A furadeira (Saída `C+`) só deve descer se a peça estiver posicionada (Sensor `P2`) e a mesa indexadora travada (Sensor `F1`).
- **Teste de Falha:** No **XG-SIM**, force o sensor de peça `P2` para **OFF** enquanto a máquina tenta iniciar o ciclo. Se a saída da furadeira ativar, sua lógica falhou no requisito de segurança.
- **Teste de Emergência:** Simule o acionamento do botão de emergência físico (forçando o bit correspondente no CLP) e verifique se todas as bobinas de movimento entram em estado **Reset** instantaneamente [26, Conversation History].

---

4. Vantagens da Simulação em Ambientes Industriais

- **Proteção de Ativos:** Testar falhas no simulador evita colisões mecânicas reais que poderiam custar milhares de reais em reparos.
- **Redução do Comissionamento:** Validar a lógica em bancada permite que, ao chegar no cliente para o **SAT (Site Acceptance Test)**, você foque apenas em ajustes finos de sensores e cabos, pois o software já é resiliente.
- **Treinamento Seguro:** Permite treinar operadores em cenários de alarme e erro sem colocar ninguém em risco físico.

---

Exercício Prático de Validação

1. **Provoque um Erro:** Insira uma bobina duplicada em seu programa e execute o `Check Program`. Use a função **Go To** na janela de resultados para localizar o erro.
2. **Inicie o XG-SIM:** Coloque o CLP virtual em modo **Run**.
3. **Simule o Sincronismo:** Altere manualmente o valor do registrador de frequência do Inversor 1 (Mestre) e observe se o cálculo PID ou a máquina de estados reage conforme o esperado no Inversor 2 (Escravo).
4. **Crie uma Condição de Falha:** Desligue o bit de "Comunicação OK" e verifique se o sistema transita para o estado de **ALERTA** definido na sua máquina de estados