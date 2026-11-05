# Layout SRM — Mapa de Colaboradores

Aplicação web de mapa de lugares para gerenciamento de colaboradores por andar.

## Como usar

Abra o arquivo `index.html` diretamente no navegador (Chrome ou Edge recomendado).

> **Importante:** Use Chrome ou Edge para habilitar o salvamento automático no próprio arquivo HTML via File System Access API.

## Funcionalidades

- Visualização do mapa de lugares por andar
- Alocação e movimentação de colaboradores
- Filtro por área, subárea e status
- Cores por área configuráveis
- Publicação das alterações diretamente no arquivo HTML
- Visão geral de ocupação, devices e distribuição por área
- Renumeração de lugares em cascata
- Adição de múltiplos lugares de uma vez
- Menu de contexto (botão direito) em lugares ocupados e livres

## Tecnologia

- HTML/CSS/JS puro — sem dependências externas
- Dados embutidos na própria página (`<script id="app-data">`)
- Persistência via File System Access API + localStorage
