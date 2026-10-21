# 02 — Case B e diagnóstico

Fonte primária: [case EloGroup, parte 02](fontes/case-hackathon.html). Nexora é a organização apresentada no cenário; os números do case não devem ser atribuídos automaticamente à operação real da EloGroup.

## Problema a resolver

A Nexora possui mais de mil profissionais e participa de licitações e concorrências privadas. A nota técnica de uma proposta depende da equipe-chave indicada: formação, certificações e experiências específicas podem pontuar, desde que comprovadas.

A pergunta central é encontrar os profissionais mais aderentes a cada disputa, apresentar sua experiência de maneira fiel e manter as informações atualizadas. Organizar arquivos é uma etapa desse processo; a entrega de valor é uma equipe viável, bem documentada e com pontuação calculável.

| Dor observada no case | Causa provável a investigar | Efeito | Resposta proposta |
|---|---|---|---|
| Arquivos dispersos | Ausência de cadastro central e responsável | Tempo de busca e perda de versões | Importação e perfil único |
| Busca manual | Dados em texto livre | Perfis aderentes ficam esquecidos | Filtros estruturados e busca assistida |
| Falta de padrão | Currículos montados individualmente | Experiência relevante fica escondida | Perfil canônico e currículo por modelo |
| Comprovantes perdidos | Qualificação não ligada à evidência | Pontuação não defensável | Vínculo entre fato, fonte e documento |
| Base envelhecida | Atualização sem rotina | Certificados vencidos e projetos ausentes | Responsáveis, lembretes e eventos de atualização |

As causas acima são hipóteses de diagnóstico, não entrevistas já realizadas.

## Quantificação inicial

O case fornece cerca de 40 disputas anuais e 15 a 25 horas de esforço **conjunto** de 2 a 3 gestores por disputa. A leitura inicial é de horas-pessoa agregadas, evitando multiplicar outra vez pelo número de gestores:

`40 × (15 a 25) = 600 a 1.000 horas-pessoa/ano`.

Confirmar com os mentores se a intenção era esforço agregado ou tempo de calendário por gestor. O case pede validação ou ajuste desses valores. Não há dados suficientes para estimar receita perdida ou probabilidade de vitória.

Além do tempo, medir perda de pontuação por: profissional não encontrado; comprovante ausente; currículo incompleto; certificado vencido; indisponibilidade descoberta tarde. Registrar cada ocorrência sem presumir que teria mudado o vencedor da disputa.

## Usuários e responsabilidades

| Papel | Necessidade | Responsabilidade operacional |
|---|---|---|
| Profissional | Conferir seu histórico e reaproveitar dados | Corrigir perfil e anexar documentos |
| Responsável pelo acervo/RH | Qualidade da base | Deduplicar, revisar e cobrar atualização |
| Gestor comercial | Montar proposta sob prazo | Aprovar critérios, composição e exportação |
| Revisor técnico | Interpretar exigências e provas | Validar aderência documental |
| Administrador | Operação e acessos | Gerenciar permissões e configurações |

Uma mesma pessoa pode exercer mais de um papel, mas o sistema deve registrar em qual função aprovou cada ação.

## Perguntas prioritárias aos mentores

1. Os tempos informados são esforço agregado? Incluem validação de comprovantes e montagem da proposta?
2. Quais funções e restrições de equipe são mais frequentes? É permitida acumulação de papéis?
3. Há exigência de vínculo, disponibilidade, dedicação exclusiva ou anuência dos profissionais?
4. Como se comprovam experiências, duração, responsabilidade e setor de atuação?
5. Existem limites globais, critérios eliminatórios e vedação de reutilizar um mesmo projeto em vários itens?
6. Quem validaria evidências e resolveria informações contraditórias?
7. Quais dados fictícios demonstram melhor o problema sem reproduzir pessoas reais?
8. Qual rubrica final e quais orientações de uso de IA e publicação valem para o desafio?

## Critério de sucesso do diagnóstico

Explicar de onde vieram cada número e cada restrição, obter uma referência manual de comparação e documentar as lacunas. A qualidade da proposta depende de reconhecer o que ainda não foi medido.
