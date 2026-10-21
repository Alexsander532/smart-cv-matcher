# 06 — Regras de pontuação e composição de equipes

**Aplicação na PoC:** as regras da demonstração serão executadas inicialmente no front, sobre a base fictícia. Isso permite validar a lógica sem esperar o back. Seleção de múltiplos papéis e otimização são evolução; a primeira entrega cobre uma função com uma vaga. Quando houver API, o servidor deverá recalcular os resultados a partir de seus dados e regras autorizadas.

## Princípio

Uma nota pertence a um profissional **em uma função, disputa e versão de regras específicas**. O motor executa regras explícitas e reproduzíveis. A IA pode sugerir um mapeamento de fatos ou cláusulas, mas a configuração precisa de aprovação humana.

Há três tipos distintos de pontuação neste projeto: alocação de equipes do hackathon; avaliação da solução pela banca; aderência profissional dentro do produto. Este documento trata apenas do terceiro.

## Estrutura de um critério

Guardar identificador, função, descrição, fonte/cláusula, tipo, operador, valor esperado, pontos por unidade, teto, obrigatoriedade, condição de validade, política de evidência e regras de acúmulo. Exemplos de tipos: presença; contagem; duração; faixa; condição eliminatória. Usar operadores enumerados e evitar código arbitrário ou expressão livre executável.

O gestor escolhe os critérios pertinentes, configura valores e confirma sua relação com a disputa. Na reprodução de um edital real, pesos e tetos seguem o documento; não podem ser ajustados livremente mantendo o rótulo de “nota oficial”. Alterações exploratórias são simulações separadas.

## Elegibilidade e evidência

Antes da ordenação, verificar obrigações como função, disponibilidade e experiência mínima. Estados: `elegivel`, `inelegivel`, `pendente`. Informação ausente em requisito obrigatório produz pendência, não aprovação automática.

Um fato pode estar declarado, extraído, revisado ou rejeitado. Um documento pode estar ausente, pendente, aceito, rejeitado ou vencido. A aprovação interna demonstra que um responsável examinou a evidência; não significa autenticação junto ao emissor.

- **Pontos comprovados:** calculados com fatos revisados e documentos aceitos, válidos e aderentes à política aprovada.
- **Pontos potenciais pendentes:** dependem de comprovação ou esclarecimento. Não entram na nota comprovada nem autorizam recomendação final.
- **Não atende:** o dado contradiz a regra ou está fora da validade; sem pontos no cenário atual.

## Cálculo básico

Para um critério binário, `ponto_i = teto_i` se atendido e comprovado; caso contrário, zero. Para contagem, `ponto_i = min(teto_i, unidades_validas_distintas × pontos_por_unidade)`.

`score_comprovado = soma(ponto_i)`.

Quando há regras independentes e aditivas, `aderencia_percentual = 100 × score_comprovado / soma(tetos_ativos)`. Se o denominador for zero, apresentar “sem critérios pontuáveis”. Editais com grupos exclusivos, fórmula não aditiva ou teto global precisam de um cálculo específico validado; não reutilizar essa normalização automaticamente.

O cenário potencial deve ser recalculado sob todas as regras e tetos. Não somar pontos pendentes brutos quando existe exclusão ou limite compartilhado. Exibir os dois resultados com rótulos claros.

## Exemplo ilustrativo do case — uma função

| Critério | Regra | Teto |
|---|---|---:|
| C1 — PMP | Certificação comprovada e válida na data de corte | 10 |
| C2 — Energia | 2 pontos por projeto distinto comprovado | 8 |
| C3 — Pós-graduação | Formação concluída na área do objeto, com diploma aceito | 5 |
| C4 — Experiência | Mais de 10 anos, com períodos comprovados | 5 |
| Total | Soma aditiva para esta demonstração | **28** |

Essas regras são inspiradas na tabela ilustrativa do case. A exigência detalhada de comprovação e a função única são especificações da demonstração. A regra “mais de dez anos” usa limite estrito; exatamente dez não pontua.

Os arquivos em [exemplos](exemplos/criterios-demonstracao.json) usam a data de corte de 21/10/2026, uma vaga, disponibilidade obrigatória e pelo menos cinco anos comprovados. Todos os estados de comprovação desses dados são **simulados**, sem certificados reais associados.

| Perfil fictício | PMP | Energia | Pós | Mais de 10 anos | Comprovado | Pendente | Elegibilidade |
|---|---:|---:|---:|---:|---:|---:|---|
| Ana Exemplo | 10 | 8 | 5 | 5 | **28** | 0 | Elegível |
| Bruno Exemplo | 10 | 6 | 5 | 5 | **26** | 0 | Elegível |
| Carla Exemplo | 0 | 8 | 5 | 5 | **18** | 10 | Elegível; PMP pendente |
| Diego Exemplo | 10 | 8 | 5 | 5 | **28** | 0 | Inelegível; indisponível |

O ranking comprovado entre esses quatro recomenda Ana. Carla tem cenário potencial de 28, mas a pendência não muda sua posição comprovada. Diego não pode preencher a vaga, apesar dos 28 pontos.

## Datas, limites e duplicidades

- Congelar a data de corte e registrar como ela deriva do edital. Válido hoje pode estar vencido no prazo exigido pela disputa.
- Medir experiência pela união dos intervalos aceitos; projetos paralelos não duplicam tempo de carreira.
- Comparar datas com o aniversário de dez anos, considerando o calendário; não aproximar anos por 365 dias sem definir a convenção.
- Contar projetos por identidade validada. Nome diferente não prova que são projetos distintos.
- Definir se documentos podem apoiar vários critérios e se um mesmo projeto pode contar em várias funções. Isso depende da disputa.
- Regras eliminatórias não podem ser compensadas por pontos em outros itens.
- Empates do ranking profissional seguem política explícita da disputa. Para demo: maior cobertura comprovada, depois revisão humana; ordenação estável por ID serve só à visualização.

## Melhor indivíduo e melhor equipe

Escolher isoladamente o maior score de cada função pode gerar uma equipe inferior ou inválida. Exemplo independente da escala de 28 pontos:

| Profissional | Pontos como gerente | Pontos como especialista |
|---|---:|---:|
| A | 100 | 99 |
| B | 98 | 10 |

Se ninguém pode acumular funções, selecionar A primeiro como gerente leva a 110 pontos com B especialista. A combinação B gerente e A especialista alcança 197. Esse exemplo mostra por que o produto deve avaliar combinações quando há várias vagas.

### Modelo de seleção proposto

Definir variável binária `x[p,r]`, igual a 1 quando a pessoa `p` ocupa a função `r`.

- Preencher o número de vagas de cada função: `soma_p x[p,r] = vagas[r]`.
- Se acumulação for proibida: `soma_r x[p,r] <= 1`.
- Proibir alocações incompatíveis com elegibilidade, disponibilidade ou dedicação.
- Aplicar limites globais, de orçamento e de cobertura somente se previstos.
- No caso aditivo, maximizar `soma_p,r score[p,r] × x[p,r]`.

Para poucos perfis e papéis, avaliar combinações viáveis pode ser suficiente. Para escala ou regras complexas, estudar um solucionador de otimização. Uma busca com corte de candidatos ou heurística deve ser identificada como aproximação; não afirmar ótimo global. Quando não existir equipe viável, mostrar quais restrições impedem a montagem e as alternativas que exigem decisão humana.

## Relatório de execução

Preservar `run_id`, versão das regras, perfis e evidências, data de corte, função, score por critério, exclusões, pendências, restrições e responsável pela aprovação. Recalcular cria nova execução. O relatório de uma proposta já enviada deve continuar reconstituível.
