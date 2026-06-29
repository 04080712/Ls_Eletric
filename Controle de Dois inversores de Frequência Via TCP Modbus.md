
                Ethernet (Modbus TCP)
                     │
               CLP XBC-DN32UP
                 /         \
                /           \
         S100 Motor 1    S100 Motor 2

Dispositivos utilizados: 
CLP: [[XBC-DN32UP (15L3998S)]]
Inversor de Frequência: [[S100 (08L0623S)]]
Módulo de comunicação Ethernet: [[Módulo Ethernet S100 (08L0721S)]]

### Configurando os inversor de Frequência.

|  Parâmetro   |               Valor/Função               |
| :----------: | :--------------------------------------: |
|     DRV      |      4 = FONTE DE COMANDO(ETHERNET)      |
|     FRQ      |        8 = CONTROLE DE VELOCIDADE        |
|     CM94     |     1 = APLICAR NOVAS CONFIGURAÇÕES      |
|    CNF48     | 1 = SALVAR CONFIGURAÇÕES PERMANENTEMENTE |
| CM10 AO CM13 |  DEFINIR O IP DO INVERSOR DE FREQUÊNCIA  |
| CM14 AO CM17 |         DEFINIR MÁSCARA DE REDE          |


### Configurando o CLP antes do Programa Ladder.

No CLP é necessário configurar o Internal FEnet/Ethernet (Canal Ethernet do CLP). Clicar duas vezes em internal FEnet/Ethernet e configure o IP que seu CLP terá a partir de agora, configure o Driver Setting com o Server Mode: **Modbus Server** e clicar em "OK".

![[Pasted image 20260629110012.png]]

A partir de agora deve-se clicar com o botão direito em internal FEnet/Ethernet, seguindo para Add e adicionar o P2P communication / Modbus TCP. 

![[Pasted image 20260629110757.png]]


##### Configurando P2P Chanel

Para configurar o P2P Chanel  deve-se clicar duas vezes no P2P Chanel para abrir o painel "Chanel Settings". Ao abrir configure o P2P Drive em **Modbus TCP Client** 

![[Pasted image 20260629111524.png]]

Ao configurar P2P Drive vai ficar disponível a opção de colocar o IP de cada inversor que o CLP vai comunicar. Coloque o IP de cada inversor. 

Obs.:
Deve-se configurar a quantidade de Canais P2P conforme a quantidade de Dispositivos que você vai comunicar. Por exemplo, nesta atividade vamos comunicar com dois inversores de Frequência: 

![[Pasted image 20260629112347.png|495]]


Após todas as configurações clicar em "OK"

##### Configurando o P2P Block

A partir de agora deve-se configurar o P2P Block que é responsável pela gestão das memórias de escrita e leitura dos inversores de frequência. 

| Chanel | P2P Function | Conditional Flag | Comand Type | Data Type | Data Size | Destination Statio |
| :----: | :----------: | :--------------: | :---------: | :-------: | :-------: | :----------------: |
|   0    |    WRITE     |       F90        |  CONTINUOS  |   WORD    |     5     |         1          |
|   1    |    WRITE     |       F90        |  CONTINUOS  |   WORD    |     5     |         2          |
|   0    |     READ     |       F90        |  CONTINUOS  |   WORD    |     5     |         1          |
|   1    |     READ     |       F90        |  CONTINUOS  |   WORD    |     5     |         2          |
Ao Clicar em "Settings" deve-se configurar a Read Area e Save Area, após isso clicar em "Ok" da seguinte forma: 

![[Pasted image 20260629114718.png]]


Então os comandos para o Inversor de frequência 1 serão enviados por meio das memórias D0 até D5. Para o Inversor será enviado das memórias D10 até D15. 
Para a Leitura os dados do Inversor 1 serão lidos em D100 até D105, e os dados do Inversor 2 serão de D110 até D115. 

A partir de agora os inversores de frequência já poderão comunicar com o CLP via Ethernet. 

### Programa Ladder.



