# Knowledge Base - Sistema Multi-Perfil Link in Bio

## 📋 Visão Geral do Projeto

### Conceito
Fork do Linktree com diferencial único: **múltiplos perfis de links** para diferentes contextos.

### Diferencial Competitivo
- Usuário pode criar vários "link na bio" separados
- Cada perfil tem seu próprio conjunto de links
- Perfis categorizados por contexto: social, pessoal, profissional, negócio, etc.

## 🎯 Casos de Uso

### Usuário Típico
Um influencer que possui:
- **Perfil Social**: Links para Instagram, TikTok, YouTube
- **Perfil Profissional**: LinkedIn, portfólio, currículo
- **Perfil Negócio**: Loja online, produtos, serviços
- **Perfil Pessoal**: Blog, hobbies, projetos pessoais

### Benefícios
1. Organização por contexto
2. URLs diferentes para cada perfil
3. Personalização individual de cada perfil
4. Análise de cliques separada por perfil
5. Ativação/desativação de perfis específicos

## 🏗️ Arquitetura do Sistema

### Stack Tecnológica
- **Backend**: Laravel 10+ (PHP)
- **Frontend**: Inertia.js + Vue.js/React
- **Autenticação**: Laravel Jetstream (Sanctum)
- **Banco de Dados**: MySQL/PostgreSQL
- **Estilização**: Tailwind CSS
- **Build**: Vite

### Estrutura de Dados

#### Tabela: `users`
```
- id (PK)
- name
- email
- password
- profile_photo_path
- timestamps
```

#### Tabela: `profiles`
```
- id (PK)
- user_id (FK -> users.id)
- name (varchar) - Ex: "Social", "Profissional"
- slug (varchar, unique) - Ex: "joao-social", "joao-profissional"
- type (enum) - social, professional, business, personal, other
- description (text, nullable)
- avatar (varchar, nullable)
- cover_image (varchar, nullable)
- bio (text, nullable)
- theme (json) - Cores, fontes, estilo
- is_active (boolean, default: true)
- order (integer, default: 0)
- meta_title (varchar, nullable)
- meta_description (text, nullable)
- custom_css (text, nullable)
- view_count (integer, default: 0)
- timestamps
- soft_deletes
```

#### Tabela: `links`
```
- id (PK)
- profile_id (FK -> profiles.id)
- title (varchar)
- url (text)
- description (text, nullable)
- icon (varchar, nullable) - Font Awesome, SVG, ou emoji
- thumbnail (varchar, nullable)
- order (integer, default: 0)
- is_active (boolean, default: true)
- click_count (integer, default: 0)
- scheduled_start (datetime, nullable)
- scheduled_end (datetime, nullable)
- timestamps
- soft_deletes
```

#### Tabela: `link_clicks` (Opcional - Analytics)
```
- id (PK)
- link_id (FK -> links.id)
- profile_id (FK -> profiles.id)
- ip_address (varchar, nullable)
- user_agent (text, nullable)
- referer (varchar, nullable)
- country (varchar, nullable)
- city (varchar, nullable)
- clicked_at (timestamp)
- timestamps
```

### Relacionamentos

```
User (1) ──── (N) Profile
Profile (1) ──── (N) Link
Link (1) ──── (N) LinkClick
```

## 🎨 Funcionalidades

### 1. Gestão de Perfis

#### Dashboard do Usuário
- Listar todos os perfis criados
- Criar novo perfil
- Editar perfil existente
- Excluir perfil (soft delete)
- Reordenar perfis
- Ativar/desativar perfis
- Visualizar estatísticas por perfil

#### Criação de Perfil
Campos obrigatórios:
- Nome do perfil
- Tipo (categoria)
- Slug único

Campos opcionais:
- Avatar
- Imagem de capa
- Bio/descrição
- Tema personalizado
- Meta tags (SEO)
- CSS customizado

#### Tipos de Perfil Predefinidos
1. **Social** - Redes sociais pessoais
2. **Professional** - Carreira e networking
3. **Business** - Negócios e vendas
4. **Personal** - Projetos pessoais
5. **Other** - Personalizado

### 2. Gestão de Links

#### Por Perfil
- Adicionar novo link
- Editar link existente
- Excluir link
- Reordenar links (drag & drop)
- Ativar/desativar link
- Agendar exibição de link (início/fim)
- Visualizar cliques por link

#### Campos do Link
- Título
- URL destino
- Descrição (opcional)
- Ícone/emoji
- Thumbnail (opcional)
- Ordem de exibição

### 3. Personalização Visual

#### Temas Predefinidos
- Light
- Dark
- Gradient
- Minimal
- Bold
- Custom

#### Opções de Customização
```json
{
  "background": {
    "type": "color|gradient|image",
    "value": "#ffffff"
  },
  "card": {
    "background": "#f0f0f0",
    "borderRadius": "8px",
    "shadow": "md"
  },
  "text": {
    "color": "#000000",
    "fontFamily": "Inter"
  },
  "button": {
    "background": "#3b82f6",
    "color": "#ffffff",
    "hoverBackground": "#2563eb"
  }
}
```

### 4. URLs Públicas

#### Estrutura de URLs
```
https://iami.ao/@username              -> Lista todos os perfis públicos do usuário
https://iami.ao/@username/social       -> Perfil "Social" específico
https://iami.ao/@username/profissional -> Perfil "Profissional" específico
```

#### Página de Perfil Público
- Avatar/foto
- Nome do perfil
- Bio
- Lista de links ativos
- Contador de visualizações
- Compartilhamento social
- Tema personalizado aplicado

### 5. Analytics

#### Métricas por Perfil
- Total de visualizações
- Total de cliques
- Taxa de cliques (CTR)
- Gráfico de visualizações (últimos 30 dias)
- Top 5 links mais clicados

#### Métricas por Link
- Total de cliques
- Última clique
- Gráfico de cliques (últimos 30 dias)
- Origem dos cliques (referer)
- Localização geográfica (opcional)

### 6. Funcionalidades Adicionais

#### Compartilhamento
- QR Code para cada perfil
- Botões de compartilhamento social
- Link curto (opcional)

#### SEO
- Meta tags personalizadas por perfil
- Open Graph tags
- Twitter Cards
- Sitemap.xml automático

#### Segurança
- Perfis privados (requer senha)
- Perfis apenas para usuários autenticados
- Rate limiting em cliques
- Proteção contra bots

## 🚀 Roadmap de Implementação

### Fase 1: Estrutura Base (Semana 1)
- [ ] Criar migrations (users, profiles, links)
- [ ] Criar models com relacionamentos
- [ ] Criar seeders para testes
- [ ] Configurar rotas básicas

### Fase 2: Backend API (Semana 2)
- [ ] Controllers para Profiles (CRUD)
- [ ] Controllers para Links (CRUD)
- [ ] Middleware de autorização
- [ ] Validações e Form Requests
- [ ] API Resources para JSON

### Fase 3: Dashboard Admin (Semana 3)
- [ ] Interface de listagem de perfis
- [ ] Formulário de criação/edição de perfil
- [ ] Interface de gestão de links
- [ ] Drag & drop para reordenação
- [ ] Upload de imagens (avatar, cover)

### Fase 4: Páginas Públicas (Semana 4)
- [ ] Layout público responsivo
- [ ] Página de listagem de perfis (@username)
- [ ] Página de perfil individual (@username/slug)
- [ ] Sistema de temas
- [ ] Aplicação de CSS customizado

### Fase 5: Analytics (Semana 5)
- [ ] Tracking de cliques
- [ ] Tracking de visualizações
- [ ] Dashboard de analytics
- [ ] Gráficos e relatórios
- [ ] Exportação de dados

### Fase 6: Funcionalidades Extras (Semana 6)
- [ ] QR Code generator
- [ ] Sistema de temas avançado
- [ ] Agendamento de links
- [ ] Perfis privados
- [ ] Integração com redes sociais

### Fase 7: Otimização (Semana 7)
- [ ] Cache de perfis públicos
- [ ] Otimização de queries
- [ ] Compressão de imagens
- [ ] CDN para assets
- [ ] SEO completo

### Fase 8: Testes e Deploy (Semana 8)
- [ ] Testes unitários
- [ ] Testes de integração
- [ ] Testes E2E
- [ ] Documentação de API
- [ ] Deploy em produção

## 📁 Estrutura de Arquivos

```
app/
├── Models/
│   ├── User.php
│   ├── Profile.php
│   ├── Link.php
│   └── LinkClick.php
├── Http/
│   ├── Controllers/
│   │   ├── ProfileController.php
│   │   ├── LinkController.php
│   │   ├── PublicProfileController.php
│   │   └── AnalyticsController.php
│   ├── Requests/
│   │   ├── StoreProfileRequest.php
│   │   ├── UpdateProfileRequest.php
│   │   ├── StoreLinkRequest.php
│   │   └── UpdateLinkRequest.php
│   ├── Resources/
│   │   ├── ProfileResource.php
│   │   └── LinkResource.php
│   └── Middleware/
│       └── ProfileOwnership.php
├── Services/
│   ├── ProfileService.php
│   ├── LinkService.php
│   └── AnalyticsService.php
└── Traits/
    └── HasSlug.php

database/
├── migrations/
│   ├── 2024_xx_xx_create_profiles_table.php
│   ├── 2024_xx_xx_create_links_table.php
│   └── 2024_xx_xx_create_link_clicks_table.php
├── seeders/
│   ├── ProfileSeeder.php
│   └── LinkSeeder.php
└── factories/
    ├── ProfileFactory.php
    └── LinkFactory.php

resources/
├── js/
│   ├── Pages/
│   │   ├── Dashboard.vue
│   │   ├── Profiles/
│   │   │   ├── Index.vue
│   │   │   ├── Create.vue
│   │   │   ├── Edit.vue
│   │   │   └── Show.vue
│   │   ├── Links/
│   │   │   ├── Index.vue
│   │   │   └── Manage.vue
│   │   ├── Analytics/
│   │   │   └── Index.vue
│   │   └── Public/
│   │       ├── UserProfiles.vue
│   │       └── ProfileView.vue
│   └── Components/
│       ├── ProfileCard.vue
│       ├── LinkCard.vue
│       ├── LinkForm.vue
│       ├── ThemeEditor.vue
│       ├── DragDropList.vue
│       └── AnalyticsChart.vue
└── views/
    └── public/
        └── profile.blade.php (fallback)

routes/
├── web.php
└── api.php

public/
├── profiles/
│   ├── avatars/
│   └── covers/
└── links/
    └── thumbnails/
```

## 🔐 Regras de Negócio

### Perfis
1. Usuário pode ter quantos perfis quiser
2. Slug deve ser único globalmente (username + slug)
3. Perfis inativos não aparecem na listagem pública
4. Apenas o dono pode editar seus perfis
5. Soft delete para preservar histórico

### Links
1. Link pertence a apenas um perfil
2. URL deve ser válida
3. Links inativos não aparecem no perfil público
4. Links agendados só aparecem no período definido
5. Ordem dos links pode ser customizada

### Segurança
1. Rate limiting: 100 requisições/minuto por IP
2. Validação de uploads (max 2MB, apenas imagens)
3. Sanitização de CSS customizado
4. Proteção XSS em bio e descrições
5. CORS configurado para tracking

### Performance
1. Cache de perfis públicos (5 minutos)
2. Eager loading de relacionamentos
3. Paginação em listas grandes
4. Lazy loading de imagens
5. CDN para assets estáticos

## 🎯 Métricas de Sucesso

### Técnicas
- Tempo de carregamento < 2s
- Uptime > 99.5%
- Zero erros críticos
- Cobertura de testes > 80%

### Negócio
- Taxa de conversão (visitante → clique): > 30%
- Tempo médio na página: > 30s
- Taxa de retenção: > 60%
- NPS (Net Promoter Score): > 8

## 📚 Referências

### Inspirações
- Linktree (https://linktr.ee)
- Bio.link (https://bio.link)
- Beacons (https://beacons.ai)
- Carrd (https://carrd.co)

### Documentação Técnica
- Laravel: https://laravel.com/docs
- Inertia.js: https://inertiajs.com
- Tailwind CSS: https://tailwindcss.com
- Vue.js: https://vuejs.org

## 🤝 Convenções de Código

### PHP
- PSR-12 para estilo de código
- Type hints obrigatórios
- DocBlocks em métodos públicos
- Usar Form Requests para validação

### JavaScript
- ESLint + Prettier
- Composition API (Vue 3)
- Nomenclatura camelCase
- Componentes reutilizáveis

### Database
- Nomenclatura snake_case
- Timestamps em todas as tabelas
- Soft deletes quando aplicável
- Índices em foreign keys

### Git
- Commits em português
- Formato: "tipo: descrição"
- Tipos: feat, fix, docs, style, refactor, test, chore
- Branch: feature/nome-da-feature

## 🐛 Possíveis Desafios

### Técnicos
1. **Performance**: Muitos perfis/links podem causar lentidão
   - Solução: Cache, paginação, lazy loading

2. **Slug Collision**: Conflito de slugs entre usuários
   - Solução: slug = username + profile-name

3. **Abuse**: Spam, links maliciosos
   - Solução: Moderação, rate limiting, blacklist

4. **Storage**: Upload de muitas imagens
   - Solução: S3/Cloud storage, compressão

### UX
1. **Complexidade**: Muitos perfis pode confundir
   - Solução: UI/UX simples, onboarding claro

2. **Mobile**: Gestão em mobile pode ser difícil
   - Solução: Design mobile-first, PWA

---

**Última atualização**: 2025-11-22
**Versão**: 1.0
**Status**: 📝 Planejamento
