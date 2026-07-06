Objetivo da Aula

Dominar a configuração dos **Parâmetros Básicos** e de **E/S (I/O)** no XG5000. O foco analítico aqui é definir o "estado seguro" da máquina, garantindo que o hardware responda corretamente a falhas e que a memória seja gerida de forma profissional.

---

1. Parâmetros Básicos (Basic Parameters): O Coração da CPU

Os parâmetros básicos ditam como a CPU processa a lógica e como ela se recupera de interrupções.

**A. Configuração de Operação em Erro (Error Operation)**

- **Ação Profissional:** Determine se a CPU deve entrar em **STOP** ou continuar em **RUN** ao detectar erros não fatais (como erros de cálculo ou falhas de comunicação P2P).
- **Critério Analítico:** Em sistemas de transporte de carga, o ideal é o **STOP** imediato. Em sistemas de controle de temperatura crítica, a continuidade (**RUN**) pode ser necessária para evitar o resfriamento brusco, desde que lógicas de segurança redundantes estejam ativas.

**B. Gestão de Área de Latch (Retentividade)**

- Define quais memórias (**M, D, T, C**) manterão seus valores após uma queda de energia ou transição de STOP para RUN.
- **Boa Prática:** Utilize a área de Latch para armazenar _setpoints_ de processo e receitas, mas evite usá-la para bits de estado de ciclo (máquinas de estado), garantindo que a máquina sempre inicie em um estado conhecido (Home/Repouso) após o reset.

---

2. Parâmetros de E/S e a Saída de Emergência

A configuração profissional de E/S vai além de simplesmente listar os módulos; ela define a interface física com o mundo real.

**A. Emergency Output Setting (Configuração de Saída em Emergência)**

Esta é a configuração de segurança mais crítica no XG5000:

- **Clear (Limpar):** As saídas físicas são desligadas (0) se a CPU parar ou entrar em erro. **Este é o padrão para motores e atuadores perigosos**.
- **Hold (Manter):** As saídas mantêm o último estado. Útil apenas para indicadores visuais ou válvulas de retenção específicas que não podem mudar de estado abruptamente.

**B. Abstração de Hardware: Register Module Variable Comments**

No menu `[Edit]`, utilize esta função para importar automaticamente variáveis de sistema e comentários para os módulos configurados. Isso garante que, se você trocar um módulo de posição física no futuro, a atualização de endereçamento seja feita de forma analítica e centralizada no dicionário de variáveis.

---

3. Configurações de Detalhe (Special Modules)

Para módulos analógicos (A/D) ou contadores de alta velocidade (HSC), o acesso ao botão **[Details]** nos parâmetros de E/S é obrigatório:

- **Filtros de Entrada:** Configure constantes de filtro para entradas analógicas para eliminar ruídos elétricos antes que os dados cheguem à lógica PID.
- **Escalonamento (Scaling):** Defina os limites (ex: 4-20mA correspondendo a 0-16000 unidades) diretamente nos parâmetros, liberando o programa Ladder de cálculos matemáticos desnecessários e pesados.

---

4. Boas Práticas de Padronização Industrial

- **Identificação Visual:** No XG5000, parâmetros alterados pelo usuário em relação ao padrão de fábrica aparecem na cor **Azul**, enquanto os valores padrão permanecem em **Preto**. Utilize isso como uma trilha de auditoria rápida.
- **Configuração em Bloco:** Utilize a opção _"All Parameters Settings"_ para aplicar a mesma configuração (ex: filtro de 10ms) em todos os canais de um módulo simultaneamente, reduzindo o risco de erro humano.

---

Exercício Prático de Arquitetura

1. Abra o **I/O Parameter** do seu projeto.
2. Configure o **Emergency Output** de todas as suas saídas de controle de motor para **Clear**.
3. Defina uma **Área de Latch** para os registradores de tempo de aceleração/desaceleração (D2 e D3) para que a máquina não perca essas rampas após um desligamento.
4. Execute a função **Register Module Variable Comments** e observe as novas tags surgindo no seu dicionário