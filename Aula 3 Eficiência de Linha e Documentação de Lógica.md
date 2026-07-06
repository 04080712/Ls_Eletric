Objetivo da Aula

Garantir a integridade estrutural das linhas de programação (**Rungs**) e aplicar uma estratégia de documentação que permita a qualquer técnico entender a _intenção_ do software, não apenas o seu funcionamento elétrico.

---

1. Eficiência Estrutural: Eliminando a "Linha Rosa" e o Erro L0200

No **XG5000**, uma linha incompleta ou com erros de endereçamento é frequentemente sinalizada visualmente. A eficiência de linha começa com a validação técnica.

- **A "Linha Rosa":** Se você registrar uma variável sem nome ou endereço, ou deixar um contato/bobina vazio, o sistema indicará uma documentação incompleta [Conversation History].
- **Erro L0200 (Missing Device/Variable):** Este erro ocorre quando um contato ou bobina é inserido, mas nenhum endereço físico ou variável foi associado a ele.
- **Erro L0100 (Curto-circuito):** Acontece quando áreas conectadas em paralelo (**OR**) são interligadas por linhas horizontais sem um contato intermediário, criando um erro de lógica que o **Check Program** detectará.

**Boa Prática Industrial:** Nunca descarregue um programa que contenha erros de sintaxe ou avisos de "incomplete rung". Utilize o comando `[View] - [Check Program]` para varrer todo o projeto em busca desses problemas antes do teste em campo.

---

2. Documentação Estratégica: Rung Comments vs. Output Comments

A documentação profissional no XG5000 é dividida em duas categorias principais para facilitar o diagnóstico:

**A. Rung Comment (Comentário de Degrau)**

- **O que é:** Um bloco de texto posicionado no início de cada linha de lógica.
- **Como usar:** Deve descrever a **função do bloco lógico** (ex: "Controle de Sincronismo entre INV1 e INV2").
- **Vantagem Analítica:** Você pode navegar rapidamente pelo programa usando a função **Go To Rung Comment**, que lista todos os títulos de lógica do projeto, permitindo saltar para a seção de "Emergência" ou "Manual" instantaneamente.

**B. Output Comment (Comentário de Saída)**

- **O que é:** Um texto associado especificamente ao fator de saída (bobina ou instrução de aplicação).
- **Como usar:** Descreve a ação física daquele endereço (ex: "D1 - Comando de Giro INV 1").
- **Visualização:** Se configurado em `[View] - [Devices/Comments]`, o XG5000 exibirá tanto o endereço quanto a descrição abaixo da bobina no editor LD.

---

3. Otimização Automática e Limpeza

Para manter a arquitetura limpa e profissional, o XG5000 oferece ferramentas de "higienização" de código:

- **Optimize Program:** Esta função deleta automaticamente linhas horizontais órfãs e espaços vazios desnecessários entre contatos e bobinas, comprimindo o diagrama para que mais lógica seja visível na tela sem necessidade de rolagem excessiva.
- **Check Duplicate Coil:** Essencial para a eficiência. Se você tentar escrever na mesma variável em duas linhas diferentes, o software emitirá um aviso, prevenindo o erro clássico onde a última bobina do scan anula a lógica anterior.

---

4. Gestão de Navegação Profissional

Em programas extensos, a eficiência depende da velocidade de encontrar informações:

1. **Bookmarks (Favoritos):** Marque linhas críticas do processo para retornar a elas com um clique durante o comissionamento.
2. **Cross Reference:** Ao clicar em uma variável, o XG5000 mostra instantaneamente todos os pontos do programa (em todos os módulos e tarefas) onde aquela informação é lida ou escrita.

---

Checklist de Documentação e Eficiência

Antes de finalizar o Módulo 2, valide seu projeto:

1. [ ] **Execute o Optimize Program** para limpar espaços vazios no Ladder.
2. [ ] **Insira Rung Comments** em cada seção lógica importante (Emergência, Partida, Sincronismo).
3. [ ] **Verifique Erros L0200/L0100** através do menu `Check Program`.
4. [ ] **Confirme as Saídas:** Garanta que todas as bobinas de saída e instruções de aplicação tenham comentários claros.
5. [ ] **Instrução END:** Certifique-se de que o programa termina obrigatoriamente com a instrução `END`.