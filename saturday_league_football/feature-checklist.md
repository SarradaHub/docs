# Checklist de funcionalidades — Saturday League Football

Este checklist separa **Backend** e **Frontend** por funcionalidade para facilitar planejamento e acompanhamento.

## Estado Atual do Projeto

**Última atualização:** 28 de Janeiro de 2026

### Sistema de Autenticação
- ✅ Autenticação implementada via `IdentityServiceClient` (serviço externo)
- ⚠️ **Nota:** O sistema suporta tanto `IdentityServiceClient` quanto Devise. A autenticação é feita através de um serviço de identidade externo que valida tokens Bearer, ou via Devise para desenvolvimento.
- ✅ Relação User-Championship implementada (`user_id` em `championships`)
- ✅ Filtros por administrador implementados em todas as queries (backend)
- ✅ Autenticação no frontend implementada (`AuthContext`, `ProtectedRoute`, `LoginPage`)

### Funcionalidades Implementadas
- ✅ CRUD completo para todas as entidades (Campeonatos, Jogadores, Times, Partidas)
- ✅ Otimização de API (paginação, sparse fieldsets, includes)
- ✅ Sistema completo de estatísticas (gols, assistências, goleiro)
- ✅ Finalizar partida com cálculo automático de vencedor
- ✅ Estatísticas da rodada e leaderboards
- ✅ Auto-balanceamento automático de times (`RoundTeamGenerator`)

### Funcionalidades Pendentes
- ❌ Regras de negócio (escolha de goleiro, sequência de partidas, substituições)
- ❌ Montador automático da próxima partida
- ❌ Editar/Excluir rodada no frontend (métodos no repository existem, mas faltam componentes UI)
- ❌ Botão manual de rebalancear times

## CRUD Campeonatos

### Backend

- [x] CRUD REST (`championships`)
- [x] Otimizar retorno de (`championships`)
  - [x] Implementar paginação para listagem (`?page=1&per_page=20`)
  - [x] Adicionar sparse fieldsets (`?fields=id,name,description`)
  - [x] Evitar N+1 queries com `includes` apropriados
  - [x] Retornar apenas campos necessários (não incluir `rounds`, `players` por padrão)
  - [x] Adicionar query param `include` para relações opcionais (`?include=rounds,players`)

### Frontend

- [x] Criar campeonato
- [x] Listar/visualizar campeonato
- [x] Editar campeonato (nome/descrição/limites de jogadores)
- [x] Excluir campeonato (confirmação + erros)
- ⚠️ **Nota:** A exclusão exibe erro quando há rodadas/partidas associadas, mas a validação é feita via resposta do backend.

## CRUD Rodadas

### Backend

- [x] CRUD REST (`rounds`)

### Frontend

- [x] Criar rodada
- [x] Listar/visualizar rodada
- [ ] Editar rodada (nome/data)
  - **Nota:** Método `updateRound` existe no `roundRepository`, mas falta componente UI
  - [ ] Criar componente `EditRoundModal.tsx`
  - [ ] Integrar na `RoundDetailsPage.tsx`
- [ ] Excluir rodada (confirmação + erros)
  - **Nota:** Método `deleteRound` existe no `roundRepository`, mas falta componente UI
  - [ ] Criar componente `DeleteRoundModal.tsx`
  - [ ] Integrar na `RoundDetailsPage.tsx`

## CRUD Jogador

### Backend

- [x] CRUD REST (`players`)
- [x] Otimizar retorno de (`players`)
  - [x] Implementar paginação para listagem (`?page=1&per_page=20`)
  - [x] Adicionar sparse fieldsets (`?fields=id,name,championship_id`)
  - [x] Evitar N+1 queries com `includes` apropriados
  - [x] Retornar apenas campos básicos por padrão (não incluir `player_stats`, `rounds`, `teams`)
  - [x] Adicionar query param `include` para relações opcionais (`?include=player_stats,rounds`)
  - [x] Filtrar por `championship_id` quando aplicável

### Frontend

- [x] Criar jogador
- [x] Listar/visualizar jogador
- [x] Editar jogador (nome/vínculos com rodada/time)
- [x] Excluir jogador (confirmação + erros)

## CRUD Time

### Backend

- [x] CRUD REST (`teams`)
- [x] Otimizar retorno de (`teams`)
  - [x] Implementar paginação para listagem (`?page=1&per_page=20`)
  - [x] Adicionar sparse fieldsets (`?fields=id,name,round_id`)
  - [x] Evitar N+1 queries com `includes` apropriados
  - [x] Retornar apenas campos básicos por padrão (não incluir `players`, `matches`)
  - [x] Adicionar query param `include` para relações opcionais (`?include=players,matches`)
  - [x] Filtrar por `round_id` quando aplicável

### Frontend

- [x] Criar time
- [x] Listar/visualizar time
- [x] Editar time
- [x] Excluir time

## CRUD Partida

### Backend

- [x] CRUD REST (`matches`)
- [x] Otimizar retorno de (`matches`)
  - [x] Implementar paginação para listagem (`?page=1&per_page=20`)
  - [x] Adicionar sparse fieldsets (`?fields=id,name,team_1_id,team_2_id,round_id`)
  - [x] Evitar N+1 queries com `includes` apropriados
  - [x] Retornar apenas campos básicos por padrão (não incluir `team_1_players`, `team_2_players`, `player_stats`)
  - [x] Adicionar query param `include` para relações opcionais (`?include=teams,players,stats`)
  - [x] Filtrar por `round_id` quando aplicável
- [x] Finalizar partida
  - [x] Criar service `Matches::Finalize` para calcular vencedor baseado em estatísticas
  - [x] Adicionar endpoint `POST /api/v1/matches/:id/finalize`
  - [x] Atualizar `winning_team_id` e `draw` baseado nos gols calculados das estatísticas

### Frontend

- [x] Criar partida
- [x] Listar/visualizar partida
- [x] Editar partida (dados básicos)
- [x] Excluir partida
- [x] Finalizar partida
  - [x] Botão "Finalizar" na `MatchDetailsPage`
  - [x] Desabilitar botão quando partida já finalizada ou sem estatísticas
  - [x] Atualizar interface após finalizar (refetch match)

## Definir goleiros da partida

### Backend

- [x] Campo `was_goalkeeper` em `player_stats`
- [x] Endpoints `player_stats` (inclui bulk por match)
- [x] Otimizar retorno de (`player_stats`)
  - [x] Implementar paginação para listagem (`?page=1&per_page=50`)
  - [x] Adicionar sparse fieldsets (`?fields=id,goals,assists,own_goals,was_goalkeeper`)
  - [x] Evitar N+1 queries com `includes` apropriados
  - [x] Retornar apenas campos básicos por padrão (não incluir `player`, `team` completos)
  - [x] Adicionar query param `include` para relações opcionais (`?include=player,team`)
  - [x] Filtrar por `match_id` e `team_id` quando aplicável

### Frontend

- [x] Exibir "Goleiro" em estatísticas do jogador
- [x] UI no detalhe da partida para marcar goleiros por jogador/time
  - [x] Integrar na mesma UI de edição de stats (`EditMatchStatsModal.tsx`)
  - [x] Checkbox ou toggle "Foi goleiro" por jogador
  - [x] Validação: pelo menos um goleiro por time (opcional)
- [x] Persistir a alteração (ideal: bulk update)
  - [x] Incluir `was_goalkeeper` no payload do bulk update
  - [x] Usar mesmo fluxo de `bulkUpdate` das stats

## Atualizar/definir resultado da partida (gols, assistências, gols contra)

### Backend

- [x] `player_stats` com `goals/assists/own_goals`
- [x] Bulk update por partida
- [x] Placar/estatísticas derivadas via presenter

### Frontend

- [x] Exibir placar e listas de gols/assistências no detalhe da partida
- [x] UI para lançar/editar stats por jogador (gols/assists/own_goals)
  - [x] Criar componente `EditMatchStatsModal.tsx` ou seção inline na `MatchDetailsPage.tsx`
  - [x] Formulário por jogador com campos: gols, assistências, gols contra, goleiro
  - [x] Agrupar por time (team_1 e team_2)
  - [x] Validação de inputs (números não negativos)
- [x] Usar `bulkUpdate(matchId, payload[])` na prática
  - [x] Integrar chamada ao `playerStatsRepository.bulkUpdate()`
  - [x] Preparar payload com array de `PlayerStatPayload[]`
- [x] Recalcular/atualizar tela após salvar (invalidate queries)
  - [x] Invalidar query de match após bulk update
  - [x] Atualizar placar e listas automaticamente

## Mostrar estatísticas dos jogadores na rodada (fim de semana)

### Backend

- [x] Endpoint/contrato explícito de "stats por rodada" (filtrado na rodada)
  - [x] Criar `app/services/round_statistics.rb` para calcular stats agregadas
  - [x] Adicionar action `statistics` em `app/controllers/api/v1/rounds_controller.rb`
  - [x] Definir métricas oficiais (gols/assists/partidas/goleiro etc.)
  - [x] Retornar JSON estruturado com stats por jogador na rodada

### Frontend

- [x] Seção/página de estatísticas na rodada (tabela/cards + ordenação)
  - [x] Criar componente `RoundStatisticsSection.tsx`
  - [x] Integrar na `RoundDetailsPage.tsx`
  - [x] Implementar tabela ordenável por métricas
  - [x] Adicionar cards com resumo de stats

## Mostrar líderes da rodada (mais gols/assist/partidas/vit/der/emp/goleiro)

### Backend

- [x] Leaderboards por rodada (gols, assistências, partidas, goleiro)
  - [x] Criar `app/services/round_statistics.rb` para calcular rankings (implementado como parte de statistics)
  - [x] Implementar cálculo de vitórias/derrotas/empates por jogador (regra clara)
  - [x] Adicionar action `statistics` em `app/controllers/api/v1/rounds_controller.rb` (inclui leaderboards)
  - [x] Retornar JSON com tops por métrica (gols, assistências, goleiro, vitórias, etc.)

### Frontend

- [x] Widget/página de ranking da rodada (tops por métrica)
  - [x] Criar componente `RoundStatisticsSection.tsx` (inclui leaderboards)
  - [x] Integrar na `RoundDetailsPage.tsx`
  - [x] Exibir cards/widgets com top por cada métrica (artilheiro, mais assists, mais goleiro)
  - [x] Adicionar navegação para detalhes do jogador (click na linha da tabela)

## Montador automático de times

**Status:** Auto-balanceamento automático implementado via `RoundTeamGenerator`. Botão manual ainda pendente.

### Backend

- [x] Auto-balance ao adicionar/remover jogador da rodada
  - [x] `RoundTeamGenerator` implementado e funcionando
  - [x] Auto-balanceamento acionado automaticamente via `after_commit` em `PlayerRound`
- [ ] (Opcional p/ "100% produto") Endpoint manual "rebalancear times"
  - [ ] Adicionar action `rebalance_teams` em `app/controllers/api/v1/rounds_controller.rb`
  - [ ] Chamar `RoundTeamGenerator.call(round)` manualmente
  - [ ] Retornar feedback do resultado (times atualizados, distribuição)
- [ ] (Se requisito) Balanceamento por "habilidade" (modelar + algoritmo)

### Frontend

- [ ] (Opcional) Botão "Montar/Rebalancear times" + feedback do resultado
  - [ ] Adicionar botão na `RoundDetailsPage.tsx`
  - [ ] Implementar chamada ao endpoint de rebalanceamento
  - [ ] Exibir feedback visual (toast/alert) com resultado
  - [ ] Invalidar queries para atualizar lista de times

## Montador automático da próxima partida

**Status:** Não implementado. Partidas são criadas manualmente. As regras de negócio para sequência automática estão documentadas na seção "Regras de Negócio".

### Backend

- [ ] Definir regra de "próxima partida" (times/ordem/descanso etc.)
  - [ ] Documentar regras de negócio (quais times jogam, ordem, descanso mínimo)
  - [ ] Criar `app/services/next_match_generator.rb`
  - [ ] Implementar algoritmo de seleção de times
- [ ] Endpoint para sugerir e/ou criar a próxima partida
  - [ ] Adicionar action `suggest_next_match` em `app/controllers/api/v1/rounds_controller.rb`
  - [ ] Adicionar action `create_next_match` (opcional, ou usar `suggest` + confirmação)

### Frontend

- [ ] Botão "Gerar próxima partida" na rodada + fluxo de confirmação
  - [ ] Adicionar botão na `RoundDetailsPage.tsx`
  - [ ] Criar modal de confirmação com preview da partida sugerida
  - [ ] Implementar criação após confirmação
- [ ] Exibir sugestão (antes de criar) ou navegar para a partida criada
  - [ ] Mostrar preview da partida (times, data sugerida)
  - [ ] Navegar para `MatchDetailsPage` após criação

---

## Regras de Negócio

### Escolha do Goleiro

**Status:** Não implementado. O sistema permite marcar goleiros via `was_goalkeeper`, mas não valida as regras de negócio abaixo.

- [ ] Implementar validação: goleiro não pode ser jogador de linha na partida em questão
- [ ] Permitir que goleiro seja jogador de linha em algum time de fora
- [ ] Validar que goleiro esteja cadastrado em algum time como jogador de linha (se aplicável)
- [ ] Exemplo: O Time 1 é composto por Jogador A, B, C, D, E. Time 2 é composto por Jogador F, G, H, I, J. O goleiro do Time 1 pode ser o Jogador K do Time 3 ou o Jogador U do Time 5.

### Sequência das Partidas e Preenchimento de Vagas

**Status:** Não implementado. O sistema permite criar partidas manualmente, mas não implementa a lógica automática de sequência e preenchimento de vagas.

- [ ] Organizar partidas na mesma ordem em que os times são criados
- [ ] Primeira partida: Time 1 × Time 2
- [ ] Vencedor da primeira partida enfrenta o Time 3
- [ ] Se Time 3 estiver incompleto, completar com os primeiros jogadores da lista do time perdedor
- [ ] Em caso de empate, manter em campo o time que já estava atuando
  - [ ] Exemplo: Se o Time 1 vencer o Time 2 e empatar com o Time 3, o Time 3 jogará a terceira partida contra o Time 2
- [ ] Time que perder retorna ao final da fila de espera

### Substituições

**Status:** Não implementado. O sistema não possui lógica para substituições automáticas de jogadores.

- [ ] Se jogador sair durante uma partida, substituir pelo próximo jogador disponível na lista geral
- [ ] Se jogador sair entre partidas, substituir pelo próximo jogador disponível na lista geral
- [ ] Próximo jogador disponível = primeiro jogador do primeiro time que estiver fora de campo

### Administrador de Campeonato

**Nota:** O sistema possui autenticação via `IdentityServiceClient` (serviço externo) e Devise. O escopo de dados por administrador está implementado no backend e o frontend filtra automaticamente via token Bearer.

#### Backend

- [x] Autenticação implementada (via `IdentityServiceClient` e Devise)
  - [x] Sistema de autenticação via token Bearer já funcional
  - [x] `IdentityAuthentication` concern implementado em `Api::V1::BaseController`
  - [x] Validação de token via serviço externo
  - [x] Suporte a Devise para desenvolvimento
- [x] Criar relação entre User e `Championship` (um user tem n campeonatos)
  - [x] Adicionado `user_id` na tabela `championships` (migration `20260123220755`)
  - [x] Foreign key `championships.user_id` → `users.id` criada
  - [x] `belongs_to :user` no model `Championship`
  - [x] `has_many :championships` no model `User`
- [x] Implementar escopo de dados por administrador
  - [x] Filtrar campeonatos por `user_id` do usuário autenticado
  - [x] Filtrar rodadas pelos campeonatos do usuário
  - [x] Filtrar partidas pelas rodadas dos campeonatos do usuário
  - [x] Filtrar jogadores pelos campeonatos do usuário
  - [x] Filtrar times pelas rodadas dos campeonatos do usuário
  - [x] Filtrar estatísticas de jogadores pelas partidas dos campeonatos do usuário
- [x] Autenticação obrigatória nos controllers
  - [x] `IdentityAuthentication` já incluído em `Api::V1::BaseController`
  - [x] Validação de token via `IdentityServiceClient` implementada
  - [x] Todos os controllers filtram por `current_user.id`
- [x] Atualizar queries para incluir filtro por administrador
  - [x] `Championships::CollectionQuery` filtra por `user_id`
  - [x] `Players::CollectionQuery` filtra por campeonatos do usuário
  - [x] `Teams::CollectionQuery` filtra por rodadas dos campeonatos do usuário
  - [x] `Matches::CollectionQuery` filtra por rodadas dos campeonatos do usuário
  - [x] `Rounds::CollectionQuery` filtra por campeonatos do usuário
  - [x] `PlayerStats::CollectionQuery` filtra por partidas dos campeonatos do usuário

#### Frontend

- [x] Implementar autenticação (integração com IdentityService e Devise)
  - [x] Criadas páginas de login/logout (`LoginPage.tsx`)
  - [x] Configuradas rotas de autenticação
  - [x] Implementada proteção de rotas (`ProtectedRoute.tsx`)
  - [x] Middleware/interceptor para incluir token Bearer nas requisições (`BaseService` com `tokenStorage`)
- [x] Implementar contexto/estado de autenticação
  - [x] Criado `AuthContext` para gerenciar estado do usuário autenticado
  - [x] Armazenamento de token/sessão via `tokenStorage`
  - [x] Verificação de autenticação em componentes protegidos
  - [x] `AuthProvider` integrado no `AppProviders`
- [x] Atualizar repositórios para filtrar por administrador
  - [x] Filtros automáticos via token Bearer (implícito via `current_user.id` no backend)
  - [x] Todas as listagens respeitam o escopo do usuário autenticado
- [x] Adicionar proteção de rotas no frontend
  - [x] Componente `ProtectedRoute` implementado
  - [x] Redirecionamento para login se não autenticado
  - [x] Todas as rotas de campeonatos, rodadas, partidas e jogadores protegidas
- [x] Atualizar UI para exibir apenas dados do administrador
  - [x] Listagens mostram apenas campeonatos do usuário autenticado
  - [x] Detalhes de campeonatos/rodadas/partidas acessíveis apenas se pertencerem ao usuário
  - [x] Tratamento de erro para acesso não autorizado (via filtros do backend)

---

## Otimização de Responses de API

### Problema Atual
- Load time alto devido a responses muito grandes
- Carregamento de dados desnecessários para cada tela
- Falta de controle sobre quais campos são retornados
- Possível N+1 queries em relações

### Estratégias de Otimização

#### Backend

- [x] **Implementar Paginação**
  - [x] Adicionar suporte a `page` e `per_page` em todos os endpoints de listagem
  - [x] Retornar metadados de paginação: `total`, `page`, `per_page`, `total_pages`
  - [x] Definir limites máximos (ex: `per_page` máximo de 100)
  - [x] Implementar em: `championships`, `players`, `teams`, `matches`, `rounds`, `player_stats`

- [x] **Sparse Fieldsets (Seleção de Campos)**
  - [x] Adicionar query param `fields` para permitir seleção de campos específicos
  - [x] Exemplo: `GET /players?fields=id,name,championship_id`
  - [x] Validar campos solicitados contra schema
  - [ ] Documentar campos disponíveis em cada endpoint

- [x] **Include Relations (Carregamento Opcional de Relações)**
  - [x] Adicionar query param `include` para relações opcionais
  - [x] Exemplo: `GET /matches/123?include=teams,players,stats`
  - [x] Não incluir relações por padrão (apenas IDs quando aplicável)
  - [x] Suportar includes aninhados: `?include=teams.players`

- [x] **Otimizar Queries (Evitar N+1)**
  - [x] Usar `includes` e `preload` do ActiveRecord de forma estratégica
  - [x] Implementar `counter_cache` para contagens (ex: `rounds_count`, `matches_count`, `players_count`, `player_stats_count`)
  - [x] Migration `20260124193201_add_counter_cache_columns.rb` adiciona colunas de contagem
  - [x] Models atualizados com `counter_cache: true` ou `counter_cache: :column_name`
  - [ ] Usar `select` para limitar colunas do banco quando possível
  - [x] Adicionar índices apropriados no banco de dados (migration `20260124193202_add_performance_indexes.rb`)

- [ ] **Response Compression**
  - [ ] Habilitar gzip/brotli compression no servidor
  - [ ] Configurar nginx/load balancer para compressão automática
  - [ ] Adicionar header `Content-Encoding: gzip` quando aplicável

- [ ] **Caching**
  - [ ] Implementar cache HTTP com headers apropriados (`Cache-Control`, `ETag`)
  - [ ] Cache de dados estáticos (campeonatos, configurações)
  - [ ] Cache condicional com `If-None-Match` e `If-Modified-Since`
  - [ ] Invalidar cache apropriadamente em mutations

- [ ] **Limitar Dados por Contexto**
  - [ ] Criar endpoints específicos para diferentes contextos
  - [ ] Exemplo: `GET /rounds/:id/summary` (apenas dados básicos) vs `GET /rounds/:id` (completo)
  - [ ] Usar presenters/serializers diferentes para list vs detail

- [ ] **Headers de Performance**
  - [ ] Adicionar `X-Response-Time` header para debugging
  - [ ] Adicionar `X-Total-Count` em listagens paginadas
  - [ ] Implementar `Link` header para paginação (RFC 5988)

#### Frontend

- [x] **Request Otimizado**
  - [x] Usar sparse fieldsets nas queries do React Query
  - [x] Solicitar apenas campos necessários para cada componente
  - [x] Usar `include` apenas quando a relação for realmente necessária
  - [ ] Implementar paginação no frontend para listagens grandes

- [ ] **Lazy Loading de Dados**
  - [ ] Carregar dados relacionados apenas quando necessário (on-demand)
  - [ ] Usar React Query `enabled` para controlar quando fazer fetch
  - [ ] Implementar infinite scroll ou "load more" para listagens

- [x] **Cache e Estado**
  - [x] Configurar `staleTime` apropriado no React Query
  - [x] Usar `cacheTime` para manter dados em cache
  - [x] Invalidar cache seletivamente (apenas queries afetadas)

- [ ] **Otimização de Renderização**
  - [ ] Evitar re-renders desnecessários com `React.memo`
  - [ ] Usar `useMemo` para cálculos derivados
  - [ ] Implementar virtualização para listas grandes (react-window)

- [ ] **Request Batching**
  - [ ] Agrupar múltiplas queries quando possível
  - [ ] Usar `useQueries` para queries paralelas
  - [ ] Evitar waterfall de requests (request 2 depende de request 1)

### Exemplos de Implementação

#### Backend - Endpoint Otimizado

```ruby
# GET /api/v1/players?page=1&per_page=20&fields=id,name&include=championship
def index
  players = Players::CollectionQuery.new(
    championship_id: params[:championship_id],
    page: params[:page] || 1,
    per_page: [params[:per_page]&.to_i || 20, 100].min,
    fields: params[:fields]&.split(','),
    includes: params[:include]&.split(',')
  ).call
  
  render json: {
    data: players,
    meta: {
      page: params[:page] || 1,
      per_page: players.limit_value,
      total: players.total_count,
      total_pages: players.total_pages
    }
  }
end
```

#### Frontend - Query Otimizada

```typescript
// Solicitar apenas campos necessários
const { data } = useQuery({
  queryKey: ['players', { page: 1, fields: 'id,name' }],
  queryFn: () => playerRepository.list({
    page: 1,
    per_page: 20,
    fields: 'id,name,championship_id',
    include: 'championship' // apenas se necessário
  })
});
```

### Métricas de Sucesso

- [ ] Reduzir tamanho médio de response em 50-70% (implementado, aguardando métricas)
- [ ] Reduzir load time de listagens para < 500ms (implementado, aguardando métricas)
- [ ] Reduzir load time de detalhes para < 300ms (implementado, aguardando métricas)
- [ ] Eliminar N+1 queries (verificar com Bullet gem)
- [x] Implementar paginação em todos os endpoints de listagem
- [ ] Documentar campos disponíveis e includes suportados

### Prioridade

**Alta** - Impacto direto na performance e experiência do usuário. Deve ser implementado antes de adicionar novas funcionalidades que aumentem o volume de dados.

---

## Priorização de Tarefas

### ✅ Tarefas Concluídas

#### Segurança e Isolamento de Dados
- ✅ **Escopo de Dados por Administrador**
  - Relação User-Championship implementada (`user_id` em `championships`)
  - Filtros por usuário em todas as queries (backend)
  - Autenticação e proteção de rotas (frontend)
  - Cada usuário vê apenas seus próprios dados

#### Otimizações de Performance
- ✅ **Otimização de Responses de API**
  - Paginação, sparse fieldsets e includes implementados
  - Counter cache para contagens
  - Índices de performance no banco de dados

#### Funcionalidades Core
- ✅ **Sistema de Autenticação**
  - Integração com `IdentityServiceClient` e Devise
  - `AuthContext`, `ProtectedRoute`, `LoginPage`
- ✅ **Estatísticas e Finalização de Partidas**
  - UI para atualizar stats (`EditMatchStatsModal.tsx`)
  - Estatísticas da rodada e leaderboards
  - Finalizar partida com cálculo automático de vencedor
- ✅ **CRUD completo de Campeonatos (frontend)**
  - Criar, editar, excluir e listar campeonatos
- ✅ **Auto-balanceamento de Times**
  - `RoundTeamGenerator` implementado
  - Balanceamento automático ao adicionar/remover jogadores

---

### 🔴 Prioridade Crítica (Bloqueadores e Essenciais)

#### 1. Regras de Negócio - Validação de Goleiros
**Status:** Pendente - Sistema permite marcar goleiros, mas não valida regras  
**Impacto:** Crítico - Essencial para garantir que as regras do jogo sejam respeitadas

- [ ] Implementar validação: goleiro não pode ser jogador de linha na partida em questão
- [ ] Permitir que goleiro seja jogador de linha em algum time de fora
- [ ] Validar que goleiro esteja cadastrado em algum time como jogador de linha
- [ ] Exemplo: Time 1 (A, B, C, D, E) vs Time 2 (F, G, H, I, J). Goleiro do Time 1 pode ser Jogador K do Time 3 ou Jogador U do Time 5

**Dependências:** Nenhuma  
**Estimativa:** Média

---

### 🟠 Prioridade Alta (Regras de Negócio Essenciais)

#### 2. Sequência Automática de Partidas
**Status:** Pendente - Partidas são criadas manualmente  
**Impacto:** Alto - Essencial para o funcionamento correto do sistema de partidas

- [ ] Organizar partidas na mesma ordem em que os times são criados
- [ ] Primeira partida: Time 1 × Time 2
- [ ] Vencedor da primeira partida enfrenta o Time 3
- [ ] Completar time incompleto com jogadores do time perdedor
- [ ] Em caso de empate, manter em campo o time que já estava atuando
- [ ] Time que perder retorna ao final da fila de espera

**Dependências:** Nenhuma  
**Estimativa:** Alta

#### 3. Sistema de Substituições
**Status:** Pendente  
**Impacto:** Alto - Essencial para gerenciar saídas de jogadores

- [ ] Implementar lógica de substituição durante partida
- [ ] Implementar lógica de substituição entre partidas
- [ ] Definir regra: próximo jogador = primeiro do primeiro time fora de campo

**Dependências:** Nenhuma  
**Estimativa:** Média

---

### 🟡 Prioridade Média (Melhorias de UX e Funcionalidades Importantes)

#### 4. CRUD Completo de Rodadas
**Status:** Pendente - Apenas criar e visualizar implementados  
**Impacto:** Médio - Melhora a experiência de gerenciamento de rodadas

- [ ] Criar `EditRoundModal.tsx`
- [ ] Criar `DeleteRoundModal.tsx`
- [ ] Integrar na `RoundDetailsPage.tsx`
- [ ] Validação: verificar se há partidas associadas antes de excluir

**Nota:** Métodos `updateRound` e `deleteRound` já existem no `roundRepository`

**Dependências:** Nenhuma  
**Estimativa:** Baixa

#### 5. Montador Automático da Próxima Partida
**Status:** Pendente  
**Impacto:** Médio - Automatiza processo manual, melhora eficiência

- [ ] Implementar service `NextMatchGenerator` baseado nas regras de negócio
- [ ] Criar endpoint `POST /api/v1/rounds/:id/suggest_next_match`
- [ ] Frontend: Botão "Gerar próxima partida" na `RoundDetailsPage`
- [ ] Modal de confirmação com preview da partida sugerida
- [ ] Navegar para `MatchDetailsPage` após criação

**Dependências:** Item #2 (Sequência Automática de Partidas)  
**Estimativa:** Média

#### 6. Botão Manual de Rebalancear Times
**Status:** Pendente - Auto-balance existe, botão manual pendente  
**Impacto:** Médio - Útil quando balanceamento automático não atende necessidades específicas

- [ ] Adicionar action `rebalance_teams` em `RoundsController`
- [ ] Chamar `RoundTeamGenerator.call(round)` manualmente
- [ ] Retornar feedback do resultado (times atualizados, distribuição)
- [ ] Botão na `RoundDetailsPage` para acionar rebalanceamento
- [ ] Feedback visual do resultado (toast/alert)

**Dependências:** Nenhuma  
**Estimativa:** Baixa

---

### 🟢 Prioridade Baixa (Otimizações e Nice to Have)

#### 7. Otimizações de Performance Adicionais
**Status:** Parcial - Paginação, sparse fieldsets, includes e counter_cache já implementados  
**Impacto:** Baixo - Melhora performance, mas não bloqueia funcionalidades

- [ ] Implementar cache HTTP (Cache-Control, ETag)
- [ ] Response compression (gzip/brotli)
- [ ] Usar `select` para limitar colunas do banco quando possível
- [ ] Criar endpoints específicos para diferentes contextos (ex: `/rounds/:id/summary`)
- [ ] Adicionar headers de performance (`X-Response-Time`, `X-Total-Count`)

**Dependências:** Nenhuma  
**Estimativa:** Média

#### 8. Melhorias de Frontend
**Status:** Parcial - Cache e estado já configurados  
**Impacto:** Baixo - Melhora UX em listagens grandes

- [ ] Implementar paginação no frontend para listagens grandes
- [ ] Lazy loading de dados relacionados (React Query `enabled`)
- [ ] Otimização de renderização (React.memo, useMemo)
- [ ] Virtualização para listas grandes (react-window)
- [ ] Request batching (agrupar múltiplas queries)

**Dependências:** Nenhuma  
**Estimativa:** Média

#### 9. Balanceamento por Habilidade
**Status:** Pendente  
**Impacto:** Baixo - Funcionalidade avançada, não essencial

- [ ] Modelar sistema de habilidade de jogadores
- [ ] Implementar algoritmo de balanceamento considerando habilidade
- [ ] Adicionar opção de escolha entre balanceamento simples e por habilidade

**Dependências:** Nenhuma  
**Estimativa:** Alta

---

## Histórico de Implementações

### Janeiro 2026 - Implementação de Prioridades Altas

#### Otimização de Responses de API ✅
**Backend:**
- Criados concerns reutilizáveis: `Paginatable`, `SparseFieldsets`, `Includable` em `app/controllers/concerns/`
- Atualizadas todas as `CollectionQuery` classes para suportar paginação, fields e includes
- Implementado método `render_collection` no `ApplicationController` para padronizar respostas
- Todos os controllers atualizados: `championships`, `players`, `teams`, `matches`, `rounds`, `player_stats`

**Frontend:**
- Atualizado `BaseService` para suportar respostas paginadas (`PaginatedResponse`)
- Todos os repositórios atualizados com métodos `listPaginated()` e suporte a query params
- Tipos TypeScript atualizados para suportar `QueryParams` com `page`, `per_page`, `fields`, `include`

#### UI para Atualizar Stats da Partida ✅
- Criado componente `EditMatchStatsModal.tsx` com formulário completo
- Integrado na `MatchDetailsPage` com botão "Estatísticas"
- Suporte para editar gols, assistências, gols contra e goleiro por jogador
- Agrupamento por time (team_1 e team_2)
- Validação de inputs (números não negativos)
- Busca e preenchimento automático de stats existentes
- Botão desabilitado quando não há jogadores nos times
- Integração com `bulkUpdate` do `playerStatsRepository`
- Atualização automática da interface após salvar (refetch)

#### Estatísticas da Rodada ✅
**Backend:**
- Criado service `RoundStatistics` em `app/services/round_statistics.rb`
- Endpoint `GET /api/v1/rounds/:id/statistics`
- Cálculo de métricas: gols, assistências, gols contra, partidas, goleiro, vitórias, derrotas, empates

**Frontend:**
- Criado componente `RoundStatisticsSection.tsx`
- Integrado na `RoundDetailsPage`
- Tabela ordenável por qualquer métrica (asc/desc)
- Cards com resumo: top scorer, top assist, top goleiro
- Links para detalhes do jogador (click na linha)

#### Finalizar Partida ✅
**Backend:**
- Criado service `Matches::Finalize` em `app/services/matches/finalize.rb`
- Endpoint `POST /api/v1/matches/:id/finalize`
- Calcula vencedor baseado nas estatísticas (gols + own_goals do oponente)
- Atualiza `winning_team_id` e `draw` automaticamente

**Frontend:**
- Botão "Finalizar" na `MatchDetailsPage`
- Desabilitado quando partida já finalizada ou sem estatísticas
- Tooltip explicativo
- Atualização automática após finalizar (refetch)

#### Escopo de Dados por Administrador ✅
**Backend:**
- Adicionado `user_id` em `championships` (migration `20260123220755`)
- Implementados filtros por `user_id` em todas as queries:
  - `Championships::CollectionQuery`
  - `Rounds::CollectionQuery`
  - `Matches::CollectionQuery`
  - `Teams::CollectionQuery`
  - `Players::CollectionQuery`
  - `PlayerStats::CollectionQuery`
- Todos os controllers atualizados para usar `current_user.id`
- Foreign key `championships.user_id` → `users.id` criada

**Frontend:**
- Implementado `AuthContext` e `AuthProvider` para gerenciar autenticação
- Criado `ProtectedRoute` para proteger rotas
- Implementada página de login (`LoginPage.tsx`)
- Integração com `IdentityServiceClient` e Devise
- Token Bearer incluído automaticamente nas requisições via `BaseService`
- Todas as rotas protegidas configuradas no `AppRoutes`

#### Otimização de Queries ✅
- Implementado `counter_cache` para contagens (migration `20260124193201`)
  - `championships.rounds_count`, `championships.players_count`
  - `rounds.matches_count`, `rounds.players_count`
  - `teams.players_count`
  - `players.player_stats_count`
- Adicionados índices de performance (migration `20260124193202`)
- Models atualizados com `counter_cache: true` ou `counter_cache: :column_name`

#### Correções e Melhorias
- Corrigido `MatchPresenter` para retornar arrays de Players em vez de hashes
- Adicionados campos `team_1_goals`, `team_2_goals`, `team_1_goals_scorer`, etc. ao JSON
- Corrigido cálculo de own_goals (own_goals do time 2 contam para time 1)
- Corrigido tratamento de parâmetros não permitidos no `bulk_update`
- Adicionado tratamento de erros para arrays vazios ou inválidos
