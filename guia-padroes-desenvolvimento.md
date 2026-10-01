# Guia de Padrões de Desenvolvimento de Software
> **Objetivo:** estabelecer um padrão mínimo de engenharia para todos os
> projetos da empresa, aumentando legibilidade, segurança,
> previsibilidade, qualidade das entregas e facilidade de manutenção.
>
> **Palavras-chave:** **DEVE** = obrigatório; **NÃO DEVE** = proibido;
> **RECOMENDADO** = padrão preferencial, podendo haver exceção
> justificada.
### Como ler este guia
O conteúdo está organizado na sequência natural do trabalho de engenharia:
1. princípios e organização do código-fonte;
2. colaboração, versionamento e revisão;
3. tecnologias, arquitetura e implementação;
4. qualidade, entrega e operação;
5. documentação, governança e checklists.
As regras gerais se aplicam a todos os projetos. Regras específicas de uma
stack complementam este guia, mas não substituem suas exigências.
## 1. Princípios gerais
-   Código deve ser escrito pensando primeiro em **manutenção e
    leitura**, não apenas em "funcionar".
-   Toda alteração deve ser rastreável por tarefa/chamado quando existir
    ferramenta de gestão.
-   Mudanças relevantes devem passar por **Pull Request (PR)** e revisão
    antes de entrar na branch principal.
-   Credenciais, tokens, chaves e segredos **nunca** devem ser
    versionados.
-   Evitar soluções excessivamente complexas quando uma implementação
    simples atende ao requisito.
-   Novos projetos devem seguir os padrões deste documento desde o
    início. Projetos legados devem adotá-los progressivamente.
## 2. Nomenclatura de projetos e repositórios
Use nomes curtos e descritivos, em inglês quando possível, com palavras
unidas e iniciais maiúsculas (`PascalCase`).
**Recomendado**
``` text
CustomerApi
BillingService
AdminPortal
NotificationWorker
```
**Evitar**
``` text
projeto-novo
api_final_v2
teste-api
sistemaDoCliente
```
O nome não deve conter versão (`v2`, `new`, `final`) salvo quando isso
fizer parte formal da arquitetura/produto.
## 3. Git e estratégia de branches
### 3.1 Branches principais
Para a maioria dos times, recomenda-se um fluxo simples baseado em:
-   `main`: código estável/produção.
-   branches curtas: criadas para cada feature, correção ou tarefa.
### 3.2 Nome das branches
Formato:
``` text
<tipo>/<id-opcional>-<descricao-curta>
```
Tipos recomendados:
  Tipo          Uso
  ------------- ----------------------------------------
  `feature/`    Nova funcionalidade
  `fix/`        Correção de bug
  `hotfix/`     Correção urgente em produção
  `refactor/`   Refatoração sem mudança funcional
  `chore/`      Manutenção, configuração, dependências
  `docs/`       Documentação
  `test/`       Testes
Exemplos:
``` text
feature/123-user-registration
fix/456-invalid-token
refactor/payment-service
docs/api-authentication
```
Regras:
-   usar letras minúsculas;
-   não usar espaços, acentos ou caracteres especiais;
-   usar hífen para separar palavras;
-   preferir uma descrição curta e objetiva;
-   incluir o identificador da tarefa ou do chamado, quando existir.
### 3.3 Antes de começar
Atualize sua base:
``` bash
git checkout main
git pull origin main
git checkout -b feature/123-user-registration
```
Para branches longas, sincronize regularmente com a branch base conforme
a estratégia definida pelo time.
### 3.4 Commits
Commits devem ser pequenos, coerentes e representar uma mudança lógica.
Recomenda-se **Conventional Commits**:
``` text
<tipo>(<escopo-opcional>): <descricao>
```
Tipos comuns:
``` text
feat:     nova funcionalidade
fix:      correção
refactor: refatoração
docs:     documentação
test:     testes
chore:    manutenção/configuração
perf:     melhoria de performance
build:    build/dependências
```
Exemplos:
``` text
feat(auth): adicionando endpoint de atualizacao de token
fix(payment): lidando com transações duplicadas
refactor(user): validacao de formulario
docs(api): documentando endpoints de autenticacao
```
Evitar:
``` text
ajustes
fix
alteracoes
final
teste
agora vai
```
Não incluir senhas, tokens ou informações confidenciais em mensagens de
commit.
## 4. Pull Requests
Toda mudança relevante **DEVE** passar por PR antes do merge em `main`,
salvo procedimento emergencial formalmente definido.
Um PR deve:
-   possuir título claro;
-   explicar o que foi alterado e por quê;
-   referenciar a tarefa ou o chamado, quando aplicável;
-   informar como testar;
-   destacar migrations, variáveis de ambiente e impactos de implantação;
-   ser pequeno o suficiente para permitir uma revisão efetiva;
-   estar sem conflitos;
-   passar pelas verificações disponíveis de lint, build e testes
    automatizados.
Modelo:
``` md
## O que foi feito
Breve descrição.
## Motivo
Contexto/problema resolvido.
## Como testar
1. ...
2. ...
## Impactos
- [ ] Migration
- [ ] Nova variável de ambiente
- [ ] Nova rota/API
- [ ] Alteração incompatível (breaking change)
- [ ] Mudança de infraestrutura
## Evidências
Screenshots, logs ou exemplos quando aplicável.
```
### 4.1 Aprovação e merge
-   O autor **não deve ser o único aprovador** do próprio código.
-   Recomenda-se pelo menos **1 aprovação** de outro desenvolvedor.
-   Mudanças críticas (autenticação, pagamentos, permissões,
    infraestrutura, dados sensíveis) podem exigir 2 aprovações ou
    responsável técnico.
-   Conversas relevantes da revisão devem ser resolvidas antes do merge.
-   Recomenda-se proteger `main` contra push direto.
## 5. Checklist de Code Review
O reviewer não deve verificar apenas se "funciona". Deve avaliar
manutenção, segurança, testes e impacto.
### 5.1 Código
-   [ ] O código é legível e os nomes representam sua finalidade?
-   [ ] Funções/métodos possuem responsabilidade clara?
-   [ ] Funções excessivamente grandes foram divididas quando isso
    melhora clareza/testabilidade?
-   [ ] Há duplicação que deveria ser abstraída?
-   [ ] Há código morto, logs temporários ou comentários desnecessários?
-   [ ] O código complexo ou a API pública possui documentação suficiente?
-   [ ] Erros e exceções relevantes são tratados?
-   [ ] O código não "engole" exceções silenciosamente?
-   [ ] Recursos (arquivos, conexões, streams, transações) são
    encerrados corretamente?
-   [ ] Entradas externas são validadas?
-   [ ] Casos de borda foram considerados?
-   [ ] Código assíncrono/concorrente trata erros e cancelamentos
    adequadamente quando aplicável?
> **Importante:** não existe regra universal de "toda função precisa de
> try/catch". Exceções devem ser tratadas no nível que consegue
> recuperá-las, convertê-las ou adicionar contexto. Capturar exceções
> sem ação útil piora o código.
### 5.2 Tamanho de funções e arquivos
Não será adotado um número rígido de linhas como indicador automático de
qualidade. Entretanto, devem ser consideradas as seguintes referências:
-   funções acima de \~30--50 linhas devem ser reavaliadas;
-   funções com muitos níveis de `if/else`, loops aninhados ou várias
    responsabilidades devem ser divididas;
-   arquivos/classes muito grandes devem ser revisados quanto à
    responsabilidade única.
Exceções são aceitáveis quando a divisão tornaria o código menos claro.
### 5.3 Comentários
Comentários devem explicar principalmente **por que** algo existe, e não
repetir **o que** o código já diz.
Bom:
``` text
// Retry is limited to avoid duplicating payment requests
```
Ruim:
``` text
// Incrementa i
i++;
```
Código complexo deve preferencialmente ser simplificado antes de receber
comentários extensos.
### 5.4 Testes
-   [ ] Novas regras de negócio possuem testes adequados?
-   [ ] Correção de bug inclui teste que reproduz o problema quando
    viável?
-   [ ] Casos de sucesso e erro relevantes foram testados?
-   [ ] Não dependem desnecessariamente de serviços externos?
-   [ ] Cobertura não caiu de forma injustificada?
Cobertura é um indicador, não objetivo isolado. Priorizar regras de
negócio, fluxos críticos e casos de erro.
## 6. Stacks tecnológicas
### 6.1 Tecnologias core para novos projetos
Para o desenvolvimento de novas soluções e serviços, as equipes
**DEVEM** adotar as seguintes tecnologias:
-   **Backend principal:** **C# (.NET)**, para construção de serviços
    robustos, APIs de alta performance e sistemas de missão crítica.
-   **Frontend principal:** **React + TypeScript**, para interfaces
    dinâmicas, tipagem estática segura, componentização eficiente e alta
    manutenibilidade.
### 6.2 Ecossistema de suporte e legados
As tecnologias abaixo são homologadas para contextos específicos,
automações e cenários de transição:
-   **Sistemas legados:** **PHP**, restrito à manutenção, sustentação e
    refatoração gradual de plataformas preexistentes.
-   **Automações e scraping:** **Python**, linguagem oficial para scripts
    de automação, rotinas de extração de dados (scraping) e inteligência
    de dados.
### 6.3 Persistência de dados
O padrão oficial para bancos de dados relacionais ainda está em definição
pela governança de arquitetura. **SQL Server, MySQL e PostgreSQL** estão em
avaliação e ainda não constituem uma escolha homologada por este guia.
Enquanto não houver uma definição, a escolha deve ser justificada no projeto.
Soluções não relacionais devem ser avaliadas conforme o caso de uso e
documentadas por meio de decisão arquitetural.
### 6.4 Adoção de outras tecnologias
O uso de uma stack não listada neste documento **DEVE** ser precedido
por estudo técnico que avalie a tecnologia e justifique sua escolha.
## 7. Arquitetura
A arquitetura deve ser proporcional à complexidade do sistema.
### 7.1 Projetos pequenos e médios
Uma estrutura em camadas ou modular costuma ser suficiente:
``` text
src/
  modules/
    users/
      controller
      service
      repository
      model
      dto
  shared/
  config/
```
Responsabilidades:
-   **Controller/Handler:** trata HTTP ou outro mecanismo de transporte,
    realiza a validação superficial e chama o caso de uso.
-   **Service/Use Case:** concentra as regras de negócio.
-   **Repository:** realiza o acesso aos dados.
-   **DTO/Schema:** define os contratos de entrada e saída.
-   **Model/Entity:** representa o domínio e os dados.
### 7.2 Sistemas maiores
Quando a complexidade justificar, considerar:
-   Clean Architecture;
-   Hexagonal Architecture / Ports and Adapters;
-   Domain-Driven Design (DDD);
-   arquitetura orientada a eventos;
-   monólito modular antes de microsserviços, quando apropriado.
**Microsserviços não devem ser o padrão automático.** Devem ser adotados
quando houver justificativa de domínio, escala, autonomia das equipes,
implantação independente ou requisitos operacionais claros.
### 7.3 Dependências entre camadas
A regra de negócio não deve depender desnecessariamente de HTTP, framework,
banco de dados ou infraestrutura. Essa separação melhora a testabilidade e
reduz o acoplamento.
## 8. Backend e APIs
### 8.1 Rotas
Toda nova rota deve:
-   seguir o padrão REST/API adotado pelo projeto;
-   validar parâmetros e payloads;
-   retornar códigos HTTP coerentes;
-   possuir autenticação e autorização quando necessário;
-   evitar a exposição de informações internas;
-   estar documentada.
Exemplos:
``` text
GET    /users
GET    /users/{id}
POST   /users
PATCH  /users/{id}
DELETE /users/{id}
```
Evitar:
``` text
POST /getUsers
GET /deleteUser/123
```
### 8.2 Swagger / OpenAPI
Toda API HTTP nova ou alterada **DEVE** atualizar a especificação
OpenAPI/Swagger.
Documentar:
-   método e rota;
-   descrição;
-   parâmetros;
-   corpo da requisição;
-   respostas;
-   códigos de erro relevantes;
-   autenticação;
-   exemplos, quando úteis.
A documentação deve acompanhar o mesmo PR da implementação.
A interface Swagger e os endpoints de documentação OpenAPI **NÃO DEVEM**
ser disponibilizados em produção. Devem ficar habilitados apenas nos
ambientes de desenvolvimento e homologação.
### 8.3 Respostas e erros
Recomenda-se formato consistente:
``` json
{
  "code": "USER_NOT_FOUND",
  "message": "User not found",
  "traceId": "..."
}
```
Não retornar stack trace, SQL, caminhos internos, tokens ou detalhes
sensíveis ao cliente.
## 9. Segurança e configuração
### 9.1 Variáveis de ambiente
Segredos **NÃO DEVEM** estar no repositório.
Exemplos:
-   senhas de banco de dados;
-   chaves de API;
-   tokens;
-   chaves privadas;
-   segredos JWT/OAuth;
-   credenciais de serviços em nuvem.
Usar variáveis de ambiente ou serviço de secrets.
``` env
DATABASE_URL=...
JWT_SECRET=...
PAYMENT_API_KEY=...
```
O `.env` real deve estar no `.gitignore`.
Versionar somente um exemplo sem segredos:
``` text
.env.example
```
``` env
DATABASE_URL=
JWT_SECRET=
PAYMENT_API_KEY=
```
Se um segredo for commitado, **removê-lo do Git não é suficiente**: a
credencial deve ser considerada exposta e rotacionada/revogada.
### 9.2 Dados sensíveis
-   Nunca registrar senha, token ou segredo em logs.
-   Evitar logar payload completo contendo PII/dados sensíveis.
-   Validar e sanitizar entradas conforme o contexto.
-   Usar queries parametrizadas/ORM corretamente para evitar SQL
    Injection.
-   Não construir comandos de shell com entrada não confiável.
-   Aplicar autorização no backend; não confiar apenas no frontend.
-   Manter dependências atualizadas e monitorar vulnerabilidades.
## 10. Pacotes e dependências

Antes de adicionar uma biblioteca:

- verificar se a linguagem ou o framework já resolve o problema;
- verificar se há manutenção recente;
- analisar vulnerabilidades conhecidas;
- verificar a licença;
- avaliar tamanho e impacto;
- evitar bibliotecas diferentes para a mesma finalidade;
- preferir dependências consolidadas e amplamente mantidas.

Dependências devem possuir versão controlada por lockfile quando o ecossistema oferecer isso.

Exemplos comuns por finalidade (a escolha concreta depende da stack):

| Finalidade | Exemplos |
| --- | --- |
| Validação | Data Annotations/FluentValidation, Zod, Pydantic |
| Testes | xUnit/NUnit, Vitest, Pytest, PHPUnit |
| Documentação de API | OpenAPI, Swashbuckle, NSwag |
| Análise e lint | .NET Analyzers, ESLint, Ruff, PHPStan |
| Formatação | `dotnet format`, Prettier, Ruff/Black, PHP-CS-Fixer |
| Logs | Serilog/NLog, Pino, structlog, Monolog |
| Migrations | EF Core Migrations, Alembic, Doctrine Migrations |

A lista é referência, não autorização automática para adicionar pacotes.
## 11. Banco de dados
-   Mudanças de schema devem ser feitas por migrations versionadas.
-   Evitar alteração manual diretamente em produção.
-   Migrations devem considerar rollback ou estratégia de recuperação.
-   Criar índices com base em consultas reais e volume esperado.
-   Evitar N+1 queries.
-   Operações que alterem ou possam causar perda de dados devem
    usar transação: executar `COMMIT` somente após o sucesso de todas as
    etapas e `ROLLBACK` em caso de falha, evitando gravações parciais e
    inconsistência dos dados.
-   Não retornar colunas/dados desnecessários.
-   Alterações destrutivas devem possuir plano de migração de dados e
    compatibilidade.
## 12. Logs, observabilidade e erros
Logs devem ser úteis para a operação.
Recomenda-se o uso de logs estruturados com:
-   data e hora;
-   nível (`debug`, `info`, `warn`, `error`);
-   serviço ou módulo;
-   identificador de correlação ou rastreamento;
-   contexto necessário, sem dados sensíveis.
Não usar `console.log`/prints temporários como solução definitiva de
observabilidade.
Sistemas relevantes devem possuir, conforme a necessidade:
-   health checks;
-   métricas;
-   logs centralizados;
-   rastreamento distribuído;
-   alertas para falhas críticas.
## 13. Testes e qualidade
Pirâmide de testes recomendada:
1. muitos testes unitários para regras isoladas;
2. testes de integração para bancos de dados, filas, APIs e componentes;
3. poucos testes de ponta a ponta para os fluxos críticos.
O pipeline ideal deve executar, no mínimo:
``` text
install
lint
format-check
type-check (quando aplicável)
tests
build
security/dependency checks (quando disponíveis)
```
PR não deve ser aprovado quando verificações obrigatórias falham sem
justificativa formal.
## 14. Versionamento e releases
Quando houver releases de produto/biblioteca, recomenda-se **Semantic
Versioning**:
``` text
MAJOR.MINOR.PATCH
```
Exemplo:
``` text
2.4.1
```
-   `MAJOR`: alteração incompatível;
-   `MINOR`: funcionalidade compatível;
-   `PATCH`: correção compatível.
Para aplicações com deploy contínuo, tags/releases ainda podem ser
usadas para rastreabilidade.
## 15. Como estimar horas
Estimativa **não deve ser apenas tempo de codificação**. Deve incluir:
``` text
entendimento + implementação + testes + review + correções +
documentação + deploy/homologação + margem de risco
```
### 15.1 Processo sugerido
1.  Quebrar a tarefa em partes pequenas.
2.  Estimar cada parte.
3.  Identificar dependências e incertezas.
4.  Incluir testes e documentação.
5.  Incluir tempo provável de review/retrabalho.
6.  Aplicar margem proporcional ao risco.
Exemplo:
  Atividade                    Estimativa
  -------------------------- ------------
  Entendimento/refinamento            1 h
  Implementação backend               4 h
  Migration                           1 h
  Testes                              2 h
  Swagger/documentação              0,5 h
  Review e ajustes                  1,5 h
  Homologação/deploy                  1 h
  **Base**                       **11 h**
Se houver incerteza moderada, comunicar uma faixa, por exemplo **11--14
h**, em vez de esconder a incerteza em um número artificialmente
preciso.
### 15.2 O que aumenta a estimativa
-   requisito incompleto;
-   integração desconhecida;
-   sistema legado sem testes;
-   alteração de banco complexa;
-   dependência de terceiros;
-   regra de negócio nova;
-   necessidade de migration de dados;
-   requisitos de segurança;
-   múltiplos sistemas afetados.
Estimativa deve ser revisada quando surgirem informações novas. Ela é
ferramenta de planejamento, não promessa matemática.
## 16. Definition of Ready (antes de desenvolver)
Uma tarefa está pronta para desenvolvimento quando, conforme aplicável:
-   [ ] objetivo/problema está claro;
-   [ ] critérios de aceite existem;
-   [ ] dependências são conhecidas;
-   [ ] regras de negócio relevantes estão definidas;
-   [ ] design/contrato/API foi alinhado;
-   [ ] dúvidas que impedem implementação foram resolvidas;
-   [ ] tarefa possui tamanho razoável ou foi quebrada.
## 17. Definition of Done
Uma tarefa só é considerada concluída quando, conforme aplicável:
-   [ ] critérios de aceite atendidos;
-   [ ] código implementado;
-   [ ] testes adicionados/atualizados;
-   [ ] lint/build/testes passando;
-   [ ] PR revisado e aprovado;
-   [ ] comentários de review resolvidos;
-   [ ] Swagger/OpenAPI atualizado;
-   [ ] documentação técnica atualizada;
-   [ ] `.env.example` atualizado para novas configurações;
-   [ ] nenhuma credencial foi versionada;
-   [ ] migrations incluídas e validadas;
-   [ ] logs/monitoramento considerados;
-   [ ] homologação realizada;
-   [ ] deploy realizado ou pronto para pipeline, conforme fluxo da
    equipe.
## 18. README mínimo por projeto
Todo repositório deve possuir `README.md` contendo pelo menos:
``` md
# Nome do projeto
## Objetivo
## Requisitos
## Como executar localmente
## Configuração / variáveis de ambiente
## Banco de dados / migrations
## Como executar testes
## Como executar lint/build
## Arquitetura / estrutura de pastas
## API / Swagger
## Deploy
## Troubleshooting
```
## 19. Documentação técnica
Decisões arquiteturais relevantes devem ser registradas,
preferencialmente por meio de **ADR (Architecture Decision Record)**.
Exemplo:
``` text
docs/
  adr/
    0001-use-postgresql.md
    0002-adopt-event-queue.md
```
Um ADR deve registrar contexto, decisão, alternativas consideradas e
consequências.
## 20. Padrões de código
Cada stack deve possuir configuração automática compartilhada para
evitar discussões manuais de estilo.
Exemplos:
-   formatador;
-   linter;
-   `.editorconfig`;
-   verificador de tipos, quando aplicável;
-   hooks de pre-commit, opcionalmente;
-   execução das mesmas regras no pipeline de CI.
Adicionar `.editorconfig` aos projetos é recomendado.
Nomes devem ser claros. Evitar abreviações obscuras e variáveis como
`x`, `data2`, `objFinal` fora de contextos triviais.
## 21. Regras de compatibilidade e mudanças
Alterações incompatíveis devem ser explicitadas no PR.
Antes de remover ou alterar endpoints, campos de API, eventos, colunas,
configurações ou contratos consumidos por outros sistemas, é necessário
identificar os consumidores e planejar a migração ou descontinuação.
## 22. Checklist rápido do autor antes de abrir PR
-   [ ] Revisei meu próprio diff.
-   [ ] Removi código/logs temporários.
-   [ ] Não há credenciais ou dados sensíveis.
-   [ ] Testei o fluxo principal e erros relevantes.
-   [ ] Testes automatizados foram atualizados.
-   [ ] Lint, build e testes passam localmente.
-   [ ] Novas rotas estão no Swagger/OpenAPI.
-   [ ] Novas variáveis estão no `.env.example`.
-   [ ] Migrations estão incluídas quando necessárias.
-   [ ] README/documentação foi atualizada quando necessário.
-   [ ] PR explica o motivo, teste e impactos.
## 23. Checklist rápido do reviewer
-   [ ] Entendi o objetivo da mudança.
-   [ ] A solução atende aos critérios de aceite.
-   [ ] Código está legível e com responsabilidades claras.
-   [ ] Tratamento de erros está adequado.
-   [ ] Segurança/autorização/validação foram consideradas.
-   [ ] Testes cobrem riscos importantes.
-   [ ] API e documentação estão sincronizadas.
-   [ ] Banco/migrations são seguros.
-   [ ] Não há segredos ou dados sensíveis.
-   [ ] Não há impacto incompatível não documentado.
-   [ ] A solução não adiciona complexidade desnecessária.
## 24. Exceções ao padrão
Este guia não deve impedir decisões técnicas justificadas. Quando uma regra
não fizer sentido:
1. documentar a exceção no PR ou ADR;
2. explicar o motivo;
3. avaliar os riscos;
4. obter aprovação técnica quando o impacto for relevante.
O padrão deve evoluir com a equipe. Sugere-se revisão periódica (por
exemplo, trimestral ou semestral) para remover regras obsoletas e
incorporar aprendizados.
------------------------------------------------------------------------
## Resumo do fluxo esperado
``` text
Tarefa refinada
    ↓
Criar branch
    ↓
Desenvolver em commits pequenos
    ↓
Testar + lint + build
    ↓
Atualizar documentação/Swagger
    ↓
Auto-review do diff
    ↓
Abrir Pull Request
    ↓
Code Review por outro desenvolvedor
    ↓
Ajustes
    ↓
Aprovação
    ↓
Merge
    ↓
Homologação/Deploy
    ↓
Monitoramento
```
**Versão do padrão:** 0.2\
**Responsável:** Equipe de Desenvolvimento\
**Status:** Documento base --- em construção...\
**Revisado por:** Anderson Santos