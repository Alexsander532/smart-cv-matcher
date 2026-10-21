# 11 — Rascunho do resumo e roteiro do pitch

**Material de apoio para revisão e autoria final da equipe.** Este Markdown não garante duas páginas. Diagramar no modelo oficial, incluir equipe e fontes e conferir a versão exportada. Não apresentar o texto como resultado de uma implementação ainda inexistente.

**Direção atual da PoC:** construir primeiro as telas com dados locais, cálculo no front e fluxo de importação simulado. Back e extração real são opcionais. No texto e no vídeo, apresentar a visão de produto como proposta e descrever o protótipo exatamente no nível implementado. A versão final precisa ser atualizada depois da revisão do grupo.

## Texto-base do resumo

### Smart CV Matcher: equipes aderentes, com pontuação rastreável

**Problema.** No Case B apresentado pela EloGroup, a Nexora precisa indicar equipes-chave para disputas comerciais, cuja nota técnica depende de formação, certificações e experiência comprovadas. Currículos dispersos, formatos diferentes, documentos ausentes e atualização irregular tornam essa atividade lenta e podem esconder profissionais aderentes. O case sugere cerca de 40 disputas anuais e 15 a 25 horas de esforço conjunto por disputa. Essa referência equivale a 600–1.000 horas-pessoa anuais, sob a hipótese de que o esforço já está agregado entre os gestores; os valores precisam de validação.

**Solução.** Propomos uma base central de profissionais e comprovantes, com importação assistida por IA e revisão humana. Cada informação fica associada ao arquivo e ao trecho de origem. O profissional confirma seu histórico, o responsável pelo acervo revisa os documentos e o gestor aprova os critérios da disputa. O sistema mantém datas de revisão, vencimentos e tarefas de atualização, com responsabilidades definidas.

**Lógica e diferenciais.** O gestor configura regras por função, incluindo pontos, limites, requisitos obrigatórios e data de corte. Um motor determinístico calcula a nota e explica cada parcela. A recomendação separa pontos comprovados de pendências; uma informação apenas declarada não recebe tratamento de prova aceita. Disponibilidade, quantidade de vagas e proibição de acumular funções condicionam a montagem da equipe. Em cenários com vários papéis, a combinação precisa ser comparada, pois escolher o melhor indivíduo em cada ranking isolado pode gerar resultado inferior. A saída reúne currículo padronizado, tabela de pontos e referências documentais, com aprovação humana antes do envio.

**Viabilidade e riscos.** A PoC priorizará uma interface web com perfis sintéticos locais e cálculo no navegador, apresentando a importação como simulação identificada. Se houver tempo, será acrescentado um back básico; a visão futura contempla banco relacional, arquivos privados e extração real por IA, sem treinamento de modelo próprio. Revisão, fontes obrigatórias e regras versionadas orientam o desenho. Autorização por papel, privacidade e segurança precisarão ser implementadas e validadas antes da operação com pessoas reais. A manutenção do acervo terá dono e rotina vinculada a projetos e certificações.

**Resultados previstos e validação.** Até esta versão, o trabalho compreende documentação, desenho da solução e dados ilustrativos. Não foram medidos resultados de um produto implementado. No cenário central, reduzir o esforço de 20 para 5 horas por disputa liberaria 600 horas-pessoa anuais, antes dos custos de migração, manutenção e operação. Essa redução de 75% é uma hipótese, a avaliar em comparação manual versus assistida com a mesma base e regras. Também serão medidos correção da pontuação, cobertura documental e fidelidade das informações. Ganho de nota técnica não implica garantia de vitória comercial.

### Parceria EloGroup e CEFET-MG — parágrafo proposto

A EloGroup apresenta uma atuação que integra tecnologia, analytics e gestão para transformar problemas de negócio em soluções digitais. Essa combinação oferece afinidade com competências do CEFET-MG em sistemas de informação, aprendizado de máquina e otimização, descritas na apresentação institucional do Laboratório de Sistemas Inteligentes. No desafio de gestão de currículos, essa aproximação pode conectar entendimento de processos comerciais, organização de dados, extração de informações e decisão apoiada por critérios verificáveis. A cooperação proposta permitiria aos estudantes investigar o problema, construir um protótipo com dados sintéticos e comparar seus resultados com um procedimento manual, enquanto a empresa contribuiria com esclarecimentos sobre requisitos e viabilidade operacional, por intermédio dos mentores. Como perspectivas futuras, destacam-se pesquisas sobre rastreabilidade de informações extraídas por IA, combinação de equipes sob restrições e mecanismos que incentivem a atualização do acervo. A continuidade dependeria de interesse das partes, avaliação dos resultados e formalização pelos canais institucionais competentes, respeitando confidencialidade e propriedade intelectual. Assim, o projeto articula formação acadêmica e experimentação aplicada sem pressupor contratação, implantação ou parceria comercial já estabelecida.

Base da pesquisa: [EloGroup — Sobre](https://elogroup.com/sobre-a-elogroup/) e [CEFET-MG — Laboratório de Sistemas Inteligentes](https://www.cdip.cefetmg.br/laboratorio-de-sistemas-inteligentes-lsi/). O conteúdo da página do LSI foi localizado na busca pública; sua abertura direta falhou nesta sessão. A equipe deve conferir a página antes da submissão. A existência dessas competências não significa que o laboratório participe deste projeto. O parágrafo acima contém 173 palavras nesta versão; manter entre 150 e 200 palavras após editar.

## Roteiro do vídeo — alvo de 4min40s

| Tempo | Conteúdo | Imagem ou evidência |
|---|---|---|
| 0:00–0:35 | Apresentar dor e esforço de referência | Arquivos dispersos e cálculo do esforço anual |
| 0:35–1:05 | Explicar produto e responsáveis | Fluxo de entrada, revisão, cálculo e atualização |
| 1:05–2:15 | Demonstrar critérios, ranking e comprovantes | Abrir perfil, trocar regra e explicar pontos |
| 2:15–3:05 | Mostrar equipe viável e documento padronizado | Disponibilidade, pendências e relatório |
| 3:05–3:45 | Mostrar medição obtida ou hipótese identificada | Comparação manual/assistida; custos omitidos explicitamente |
| 3:45–4:20 | Riscos, adoção e implantação | Revisão humana, dados sintéticos e base viva |
| 4:20–4:40 | Explicar contribuição da equipe e cooperação institucional | Próximo passo e fontes essenciais |

O cronograma deixa 20 segundos de margem para o limite. Ajustar ao material efetivamente disponível; não narrar métricas fictícias como experimento concluído.

## Demonstração sugerida

1. Abrir uma disputa fictícia com uma vaga e os quatro critérios de até 28 pontos.
2. Mostrar a origem de uma certificação, sua revisão e o efeito da data de corte.
3. Comparar Ana com Carla: Carla tem PMP declarado, mas pendente; somente Ana possui 28 pontos comprovados no cenário.
4. Mostrar que Diego tem score alto, mas está indisponível e não pode ocupar a vaga.
5. Alterar um critério em nova versão e recalcular, explicando que se trata de simulação quando divergir das regras da disputa.
6. Exportar currículo e quadro de evidências, ou mostrar um modelo identificado como protótipo se a exportação não estiver implementada.
7. Mostrar como uma pendência volta ao responsável para manter o cadastro atualizado.

## Revisão editorial antes da gravação

- [ ] Nome da equipe e integrantes preenchidos.
- [ ] Protótipo real e cenas simuladas identificados.
- [ ] Fontes, apoio de ferramentas e contribuição original registrados.
- [ ] Hipóteses substituídas ou mantidas explicitamente como hipóteses.
- [ ] Resumo efetivamente com duas páginas; vídeo efetivamente abaixo de cinco minutos.
- [ ] Parágrafo institucional com 150–200 palavras e fontes conferidas.
- [ ] Nenhuma alegação de economia líquida, vitória ou autenticidade automática sem evidência.
