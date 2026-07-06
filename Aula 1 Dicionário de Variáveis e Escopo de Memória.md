Objetivo da Aula

Dominar a criação de um dicionário de dados estruturado, diferenciando variáveis globais e locais, e aplicando regras de nomenclatura industrial para eliminar a ambiguidade no código.

---

1. A Filosofia das Variáveis vs. Endereços Físicos

Na programação profissional, evitamos usar endereços diretos (ex: `P00020`) soltos na lógica. A prática recomendada é especificar uma **Variável** (ou _Tag_) para o dispositivo e utilizá-la em todo o programa. Isso permite que, se o hardware mudar, você altere o endereço em um único lugar (o dicionário), e todo o código seja atualizado automaticamente.

2. Escopo de Memória: Global vs. Local

A arquitetura do software industrial depende de onde os dados podem ser acessados. No XG5000 (especialmente na série XGK/XGB com auto-alocação), temos dois escopos principais:

- **Global Variables (Variáveis Globais):** São acessíveis por **todos os programas** dentro do projeto. São ideais para sinais de segurança (emergência), intertravamentos entre estações e dados que precisam ser lidos pela IHM (XP-Builder).
- **Local Variables (Variáveis Locais):** São acessíveis **apenas dentro de um programa específico**. Use-as para cálculos temporários, passos de uma máquina de estados específica ou lógicas que não precisam ser compartilhadas, protegendo o restante do sistema de alterações acidentais nessas memórias.
- **External Variables:** Se você precisar usar uma variável global dentro de um programa configurado apenas com variáveis locais, você deve registrá-la como **VAR_EXTERNAL** para "importar" o acesso.

3. Regras de Nomenclatura e Tipagem

Para que o software seja profissional, ele deve seguir regras rígidas de escrita. O XG5000 impõe limitações técnicas, mas a "boa prática" impõe a estética industrial:

**Regras Técnicas****:**

- **Início:** Não pode começar com números.
- **Caracteres Especiais:** Não são permitidos espaços ou símbolos (como `@`, `#`, `!`), exceto o _underscore_ (`_`).
- **Conflitos:** O nome da variável não pode ser igual a um nome de dispositivo (ex: você não pode nomear uma variável como `M0` ou `P0`).

**Tipos de Dados Essenciais****:**

- **BIT/BOOL:** Para contatos de sensores e bobinas (Liga/Desliga).
- **WORD/INT:** Para valores inteiros (ex: frequência alvo em 0.01Hz).
- **REAL:** Para cálculos de ponto flutuante (ex: temperatura ou pressão precisa).
- **STRING:** Para mensagens de texto.

**Padrão Industrial:** Utilize prefixos ou nomes hierárquicos para facilitar a busca, como `EST01_MOTOR_LIGA` ou `CELL01.MOTOR.M1.RunCmd`.

4. Ferramentas de Gestão no XG5000

O software oferece janelas dedicadas para organizar seu dicionário:

- **View Variable:** Exibe a lista completa de variáveis declaradas.
- **View Device:** Permite ver as variáveis organizadas pelo endereço físico (P, M, D, etc.), facilitando a detecção de buracos ou sobreposições na memória.
- **Export to File:** Você pode exportar seu dicionário para um arquivo **CSV**. Isso é vital para compartilhar os _tags_ com outros softwares como o XP-Builder ou softwares de supervisão (SCADA) sem precisar redigitar tudo.

5. Boas Práticas de Produtividade

- **Comentários de Dispositivo:** Mesmo que não crie uma variável, adicione sempre um comentário ao endereço físico. O XG5000 permite visualizar ambos simultaneamente.
- **Eficiência de Linha:** Se você registrar uma variável sem nome ou comentário, o sistema a exibirá em **rosa** no editor, indicando que a documentação está incompleta.
- **Uso de Latch:** Ao declarar variáveis automáticas, defina se elas devem ser **Retentivas (Latch)** para manter o valor após uma queda de energia.

---

Exercício Prático de Dicionário

1. Abra a janela **Variable/Comment** do seu projeto.
2. Crie 3 variáveis globais seguindo o padrão industrial:
    - `GLB_EMERG_OK` (BIT) associada ao seu botão físico.
    - `INV01_FREQ_ALVO` (WORD) para o comando de velocidade.
    - `SIST_AUTO_ATIVO` (BIT) para o estado do processo.
3. Tente criar uma variável começando com um número e observe o erro retornado pelo software.
4. Exporte sua lista para CSV e abra no Excel para visualizar a estrutura de dados