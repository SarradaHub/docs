# Priorização de Testes - Saturday League Football

## Resumo Executivo

**Documento:** Finalizado (Janeiro 2026)

**Status Atual**: ✅ Testes estáveis e passando
- **Testes executando com sucesso**: 565 exemplos, 0 pendentes ✅
- **Cobertura de linhas**: 87.55% ✅ (meta de 80% atingida)
- **Cobertura de branches**: 70.03% ✅ (meta de 70% atingida)

**Fase Atual**: Fase 1-5 completas ✅
- ✅ **Fase 1**: 100% completo ✅ (fundação completa)
- ✅ **Fase 2**: 100% completo ✅ (CRUD básico completo)
- ✅ **Fase 3**: 100% completo ✅ (funcionalidades complexas completas)
- ✅ **Fase 4**: 100% completo ✅ (lógica de negócio completa)
- ✅ **Fase 5**: 100% completo ✅ (melhorias e edge cases)

**Melhorias Recentes**:
- ✅ **Teste pendente resolvido**: `Matches::Finalize` "when team is blank" — implementado com stub de `team_1`/`team_1_id` nil para exercitar o ramo `calculate_goals_for(team.blank?)` (modelo exige times).
- ✅ **Fase 5 Completada**: RecordNotFound em ChampionshipsController, fallback base_relation em ApplicationController, branches em presenters e MatchPresenter, correções em specs (FindQuery, rounds, api_flow GET).
- ✅ **Fase 4 Completada**: Todos os componentes de lógica de negócio agora têm testes completos.
- ✅ Correção do cálculo de own goals em `Matches::Finalize`.
- ✅ Remoção de todos os warnings de depreciação (`:unprocessable_entity` → `:unprocessable_content`).

## Situação Atual

- **Cobertura Total**: 87.55% (1111 de 1269 linhas cobertas) ✅
- **Cobertura de Branches**: 70.03% (236 de 337 branches cobertas) ✅
- **Status dos Testes**: 565 exemplos, 0 falhas, 0 pendentes ✅
- **Última Atualização**: Janeiro 2026

## Critérios de Priorização

1. **Impacto no Negócio**: Funcionalidades core do sistema
2. **Cobertura Atual**: Arquivos com 0% de cobertura têm prioridade máxima
3. **Complexidade**: Arquivos com mais linhas não cobertas
4. **Dependências**: Controllers que dependem de services/queries não testados

---

## PRIORIDADE ALTA - Controllers Críticos

### 1. MatchesController ✅ TESTADO

**Status**: Testes implementados e passando
**Últimas correções**:
- Teste de finalização com own goals corrigido
- Warnings de `:unprocessable_entity` corrigidos (substituído por `:unprocessable_content`)

**Arquivo**: `app/controllers/api/v1/matches_controller.rb`
**Funcionalidades**:

- `index` - Listar partidas (com filtro por round_id)
- `show` - Detalhes de uma partida
- `create` - Criar nova partida
- `update` - Atualizar partida
- `destroy` - Deletar partida
- `finalize` - Finalizar partida (calcula gols e vencedor)

**Impacto**: CRUD completo de partidas + lógica de finalização (crítica)
**Dependências**: Usa `Matches::CollectionQuery`, `Matches::FindQuery`, `Matches::Finalize`

### 2. PlayerStatsController ✅ TESTADO

**Status**: Testes implementados e passando
**Arquivo**: `app/controllers/api/v1/player_stats_controller.rb`
**Funcionalidades**:

- `index` - Listar estatísticas de jogadores (com includes e sparse fieldsets)
- `show` - Detalhes de uma estatística
- `create` - Criar estatística
- `update` - Atualizar estatística
- `destroy` - Deletar estatística
- `by_match` - Estatísticas por partida (com includes e sparse fieldsets)
- `bulk_update` - Atualização em massa (crítica para entrada de dados)

**Impacto**: CRUD completo + operações em massa
**Dependências**: Usa `PlayerStats::CollectionQuery`, `PlayerStats::BulkUpsert`

### 3. PlayersController ✅ TESTADO

**Status**: Testes implementados e passando
**Últimas correções**:
- Teste de listagem com paginação corrigido
- Warnings de `:unprocessable_entity` corrigidos

**Arquivo**: `app/controllers/api/v1/players_controller.rb`
**Funcionalidades**:

- `index` - Listar jogadores (com filtro por championship_id)
- `show` - Detalhes de um jogador
- `create` - Criar jogador
- `update` - Atualizar jogador
- `destroy` - Deletar jogador
- `add_to_round` - Adicionar jogador a uma rodada
- `add_to_team` - Adicionar jogador a um time
- `match_stats` - Estatísticas do jogador em uma partida

**Impacto**: CRUD completo + operações de associação
**Dependências**: Usa `Players::CollectionQuery`, `Players::AddToRound`, `Players::AddToTeam`, `Players::MatchStatistics`

### 4. RoundsController ✅ TESTADO

**Arquivo**: `app/controllers/api/v1/rounds_controller.rb`
**Funcionalidades**:

- `index` - Listar rodadas
- `show` - Detalhes de uma rodada
- `create` - Criar rodada
- `update` - Atualizar rodada
- `destroy` - Deletar rodada
- `statistics` - Estatísticas da rodada

**Impacto**: CRUD completo + relatórios
**Dependências**: Usa `Rounds::CollectionQuery`, `Rounds::FindQuery`, `RoundStatistics`

### 5. TeamsController ✅ TESTADO

**Arquivo**: `app/controllers/api/v1/teams_controller.rb`
**Funcionalidades**:

- `index` - Listar times (com filtro por round_id)
- `show` - Detalhes de um time
- `create` - Criar time
- `update` - Atualizar time
- `destroy` - Deletar time

**Impacto**: CRUD completo de times
**Dependências**: Usa `Teams::CollectionQuery`, `Teams::FindQuery`

### 6. BaseController ✅ TESTADO

**Arquivo**: `app/controllers/api/v1/base_controller.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- `render_collection` - Método helper para renderizar coleções paginadas
- Lógica de serialização e sparse fieldsets
- Testes cobrem diferentes combinações de serializer_class, presenter_class, e edge cases

**Impacto**: Base para todos os controllers da API
**Dependências**: Usa concerns `Paginatable`, `SparseFieldsets`, `Includable`

### 7. HealthController ✅ TESTADO

**Arquivo**: `app/controllers/health_controller.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- `health` - Health check básico
- `ready` - Readiness check
- Testes cobrem casos de sucesso e falha de conexão com banco de dados

**Impacto**: Endpoints de monitoramento (importante para produção)

---

## PRIORIDADE ALTA - Services Críticos

### 8. Matches::Finalize ✅ TESTADO

**Arquivo**: `app/services/matches/finalize.rb`
**Status**: Testes implementados e passando (incl. edge case "team blank")
**Funcionalidades**:

- Calcula gols de cada time baseado em PlayerStats
- Determina vencedor ou empate
- Atualiza match com resultado
- **Correção**: Agora lida corretamente com own goals (inclui own goals do oponente no cálculo)
- **Edge case**: `calculate_goals_for(team.blank?)` coberto via stub de `team_1` nil no spec (modelo exige times)

**Impacto**: Lógica de negócio crítica para finalização de partidas
**Usado por**: `MatchesController#finalize`

### 9. PlayerStats::BulkUpsert ✅ TESTADO

**Arquivo**: `app/services/player_stats/bulk_upsert.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- Valida regras de assistências (não pode ter assistência em gol contra)
- Valida que assistências não excedam gols
- Faz upsert em massa de estatísticas

**Impacto**: Validações críticas de integridade de dados
**Usado por**: `PlayerStatsController#bulk_update`

### 10. Players::AddToRound ✅ TESTADO

**Arquivo**: `app/services/players/add_to_round.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- Adiciona jogador a uma rodada
- Evita duplicatas

**Impacto**: Operação de associação importante
**Usado por**: `PlayersController#add_to_round`

### 11. Players::AddToTeam ✅ TESTADO

**Arquivo**: `app/services/players/add_to_team.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- Adiciona jogador a um time
- Evita duplicatas

**Impacto**: Operação de associação importante
**Usado por**: `PlayersController#add_to_team`

### 12. RoundStatistics ✅ TESTADO

**Arquivo**: `app/services/round_statistics.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- Calcula estatísticas agregadas por jogador em uma rodada
- Inclui gols, assistências, vitórias, derrotas, empates, matches jogados, goalkeeper_count

**Impacto**: Relatórios e estatísticas importantes
**Usado por**: `RoundsController#statistics`

### 13. Players::MatchStatistics ✅ TESTADO

**Arquivo**: `app/services/players/match_statistics.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- Agrega estatísticas do jogador em uma partida específica (goals, assists, own_goals)
- Conta total de matches do time na rodada

**Impacto**: Estatísticas importantes para relatórios de jogadores
**Usado por**: `PlayersController#match_stats`

### 14. RoundTeamGenerator ✅ TESTADO

**Arquivo**: `app/services/round_team_generator.rb`
**Status**: Testes implementados e passando
**Funcionalidades**:

- Auto-balanceamento automático de times em uma rodada
- Distribui jogadores entre times respeitando limites min/max
- Cria times adicionais quando necessário
- Mantém distribuição equilibrada

**Impacto**: Funcionalidade crítica para organização automática de times
**Usado por**: Auto-executado via `after_commit` em `PlayerRound`

---

## PRIORIDADE MÉDIA - Queries

### 15. Matches::CollectionQuery ✅ TESTADO

**Arquivo**: `app/queries/matches/collection_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `MatchesController#index`

### 16. Matches::FindQuery ✅ TESTADO

**Arquivo**: `app/queries/matches/find_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `MatchesController#show`

### 17. PlayerStats::CollectionQuery ✅ TESTADO

**Arquivo**: `app/queries/player_stats/collection_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `PlayerStatsController#index`, `PlayerStatsController#by_match`

### 18. Players::CollectionQuery ✅ TESTADO

**Arquivo**: `app/queries/players/collection_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `PlayersController#index`

### 19. Rounds::CollectionQuery ✅ TESTADO

**Arquivo**: `app/queries/rounds/collection_query.rb`
**Usado por**: `RoundsController#index`

### 20. Rounds::FindQuery ✅ TESTADO

**Arquivo**: `app/queries/rounds/find_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `RoundsController#show`

### 21. Teams::CollectionQuery ✅ TESTADO

**Arquivo**: `app/queries/teams/collection_query.rb`
**Usado por**: `TeamsController#index`

### 22. Teams::FindQuery ✅ TESTADO

**Arquivo**: `app/queries/teams/find_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `TeamsController#show`

### 23. Championships::FindQuery ✅ TESTADO

**Arquivo**: `app/queries/championships/find_query.rb`
**Status**: Testes implementados e passando
**Usado por**: `ChampionshipsController#show` (já tem 82% de cobertura)

---

## PRIORIDADE MÉDIA - Presenters e Serializers

### 24. RoundPresenter ✅ TESTADO

**Arquivo**: `app/presenters/round_presenter.rb`
**Usado por**: `RoundsController`

### 25. TeamPresenter ✅ TESTADO

**Arquivo**: `app/presenters/team_presenter.rb`
**Usado por**: `TeamsController`

### 26. PlayerStatSerializer ✅ TESTADO

**Arquivo**: `app/serializers/player_stat_serializer.rb`
**Status**: Testes implementados e passando
**Usado por**: `PlayerStatsController`

### 27. PlayerSerializer ✅ TESTADO

**Arquivo**: `app/serializers/player_serializer.rb`
**Status**: Testes implementados e passando
**Usado por**: `PlayersController`

### 28. RoundSerializer ✅ TESTADO

**Arquivo**: `app/serializers/round_serializer.rb`
**Status**: Testes implementados e passando

### 29. TeamSerializer ✅ TESTADO

**Arquivo**: `app/serializers/team_serializer.rb`
**Status**: Testes implementados e passando

---

## PRIORIDADE MÉDIA - Concerns e Helpers

### 30. IdentityAuthentication ✅ TESTADO

**Arquivo**: `app/controllers/concerns/identity_authentication.rb`
**Status**: Testes implementados e passando
**Funcionalidades**: Autenticação via token Bearer, validação de token, renderização de erros
**Impacto**: Segurança da API
**Usado por**: Todos os controllers da API via `BaseController`

### 31. Includable ✅ MELHORADO

**Arquivo**: `app/controllers/concerns/includable.rb`
**Status**: Cobertura de branches significativamente melhorada
**Testes adicionados**: Includes aninhados, arrays vazios, casos de edge, múltiplos níveis de nesting

### 32. Paginatable ✅ MELHORADO

**Arquivo**: `app/controllers/concerns/paginatable.rb`
**Status**: Cobertura de branches significativamente melhorada
**Testes adicionados**: Total zero, divisões decimais, edge cases de paginação, diferentes tipos de entrada

### 33. SparseFieldsets ✅ MELHORADO

**Arquivo**: `app/controllers/concerns/sparse_fieldsets.rb`
**Status**: Cobertura de branches significativamente melhorada
**Testes adicionados**: Arrays vazios, hashes vazios, campos inexistentes, arrays aninhados, casos de edge

---

## PRIORIDADE BAIXA - Melhorias em Código Parcialmente Testado

### 34. ChampionshipsController ✅ MELHORADO

**Arquivo**: `app/controllers/api/v1/championships_controller.rb`
**Status**: Teste de `RecordNotFound` em `#show` adicionado; branches revisados.

### 35. ApplicationController ✅ MELHORADO

**Arquivo**: `app/controllers/api/v1/application_controller.rb`
**Status**: Teste do fallback `base_relation` e branches de `render_collection` adicionados.

### 36. Championships::CollectionQuery ✅ TESTADO

**Arquivo**: `app/queries/championships/collection_query.rb`
**Status**: Testes implementados e passando. `CollectionQuery` com spec próprio completo; `FindQuery` spec usa `includes: []` ao chamar `CollectionQuery`.
**Usado por**: `ChampionshipsController#index`

### 37. MatchPresenter ✅ MELHORADO

**Arquivo**: `app/presenters/match_presenter.rb`
**Status**: Testes para `hash_to_player_array` count 0, `team_1_players`/`team_2_players`, e `draw` adicionados.

### 38. ChampionshipPresenter ✅ MELHORADO

**Arquivo**: `app/presenters/championship_presenter.rb`
**Status**: Testes para "rounds mas sem players" e "um round e um player" adicionados.

### 39. PlayerPresenter ✅ MELHORADO

**Arquivo**: `app/presenters/player_presenter.rb`
**Status**: Testes para `serialized_rounds` e `serialized_stats` vazios vs preenchidos adicionados.

---

## PRIORIDADE BAIXA - Bibliotecas e Utilitários

**Fora do escopo** da priorização atual. Itens opcionais para expansão futura do escopo.

### 40. CircuitBreakerService (0% - 45 linhas)

**Arquivo**: `lib/circuit_breaker_service.rb`
**Impacto**: Baixo (infraestrutura)

### 41. ConsulService (0% - 55 linhas)

**Arquivo**: `lib/consul_service.rb`
**Impacto**: Baixo (infraestrutura)

### 42. IdentityServiceClient (0% - 50 linhas)

**Arquivo**: `lib/identity_service_client.rb`
**Impacto**: Médio (usado por autenticação, mas pode ser mockado em testes)

---

## Status Atual das Fases

**Fase Atual**: Fase 1-5 completas ✅

### ✅ Fase 1 - Fundação (Sprint 1-2) - ✅ COMPLETA

- ✅ `BaseController` - **TESTADO** ✅ (cobertura completa de render_collection e edge cases)
- ✅ Concerns - **MELHORADOS** ✅ (IdentityAuthentication, Includable, Paginatable, SparseFieldsets com cobertura abrangente)
- ✅ `HealthController` - **TESTADO** ✅ (health e ready com casos de sucesso e falha)
- ✅ Queries `FindQuery` - **TESTADAS** ✅ (Matches, Rounds, Teams, Championships)

**Progresso**: 100% completo ✅

### ✅ Fase 2 - CRUD Básico (Sprint 3-4) - ✅ COMPLETA

- ✅ `MatchesController` - **TESTADO** ✅
- ✅ `TeamsController` - **TESTADO** ✅ (inclui testes de includes e sparse fieldsets)
- ✅ `RoundsController` - **TESTADO** ✅ (inclui testes de includes)
- ✅ Queries de coleção - **TESTADAS** ✅ (`Teams::CollectionQuery`, `Rounds::CollectionQuery`)
- ✅ Presenters básicos - **TESTADOS** ✅ (`TeamPresenter`, `RoundPresenter`)

**Progresso**: 100% completo ✅

**Detalhes da Implementação**:
- Criados 4 novos arquivos de teste (2 queries, 2 presenters)
- Adicionados testes para includes e sparse fieldsets em controllers
- Corrigido `SparseFieldsets` concern para lidar com `ActionController::Parameters`

### ✅ Fase 3 - Funcionalidades Complexas (Sprint 5-6) - ✅ COMPLETA

- ✅ `PlayersController` - **TESTADO** ✅
- ✅ `PlayerStatsController` - **TESTADO** ✅ (inclui testes de includes e sparse fieldsets)
- ✅ `Players::AddToRound` - **TESTADO** ✅
- ✅ `Players::AddToTeam` - **TESTADO** ✅
- ✅ `PlayerStats::BulkUpsert` - **TESTADO** ✅
- ✅ `PlayerStats::CollectionQuery` - **TESTADO** ✅

**Progresso**: 100% completo ✅

**Detalhes da Implementação**:
- Criado arquivo de teste para `PlayerStats::CollectionQuery` com cobertura completa
- Adicionados testes de includes e sparse fieldsets no `PlayerStatsController`

### ✅ Fase 4 - Lógica de Negócio (Sprint 7-8) - ✅ COMPLETA

- ✅ `Matches::Finalize` - **TESTADO** ✅
- ✅ `RoundStatistics` - **TESTADO** ✅
- ✅ `Players::MatchStatistics` - **TESTADO** ✅

**Progresso**: 100% completo ✅

**Detalhes da Implementação**:
- `RoundStatistics`: Testes completos já existiam, validados como adequados
- `Players::MatchStatistics`: Testes expandidos cobrindo todos os edge cases (player sem stats, não no time, múltiplos matches, own goals, etc.)

### ✅ Fase 5 - Melhorias e Edge Cases (Sprint 9+) - COMPLETA

**Queries Restantes**:
- ✅ `Players::CollectionQuery` - **TESTADO** ✅
- ✅ `Matches::CollectionQuery` - **TESTADO** ✅

**Serializers**:
- ✅ `PlayerStatSerializer` - **TESTADO** ✅
- ✅ `PlayerSerializer` - **TESTADO** ✅
- ✅ `RoundSerializer` - **TESTADO** ✅
- ✅ `TeamSerializer` - **TESTADO** ✅

**Services**:
- ✅ `RoundTeamGenerator` - **TESTADO** ✅

**Testes de Integração**:
- ✅ Testes de integração end-to-end - **IMPLEMENTADOS** ✅
  - ✅ `api_flow_spec.rb` - Testes de fluxos de API com includes e sparse fieldsets (corrigido GET + params -> POST)
  - ✅ `championship_flow_spec.rb` - Testes de fluxo completo de campeonato
  - ✅ `match_flow_spec.rb` - Testes de fluxo completo de partida

**Melhorias em Código Parcialmente Testado**:
- ✅ `ChampionshipsController` - **MELHORADO** (RecordNotFound em `#show`)
- ✅ `ApplicationController` - **MELHORADO** (fallback `base_relation`, branches de `render_collection`)
- ✅ `Championships::CollectionQuery` - **TESTADO** (spec próprio; FindQuery usa `includes: []`)
- ✅ `MatchPresenter` - **MELHORADO** (count 0, team_1/2_players, draw)
- ✅ `ChampionshipPresenter` - **MELHORADO** (rounds sem players, um round/player)
- ✅ `PlayerPresenter` - **MELHORADO** (serialized_rounds/stats vazios vs preenchidos)

**Bibliotecas e Utilitários** (fora do escopo; itens 40–42):
- ❌ `CircuitBreakerService` (0% - 45 linhas)
- ❌ `ConsulService` (0% - 55 linhas)
- ❌ `IdentityServiceClient` (0% - 50 linhas)

**Progresso**: 100% completo (meta de 70% de branches atingida: 70.03%)

---

## Recomendações de Implementação

### Fase 1 - Fundação (Sprint 1-2) ✅ COMPLETA

1. ✅ Testar `BaseController` e concerns (`IdentityAuthentication`, `Paginatable`, `SparseFieldsets`, `Includable`) ✅
2. ✅ Testar `HealthController` ✅
3. ✅ Testar queries básicas (`FindQuery` para todos os recursos) ✅

### Fase 2 - CRUD Básico (Sprint 3-4) ✅ COMPLETA

4. ✅ Testar controllers de CRUD: ~~`MatchesController`~~ ✅, ~~`TeamsController`~~ ✅, ~~`RoundsController`~~ ✅
5. ✅ Testar queries de coleção correspondentes: ~~`Teams::CollectionQuery`~~ ✅, ~~`Rounds::CollectionQuery`~~ ✅
6. ✅ Testar presenters básicos: ~~`TeamPresenter`~~ ✅, ~~`RoundPresenter`~~ ✅

### Fase 3 - Funcionalidades Complexas (Sprint 5-6) ✅ COMPLETA

7. ✅ Testar ~~`PlayersController`~~ ✅ e ~~`PlayerStatsController`~~ ✅
8. ✅ Testar services: ~~`Players::AddToRound`~~ ✅, ~~`Players::AddToTeam`~~ ✅
9. ✅ Testar ~~`PlayerStats::BulkUpsert`~~ ✅ (validações críticas)
10. ✅ Testar ~~`PlayerStats::CollectionQuery`~~ ✅

### Fase 4 - Lógica de Negócio (Sprint 7-8) ✅ COMPLETA

11. ✅ Testar ~~`Matches::Finalize`~~ ✅ (lógica crítica de finalização)
12. ✅ Testar ~~`RoundStatistics`~~ ✅
13. ✅ Testar ~~`Players::MatchStatistics`~~ ✅

### Fase 5 - Melhorias e Edge Cases (Sprint 9+) ✅ COMPLETA

**Queries Restantes**:
14. ✅ Testar ~~`Players::CollectionQuery`~~ ✅
15. ✅ Testar ~~`Matches::CollectionQuery`~~ ✅

**Serializers**:
16. ✅ Testar ~~`PlayerStatSerializer`~~ ✅
17. ✅ Testar ~~`PlayerSerializer`~~ ✅
18. ✅ Testar ~~`RoundSerializer`~~ ✅
19. ✅ Testar ~~`TeamSerializer`~~ ✅

**Services**:
20. ✅ Testar ~~`RoundTeamGenerator`~~ ✅

**Testes de Integração**:
21. ✅ Adicionar testes de integração end-to-end ✅

**Melhorias em Código Parcialmente Testado**:
22. ✅ Melhorar cobertura em `ChampionshipsController` (RecordNotFound em `#show`)
23. ✅ Melhorar cobertura em `ApplicationController` (fallback `base_relation`, branches)
24. ✅ Ajustar `Championships::CollectionQuery` / FindQuery spec
25. ✅ Melhorar cobertura em `MatchPresenter`, `ChampionshipPresenter`, `PlayerPresenter`

---

## Métricas de Sucesso

- **Meta de Cobertura**: 80% de linhas, 70% de branches
- **Cobertura Atual**: 87.55% de linhas ✅ (meta atingida), 70.03% de branches ✅ (meta atingida)
- **Testes**: 565 exemplos, 0 falhas, 0 pendentes ✅
- **Prioridade**: Focar em funcionalidades críticas primeiro (controllers e services)
- **Estratégia**: Testar em camadas (controllers → services → queries → presenters)

## Conclusão

O plano de priorização de testes está **finalizado**. Fases 1–5 concluídas; metas de cobertura (80% linhas, 70% branches) atingidas. As bibliotecas e utilitários (itens 40–42: CircuitBreakerService, ConsulService, IdentityServiceClient) permanecem **fora do escopo** do plano atual. Expansão futura opcional: testes para essas libs ou edge cases adicionais.

## Melhorias Recentes

**Nota:** Fases 1–4 foram concluídas em 2025; Fase 5 e fechamento do plano de priorização em Janeiro 2026.

### Janeiro 2026 - Fase 5 Completada ✅

- **Teste pendente resolvido**: `Matches::Finalize` "when team is blank" — stub de `team_1`/`team_1_id` nil para cobrir `calculate_goals_for(team.blank?)`; 0 pendentes.
- **ChampionshipsController**: Teste de `RecordNotFound` em `#show` (404 para ID inexistente)
- **ApplicationController**: Teste do fallback `base_relation` quando coleção não é Relation nem array de AR
- **Presenters**: Novos testes em `ChampionshipPresenter` (rounds sem players, um round/player), `PlayerPresenter` (serialized_rounds/stats vazios), `MatchPresenter` (count 0, team_1/2_players, draw)
- **Specs**: FindQuery spec ajustado para `includes: []`; rounds_controller com `per_page: 100`; api_flow GET com query string (evita bug Rails GET+params+json→POST)
- **Cobertura**: 87.55% linhas, 70.03% branches ✅ (meta 70% atingida), 565 testes passando

### Janeiro 2025 - Fase 4 Completada ✅

- **Fase 4 - Lógica de Negócio Completa**: Todos os componentes de lógica de negócio agora têm testes completos
- **Cobertura Melhorada**: Cobertura de branches aumentou de 62.69% para 63.28%
- **Testes Adicionados**: 9 novos testes (de 382 para 391)

### Janeiro 2025 - Fase 3 Completada ✅

- **Fase 3 - Funcionalidades Complexas Completa**: Todos os componentes de funcionalidades complexas agora têm testes completos
- **Cobertura Melhorada**: Cobertura de linhas aumentou de 82.75% para 83.7%, branches de 61.49% para 62.69%
- **Testes Adicionados**: 27 novos testes (de 355 para 382)

### Janeiro 2025 - Fase 2 Completada ✅

- **Fase 2 - CRUD Básico Completo**: Todos os componentes de CRUD básico agora têm testes completos
- **Cobertura Melhorada**: Cobertura de linhas aumentou de 80.9% para 82.75%
- **Testes Adicionados**: 81 novos testes (de 274 para 355)

### Janeiro 2025 - Fase 1 Completada ✅

- **Fase 1 - Fundação Completa**: Todos os componentes de fundação agora têm testes abrangentes
- **Cobertura Atingida**: Meta de 80% de linhas alcançada (80.9%)
- **Testes Adicionados**: 77 novos testes (de 197 para 274)

### Dezembro 2024

- **Matches::Finalize**: Corrigido cálculo de own goals
- **Warnings de Depreciação**: Todos os `:unprocessable_entity` foram substituídos por `:unprocessable_content`

### Próximos Passos (opcional)

Em linha com a [Conclusão](#conclusão), o plano está fechado. Opcionalmente, para expandir o escopo no futuro: testes para libs (itens 40–42) ou edge cases adicionais.
