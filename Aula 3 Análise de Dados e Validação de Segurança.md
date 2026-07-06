Objetivo da Aula

Dominar a função **Data Trace** para capturar variações de memória em milissegundos e aplicar os protocolos de testes industriais para garantir que a máquina opere conforme os requisitos funcionais e de segurança.

---

1. Análise de Dados em Alta Velocidade: Data Trace

Diferente do monitoramento de tendência comum, o **Data Trace** permite coletar dados diretamente da CPU com precisão muito superior, sendo essencial para diagnosticar problemas intermitentes ou malhas de controle rápidas.

**A. Configuração e Gatilhos (Triggers)**

Para realizar uma análise analítica, você deve definir o que disparará a captura de dados:

- **Bit Condition:** A captura inicia quando um bit muda de estado (ex: `Rising Edge` quando um erro ocorre).
- **Word Condition:** Inicia quando um valor atinge um limite (ex: `>` que 5000 RPM).
- **Amostragem (Sampling):** Você pode definir a frequência de coleta (ex: a cada 10ms) e o número total de amostras (ex: 1000 amostras).

**B. Execução e Leitura**

1. Configure os dispositivos em `[Monitor] -> [Data Trace]`.
2. Descarregue a configuração na CPU via **Write Trace Setting**.
3. Após o disparo do gatilho, utilize o **Read Trace** para visualizar o gráfico e a grade de dados (**Data Grid**).
4. **Exemplo Prático:** Se o sincronismo entre seus inversores oscila durante a aceleração, utilize o Data Trace para monitorar a frequência real do mestre (`D101`) e a resposta do escravo (`D111`) exatamente no momento da partida.

---

2. Validação de Segurança e "Fail-Safe"

A segurança profissional é baseada na filosofia **Fail-Safe** (falha segura), onde qualquer anomalia deve levar a máquina a um estado de parada segura.

- **Intertravamentos:** Devem existir tanto em software quanto em hardware (_hardwired_) para funções críticas.
- **Testes de Emergência:** Devem ser documentados e repetíveis, verificando se os botões de emergência desenergizam os atuadores de forma rápida e segura.
- **Estado de Erro:** Utilize o histórico anterior para garantir que falhas de comunicação P2P forcem a máquina de estados para um ID de "ERRO", desligando as saídas.

---

3. Protocolos de Teste Industrial: FAT e SAT

A validação final de um software profissional segue duas etapas formais de aceitação:

**A. FAT (Factory Acceptance Test)**

Realizado nas instalações do fornecedor (ou via simulador **XG-SIM**) antes do envio da máquina.

- **Objetivo:** Verificar cada requisito da Especificação Funcional (URS) usando checklists formais.
- **Evidências:** Coleta de logs de eventos e capturas de tela do Data Trace como prova de conformidade.

**B. SAT (Site Acceptance Test)**

Realizado no local definitivo de operação com o equipamento real e carga.

- **Ajustes Finos:** Calibração final de sensores e sintonia fina da malha PID em condições reais de produção.
- **Validação em Campo:** Teste completo de todos os estados operacionais e intertravamentos físicos.

---

4. Entrega e Suporte (Handover)

Um projeto profissional termina com a entrega de um pacote completo ao cliente (**As-Built**):

1. **Código-fonte:** Comentado, estruturado e organizado em módulos.
2. **Manuais:** Documentação de operação e guia de solução de problemas (Troubleshooting).
3. **Bibliotecas:** Entrega de blocos **UFB/UDF** protegidos ou abertos, conforme contrato.

---

Checklist Final do Curso

Ao concluir seu programa no XG5000, valide os seguintes pontos:

1. [ ] **Data Trace:** Capturou e validou o comportamento de malha fechada?
2. [ ] **Fail-Safe:** A máquina entra em estado seguro ao perder comunicação ou acionar emergência?
3. [ ] **FAT/SAT:** Todos os requisitos da especificação foram testados e assinados?
4. [ ] **Documentação:** O dicionário de variáveis está completo e os programas otimizados?