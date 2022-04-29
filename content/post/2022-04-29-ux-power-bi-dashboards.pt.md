---
title: UX/UI em Dashboards de Business Intelligence
date: 2022-04-29 11:14:00 -0300
categories: [Blog]
tags: [UX, experiência de usuário, dashboard, powerbi, visualização de dados, business intelligence, storytelling, figma]
showtoc: true
---

# O que é UX/UI?

UX é a sigla em inglês para Experiência de Usuário, é um conceito recente que fala sobre tomar decisões de design pensando na experiência do usuário final. O designer de UX precisa se preocupar se seu produto é fácil de usar e intuitivo, fazendo mudanças nele sempre que necessário para se adequar as necessidades do usuário.

UI, do inglês, significa Interface do Usuário. É tudo aquilo que está envolvido na interação do usuário e o produto. O designer de UI é responsável por desenvolver interfaces, não limitado apenas aos aspectos visuais, também é importante garantir que sejam funcionais, usáveis, e que em geral, contribuem para uma boa experiência do usuário.

# Como Melhorar a Experiência dos Painéis de BI?

Muitos pessoas veem os painéis de BI como páginas de web e isso traz algumas expectativas de uso. Por exemplo, a maioria dos sites que possuem algum sistema de navegação usam um menu superior com botões, um menu lateral (mais comum no Brasil sendo na esquerda, mas em alguns países é na direita) ou um menu sanduíche (aquele que clicamos no botão e aparece as opções).

{{< figure src="https://ik.imagekit.io/devmedeiros/site_layout_C0BWt0-Rh.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651180441785" caption="Jaqueline Medeiros - Todos os direitos reservados" alt="Imagem com três layouts de website, o primeiro com barra de navegação superior, o segundo com menu lateral a esquerda e o terceiro com um menu sanduíche">}}

Com isso uma grande parcela das pessoas que utilizam os painéis esperam encontrar botões de navegação e segmentação (filtro) de dados nestes locais, além de outras informações como logo e título.

## Segmentação de Dados

Também comummente chamado de filtro de dados é uma peça fundamental de diversos _dashboards_, seu posicionamento precisa ser definido com cuidado, pois se estiver num lugar que o usuário não espera pode impedir que seu painel seja usado eficientemente, além disso manter um padrão visual para todos os seus filtros ajudam as pessoas a reconhecerem mais facilmente o que é ou não é um filtro.

{{< figure src="https://ik.imagekit.io/devmedeiros/base_7_vbzsb9L.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651183332760" alt="Imagem com seis filtros, distribuídos em duas colunas, cada uma com três. Um filtro é de cidade, o outro é de restaurante e o último é aceita reserva? A única diferença entre eles é o design, na esquerda são consistentes e na direita cada um tem um design único.">}}

Você pode e deve usar e experimentar com diversos temas nos seus trabalhos. O que importa, na hora de facilitar para os usuários, é a consistência, escolha um modelo de filtro com as cores desejadas e todas as especificações gráficas que são do seu interesse e utilize ela em todos os filtros, pois isso irá ajudar as pessoas a baterem o olho e reconhecer que aquilo é um filtro.

![Três filtros com, de cidade, restaurante e aceita reserva?. Em que o da cidade mostra uma borracha ao lado do título e o texto Limpar seleções](https://ik.imagekit.io/devmedeiros/borracha_Xwi_TYTHB.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651183773246#center)

É muito comum as pessoas quando estão lendo algo pelo computador posicionarem o ponteiro do mouse onde elas estão lendo. No caso do Power BI, isso torna mais fácil para o usuário encontrar o botão de limpar a segmentação de dados, pois ao passar o mouse no nome do filtro aparece uma borrachinha no lado esquerdo onde fica o nome do filtro. Tá mais se você usa o Power BI provavelmente já sabia disso, mas podemos melhorar isso usando algo que o usuário já conhece, que é a **borrachinha**, e criar um botão usando a borrachinha como ícone e fazer com que esse botão limpe todos os filtros ao mesmo tempo. Caso queira saber como fazer esse botão [aqui](https://community.powerbi.com/t5/Desktop/Clear-All-Slicers-by-one-button-in-power-bi-desktop/m-p/494518) no fórum do Power BI explica como.

Essa é uma característica que pode ter sua importância despercebida num primeiro momento, mas algo que ocorre muito é os usuários do painel não perceberem quais filtros eles usaram ou usarem tantos que só querem poder limpar a seleção de forma mais rápida e eficiente, então esse botão torna o processo muito mais _user friendly_.

## Navegação de Página

A navegação de páginas nativa do Power BI não é intuitiva para a maioria das pessoas e em muitas vezes você pode querer direcionar a navegação num fluxo específico que facilita o entendimento e contribui para o _storytelling_ planejado. Neste caso temos a opção de ocultar todas as abas do relatório, exceto uma que é a página de abertura/inicial. Mas qual a melhor forma de direcionar o usuário para as demais páginas? Bem, isso depende do que você está fazendo. Suponha que seu relatório seja bem simples, você tem uma aba de visão geral e outra aba com um detalhamento, um simples botão resolveria o seu problema, mas caso seu relatório seja muito extenso pode ser inviável colocar um botão para cada aba em todas as páginas.

{{< figure src="https://ik.imagekit.io/devmedeiros/infografico_kQJe_yhQD.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651241421864" caption="Jaqueline Medeiros - Todos os direitos reservados" alt="Um círculo com quatro retângulos em volta, uma linha liga os retângulos ao círculo" width="100%">}}

Neste caso pode ser interessante considerar ter uma página inicial que leva a todas as páginas do relatório e colocar um botão de `home` ou `voltar` nas outras páginas.

# Como Melhorar a Interface dos Dashboards?

## Programas de Prototipagem

Usar um programa específico para fazer o protótipo do seu painel permite uma liberdade artística maior comparado ao que os aplicativos de _Business Intelligence_ normalmente fornecem. O Figma é ótimo para isso, você pode criar backgrounds e protótipos avançados com ótima qualidade para usar em seus painéis de BI.

Veja um exemplo de um painel que eu fiz a alguns meses atrás:

{{< figure src="https://raw.githubusercontent.com/devmedeiros/Alura-Challenge-BI-2/main/Alura%20Food/alura%20food%20resume.png" title="Painel com os dados" alt="Na imagem pode-se ler o texto: Visão Geral do Mercado de Restaurantes Indianos Dev_Medeiros Cidade Restaurante Aceita Reservas? 19,21 % de Restaurantes com Entrega Online Avaliação Média 3,72 R$ 39,48 Preço Médio por Pessoa 9577 Total de Restaurantes 5 Cidades com mais Restaurantes 5 Cozinhas mais Populares">}}

O plano de fundo desse painel foi feito completamente no Figma, até mesmo alguns dos títulos dos visuais do BI.

{{< figure src="https://raw.githubusercontent.com/devmedeiros/Alura-Challenge-BI-2/main/Alura%20Food/Alura%20Food.png" title="Background feito no Figma" alt="Na imagem pode-se ler o texto: Visão Geral do Mercado de Restaurantes Indianos Dev_Medeiros Cidade Restaurante Aceita Reservas? % de Restaurantes com Entrega Online Avaliação Média Preço Médio por Pessoa Total de Restaurantes 5 Cidades com mais Restaurantes 5 Cozinhas mais Populares">}}

Com uma ferramenta dessa, a única coisa que o para é a sua imaginação.

## Figuras e Ícones

{{< figure src="https://img.freepik.com/vetores-gratis/funcionarios-dando-as-maos-e-ajudando-os-colegas-a-subir-as-escadas_74855-5236.jpg?w=1380&t=st=1651239796~exp=1651240396~hmac=1b2ab902322bbf8ce26e3bc83f602e69075fcc2e18f0afdc91a328c7942ba746" caption="Vetor criado por pch.vector - br.freepik.com" align="center">}}

Figuras e ícones quando usados corretamente ajudam a destacar o painel, o torna mais chamativo e bonito. Há diversas formas de se conseguir imagens, caso você ou sua equipe não consigam criar vocês mesmo existe a opção de usar plataformas online que disponibilizam imagens vetorizadas. Nessas plataformas existem as opções gratuitas, em que exigem algum tipo de atribuição, e opções _premium_ (pagas), em que muitas vezes não precisa atribuir o autor e possuem uma qualidade maior.

{{< figure src="https://img.icons8.com/dusk/344/cash-register.png" caption="Ícone de ICONS8" alt="Ícone de caixa registradora" align="center">}}
