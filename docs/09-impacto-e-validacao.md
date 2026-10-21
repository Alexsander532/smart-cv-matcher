# 09 — Impacto, métricas e validação

## O que pode ser afirmado hoje

Há um diagnóstico baseado no case, uma arquitetura proposta e regras de demonstração documentadas. Ainda não foram realizadas entrevistas, extrações com um produto implementado, testes de uso ou medições de desempenho. Todas as economias abaixo são cenários calculados, não resultados observados.

## Custo atual e economia potencial

Premissas do case: 40 disputas por ano e 15–25 horas de trabalho conjunto por disputa. Interpretamos inicialmente esse tempo como horas-pessoa agregadas dos gestores, a confirmar. Assim, a referência é de 600–1.000 horas-pessoa anuais.

Definir:

- `N`: quantidade de disputas por ano.
- `H_atual`: esforço de busca, revisão e montagem por disputa.
- `H_novo`: esforço das mesmas etapas com a solução, incluindo revisão humana.
- `C_hora`: custo médio por hora-pessoa, a obter com o responsável.
- `M`: esforço anual de manutenção da base e revisão fora das disputas.
- `I`: esforço inicial de migração, saneamento e treinamento.

`economia_bruta_horas = N × (H_atual − H_novo)`.

`economia_recorrente_horas = economia_bruta_horas − M`.

`capacidade_liberada_primeiro_ano = economia_bruta_horas − M − I`.

### Simulações com residual de 5 horas por disputa

| Cenário | Atual por disputa | Novo, hipótese | Atual anual | Novo anual | Economia bruta | Redução |
|---|---:|---:|---:|---:|---:|---:|
| Referência inferior | 15h | 5h | 600h | 200h | 400h | 66,7% |
| Referência central | 20h | 5h | 800h | 200h | 600h | 75% |
| Referência superior | 25h | 5h | 1.000h | 200h | 800h | 80% |

As cinco horas são uma hipótese de trabalho a medir. As reduções não incluem `M` nem `I`. Se `H_novo = 10h`, no cenário central a economia bruta cai para 400h/ano, ou 50%.

Com **R$ 100/h apenas como parâmetro ilustrativo**, as economias brutas correspondem a R$ 40 mil, R$ 60 mil e R$ 80 mil anuais em capacidade de trabalho. Isso não significa redução efetiva de desembolso, receita adicional ou economia líquida.

`valor_liquido_estimado = horas_liquidas × C_hora − custos_de_IA − infraestrutura − suporte − outros_custos`.

Não publicar ROI numérico sem medir migração e manutenção e levantar custos de implantação. Os custos de revisão documental podem ser relevantes para mil profissionais e não devem ser omitidos.

## Pontuação técnica e resultado comercial

Comparar uma equipe manual e uma equipe assistida sob **as mesmas regras e evidências disponíveis**. Exemplo sintético: passar de 26 para 28 pontos representa ganho absoluto de 2 pontos, sem extrapolar para uma disputa real.

Se a base for enriquecida com novos comprovantes entre as comparações, separar o efeito de melhoria do acervo do efeito do algoritmo. Ganho de pontuação não prova aumento de taxa de vitória: preço, concorrência e outros fatores interferem. Taxa de vitória é indicador futuro de acompanhamento, não promessa do MVP.

## Protocolo de experimento

**Recorte da PoC de front:** medir compreensão, funcionamento das regras e tempo da jornada de interface. Se a extração for simulada e a base estiver pré-carregada, esse experimento não mede o tempo real de ler currículos, validar documentos ou construir o acervo. A economia do processo completo continua uma hipótese. O protocolo abaixo pode ser aplicado integralmente quando essas etapas reais estiverem disponíveis, sempre diferenciando as partes implementadas das simuladas.

1. Preparar 10–30 perfis sintéticos com variedade de formato e qualidade. Os 12 perfis estruturados deste repositório são um ponto de partida; ainda faltam currículos e comprovantes sintéticos em arquivo para avaliar extração.
2. Fixar dois cenários de disputa, data de corte, critérios, disponibilidade e conjunto de evidências.
3. Construir referência manual revisada por dois integrantes e resolver discordâncias.
4. Medir o percurso manual: localizar, verificar, calcular, montar e formatar. Registrar início/fim por etapa e esforço por participante.
5. Medir o percurso assistido com a mesma cobertura de tarefas. Alternar ordem entre participantes/cenários para reduzir efeito de aprendizado.
6. Separar tempo de processamento, tempo de espera e esforço humano; todos importam de maneiras diferentes.
7. Avaliar pontos, violações de restrição, documentos faltantes e fidelidade da exportação.
8. Registrar falhas e intervenções. Com amostra pequena, apresentar tempos individuais, mediana e amplitude; não afirmar validade estatística ou generalização para mil pessoas.

## Indicadores

| Indicador | Cálculo | Meta inicial proposta |
|---|---|---|
| Esforço por disputa | Soma das horas-pessoa nas mesmas etapas | Medir redução; validar hipótese de 5h no cenário completo |
| Extração correta | Fatos corretos / fatos extraídos | A definir por tipo de campo após primeiro experimento |
| Cobertura da extração | Fatos corretos recuperados / fatos da referência | Medir por formação, certificação e experiência |
| Pontuação correta | Casos iguais à referência manual / casos avaliados | 100% nos critérios suportados |
| Afirmações sem suporte em perfil aprovado | Contagem de fatos sem evidência de origem | Zero |
| Equipes inviáveis recomendadas como finais | Contagem de violações de obrigatórios | Zero |
| Cobertura documental | Fatos pontuados com evidência aceita / fatos pontuados | 100% da nota comprovada |
| Atualização | Perfis confirmados no período / perfis ativos | Definir no piloto; não inventar adesão |
| Custo por documento | Consumo de IA e processamento / documentos tratados | Medir após escolher a infraestrutura |

## Registro de resultados

| Experimento | Base/versão | Manual | Assistido | Correção | Limitações |
|---|---|---|---|---|---|
| A executar | A definir | Não medido | Não medido | Não medido | Produto ainda em planejamento |

Atualizar esta tabela com os dados reais, preservando também as hipóteses originais para explicar desvios.
