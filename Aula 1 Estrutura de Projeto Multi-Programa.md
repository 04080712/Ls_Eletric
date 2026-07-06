Objetivo da Aula

Aprender a estruturar um projeto escalável, garantindo que o software seja robusto contra falhas, fácil de manter e otimizado para a eficiência operacional (OEE).

---

1. Estrutura de Projeto Multi-Programa

Diferente da programação amadora, onde todo o código reside em um único bloco, o padrão profissional exige a **modularização**. O XG5000 permite gerenciar múltiplos CLPs e múltiplos programas dentro de um único projeto.

**Scan Programs vs. Task Programs**

A arquitetura profissional separa as funções por prioridade de execução:

- **Scan Program (Varredura Contínua):** Executa a lógica principal da máquina de forma cíclica. Ideal para intertravamentos e lógicas discretas (LD) que requerem monitoramento constante.
- **Task Program (Programas de Tarefa):** Executados apenas quando condições específicas são atendidas.
    - **Fixed Cycle Task:** Para malhas de controle críticas, como PID ou sincronismo, onde o determinismo temporal (ex: a cada 10ms) é essencial para a estabilidade.
    - **Initialization Task:** Executa apenas uma vez na transição para o modo RUN, ideal para carregar valores padrão e presets.
    - **I/O Task:** Ativado por interrupções físicas de entradas digitais para processos de altíssima velocidade.

**Boa Prática Industrial:** Divida sua máquina em áreas lógicas (ex: `Prog_Emergencia`, `Prog_Manual`, `Prog_Automatico`) e organize a sequência de execução arrastando os programas na árvore de projeto; o XG5000 executa de cima para baixo.

---

2. Gestão de Parâmetros e Segurança de Hardware

A configuração do hardware define o comportamento do sistema em situações de crise. Não basta apenas escrever lógica; é preciso parametrizar a CPU.

**Parâmetros Básicos (Basic Parameters)**

Acesse para definir a resiliência do processo:

- **Error Operation Setup:** Configure se o CLP deve parar imediatamente ou continuar operando em caso de erros não fatais, como falhas de comunicação ou erros aritméticos. Em processos contínuos, a continuidade pode evitar perdas de matéria-prima, mas em máquinas de movimento, a parada imediata é a norma de segurança.
- **Watchdog Timer:** Defina o limite máximo de tempo para a execução do ciclo. Se o programa travar em um loop infinito, o Watchdog forçará a CPU ao modo STOP e desligará as saídas para proteção.

**Parâmetros de E/S (I/O Parameters)**

- **Abstração de Hardware:** Utilize a função **Register Module Variable Comments** para importar automaticamente as descrições dos módulos. Isso evita erros de endereçamento e facilita o mapeamento analítico discutido anteriormente.

---

3. Otimização do Ambiente para Zero Bugs

Um programador profissional utiliza ferramentas de inspeção em tempo real para evitar erros humanos antes mesmo de compilar o código.

**Configurações de Inspeção Instantânea**

No menu `[Tools] -> [Options] -> [Common Editor]`, ative:

1. **Check duplicate coil instantly:** O software avisará imediatamente se você tentar usar a mesma bobina de saída em dois lugares diferentes, evitando o comportamento imprevisível da lógica.
2. **Output cross reference instantly:** Exibe automaticamente onde cada variável está sendo utilizada, essencial para auditorias de código e depuração rápida.

**Dica de Produtividade:** Ative o **Auto-save** (1 a 60 minutos) para garantir que alterações complexas de arquitetura não sejam perdidas por quedas de energia ou falhas de conexão.

---

Checklist de Implementação Profissional

Ao iniciar qualquer projeto industrial, siga estes passos:

1. [ ] **Crie o projeto** selecionando o modelo de CPU exato (ex: XBC-DN32UP).
2. [ ] **Segmente a lógica** em pelo menos 3 programas: Inicialização, Emergência e Lógica Principal.
3. [ ] **Configure o Watchdog** e as ações em caso de erro nos Basic Parameters.
4. [ ] **Habilite as ferramentas de inspeção** automática no editor.
5. [ ] **Documente a intenção** de cada programa nas propriedades do item (Project Description).