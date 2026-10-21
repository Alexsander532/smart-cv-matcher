# 14 — Backlog inicial e colaboração do grupo

Este documento é um ponto de partida para os integrantes discutirem e assumirem o trabalho. As frentes não estão atribuídas a pessoas. O roteiro de referência é [13 — Desenvolvimento da PoC](13-desenvolvimento-da-poc.md).

**Prioridades:** P0 = necessário para o fluxo principal da PoC; P1 = complemento visual ou de demonstração; P2 = back e extração real, após o front estável. As prioridades desta tabela são da PoC, enquanto o capítulo 04 descreve os requisitos do produto completo.

## Tarefas de preparação e front

| ID | Prioridade | Tarefa | Entrega | Depende de |
|---|---|---|---|---|
| G01 | P0 | Revisar a proposta com o grupo | Escopo e decisões registrados | Leitura inicial |
| G02 | P0 | Definir responsáveis e integração | Nomes e frentes preenchidos | G01 |
| G03 | P0 | Esboçar telas e navegação | Fluxo validado pelo grupo | G01 |
| F01 | P0 | Iniciar projeto de front | Execução local documentada | G02 |
| F02 | P0 | Criar estrutura visual e navegação | Acesso a todas as telas principais | F01, G03 |
| D01 | P0 | Mapear JSON para tipos compartilhados | Contrato único de perfil e critérios | G01 |
| D02 | P0 | Criar acesso a dados locais | Lista, detalhe e critérios disponíveis | F01, D01 |
| F03 | P0 | Construir lista e filtros | Busca, filtros e limpeza funcionando | F02, D02 |
| F04 | P0 | Construir perfil e evidências | Fatos e pendências por profissional | F02, D02 |
| R01 | P0 | Implementar elegibilidade | Disponibilidade e tempo mínimo | D01, D02 |
| R02 | P0 | Implementar pontos e tetos | Resultado por critério | D01, D02 |
| R03 | P0 | Separar comprovado e pendente | Dois cenários identificados | R01, R02 |
| F05 | P0 | Construir configuração da disputa | Critérios editáveis e validados | F02, D01 |
| F06 | P0 | Conectar ranking ao formulário | Resultado muda com a configuração | F05, R03 |
| F07 | P0 | Mostrar explicação da nota | Regra, fato, prova e pontos | F04, F06 |
| F08 | P0 | Implementar escolha de uma vaga | Seleção válida e possibilidade de troca | F06 |
| F09 | P0 | Construir relatório da seleção | Resumo imprimível/exportável | F07, F08 |
| Q01 | P0 | Conferir cenários de referência | Erros corrigidos e resultados registrados | R03, F06 |
| Q02 | P0 | Percorrer fluxo com outro integrante | Feedback e bloqueios resolvidos | F03–F09 |
| F10 | P1 | Criar importação simulada | Fluxo com rótulo de simulação | F04, D02 |
| D03 | P1 | Produzir arquivo sintético de currículo | Documento para prévia/demonstração | G01 |
| F11 | P1 | Editar/revisar dados simulados | Alteração local refletida na base | F10, R03 |
| F12 | P1 | Restaurar cenário inicial | Repetição previsível do pitch | F06, F08 |
| F13 | P1 | Refinar currículo padronizado | Prévia fiel aos fatos selecionados | F04, F09 |
| F14 | P1 | Melhorar apresentação em notebook | Legibilidade, foco e estados claros | F02–F09 |

## Trabalho de pesquisa e entrega que acompanha o desenvolvimento

| ID | Prioridade | Tarefa | Entrega | Pode começar quando |
|---|---|---|---|---|
| C01 | P0 | Esclarecer rubrica com mentores | Resposta ou pendência registrada | Imediatamente |
| C02 | P0 | Confirmar formato e canal de submissão | Orientações anotadas | Imediatamente |
| C03 | P0 | Revisar diagnóstico e premissas | Cálculos e lacunas revisados | Imediatamente |
| C04 | P0 | Conferir pesquisa institucional | Fontes e parágrafo de 150–200 palavras | Imediatamente |
| C05 | P0 | Preparar resumo | Arquivo diagramado em duas páginas | Após G01; concluir com estado real da PoC |
| C06 | P0 | Preparar roteiro do pitch | Roteiro cronometrado | Após G03; ajustar às telas prontas |
| C07 | P0 | Gravar demonstração e pitch | Vídeo final com até cinco minutos | Após Q02 |
| C08 | P0 | Submeter materiais | Recibo e versão final preservados | C02, C05, C07 |
| C09 | P1 | Avaliar compreensão da interface | Observações de integrantes e mentores | Após primeira jornada funcional |

## Back opcional

| ID | Prioridade | Tarefa | Entrega | Depende de |
|---|---|---|---|---|
| B01 | P2 | Definir contratos mínimos com quem faz o front | Formatos de entrada, saída e erros | D01; implementação após liberação da etapa 7 |
| B02 | P2 | Criar serviço e servir base sintética | Consulta de lista, detalhe e critérios | B01 |
| B03 | P2 | Implementar cálculo no servidor | Execução comparável à referência | B02, R01–R03 |
| B04 | P2 | Conectar adaptador HTTP | Jornada funcionando com API | B02, B03, Q02 |
| B05 | P2 | Persistir cenários, se necessário | Salvar e recuperar execução | B04 |
| B06 | P2 | Extrair um PDF sintético com IA | Saída real validada e revisável | B04, D03; tempo e provedor definidos |

Planejar contratos pode ocorrer antes de terminar o front. A implementação do back só entra quando o grupo confirmar que não compromete a PoC nem as entregas obrigatórias.

## Frentes que podem trabalhar ao mesmo tempo

| Frente | Atividades | Alinhamento necessário |
|---|---|---|
| Interface e navegação | Estrutura, lista, perfil e componentes | Tipos e identidade visual compartilhados |
| Regras e dados | Adaptador local, elegibilidade, score e fixtures | Contrato aprovado com a interface |
| Jornada da decisão | Formulário, ranking, escolha e relatório | Regras e formato dos resultados |
| Pesquisa e comunicação | Regulamento, mentores, impacto, resumo e pitch | Estado real do que a PoC consegue demonstrar |

Um integrante pode assumir mais de uma frente; dois podem dividir uma frente por tarefas. Para três pessoas, uma combinação possível é interface/base; regras/seleção; pesquisa/pitch com apoio na revisão. Para quatro ou cinco, separar dados, telas e comunicação. Isso é uma sugestão para o grupo ajustar, sem presumir a experiência ou disponibilidade de ninguém.

## Acordos para evitar retrabalho

1. Definir os tipos compartilhados e nomes dos estados antes de conectar telas. “Pendente” e “inelegível” não podem ser tratados como o mesmo estado.
2. Combinar quem altera estrutura geral, navegação e arquivos de configuração. Mudanças nesses pontos precisam ser comunicadas ao grupo.
3. Trabalhar em tarefas pequenas, com descrição da tela ou comportamento entregue. Usar branches separadas quando o grupo adotar esse fluxo.
4. Integrar frequentemente para verificar a jornada, evitando juntar todas as telas somente na véspera.
5. Cada entrega deve indicar como abrir a tela, qual cenário conferir e o que ainda está simulado.
6. Quem revisa uma tarefa deve comparar comportamento com o requisito e os dados de referência, além da aparência.
7. Atualizar documentação e roteiro quando uma funcionalidade sair do escopo ou ficar pronta.

## Definição de pronto de uma tarefa

- Funciona no fluxo esperado e apresenta os estados pertinentes de erro ou vazio.
- Usa os dados compartilhados; não duplica nomes e pontos fixos em componentes diferentes.
- Não identifica simulação como processamento real.
- Não apresenta erro que interrompa a jornada principal.
- Tem instrução curta para outro integrante conferir a entrega.
- Teve integração e revisão por alguém do grupo, quando possível.

## Quadro de acompanhamento para preencher

Estados sugeridos: `a fazer`, `em andamento`, `em revisão`, `concluído`, `bloqueado`.

| Tarefa | Responsável | Estado | Próximo passo/bloqueio | Referência da entrega |
|---|---|---|---|---|
| G01 — Revisão da proposta | A definir | A fazer | Discussão entre os integrantes | Este conjunto de documentos |
| F01 — Estrutura do front | A definir | A fazer | Escolher tecnologia conhecida | A preencher |
| D01 — Tipos compartilhados | A definir | A fazer | Revisar exemplos JSON | A preencher |
| C01 — Rubrica com mentores | A definir | A fazer | Levar divergência documentada | A preencher |

Esta tabela não afirma que o desenvolvimento já começou. Ao distribuir tarefas, adicionar as linhas necessárias e mover decisões confirmadas para o registro do capítulo 12.
