# Dashboard de Estudos

Dashboard de estudos em página única (HTML + Tailwind + JavaScript puro) que monta automaticamente um cronograma de blocos de foco e pausas a partir do tempo que você tem disponível.

## Funcionalidades

- **Menu lateral responsivo** para configurar a sessão (fixo em telas grandes, gaveta no celular).
- **Divisão automática** de estudo e descanso a partir do tempo total e do tempo de foco.
- **Timer circular** por bloco, com início/pausa, reinício, avanço manual e aviso sonoro ao fim de cada bloco.
- **Cronograma com lista de verificação**: clique em um bloco para selecioná-lo e use a caixa para marcar como concluído.
- **Barra de progresso** da sessão, também exibida no cabeçalho.
- **Anotações rápidas** com salvamento automático e botão de copiar.
- **Persistência local** (`localStorage`): configuração, progresso e anotações ficam salvos no navegador.

## Como usar

1. Abra o arquivo `dashboard_de_estudos.html` no navegador. Não há build nem instalação.
2. Abra o menu lateral (botão de menu no cabeçalho) e preencha:
   - **Nome da aula**
   - **Horário de início**
   - **Tempo total da sessão** (estudo + pausas)
   - **Tempo de foco por bloco**
   - **Revisão final** (opcional, 10 min)
3. Confira a prévia da divisão automática e clique em **Gerar plano**.
4. Use o timer e a lista de verificação durante o estudo.

> É necessária conexão com a internet na primeira abertura, pois Tailwind CSS, Lucide Icons e a fonte Inter são carregados por CDN.

## Como a divisão é calculada

| Parâmetro | Regra |
|---|---|
| Revisão final | 10 min, só se a sessão tiver 60 min ou mais e a opção estiver marcada |
| Pausa curta | ~25% do foco, arredondada para 5 min, entre 5 e 20 min |
| Pausa longa | O dobro da pausa curta, entre 10 e 30 min |
| Frequência da pausa longa | A cada 3 blocos (a cada 4 se o foco for de 30 min ou menos) |
| Número de blocos | O plano testa várias quantidades e escolhe a que deixa os blocos mais próximos do foco desejado |

Os minutos que sobram são distribuídos entre os blocos, então eles podem diferir em 1 minuto. Se o foco for maior que o tempo total, é criado um único bloco.

**Exemplo:** início às 13:30, 7h30 de sessão, foco de 60 min e revisão final resultam em 6 blocos de cerca de 58 min, pausas curtas de 15 min, pausa longa de 30 min e 10 min de revisão.

## Limites de validação

- Tempo total: de 10 min a 16 h
- Tempo de foco: de 5 a 180 min

## Armazenamento local

| Chave | Conteúdo |
|---|---|
| `sprint_cfg` | Configuração da sessão |
| `sprint_completed` | Blocos concluídos (ligados à configuração atual) |
| `study_notes_matematica` | Texto das anotações |

Ao gerar um plano com configuração diferente, o progresso é reiniciado. O botão **Reiniciar progresso** faz o mesmo sem mudar a configuração. As anotações não são apagadas.

## Tecnologias

- [Tailwind CSS](https://tailwindcss.com/) (CDN)
- [Lucide Icons](https://lucide.dev/) (CDN)
- Fonte [Inter](https://fonts.google.com/specimen/Inter)
- JavaScript puro, sem dependências nem build

## Estrutura

```
.
├── dashboard_de_estudos.html   # página completa (HTML, CSS e JS)
└── README.md
```
