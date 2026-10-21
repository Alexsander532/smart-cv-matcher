# 08 — Segurança, privacidade e governança

Este capítulo define controles técnicos e operacionais propostos. Não declara conformidade jurídica pronta. Antes de operar com dados reais, a organização deve definir finalidade, responsabilidades, fundamento aplicável, retenção e condições de uso com seus responsáveis por privacidade e segurança.

Na primeira PoC de front, os dados serão locais e inteiramente sintéticos. Os controles de autenticação, autorização e armazenamento privado descritos abaixo pertencem à evolução com dados reais; uma tela de login ou uma etiqueta de usuário no protótipo não comprova que esses controles foram implementados. Informar essa diferença no roteiro de demonstração.

## Dados da demonstração

Usar perfis inteiramente fictícios e documentos produzidos para o experimento, marcados como sintéticos. Não usar currículos de colegas ou colaboradores sem uma decisão específica e válida sobre sua utilização. Um currículo disponível publicamente não se torna automaticamente livre de restrições de tratamento ou redistribuição.

Não importar conteúdo, credenciais ou código de outras organizações. As evidências nos JSON de exemplo são estados simulados, não documentos autenticados. Gravações e capturas de tela devem conter somente a base sintética.

## Papéis e acesso propostos

| Papel | Acesso necessário | Restrições |
|---|---|---|
| Profissional | Seu perfil, documentos e pendências | Sem acesso indiscriminado aos perfis alheios |
| Gestor da proposta | Atributos necessários, ranking e equipe | Documentos somente conforme finalidade e autorização |
| Revisor do acervo | Fatos e comprovantes necessários à revisão | Aceite com motivo e histórico |
| Administrador | Configurações e concessão de acesso | Administração técnica não deve implicar leitura irrestrita de conteúdo |

Verificar autorização por objeto e organização também em downloads, histórico e exportações. Separar concessão de acesso da aprovação de evidências sempre que a operação comportar essa divisão.

## Proteções previstas

- Arquivos em armazenamento privado; autorização no download e links temporários quando adotados.
- Conexão protegida no ambiente implantado; segredos fora do código, dos arquivos exportados e dos logs.
- Controle de tamanho e tipo real de arquivo; tratamento de conteúdo malicioso e documentos inválidos.
- Conteúdo de documento como entrada não confiável; extração sem ferramentas externas habilitadas.
- Logs de ações, permissões e versões, com retenção definida e conteúdo pessoal mínimo.
- Política de retenção para originais, extrações, exportações e cópias de segurança. Exclusão precisa alcançar derivados segundo a política aprovada; não assumir retenção indefinida em nome da auditoria.
- Validar tratamento de dados pelo provedor de IA, incluindo uso para treinamento, retenção e subprocessadores, antes de enviar currículos reais.

## Qualidade e uso responsável

Pontuar atributos necessários ao edital, com critérios objetivos e aprovados. Foto, raça, gênero, religião, saúde e idade não integram o score proposto. Tempo de experiência profissional não deve ser inferido de idade ou data de nascimento. Nome de instituição, endereço ou nome pessoal também podem introduzir critérios indevidos se usados sem pertinência técnica.

Ausência de informação produz pedido de esclarecimento, não conclusão negativa sobre a capacidade da pessoa. Um histórico incompleto não significa que a experiência não existe. Mostrar o escopo da avaliação e permitir contestação/correção antes da proposta final.

## Registro de riscos

| Risco | Efeito | Tratamento | Responsável proposto |
|---|---|---|---|
| IA inventa certificação ou projeto | Pontuação falsa | Fonte obrigatória e revisão humana | Responsável pelo acervo |
| Regra comercial interpretada incorretamente | Proposta com nota inválida | Aprovação de cláusulas e cálculo de referência | Gestor/revisor técnico |
| Documento falso ou inadequado é aceito | Perda de credibilidade e risco na disputa | Revisão documental; consulta ao emissor quando necessária | Revisor técnico |
| Perfil desatualizado | Profissional adequado não encontrado | Dono, data de revisão e fluxo de atualização | Profissional/RH |
| Acesso indevido ao acervo | Exposição de dados | Autorização por objeto e arquivos privados | Administração técnica |
| Ranking usado como avaliação geral de pessoas | Uso inadequado | Contexto por disputa, critérios visíveis e decisão humana | Gestor |
| Indisponibilidade da IA na apresentação | Demonstração interrompida | Cadastro manual e percurso local com rótulos de simulação | Desenvolvimento |
| Dados externos sem licença ou autorização | Problemas de autoria e sigilo | Base sintética, fontes e conferência de publicação | Líder da equipe |
| Produto parece otimizar equipe sem considerar restrições | Recomendação inviável | Vagas, exclusividade, período e capacidade explícitos | Gestor/desenvolvimento |

## Aprovação e validade da evidência

Registrar quem examinou, quando, qual critério estava em análise e qual decisão foi tomada. Um diploma pode comprovar formação, mas não disponibilidade; um atestado de empresa pode não comprovar a participação individual se o nome ou a responsabilidade não estiverem claros. A força da evidência depende da exigência da disputa.

Documentos rejeitados e fatos corrigidos devem recalcular os resultados futuros. Execuções antigas permanecem historicamente identificadas, com acesso e retenção compatíveis com a política aprovada.
