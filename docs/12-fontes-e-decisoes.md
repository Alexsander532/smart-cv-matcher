# 12 — Fontes, decisões e pendências

## Fontes primárias preservadas

| ID | Fonte | Uso | Situação |
|---|---|---|---|
| F01 | [Regulamento PDF](../regulamento-hackatohm-2026.pdf), 11 páginas | Regras, datas e avaliação | Lido localmente; texto extraído com PDFKit |
| F02 | [Case B original](fontes/case-hackathon.html) | Contexto da Nexora, dados de referência, entregas específicas e rubrica do case | Copiado da segunda localização indicada pelo usuário |
| F03 | [Texto do case](fontes/case-texto.txt) | Busca e leitura acessível | Extração do HTML, sem estilos, scripts e imagens |
| F04 | [Texto do regulamento](fontes/regulamento-texto.txt) | Busca e referências a páginas | Extração automática com quebras e artefatos de ordem de leitura |

Origem de F01: arquivo `regulamento-hackatohm-2026.pdf` colocado pela equipe na raiz do repositório. O [endereço oficial informado](https://www.eng-eletrica.bh.cefetmg.br/wp-content/uploads/sites/191/2026/10/regulamento-hackatohm-2026.pdf) não abriu nesta sessão; a leitura definitiva usou a cópia local. Não afirmamos ter conferido se há revisão mais recente online.

Origem de F02: arquivo fornecido em `.../7A5706DD-A17D-4E3E-8072-2C14A7851C34/case-hackathon.html`, na pasta temporária do WhatsApp. A cópia de `docs/fontes` torna a leitura independente dessa pasta temporária. A versão anterior indicada pelo usuário continha o mesmo texto extraído, mas a última indicação foi adotada como referência.

Os hashes e tamanhos em [manifesto-fontes.json](fontes/manifesto-fontes.json) identificam os arquivos lidos. Textos extraídos servem como apoio; consultar os originais para ordem visual, nomes, tabelas e redação exata. Preservar fontes não equivale a conceder licença de publicação.

## Referências públicas complementares

- [Página oficial de acompanhamento](https://www.eng-eletrica.bh.cefetmg.br/hackatohm/acompanhe/): encontrada na busca pública em 21/10/2026; descreve etapas, submissão e mentores. Abertura direta indisponível. As regras nesta documentação foram conferidas no PDF local, não dependem dessa página.
- [EloGroup — Sobre](https://elogroup.com/sobre-a-elogroup/): página aberta em 21/10/2026; fundamenta a caracterização de tecnologia, analytics e gestão no parágrafo institucional.
- [CEFET-MG — Laboratório de Sistemas Inteligentes](https://www.cdip.cefetmg.br/laboratorio-de-sistemas-inteligentes-lsi/): resultado público localizado em 21/10/2026 descreve sistemas de informação, aprendizado de máquina, IA e otimização; abertura direta falhou. Conferir conteúdo integral antes da entrega final.

Não foram adotadas bibliotecas, serviços de banco ou provedores de IA nesta etapa. Seleções futuras devem incluir documentação oficial, versão, licença, finalidade e custo consultado. Não atribuir funcionalidades a ferramentas ainda não escolhidas.

## Decisões propostas

| ID | Decisão | Justificativa | Estado |
|---|---|---|---|
| D01 | Foco em equipe para disputa comercial | Pergunta central do case | Proposta para ratificação pela equipe |
| D02 | IA extrai; regra calcula; pessoa aprova | Rastreabilidade e controle de erro | Proposta |
| D03 | Pontos comprovados e pendentes separados | Currículo declarado não basta como comprovação | Proposta |
| D04 | Critérios e perfis versionados | Permitir explicação e reprodução histórica | Proposta |
| D05 | Base da demo inteiramente sintética | Atender ao case e facilitar demonstração sem exposição | Proposta |
| D06 | Aplicação simples com banco relacional | Viabilidade no prazo do hackathon | Proposta; stack pendente |
| D07 | Regulamento como referência inicial de avaliação | Documento geral prevê pesos iguais | Pendente de esclarecimento da divergência com case |
| D08 | Sem previsão numérica de vitória comercial | Não há histórico nem experimento que sustente causalidade | Proposta |
| D09 | Projeto e dados independentes de organizações empregadoras | Escopo pessoal deste hackathon | Diretriz da equipe nesta conversa |
| D10 | Priorizar PoC de front com dados simulados | Direção informada pelo usuário para esta entrega | Confirmada nesta conversa; detalhes para revisão do grupo |
| D11 | Acrescentar back básico somente após estabilizar o front | Preservar demonstração e prazo | Confirmada nesta conversa |
| D12 | Tratar documentação como base inicial de colaboração | Há outros integrantes que participarão das decisões e do desenvolvimento | Confirmada nesta conversa |

## Pendências priorizadas

| Prioridade | Pendência | Quem valida |
|---|---|---|
| Alta | Rubrica aplicável: pesos diferentes no case e no regulamento | Mentores e organização |
| Alta | Orientações de apoio de IA, autoria e condição de publicação dos materiais | Mentores e equipe |
| Alta | Canal de envio, modelo de resumo e acesso público/privado do vídeo | Organização |
| Alta | Responsáveis e confirmação da proposta | Integrantes |
| Alta | Esforço agregado ou por gestor e etapas incluídas nas horas do case | Mentores |
| Alta | Regras de evidência e restrições de equipe prioritárias | Mentores/revisor técnico |
| Média | Stack, provedor de IA e limites de consumo | Desenvolvimento |
| Média | Documentos sintéticos para o experimento de extração | Dados e equipe |
| Média | Medição manual versus assistida | Equipe |
| Média | Conteúdo integral da fonte institucional do LSI | Equipe |

## Registro de autoria e ferramentas

Esta documentação foi preparada com apoio do Codex/IA a pedido da equipe. O apoio incluiu leitura das fontes fornecidas, extração de texto, pesquisa pública complementar, organização dos documentos e elaboração de propostas e exemplos sintéticos. Os integrantes precisam revisar, corrigir, deliberar e registrar a contribuição original antes de submeter os materiais.

Os arts. 17–18 e 34 do regulamento tratam de ferramentas, responsabilidade, autoria e reconhecimento de fontes. Não há neste registro a afirmação de que o regulamento autorize irrestritamente qualquer uso de IA. Confirmar orientações específicas com os mentores e declarar o apoio conforme exigido.

| Integrante | Frente | Contribuição e período | Revisão |
|---|---|---|---|
| A preencher pela equipe | A definir | A registrar | Pendente |

| Data | Registro | Estado |
|---|---|---|
| 21/10/2026 | Leitura do case e do regulamento local; documentação e dados de exemplo | Realizado com apoio de IA |
| A preencher | Validação da equipe, esclarecimentos e alterações | Pendente |

## O que ainda não foi realizado

Não houve desenvolvimento da aplicação, entrevista com a Nexora/EloGroup, validação externa de documentos, medição de impacto, implantação, submissão oficial ou publicação a partir desta documentação. Nenhuma meta de desempenho aqui listada é resultado observado.
