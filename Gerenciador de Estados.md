O Gerenciador de Estados é o cérebro da máquina 

O gerenciador não muda nada, ele apenas evidencia qual estado atual. E a partir disso a maquina faz o resto.

## Princípio da Responsabilidade Única

Cada parte do software deve ter apenas uma responsabilidade.

Então:
### Gerenciador de Estados

Responsabilidade:

> Mudar de estado.

---

### Estado Rodando Horário

Responsabilidade:

> Controlar a operação em horário.

---

### Estado Emergência

Responsabilidade:

> Colocar a máquina em condição segura.

---

### Estado Alarme

Responsabilidade:

> Tratar falhas.

---

Perceba como cada bloco tem um único objetivo.


# Exercício (o mais importante até agora)

Imagine que amanhã você precise adicionar um novo estado.

```
MODO MANUTENÇÃO
```

Nesse modo:

- O operador pode movimentar a esteira por pulso (_jog_).
- A velocidade é limitada a 5 Hz.
- Os sensores de produção são ignorados.
- A emergência continua funcionando.

### Minha pergunta é:

**Onde você faria essa implementação?**

A) Dentro do Estado Rodando Horário?

B) Dentro do Gerenciador de Estados?

C) Criaria um novo estado chamado "Modo Manutenção"?


C 


### Quando criar um novo estado?

Crie um novo estado quando o comportamento da máquina mudar significativamente.

Um novo estado precisa responder uma pergunta:

> **"O comportamento da máquina mudou?"**
> ---

