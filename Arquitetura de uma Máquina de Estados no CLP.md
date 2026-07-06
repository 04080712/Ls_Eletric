
Desenho da arquitetura do software da esteira: 

                    MAIN
                      │
      ┌───────────────┴───────────────┐
      │                               │
      ▼                               ▼
Gerenciador de Estados         Gerenciador de Alarmes
      │
      ▼
Estado Atual
      │
      ▼
Lógica do Estado
      │
      ├── Controle do Inversor
      ├── Controle dos Sinaleiros
      ├── Controle da HMI
      └── Leitura dos Sensores
