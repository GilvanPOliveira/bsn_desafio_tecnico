# Documentação Técnica — Pokedex-App

## Sumário

1. [Visão Geral](#visão‐geral)  
2. [Estrutura de Diretórios](#estrutura‐de‐diretórios)  
3. [Roteamento e Bootstrap](#roteamento‐e‐bootstrap)  
4. [Modelos (Models)](#modelos‐models)  
5. [Serviços (Services)](#serviços‐services)  
   1. [LRU Cache](#lru‐cache)  
   2. [PokeAPI Service](#pokeapi‐service)  
   3. [Favorites Service](#favorites‐service)  
6. [Páginas (Pages)](#páginas‐pages)  
   1. [HomePage](#homepage)  
   2. [DetailsPage](#detailspage)  
   3. [FavoritesPage](#favoritespage)  
7. [Estilos e Temas](#estilos‐e‐temas)  
8. [Testes](#testes)  
9. [Boas Práticas & Melhorias Futuras](#boas‐práticas‐‐melhorias‐futuras)

---

## Visão Geral

A **Pokedex-App** é uma PWA móvel construída com **Ionic + Angular** (uso de *standalone components*) que consome a PokeAPI para:

- Listar pokémons com paginação infinita.  
- Exibir detalhes (sprites, habilidades, movimentos, locais de encontro).  
- Gerenciar favoritos em memória (sessão).

Adota arquitetura por componentes, roteamento centralizado (`routes.ts`), serviços para lógica de negócio e cache LRU para minimizar chamadas HTTP.

---

## Estrutura de Diretórios

```plaintext
src/
├── app/
│   ├── app.component.ts
│   ├── routes.ts
│   ├── models/
│   │   └── pokemon.model.ts
│   ├── services/
│   │   ├── lru-cache.ts
│   │   ├── pokeapi.service.ts
│   │   ├── favorites.service.ts
│   │   └── *.spec.ts
│   └── pages/
│       ├── home/
│       │   ├── home.page.ts
│       │   ├── home.page.html
│       │   └── home.page.scss
│       ├── details/
│       │   ├── details.page.ts
│       │   ├── details.page.html
│       │   └── details.page.scss
│       └── favorites/
│           ├── favorites.page.ts
│           ├── favorites.page.html
│           └── favorites.page.scss
├── environments/
│   ├── environment.ts
│   └── environment.prod.ts
├── global.scss
├── index.html
├── main.ts
├── polyfills.ts
├── test.ts
└── zone-flags.ts
```

---

## Roteamento e Bootstrap

main.ts
  ```bash
  import { bootstrapApplication } from '@angular/platform-browser';
  import { AppComponent } from './app/app.component';
  import { provideRouter } from '@angular/router';
  import { routes } from './app/routes';

  bootstrapApplication(AppComponent, {
    providers: [ provideRouter(routes) ]
  });
  ````

routes.ts
  ```bash
  import { HomePage }      from './pages/home/home.page';
  import { DetailsPage }   from './pages/details/details.page';
  import { FavoritesPage } from './pages/favorites/favorites.page';

  export const routes = [
    { path: '',            component: HomePage },
    { path: 'details/:id', component: DetailsPage },
    { path: 'favorites',   component: FavoritesPage },
  ];
  ```

Modelos (Models)
  ```bash  
  // pokemon.model.ts
  export interface PokemonListResponse { /* … */ }
  export interface PokemonSummary { /* … */ }
  export interface PokemonDetails { /* … */ }
  export interface Sprite { label: string; url: string; }
  export interface PokemonDetailsWithSprites extends PokemonDetails {
    spriteList: Sprite[];
  }
  ```

### Serviços (Services)

  LRU Cache (lru-cache.ts)
  ```bash
  export class LRUCache<K, V> {
    private cache = new Map<K, V>();
    constructor(private maxSize: number) { /* validação */ }
    get(key: K): V | undefined { /* move para recent */ }
    set(key: K, value: V): void { /* descarta LRU se necessário */ }
    clear(): void { this.cache.clear(); }
    size(): number { return this.cache.size; }
  }
  ```

  PokeAPI Service (pokeapi.service.ts)
  ```bash
  @Injectable({ providedIn: 'root' })
  export class PokeapiService {
    private listCache = new LRUCache<string, PokemonListResponse>(5);
    private detailCache = new LRUCache<string, PokemonDetailsWithSprites>(50);

    getPokemonList(offset = 0, limit = 20): Observable<PokemonListResponse> { /* … */ }
    getPokemonDetails(idOrName: number | string): Observable<PokemonDetailsWithSprites> { /* … */ }
    clearCache(): void { /* limpa caches */ }
  }
  ```

  Favorites Service (favorites.service.ts)
  ```bash
  @Injectable({ providedIn: 'root' })
  export class FavoritesService {
    private favorites: PokemonCard[] = [];
    getAll(): PokemonCard[]      { return [...this.favorites]; }
    isFavorite(id: number): boolean { /* … */ }
    add(card: PokemonCard): void  { /* … */ }
  remove(card: PokemonCard): void { /* … */ }
  toggleFavorite(card: PokemonCard): void { /* … */ }
  clear(): void { this.favorites = []; }
  }
  ```

---

## Páginas (Pages)

  HomePage
    -  Paginação infinita, busca com debounce, toggle de favoritos, navegação.
  DetailsPage
    -  Exibe sprites, habilidades, movimentos, captura e toggle de favorito com Toast.
  FavoritesPage
    -  Lista favoritos em memória, remoção direta, navegação.

---

## Estilos e Temas

  -  global.scss para resets e tipografia.
  -  theme/variables.scss para cores e fontes.
  -  Cada página tem seu .page.scss.

---

## Testes

  -  Unitários em services/*.spec.ts.
  -  Sugestões: testes de componente com IonicModule.forRoot(), E2E com Cypress.

---

## Boas Práticas & Melhorias Futuras

  -  Persistir favoritos (Storage/LocalStorage).
  -  Tratamento consistente de erros e carregamento.
  -  Acessibilidade (aria-attributes).
  -  Internacionalização (i18n).
  -  CI/CD.

---

## Aplicação híbrida (Ionic + Angular) que consome a PokeAPI para listar, detalhar e favoritar pokémons.

## 📦 Como rodar

1. Clone o repositório:  
   ```bash
   git clone https://seu-repo-url.git
   cd pokedex-app
   ```
   
2. Instale dependências:
  ```bash
  npm install
  ```

3. Rode em dev:
  ```bash
  npm start
  ```

---

## 🗂️ Estrutura

  -  src/app/
    -  routes.ts – rotas standalone
    -  models/ – interfaces de dados
    -  services/ – LRU cache, PokeAPI client, favoritos
    -  pages/ – Home, Details, Favorites
  -  src/environments/ – config de ambiente
  -  global.scss / theme/variables.scss – estilos globais e tema

---

## 📡 API

  -  Base URL: https://pokeapi.co/api/v2
  -  Endpoints usados: /pokemon, /pokemon-species, /{id}/encounters

---

## 🧪 Testes Unitários:
  ```bash
  ng test
  ```
E2E (sugestão): Cypress ou Playwright

---

Desenvolvido por Gilvan Oliveira, 2025.
