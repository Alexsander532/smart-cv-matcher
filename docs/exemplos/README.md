# Exemplos de demonstração

Os arquivos desta pasta especificam dados de um cenário fictício. Não são resultados de um sistema implementado e não contêm documentos autenticados.

- [criterios-demonstracao.json](criterios-demonstracao.json): uma função com uma vaga, quatro critérios aditivos, teto de 28 pontos e duas condições de elegibilidade.
- [perfis-demonstracao.json](perfis-demonstracao.json): 12 profissionais inteiramente sintéticos. A disponibilidade se refere ao período ilustrativo de 01/11/2026 a 30/04/2027. A data de corte do cálculo é 21/10/2026.
- [resultados-esperados.json](resultados-esperados.json): cálculo de referência para leitura e futura validação do motor de regras. Não registra execução de uma aplicação.

As referências `synthetic:...` identificam evidências simuladas. Não são caminhos de arquivos, certificados emitidos nem prova de autenticidade. Um estado `accepted` aqui existe apenas dentro da simulação. Para avaliar extração de texto e revisão documental, a equipe ainda precisará produzir os próprios currículos e comprovantes fictícios, claramente identificados como exemplos.

## Casos incluídos

| Perfil | Situação didática | Comprovado | Pendente adicional | Elegibilidade |
|---|---|---:|---:|---|
| P001 — Ana | Todos os critérios comprovados no cenário | 28 | 0 | Elegível |
| P002 — Bruno | Três projetos de energia | 26 | 0 | Elegível |
| P003 — Carla | PMP pendente de comprovação | 18 | 10 | Elegível |
| P004 — Diego | Score alto, mas indisponível | 28 | 0 | Inelegível |
| P005 — Elisa | Nove anos completos e sem pós | 12 | 0 | Elegível |
| P006 — Fabio | PMP vencido antes da data de corte | 14 | 0 | Elegível |
| P007 — Gabriela | Formação, sem PMP e sem projetos de energia | 5 | 0 | Elegível |
| P008 — Hugo | Exatamente dez anos; cinco projetos atingem o teto | 23 | 0 | Elegível |
| P009 — Iara | Pós pendente | 13 | 5 | Elegível |
| P010 — Joao | Não possui cinco anos para o obrigatório | 23 | 0 | Inelegível |
| P011 — Karen | Um projeto aceito e outro pendente | 17 | 2 | Elegível |
| P012 — Lucas | Comprovação do tempo de carreira pendente | 19 | 5 | Pendente |

Para uma vaga, a recomendação comprovada é P001. Não colocar P004 ou P010 entre elegíveis devido ao score. P012 não pode ser aprovado para a vaga enquanto sua condição obrigatória estiver pendente.

Os critérios não cobrem todas as formas de edital: a solução futura deve rejeitar configurações não suportadas ou exigir interpretação e configuração adicionais, em vez de anunciar suporte universal.
