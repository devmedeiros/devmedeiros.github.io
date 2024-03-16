---
title: "Como Adicionar Curtidas/Aplausos em um Site Estático com o Lyket"
date: 2023-08-05 07:30:00 -0300
categories: [Blog]
tags: [tutorial, like button, clap button, static site, medium clap, twitter heart, lyket, hugo, jekyll]
showtoc: true
summary: Aprenda como adicionar curtidas/aplausos em um site estático usando o Lyket.
cover:
    image: "https://ik.imagekit.io/devmedeiros/like-static-blog_tR0o74mLu.webp?tr=w-700"
    alt: "laptop preto e branco com três emojis, joinha, coração e palmas"
---

Este tutorial é focado no **Hugo**, mas pode ser facilmente adaptado para outros frameworks de sites estáticos.

Encontrei muitas alternativas para adicionar curtidas ou aplausos em um site, mas nenhuma era personalizável como eu queria ou exigiria que eu mudasse de hospedagem.

Recentemente, descobri o Lyket e comecei a usá-lo. Esse guia é para mostrar como configurá-lo em seu site.

## Configurando API do Lyket

A API do Lyket oferece uma opção gratuita, sem necessidade de cartão de crédito, para se inscrever. Para usá-lo em um site estático, você precisará adicionar a seguinte tag ao header do site.

```html
<script src="https://unpkg.com/@lyket/widget@latest/dist/lyket.js?apiKey=[SUA-CHAVE-DA-API]"></script>
```

Você pode obter uma chave de API ao se inscrever no site << https://lyket.dev/ >>. Você pode usar seu token de API público sem se preocupar com outras pessoas o usando, basta definir os sites permitidos para ser apenas o seu domínio.

Em seguida, você precisa adicionar a tag do botão onde deseja que apareça. No meu caso, eu queria que aparecesse em todas as postagens do blog, mas não nas outras páginas, então eu adicionei ao rodapé dos post, localizado no arquivo `layouts/_default/single.html` e colei a tag do botão dentro da tag `<footer class="post-footer">`.

O modelo de botão html ficará mais ou menos assim:

```html
<div
    data-lyket-type="clap"
    data-lyket-namespace="namespace"
    data-lyket-id="everybody-clap-now"
    data-lyket-template="medium"
    data-lyket-color-text="currentColor"
/>
```

Você pode alterar o `data-lyket-type` para clap, like, updown e rate. Em `data-lyket-namespace`, você pode escolher o nome que quiser, pode usá-lo para separar e organizar suas páginas se usar o mesmo token de API em vários sites ou separar o conteúdo do site em categorias, como "blog" e "projetos" por exemplo.

No `data-lyket-template` você pode escolher o modelo do seu botão, mas isso é opcional. Cada tipo de botão tem um modelo diferente e se você não adicionar, ele vai seguir a opção padrão. Você pode ver o opções disponíveis no site oficial: << https://lyket.dev/templates >>.

O `data-lyket-color-text` permite que você personalize a cor do texto do seu botão (a cor dos números). Você pode definir uma cor específica, mas se você tiver um site que tenha um modo claro e escuro, você pode prefir usar 'currentColor'.

O `data-lyket-id` define o nome do contador. Se você quiser que duas páginas compartilhem a mesma contagem de curtidas, o contador precisará ter o mesmo id. No meu site, eu usei Hugo para obter o slug da página para usá-lo como um id, você pode usar o modelo `{{ replace (path.Base .RelPermalink) " /" "-" }}` como seu id, então mesmo se você traduzir seu conteúdo, todas as versões manterão a contagem de curtidas.

```html
<div
    data-lyket-type="clap"
    data-lyket-namespace="namespace"
    data-lyket-id="{{ replace (path.Base .RelPermalink) " /" "-" }}"
    data-lyket-template="medium"
    data-lyket-color-text="currentColor"
/>
```

## Dica bônus

Você pode adicionar um parâmetro Hugo para ocultar o botão de curtidas nas páginas que não deseja exibi-lo. Basta adicionar um parâmetro ao frontmatter da sua página, neste caso `noLike`, e configurá-lo como true, para que sua página fique assim:

```yaml
---
title: "Seu incrível título do post"
layout: "single"
noLike: true
---
```

No seu arquivo single.html, adicione a cláusula if para ocultar o botão. No final, ficará assim:

```html
{{- if not .Params.noLike }}
    <div
        data-lyket-type="clap"
        data-lyket-namespace="namespace"
        data-lyket-id="{{ replace (path.Base .RelPermalink) " /" "-" }}"
        data-lyket-template="medium"
        data-lyket-color-text="currentColor"
    />
{{- end }}
```