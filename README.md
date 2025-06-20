# isphus1973.github.io

Blog pessoal construído com [Astro](https://astro.build/), pronto para deploy no GitHub Pages.

## Recursos

- Suporte a Markdown para posts
- Renderização de equações LaTeX com KaTeX (use `$...$` ou `$$...$$`)
- Tema responsivo com CSS puro
- SEO, sitemap, RSS e performance otimizados
- Deploy automático via GitHub Actions

## Estrutura do Projeto

```
├── public/
├── src/
│   ├── components/
│   ├── content/
│   ├── layouts/
│   └── pages/
├── astro.config.mjs
├── README.md
├── package.json
└── tsconfig.json
```

## Comandos

| Comando             | Ação                                         |
|--------------------|----------------------------------------------|
| `npm install`      | Instala as dependências                      |
| `npm run dev`      | Inicia o servidor local em `localhost:4321`  |
| `npm run build`    | Gera o site estático em `./dist/`            |
| `npm run preview`  | Visualiza o build localmente                 |

## Deploy no GitHub Pages

O deploy é feito automaticamente via GitHub Actions ao commitar na branch `main`.

## Como usar LaTeX

Basta escrever equações entre `$...$` (inline) ou `$$...$$` (bloco) nos seus arquivos Markdown.

---

Este projeto é baseado no template oficial de blog do Astro.
