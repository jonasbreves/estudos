# Dashboard de Estudos

Dashboard de estudos em página única (HTML + Tailwind + JavaScript puro). Você informa o tempo total disponível e o tempo líquido de estudo, e ele monta sozinho o cronograma de blocos de estudo e pausas.

## Funcionalidades

- **Menu lateral responsivo** para configurar a sessão (fixo em telas grandes, gaveta no celular).
- **Divisão automática**: a partir do tempo total e do tempo líquido de estudo, o algoritmo encontra a melhor combinação de blocos e pausas.
- **Timer circular** por bloco, com início/pausa, reinício, avanço manual e aviso sonoro ao fim de cada bloco.
- **Cronograma com lista de verificação**: clique em um bloco para selecioná-lo e use a caixa para marcar como concluído.
- **Barra de progresso** da sessão, também exibida no cabeçalho.
- **Anotações rápidas** com salvamento automático e botão de copiar.
- **Persistência local** (`localStorage`): configuração, progresso e anotações ficam salvos no navegador.

## Como usar

1. Abra o arquivo `dashboard_de_estudos.html` no navegador. Não há build nem instalação.
2. Abra o menu lateral (botão de menu no cabeçalho) e preencha:
   - **Nome da aula**: aparece exatamente como digitado, sem prefixo automático.
   - **Horário de início**
   - **Tempo total disponível**: do início ao fim da sessão, incluindo as pausas.
   - **Tempo líquido de estudo**: só o tempo efetivamente estudado.
3. Confira a prévia da divisão automática e clique em **Gerar plano**.
4. Use o timer e a lista de verificação durante o estudo.

> É necessária conexão com a internet na primeira abertura, pois Tailwind CSS, Lucide Icons e a fonte Inter são carregados por CDN.

## Como a divisão é calculada

O tempo de descanso é o que sobra: `descanso = tempo total − tempo líquido de estudo`.

O plano sempre começa e termina com um bloco de estudo, sem pausa no final, e a soma de todos os blocos e pausas é exatamente o tempo total.

| Etapa | Regra |
|---|---|
| Quantidade de blocos | O algoritmo testa de 2 a 60 blocos (com `n − 1` pausas entre eles) e escolhe a quantidade com menor "custo" |
| Custo de um plano | Mede o quanto os blocos se afastam de 60 min e as pausas se afastam de 15 min. Blocos curtos e pausas curtas são penalizados mais do que blocos longos e pausas longas |
| Blocos de estudo | Divididos igualmente entre si (diferença de no máximo 1 min) |
| Pausas | Divididas igualmente, em múltiplos de 5 min. A sobra vai para as pausas do meio da sessão |
| Pausa Principal | A pausa do meio recebe o nome "Pausa Principal: Lanche" só quando é pelo menos 10 min maior que a menor pausa |

Casos especiais:
- **Tempo total igual ao líquido:** não há descanso, então é criado um único bloco.
- **Menos de 5 min de descanso no total:** a prévia exibe um aviso.

**Exemplos:**

| Total | Líquido | Resultado |
|---|---|---|
| 5h | 4h | 4 blocos de 60 min e 3 pausas de 20 min |
| 7h30 | 5h48 | 6 blocos de 58 min, 4 pausas de 20 min e 1 de 22 min |
| 2h | 1h30 | 2 blocos de 45 min e 1 pausa de 30 min |

## Limites de validação

- Tempo total: de 15 min a 16 h
- Tempo líquido de estudo: no mínimo 5 min e no máximo igual ao tempo total

## Armazenamento local

| Chave | Conteúdo |
|---|---|
| `sprint_cfg_v2` | Configuração da sessão |
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
