Objetivo da Aula

Configurar as ferramentas de inspeção pró-ativa e as preferências do editor para eliminar bugs de sobreposição de memória em tempo real e aumentar a velocidade de desenvolvimento através de atalhos e funções de limpeza.

---

1. Ferramentas de Inspeção em Tempo Real (Escudo do Programador)

Um ambiente profissional não espera o "Check Program" final para encontrar erros. Ele avisa durante a digitação.

**A. Check Duplicated Coil Instantly (Verificação de Bobina Duplicada)**

- **O que faz:** Se você utilizar o mesmo endereço de saída (ex: `P00020`) em duas bobinas `-( )-` diferentes, o software abre imediatamente uma janela de alerta na parte inferior.
- **Por que usar:** Bobinas duplicadas causam comportamentos imprevisíveis, onde apenas o último comando do ciclo de scan prevalece. Ativar isso evita horas de depuração frustrante.
- **Onde ativar:** `[Tools] -> [Options] -> [XG5000] -> [Common Editor]`.

**B. Output Cross Reference Instantly (Referência Cruzada Instantânea)**

- **O que faz:** Ao clicar em qualquer variável ou contato no Ladder, o sistema exibe automaticamente em qual linha e programa esse endereço está sendo usado.
- **Vantagem Analítica:** Permite rastrear instantaneamente a origem de um sinal de intertravamento sem precisar abrir janelas de busca (`Ctrl+F`).

---

2. Configurações de Interface para Alta Produtividade

Pequenos ajustes na visualização do editor reduzem o cansaço mental e erros de digitação.

- **Instant Input Mode:** Quando ativo, ao inserir um símbolo (F3, F4, F9), a caixa de texto para o endereço abre automaticamente. Se desativado, você precisa dar um duplo clique no elemento.
- **Show Line Numbers & Grid:** Essencial para comunicação em equipe (ex: "Verifique o erro na linha 145") e para alinhar visualmente ramos complexos de lógica.
- **Zoom Progressivo:** Utilize `[View] -> [Zoom-In/Out]` ou atalhos de mouse para ajustar a visibilidade de rungs extensos que utilizam muitas instruções de aplicação.

---

3. Funções de Limpeza e Organização de Código

Um software profissional deve ser "limpo". O XG5000 possui ferramentas para "higienizar" sua lógica.

**A. Optimize Program (Otimizar Programa)**

- **Função:** Remove automaticamente linhas horizontais órfãs e espaços vazios entre contatos e bobinas, comprimindo o diagrama para uma leitura mais fluida.
- **Nota:** Esta função não pode ser desfeita pelo "Undo". Use-a após concluir um bloco lógico importante.

**B. Deletar Variáveis Não Utilizadas**

- No dicionário de variáveis, utilize a função **Delete all unused variables/comments** para limpar o banco de dados do projeto, garantindo que o arquivo final contenha apenas o que é estritamente necessário para a operação.

---

4. Integração e Atalhos de Teclado

A velocidade industrial exige que você tire as mãos do mouse o máximo possível.

- **Edição Estilo Excel:** O XG5000 permite copiar listas de variáveis diretamente do Excel e colá-las no editor de variáveis, adicionando linhas automaticamente e incrementando endereços se necessário.
- **Atalhos Essenciais:**
    - **F3 / F4:** Contatos NA / NF.
    - **F9:** Bobina comum.
    - **F10:** Instrução de Aplicação (MOV, PID, ADD).
    - **Ctrl + Setas:** Desenha ou apaga linhas de conexão rapidamente.

---

Exercício de Configuração do Ambiente

1. Vá ao menu **Options** e ative a verificação de bobinas duplicadas e a referência cruzada instantânea.
2. Crie uma bobina duplicada propositalmente e observe o surgimento da janela de erro **Duplicate Coil**.
3. Pratique a inserção de uma linha de comando completa (Contato -> MOV -> Saída) utilizando apenas as teclas de atalho (F3, F10, F9).
4. Execute o comando **Optimize Program** para ver o realinhamento automático da sua lógica