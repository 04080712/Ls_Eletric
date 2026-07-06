Objetivo da Aula

Aprender a encapsular lógicas complexas (como o controle de um motor ou uma malha de cálculo) em blocos personalizados, permitindo que você escreva o código uma única vez e o reutilize infinitas vezes no projeto.

---

1. UDF vs. UFB: Qual utilizar?

No **XG5000**, a modularização profissional divide-se em duas categorias fundamentais de Unidades de Organização de Programa (POU):

- **User Defined Function (UDF):** É uma unidade que produz um resultado imediato com base em suas entradas.
    - **Característica:** Não possui memória interna entre execuções (stateless).
    - **Aplicação:** Ideal para cálculos matemáticos, conversões de escala ou lógicas combinacionais puras. É criada obrigatoriamente na linguagem **LD**.
- **User Defined Function Block (UFB):** É uma unidade mais robusta que mantém o valor de suas variáveis internas de uma execução para a próxima.
    - **Característica:** Requer a criação de uma **Instância** (uma cópia nomeada na memória) para cada vez que for usado.
    - **Aplicação:** Ideal para controle de equipamentos (Motores, Válvulas) ou sequências onde o estado atual importa. Pode ser criado em **LD** ou **SFC**.

---

2. Metodologia de Criação Analítica

Para desenvolver um bloco profissional, siga estes passos no XG5000 (requisito: projeto configurado com **Auto-allocation** ativo):

**Passo A: Definição da Interface (I/O Variables)**

1. No navegador de projeto, clique com o botão direito em **User Function/Function Block** e selecione **Add Item**.
2. Defina o nome, linguagem e se utilizará os pinos de habilitação **EN/ENO**.
3. Abra a janela **Local Variables** do bloco para mapear a "cara" dele:
    - **VAR_INPUT:** Sinais externos (ex: `BT_LIGA`, `SENSOR_FIM`).
    - **VAR_OUTPUT:** Comandos de saída (ex: `CMD_MOTOR`, `ALERTA_ERRO`).
    - **VAR_IN_OUT:** Variáveis que o bloco lê e altera simultaneamente.
    - **VAR:** Memórias internas invisíveis ao resto do programa.
    - _Nota:_ O limite é de 64 variáveis por bloco.

**Passo B: Codificação Interna**

Desenvolva a lógica dentro do editor do bloco utilizando apenas as variáveis declaradas no Passo A.

- **Regra de Ouro:** Não é permitido usar um UDF ou UFB dentro de outro programa de UDF/UFB.
- **Determinismo:** Utilize as variáveis de sistema e gatilhos (como `R_EDGE` para borda de subida) diretamente nos parâmetros de entrada para garantir que o bloco responda precisamente a eventos.

---

3. Aplicação e Instanciação no Scan Program

Uma vez criado, seu bloco aparecerá na barra de ferramentas de **Extended Functions**.

Ao arrastar um **UFB** para o seu programa principal:

1. O software solicitará um **Instance Name**.
2. **Por que isso é vital?** Cada instância reserva uma área exclusiva na memória do CLP para as variáveis internas daquele bloco específico. Isso permite ter 10 motores controlados pelo mesmo código de bloco, mas cada um com seu próprio tempo de execução e status de falha independente.

---

4. Proteção de Propriedade Intelectual

Como desenvolvedor profissional, você pode proteger seu conhecimento técnico:

- **Password Individual:** Defina uma senha para cada UDF/UFB. Sem ela, outros usuários podem usar o bloco no programa, mas não podem abrir para ver a lógica interna.
- **Disable Read from PLC:** Ative esta opção para impedir que a lógica fonte do bloco seja lida de volta do CLP, mesmo que alguém tenha acesso ao upload do projeto.

---

Exercício de Prática Industrial

1. Crie um **User Function Block** chamado `FB_Motor_Sinc`.
2. Adicione as entradas `Freq_Mestre` e `Enable`, e a saída `Cmd_Freq_Escravo`.
3. Implemente internamente a lógica de sincronismo por compensação que desenvolvemos anteriormente.
4. No seu programa principal, chame este bloco duas vezes (Instâncias `Motor1` e `Motor2`) e observe como cada uma gerencia seus dados de forma isolada.