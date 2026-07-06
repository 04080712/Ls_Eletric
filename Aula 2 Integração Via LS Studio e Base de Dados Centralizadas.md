Objetivo da Aula

Dominar o uso do ambiente integrado **LS Studio** para gerenciar projetos de múltiplos dispositivos (CLP XG5000, IHM XP-Builder e Inversores DriveView) em uma base de dados única, garantindo que qualquer alteração de variável no CLP seja refletida automaticamente nos demais sistemas sem a necessidade de arquivos CSV manuais.

---

1. A Filosofia do Ambiente Integrado (LS Studio)

Tradicionalmente, a integração entre um CLP e uma IHM exigia a exportação manual de uma lista de _tags_ em formato CSV do software de programação do CLP e a importação manual no software da IHM.

O **LS Studio** rompe esse ciclo ao fornecer um ambiente onde todos os arquivos de projeto são integrados e gerenciados centralizadamente pelo **XG5000**. Isso significa que o software "enxerga" a IHM e os Inversores como extensões do próprio projeto do CLP.

---

2. Compartilhamento de Variáveis Sem CSV

Com a integração ativa, a lista de variáveis usadas no CLP pode ser utilizada imediatamente na IHM.

**Passo a Passo para o Sincronismo Profissional:**

1. **Seleção de Tags no XG5000:** Na janela de **Global Variables** do XG5000, existe uma coluna específica chamada **HMI**. Você deve marcar o _checkbox_ de cada variável, flag ou entrada/saída que deseja que apareça na tela da IHM.
2. **Acesso via XP-Builder:** No navegador de projeto do XG5000, basta dar um duplo clique no item **HMI** para abrir o **XP-Builder**.
3. **Uso Automático:** Dentro do XP-Builder, ao abrir a janela de **Tags**, você encontrará um grupo (geralmente nomeado como `NewPLC`) contendo todas as variáveis que você selecionou no passo 1. Não há necessidade de importar nada; os dados já estão lá.

**Vantagem Analítica:** Se você renomear uma variável de controle de sincronismo no CLP, ela será atualizada automaticamente na base de dados da IHM, eliminando o erro comum de "endereço inválido" ou "comunicação falha" por _tags_ desatualizadas.

---

3. Integração com Inversores (DriveView)

O LS Studio também simplifica a configuração dos inversores através da inclusão de itens **INV** no projeto.

- **Adição de Dispositivos:** No menu `[Project] -> [Add Item] -> [PLC / Add-on]`, você pode adicionar a IHM e os Inversores (ex: modelos iS7 ou S100) diretamente à árvore do projeto.
- **Configuração Paramétrica:** Ao registrar um item de inversor, o XG5000 facilita a configuração dos parâmetros de comunicação LS BUS ou Modbus, permitindo que você selecione os endereços de leitura e escrita diretamente de uma lista pré-definida do modelo do inversor, em vez de consultar manuais de endereços Modbus o tempo todo.

---

4. Vantagens para o Fluxo de Trabalho Industrial

A adoção desta arquitetura centralizada traz ganhos diretos na **Eficiência Operacional (OEE)**:

- **Redução do Tempo de Engenharia:** Menos tempo gasto com tarefas repetitivas de exportação/importação.
- **Consistência de Dados:** Garante que a IHM exiba exatamente o que o CLP está processando.
- **Manutenibilidade:** Facilita para que equipes de manutenção identifiquem a origem de sinais, pois a nomenclatura é idêntica em todos os dispositivos da rede.

---

Exercício Prático de Integração

1. No seu projeto XG5000, vá até as **Global Variables** e marque as variáveis `INV01_FREQ_REAL` e `INV02_FREQ_REAL` na coluna **HMI**.
2. Adicione um item de **HMI** (ex: série iXP2 ou eXP2) ao seu projeto através do menu **Add Item**.
3. Abra o **XP-Builder** pelo XG5000 e tente associar um objeto "Numérico" a uma das _tags_ do CLP. Verifique se elas aparecem automaticamente na lista de endereços.
4. (Opcional) Adicione um item de **Inversor S100** e explore a janela de parâmetros para ver a lista de registros Modbus já mapeada pelo software