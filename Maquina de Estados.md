### O que é maquina de Estados? 

"*A ideia desse conceito é pensar como um projetista de máquina*."

Para iniciar o estudo de de projeção de maquinas é interessante sempre fazer a pergunta: Quais são os estados dessa máquina , ou melhor, planejar quais serão os estados dessa máquina?  

Imaginando uma esteira como exemplo: 

Uma esteira tem os seguintes estados: Parado, rodando sentido horário, rodando sentido ante horário, alarme, parada por causa do botão de emergência.

Após definir os Estado que a máquina possui , deve-se questionar quais condições cada estado terá, com a seguinte pergunta: 

**=='Quais condições precisam ser verdadeiras para permitir essa mudança de estado?'==**

Por exemplo para o estado em que a esteira está rodando no sentido horário: 

Emergencia está liberada?
Existe algum alarme ativo?
O inversor está pronto?
Existe energia?
Alguma proteção disparou?

Essa maneira de pensar é chamada como **lógica de transição**.

### Eventos e Transições

Uma regra sobre Eventos e Transições: 
"*Um estado nunca muda sozinho. Algo precisa provocar essa mudança.*"

Esse algo é chamado de evento de transição

Há sempre uma lógica que deve auxiliar em relação a mudança de estado. Por exemplo lógicas de permissões de partida. 

Tem algum alarme ativo? 
O inversor esta pronto?
Botão de emergencia ativado? 
rodando em sentido ante horario?

### O funcionamento de toda máquina

O processamento e varredura de um clp funcionam conforme a seguinte analogica de consciencia:

1. Estou em qual estado?

2. Posso permanecer nele?

3. Existe algum evento que exige mudar?

4. Se sim, para qual estado? 

Exercício de lógica: 

Imagine que a máquina está em Rodando Horário. Quais eventos podem fazer essa siar desse estado? 

Botão de emergência acionado.
botão stop acionado. 
Algum tipo de trigger se houver na programação 
Alarme do inversor

### Agora falta responder a pergunta mais importante: Quem muda de Estado? 

Sempre será o CLP
Quero que você imagine que a máquina está em:

```
RODANDO HORÁRIO
```
Você aperta STOP.

O CLP muda para:
```
PARADA CONTROLADA
```
Minha pergunta é:
**O que o CLP deve fazer enquanto está no estado `PARADA CONTROLADA`?**
Não a transição.
O que ele faz **durante** esse estado?

Um Sinaleiro ficaria piscando até parar.
a mudança de estado para anti horario ficria travada por enquanto.
Uma mensagem iria aparecer na ihm dizendo o estado de operação.


Agora já não estamos pensando apenas em:

> "Liga motor."

Estamos pensando em:

- O que acontece quando entro no estado?
- O que acontece enquanto permaneço nele?
- O que acontece quando saio dele?

Essa forma de organizar o comportamento é usada em muitas máquinas industriais e também em software, robótica e sistemas embarcados.



## RESUMAO

Para a programação de uma máquina é importante ter esses padrões definidos: 

- Estados bem definidos.
    
- Eventos de transição.
    
- Permissões.
    
- Ações de entrada.
    
- Ações contínuas.
    
- Ações de saída.


### [[Atividade Prática Planejamento de Escopo Ladder]]

[[Manual de desenvolvimento de Projetos de máquinas]]
