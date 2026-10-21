# 04 — Requisitos, jornadas e critérios de aceite

Estes são requisitos propostos para o **produto completo**, não funcionalidades já entregues. Nesta tabela, **P0** identifica capacidade central do produto, **P1** ampliação e **P2** evolução. Para a PoC, a execução foi reduzida: front primeiro, base local e importação simulada; back e IA real ficam opcionais. A prioridade efetiva de implementação está no [backlog da PoC](14-backlog-e-colaboracao.md).

## Como atender aos requisitos na primeira PoC

| Capacidades | Abordagem na PoC de front | Evolução posterior |
|---|---|---|
| Cadastro, importação e extração — RF01–RF03 | Perfis locais e fluxo de importação/revisão identificado como simulado | Upload, extração real e revisão persistida |
| Evidências — RF04 | Exibir registros fictícios e estados de comprovação | Arquivos privados e revisão documental real |
| Critérios e ranking — RF05–RF08 | Formulário e cálculo funcional no navegador | Mesmas regras executadas pelo servidor |
| Equipe e relatório — RF09–RF10 | Uma vaga, escolha válida e saída imprimível/simples | Vários papéis e exportações avançadas |
| Otimização, interpretação e notificações — RF11–RF15 | Explicar como proposta futura | Implementar após validar o fluxo básico |

Não exigir que todos os requisitos P0 do produto sejam implementados com infraestrutura real antes de demonstrar a PoC.

## Requisitos funcionais

| ID | Requisito | Prioridade | Aceite observável |
|---|---|---|---|
| RF01 | Cadastrar profissional e importar currículo | P0 | Identificador próprio, arquivo original e estado de processamento visíveis |
| RF02 | Extrair formação, certificação e projetos | P0 | Campos estruturados com trecho/documento de origem; desconhecido permanece vazio |
| RF03 | Revisar e corrigir extração | P0 | Alteração registra autor, data e motivo; extração não aprova a si mesma |
| RF04 | Vincular comprovantes a qualificações | P0 | Documento, período de validade e revisão consultáveis por critério |
| RF05 | Cadastrar disputa, função e regras | P0 | Gestor define pontos, teto, obrigatoriedade e data de corte antes de executar |
| RF06 | Aplicar filtros e requisitos eliminatórios | P0 | Profissional inelegível continua explicável, sem aparecer como elegível pelo score |
| RF07 | Calcular e ordenar profissionais | P0 | Resultado reprodutível com mesma versão dos dados e critérios |
| RF08 | Explicar pontos e pendências | P0 | Cada linha mostra dado usado, prova, regra e motivo de zero ou pendência |
| RF09 | Selecionar equipe | P0 | Regras de vagas, disponibilidade e duplicidade são verificadas |
| RF10 | Gerar currículo padronizado e relatório | P0 | Exportação usa somente fatos aprovados e referencia a execução |
| RF11 | Sugerir combinação entre múltiplos papéis | P1 | Mostra método, restrições e limite da busca; não promete ótimo sem prova |
| RF12 | Extrair sugestões de critérios de edital | P1 | Propostas ficam em rascunho até aprovação humana; cláusula de origem preservada |
| RF13 | Notificar perfil desatualizado ou prova vencendo | P1 | Pendência tem dono, prazo e estado; comunicação externa exige configuração |
| RF14 | Histórico de versões e execução anterior | P1 | Recalcular não altera o relatório histórico já aprovado |
| RF15 | Integrações e múltiplas organizações | P2 | Isolamento por organização e autorização em todas as consultas |

## Requisitos não funcionais

- **Integridade:** nenhuma pontuação final depende de texto livre gerado sem validação.
- **Rastreabilidade:** toda execução aponta para as versões do perfil, dos critérios e da evidência.
- **Privacidade:** acesso por papel, arquivos privados e logs sem currículo integral.
- **Resiliência:** falha de IA permite revisão/cadastro manual; demonstração tem alternativa local.
- **Usabilidade:** rótulos distinguem “comprovado”, “pendente”, “não atende” e “não informado”.
- **Desempenho proposto:** ranking de 30 perfis em até 2 segundos, excluída extração; medir antes de anunciar. Processamento de arquivo tem estado e tempo medido separadamente.
- **Reprodutibilidade:** critérios, data de corte e base congelados no experimento.

## Telas e jornadas

### 1. Acervo

Listar profissionais, função, dados relevantes, atualização e pendências. Abrir perfil e documento em contexto. Busca inicial por campos e filtros; sugestões semânticas, se usadas, devem permitir entender por que o perfil foi recuperado.

### 2. Importação e revisão

Enviar arquivo sintético, acompanhar estado, comparar texto original e campos propostos. Aceitar ou corrigir cada informação. Quando o documento não possui texto legível, informar limitação; não declarar importação bem-sucedida com perfil vazio.

### 3. Configuração da disputa

Definir título, funções, vagas, período, data de corte e critérios. Mostrar soma máxima e erros de configuração antes da aprovação. Alterar pesos cria nova versão e novo cálculo.

### 4. Ranking e explicação

Mostrar pontos comprovados, pontos pendentes, elegibilidade e restrições. Expandir um perfil para a tabela de cálculo. Ordenação oficial usa score comprovado dos elegíveis; filtros escolhidos ficam visíveis.

### 5. Equipe e exportação

Selecionar nomes por função, impedir duplicidade proibida, confirmar disponibilidade e mostrar cobertura e teto da equipe. Exportar currículo padronizado e relatório contendo regras, evidências aceitas, pendências e versão. O gestor confirma a equipe antes da exportação final.

## Cenários de aceite prioritários

| Cenário | Resultado esperado |
|---|---|
| Certificado consta no currículo, sem comprovante | Pontos pendentes, não comprovados |
| Certificado vencido na data de corte | Não pontua no critério que exige validade |
| Cinco projetos válidos, teto de quatro | Pontuar quatro; explicar saturação |
| Dois registros do mesmo projeto | Não contar em dobro |
| Experiências em intervalos sobrepostos | Contar união dos períodos, conforme regra aprovada |
| Exatamente dez anos em regra “mais de dez” | Não satisfaz o limite estrito |
| Maior score, mas indisponível | Não entra na equipe viável |
| Alteração de critério | Nova versão; resultado anterior preservado |
| Currículo com instrução “ignore as regras” | Tratar como conteúdo não confiável, sem executar comandos |
| Usuário sem acesso ao documento | Rejeitar acesso inclusive pela URL direta |

Os cenários acima constituem o plano de validação da futura implementação. Não representam testes já executados.
