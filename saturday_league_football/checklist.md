# Checklist de Tarefas Pendentes

**Última atualização:** Janeiro 2026

Este documento lista tarefas pendentes identificadas na documentação do projeto. Para funcionalidades específicas do projeto, veja [Feature Checklist](feature-checklist.md). Para status de testes, veja [Test Prioritization](test_prioritization.md).

## Status Atual

### Concluído ✅

- **Arquitetura**: Counter cache, índices de performance, query objects, services, presenters e serializers implementados
- **Testes**: Cobertura de 87.55% (meta 80% atingida), 815 testes passando, Bullet habilitado para detectar N+1
- **Performance de Testes**: Paralelização implementada com `parallel_tests`, Database Cleaner configurado, tags de testes lentos (`:slow`)
- **Performance**: Paginação, sparse fieldsets, includes, counter cache e índices de banco implementados
- **Acessibilidade**: PWA manifest atualizado, API payloads padronizados
- **Tooling**: RuboCop configurado (rubocop-rails-omakase)

### Pendente ❌

- Cache HTTP e response compression
- Monitoramento automatizado de performance
- Linting e testes de acessibilidade

## Arquitetura

### Refatorações
- [x] Migrar qualquer lógica pesada restante de models/controllers para services, queries e presenters dedicados
  - ✅ Query objects implementados para todos os recursos
  - ✅ Services implementados (Matches::Finalize, RoundStatistics, Players::AddToRound, etc.)
  - ✅ Presenters e Serializers implementados
- [x] Adotar objetos serializer consistentes e atualizar controllers para renderizá-los
  - ✅ Serializers implementados (PlayerSerializer, TeamSerializer, RoundSerializer, PlayerStatSerializer)
  - ✅ Controllers atualizados para usar serializers
- [ ] Adicionar regras de policy/authorization quando os requisitos forem definidos
  - ⚠️ Filtros por `user_id` implementados (escopo de dados por administrador)
  - ⚠️ Regras de policy formais ainda não implementadas

## Testes

### Cobertura
- [x] Continuar expandindo a cobertura de testes para atingir a meta de 80% (baseline)
  - ✅ **Cobertura atual: 87.55%** (meta de 80% atingida e superada)
  - ✅ **Cobertura de branches: 70.03%** (meta de 70% atingida)
  - ✅ **815 testes passando, 0 falhas, 0 pendentes**
  - 📄 Ver [Test Prioritization](test_prioritization.md) para detalhes
- [x] Revisar arquivos com baixa cobertura e adicionar testes conforme necessário
  - ✅ Fases 1-5 de priorização de testes completas
  - ✅ Todos os controllers, services, queries e presenters críticos testados

### Performance de Testes
- [x] Implementar paralelização de testes usando `parallel_tests`:
  - [x] Adicionar `gem 'parallel_tests'` ao Gemfile ✅
  - [x] Executar `bundle exec rake parallel:create` ✅
  - [x] Executar `bundle exec rake parallel:migrate` ✅
  - [x] Configurar execução: `bundle exec rake parallel:spec` ✅
  - 📄 Ver [Test Performance Optimizations](test-performance-optimizations.md) para recomendações
- [x] Considerar implementar Database Cleaner se houver problemas com transactional fixtures
  - ✅ Database Cleaner implementado com estratégia `:truncation` para testes paralelos
  - ✅ Configurado para funcionar em ambientes Docker (workaround para safeguard)
  - ✅ Evita deadlocks com counter caches em execuções paralelas
- [x] Implementar separação de testes rápidos (unitários) e lentos (integração) usando tags:
  - [x] Marcar testes lentos com `:slow` ✅
  - [x] Configurar execução: `bundle exec rspec --tag '~slow'` (apenas rápidos) ✅
  - [x] Configurar execução: `bundle exec rspec --tag slow` (apenas lentos) ✅
  - 📄 Ver [Test Performance Optimizations](test-performance-optimizations.md) para detalhes

### Boas Práticas de Testes
- [ ] Revisar testes existentes para usar `build_stubbed` quando possível (mais rápido que `create`)
  - 📄 Ver [Test Performance Optimizations](test-performance-optimizations.md) para recomendações
- [x] Revisar testes para evitar N+1 queries usando `includes` e `preload`
  - ✅ Bullet habilitado em test com `raise = true` para detectar N+1 automaticamente
- [x] Executar periodicamente `bundle exec rspec --profile` para identificar testes lentos
  - ✅ Profile habilitado (`config.profile_examples = 10`)

## Performance

### Monitoramento
- [ ] Configurar monitoramento automatizado de performance (Skylight ou similar)
- [x] Revisar logs do Bullet regularmente para identificar queries N+1
  - ✅ Bullet gem habilitado em development
  - ✅ Bullet habilitado em test com `raise = true` para falhar em N+1 durante CI
- [ ] Implementar cache onde apropriado
  - ⚠️ Cache HTTP não implementado (ver seção Otimizações abaixo)

### Otimizações
- [x] Revisar e adicionar índices de banco de dados conforme necessário
  - ✅ Migration `20260124193202_add_performance_indexes.rb` adiciona índices para:
    - `rounds.round_date`, `championships.updated_at`, `matches.created_at`
    - Índices compostos para matches (`round_id + team_1_id`, `round_id + team_2_id`)
    - `players.name`, `teams.name`
- [x] Considerar implementar counter caches para associações frequentes
  - ✅ Migration `20260124193201_add_counter_cache_columns.rb` adiciona:
    - `championships.rounds_count`, `championships.players_count`
    - `rounds.matches_count`, `rounds.players_count`
    - `teams.players_count`, `players.player_stats_count`
  - ✅ Models atualizados com `counter_cache: true` ou `counter_cache: :column_name`
  - ✅ Rake task `counter_cache:recalculate` disponível
- [x] Otimizar queries complexas usando query objects
  - ✅ Query objects implementados para todos os recursos:
    - `Championships::CollectionQuery`, `Rounds::CollectionQuery`, `Matches::CollectionQuery`
    - `Teams::CollectionQuery`, `Players::CollectionQuery`, `PlayerStats::CollectionQuery`
    - `FindQuery` para cada recurso
  - ✅ Eager loading com `includes` e `preload` implementado
  - ✅ Paginação, sparse fieldsets e includes implementados
  - 📄 Ver [Feature Checklist](feature-checklist.md) para detalhes

### Otimizações Adicionais (Pendentes)
- [ ] Implementar cache HTTP com headers apropriados (`Cache-Control`, `ETag`)
- [ ] Response compression (gzip/brotli)
- [ ] Usar `select` para limitar colunas do banco quando possível
- [ ] Criar endpoints específicos para diferentes contextos (ex: `/rounds/:id/summary`)
- [ ] Adicionar headers de performance (`X-Response-Time`, `X-Total-Count`)
- 📄 Ver [Feature Checklist](feature-checklist.md) seção "Otimização de Responses de API" para detalhes

## Acessibilidade

- [ ] Implementar linting de acessibilidade automatizado
- [ ] Adicionar testes de acessibilidade para UI (quando aplicável)
- [x] Revisar e melhorar PWA manifest conforme necessário
  - ✅ PWA manifest atualizado com language, short name, categories e high-contrast theme colors
  - ✅ API payloads padronizados através de presenters/serializers
  - 📄 Ver [Performance & Accessibility](performance_accessibility.md) para detalhes

## Tooling & Debugging

- [x] Configurar RuboCop/Standard style configs (se ainda não estiver configurado)
  - ✅ RuboCop configurado com `rubocop-rails-omakase`, `rubocop-rspec`, `rubocop-factory_bot`
  - ✅ Executável via `bundle exec rubocop`
- [ ] Documentar ferramentas de debugging (pry-byebug, etc.)
- [ ] Adicionar gems de profiling de performance se necessário

## Documentação

- [x] Atualizar documentação conforme novas features são adicionadas
  - ✅ Documentação consolidada e atualizada (Janeiro 2026)
- [x] Manter referências cruzadas entre documentos atualizadas
  - ✅ Referências cruzadas adicionadas entre documentos relacionados
- [x] Revisar periodicamente a documentação para garantir precisão
  - ✅ Última revisão: Janeiro 2026

## Observações

- Algumas tarefas podem depender de requisitos ou decisões de negócio (ex: authorization rules)
- Priorizar tarefas baseado em impacto e necessidade do projeto
- Revisar este checklist periodicamente e atualizar conforme o projeto evolui
- Para funcionalidades específicas do projeto, consulte [Feature Checklist](feature-checklist.md)
- Para status detalhado de testes, consulte [Test Prioritization](test_prioritization.md)