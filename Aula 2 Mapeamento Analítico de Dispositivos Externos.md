Objetivo da Aula

Aprender a técnica analítica de converter endereços de rede (Modbus) em variáveis estruturadas no XG5000, utilizando ferramentas de automação para evitar erros de endereçamento e garantir a integridade da comunicação.

---

1. A Ponte entre Rede e Memória

Na programação industrial, os dados vindos de dispositivos externos (como os seus inversores) chegam em blocos de memória brutos. O **Mapeamento Analítico** consiste em transformar esses blocos (ex: as 5 words que você configurou) em nomes que façam sentido para o processo.

**O Caso Real: Mapeando os Inversores**

Com base na sua configuração P2P discutida anteriormente, temos dois grandes blocos de dados:

- **Bloco de Escrita (WRITE):** Começa em `0x40005` no inversor e reflete os seus registros **D0 a D4** (Inv 1) e **D10 a D14** (Inv 2).
- **Bloco de Leitura (READ):** Começa em `0x40310` no inversor e salva os dados em **D100 a D104** (Inv 1) e **D110 a D114** (Inv 2).

**Técnica Profissional:** Não use `D101` no Ladder. Use a variável mapeada `INV01_FREQ_REAL`. Se futuramente você mudar a área de leitura no P2P para `D200`, basta alterar o dicionário e o programa continuará funcionando.

---

2. Automação com "Register Module Variable Comments"

O XG5000 possui uma função vital para a produtividade: ao configurar módulos especiais ou de comunicação, você não precisa digitar todos os endereços manualmente.

- **Como aplicar:** Vá ao menu `[Edit]` -> `[Register Module Variable Comments]`.
- **Resultado:** O software varre as suas configurações de E/S e de rede, registrando automaticamente as variáveis de diagnóstico e status no dicionário.
- **Vantagem:** Isso garante que você tenha acesso imediato a flags de erro e prontidão do hardware sem risco de errar o endereço físico.

---

3. Integração Analítica via LS Studio

Uma arquitetura moderna utiliza o ambiente integrado para evitar a exportação/importação manual de arquivos CSV.

- **LS Studio:** Permite que o projeto do CLP (XG5000), da IHM (XP-Builder) e do Inversor (DriveView) compartilhem a mesma base de dados.
- **Benefício:** Ao alterar o nome de uma variável de frequência no CLP, a IHM reconhece a mudança automaticamente, eliminando a inconsistência de dados entre os dispositivos da rede.

---

4. Monitoramento e Validação da Malha de Dados

Para garantir que o mapeamento está correto antes de girar os motores, utilize as ferramentas de diagnóstico:

- **View Device Uses:** Clique com o botão direito em um endereço (ex: `D101`) para ver todos os pontos do programa onde essa leitura do inversor está sendo processada.
- **Variable Monitoring:** Arraste o bloco de registradores `D100~D114` para a janela de monitoramento. Se os valores em `D101` e `D111` oscilarem conforme o motor gira, seu mapeamento analítico de leitura está validado.
- **Status de Comunicação (NDR/ERR):** Utilize as flags `_P2P1_NDRxx` para validar que o dado mapeado é "fresco". Se a flag `ERR` ativar, o programa deve ignorar os dados mapeados para evitar comandos baseados em leituras falsas.

---

Exercício Prático de Mapeamento

1. No seu projeto, abra a área de **Global Variables**.
2. Crie as seguintes variáveis analíticas para o Inversor 1 (Mestre):
    - `INV01_CMD_GIRO` associada ao `D1` (Valor 1=FWD, 2=REV).
    - `INV01_FREQ_ALVO` associada ao `D0` (Escala 0-5000).
    - `INV01_FREQ_REAL` associada ao `D101`.
3. Execute a função **Register Module Variable Comments** para importar as flags de status da sua placa de rede.
4. Abra o **Cross Reference** e verifique se as variáveis criadas estão sendo usadas corretamente na lógica de sincronismo.