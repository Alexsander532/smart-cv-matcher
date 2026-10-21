# 10 — Plano de execução da PoC e evolução futura

Planejamento proposto em 21/10/2026. Não atribui tarefas a pessoas que ainda não foram informadas. O prazo oficial consultado é 27/10/2026 às 23h; usar uma entrega interna antecipada.

**Direção atual:** construir primeiro o front com dados simulados. O back básico é opcional e começa somente depois de estabilizar o percurso principal. Este plano é inicial e deve ser ajustado com os outros integrantes. O detalhamento por etapa está no [capítulo 13](13-desenvolvimento-da-poc.md); tarefas e dependências estão no [capítulo 14](14-backlog-e-colaboracao.md).

## Trabalho até a submissão

| Dia | Objetivo | Saída verificável |
|---|---|---|
| 21/10 | Alinhar proposta e dividir trabalho | Esboço das telas, frentes assumidas e perguntas aos mentores; comunicação começa em paralelo |
| 22/10 | Construir estrutura do front e acesso aos dados | Navegação e adaptador para os 12 perfis; identidade visual básica |
| 23/10 | Finalizar base, perfil e formulário | Busca, filtros, evidências fictícias e critérios editáveis |
| 24/10 | Conectar cálculo e ranking no navegador | Elegibilidade, tetos, comprovado/pendente e explicação funcional |
| 25/10 | Completar seleção, relatório e percurso | PoC utilizável, conferência pelo grupo e limitações registradas; extras só depois disso |
| 26/10 | Congelar demonstração, fechar resumo e gravar | Duas páginas revisadas, parágrafo de 150–200 palavras, vídeo abaixo de 5 minutos |
| 27/10, até 18h | Conferência e submissão interna | Arquivos congelados, links testados e responsáveis disponíveis |
| 27/10, antes das 23h | Submissão oficial | Recibo e cópia exata dos materiais enviados |
| 28–29/10 | Acompanhar avaliação e encerramento | Registros e participação conforme orientações oficiais |

Os horários internos são escolhas da equipe, não novos prazos oficiais. Se o protótipo atrasar, preservar as entregas obrigatórias e demonstrar a lógica com uma simulação identificada. Não é necessário implementar todas as funcionalidades documentadas para apresentar uma proposta coerente.

Resumo, pesquisa e roteiro evoluem durante todos os dias, com integração ao que estiver efetivamente pronto. Não deixar a primeira versão do texto ou do pitch para 26/10. Se o front ficar estável antes do previsto, o grupo pode usar a folga para API básica; se não houver folga, a PoC pode ser apresentada sem back.

## Divisão de trabalho sugerida

| Frente | Responsabilidade | Definição de pronto |
|---|---|---|
| Interface e navegação | Estrutura visual, lista, filtros e perfil | Telas conectadas com dados compartilhados |
| Regras e dados | JSON, tipos, elegibilidade e cálculo no front | Notas compatíveis com a referência |
| Jornada da decisão | Formulário, ranking, seleção e relatório | Percurso completo e reproduzível |
| Produto e comunicação | Mentores, escopo, pesquisa, resumo, pitch e envio | Materiais coerentes com o que foi construído |

Para três integrantes, combinar frentes; para quatro ou cinco, dividir sem perder revisão cruzada. Preencher nomes em [12 — Fontes e decisões](12-fontes-e-decisoes.md).

## Critérios para congelar a PoC de front

- Dados sintéticos locais e estados de evidência identificados como simulação.
- Pelo menos um percurso completo reproduzível de base até relatório.
- Critérios editáveis, requisitos obrigatórios e teto demonstrados no navegador.
- Comprovado e pendente claramente separados.
- Nenhuma evidência simulada apresentada como documento real validado.
- Importação simulada, quando incluída, é apresentada como simulação; IA real não é requisito para congelar o front.
- Cálculo comparado à referência manual e limitações documentadas.
- Materiais obrigatórios revisados antes de investir em acabamento extra do protótipo.

## Condição para investir no back básico

Só iniciar a implementação depois de lista, perfil, critérios, ranking, seleção e relatório funcionarem em conjunto e de o grupo confirmar disponibilidade para integrar e conferir a mudança. O primeiro back serve a base fictícia e executa o cálculo. Banco de dados, upload e IA não precisam entrar juntos. Evitar alterar a arquitetura durante a gravação do vídeo ou na janela final de envio.

## Implantação futura proposta na Nexora — além da PoC

| Etapa | Conteúdo | Condição para avançar |
|---|---|---|
| Descoberta e governança | Fluxos, responsáveis, políticas, critérios reais e acessos | Escopo e tratamento de dados aprovados pela organização |
| Piloto pequeno | Uma área, um tipo de disputa, conjunto controlado de perfis | Extração e pontos revisados; esforço de manutenção conhecido |
| Operação assistida | Usar em paralelo ao processo atual | Não aumentar erros ou tempo total; responsável pelos comprovantes definido |
| Expansão do acervo | Migração gradual, treinamento e rotinas de atualização | Cobertura documental e adesão suficientes |
| Integrações e otimização | Sistemas de projetos/RH, capacidade e múltiplas disputas | Demanda e retorno justificados pelo piloto |

Essas etapas não possuem orçamento ou duração contratual definidos. Dimensionar após levantar formatos, volume de documentos por pessoa, qualidade da base e exigências de segurança.

## Continuidade e adoção

Designar um dono do produto e um responsável pelo acervo. A atualização precisa beneficiar o profissional: reaproveitamento de currículo, visibilidade de pendências e menos retrabalho. Encerrar projeto e obter certificação devem disparar tarefas de atualização. Acompanhar uso, atraso de revisão, qualidade dos documentos e satisfação; ajustar o processo se os lembretes não resultarem em atualização.
