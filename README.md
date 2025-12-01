# BSN Desafio técnico

## Pokemon - APP

Uma **Progressive Web App** híbrida construída com **Ionic 7 + Angular 15+** (Standalone Components), que consome a [PokeAPI](https://pokeapi.co/) para:

- Listar Pokémons com paginação infinita  
- Exibir detalhes completos (sprites, habilidades, movimentos e locais de encontro)  
- Marcar e gerenciar favoritos durante a sessão  

## Imagens do projeto em execução

| Lista de Pokémons | Detalhes | Favoritos |
|:-----------------:|:--------:|:---------:|
| ![home](https://i.imgur.com/o3uNBne.png) | ![details](https://i.imgur.com/L6hDpRi.png) | ![favorites](https://i.imgur.com/lFoToLk.png) |

## Como Funciona

1. **HomePage**
  -  Carrega lista paginada de Pokémons (getPokemonList)
  -  Scroll infinito dispara novas páginas
  -  Busca por nome/ID com debounce de 300ms (getPokemonDetails)
  -  Toggle de favorito em cada card

2. **DetailsPage**
  -  Recebe id por rota
  -  Carrega dados de /pokemon, /pokemon-species e /pokemon/{id}/encounters em paralelo
  -  Calcula taxa de captura em %
  -  Exibe sprites e extras via AlertController
  -  Toggle de favorito com feedback via Toast
    
3. **FavoritesPage**
  -  Lista itens que o usuário marcou na sessão
  -  Remove favorito com ícone de lixeira

Todos os dados de favoritos ficam em memória (serviço FavoritesService). Para persistir entre sessões, pode-se integrar Capacitor Storage ou localStorage.

## 🛠 Tecnologias Utilizadas

[![My Skills](https://skillicons.dev/icons?i=angular,html,typescript,sass,n&perline=10)](https://github.com/GilvanPOliveira)

## 📬 Contato

Se tiver dúvidas ou sugestões, fique à vontade para entrar em contato:
- E-mail: gilvanoliveira06@gmail.com
- Portifólio: [Gilvan Oliveira](https://gilvanpoliveira.github.io/)