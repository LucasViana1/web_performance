# Projeto do Curso de Otimização de Performance Web do Alura

## Comandos

* Gulp-cli instalação: `npm install -g gulp-cli`
* Minificar arquivos html, css e js: `gulp minify`
* Executar todas otimizações `gulp` ou `gulp default`

## Requisições

* GZIP: Ele permite que os arquivos do site transitem, entre servidor e cliente, de forma compactada e ao chegar no cliente ocorre a descompactação dos arquivos. O [Brotli](https://github.com/google/brotli) é uma alternativa mais recente ao GZIP
* Sites que analisam a performance geral de um site:
    - [Web Page Test](https://www.webpagetest.org/)
    - [Page Speed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=pt-BR)

## Imagens

### Abordagens para diminuição do tamanho de arquivos de imagens:

* Redimensionamento: manter a dimenção da imagem de acordo com o tamanho que será exibido ao site, dessa forma não existe gasto de recurso operacional para fazer com que a imagem original seja diminuida, conforme a estilização da página;
* Lossless: Remoção de metadados que não impactaram na qualidade visual da imagem;
* Lossy: Remoção de dados da imagem, buscando a melhor otimização possível, ele pode admitir perca na qualidade de imagem. O fator qualidade visual e tamanho arquivo pode ser mensurado para trazer resultado satisfatorio, sem perdas significativas no design.

### Links sites para otimização de imagens

* SVG: [svgomg](https://jakearchibald.github.io/svgomg/)
* JPG/PNG: 
    - [kraken](https://kraken.io/web-interface)
    - [jpegmini](https://www.jpegmini.com/)
    - [tinypng](https://tinypng.com/)

### Sprites

Realiza a junção de um grupo de imagens em apenas uma, e com o CSS é controlado qual imagem será exibida.

#### Método manual (PNG)

Junção da imagens utilizando [Image Magick](https://imagemagick.org/index.php), em seguida o comando, como o exemplo:
`convert site/assets/img/*.png -append site/assets/img/diferenciais.png`
Isso irá retornar uma imagem com os sprites das imagens enviadas.

#### Método automatizado (PNG)

Utilizando cli como `sprity-cli` da [Sprity](https://www.npmjs.com/package/sprity), existindo meios tanto para geração automatica de imagens e css

#### SVG

Os sprites em imagens vetoriais são organizadas da forma em que cada imagem é uma `symbol` dentro do svg com sprites
~~~
<svg width="0" height="0" version="1.1" xmlns="http://www.w3.org/2000/svg">
<defs>

<symbol id="mobile" viewBox="0 0 22 33">
    <path d="M17.612 29.584H3.604c-.384 0-.662-.304-.662-.76v-5.51h15.332v5.51c0 .456-.278.76-.662.76zM3.604 2.942h14.008c.384 0 .662.29.662.654v17.396H2.942V3.596c0-.364.278-.654.662-.654zm17.613.697A3.64 3.64 0 0 0 17.577 0H3.64A3.64 3.64 0 0 0 0 3.64v25.24a3.64 3.64 0 0 0 3.64 3.64h13.937a3.64 3.64 0 0 0 3.64-3.64V3.64zM10.837 25h-.175C9.744 25 9 25.783 9 26.75s.744 1.75 1.662 1.75h.175c.92 0 1.663-.783 1.663-1.75S11.756 25 10.837 25"/>
</symbol>

...

</defs>
</svg>
~~~

No exemplo o `id="mobile"` é importante para posteriormente o referenciarmos no html:

~~~
<svg class="categoriaCard-item-icone"><use xlink:href="assets/img/categorias.svg#mobile"/></svg>
~~~

## Inline

`gulp useref` -> cli gulp.

O **inline** é um atributo que pode ser passado nas importações recursos do projeto, externos ao html principal (como js, svg e outros arquivos do tipo texto). Ele permite que o código escrito em arquivos separados sejam inseridos direto no html, dessa forma não tendo custo em realizar a requisição desse recurso.
**ATENÇÃO!** Utilizar esse recurso em arquivos externos muito extensos pode ser prejudicial a performance, logo é recomendado essa pratica em arquivos pequenos.

Uma desvantagem do **inline** é que as importação que a utilizaram não poderam ter controle do cash, além da página recarregar novamente esse recursos conforme a navegação do usuário (isso pode variar da arquitetura).
O tamanho do html aumenta, para se ter uma ideia de até que ponto utilizar o **inline** em questão do tamanho que a requisição inicial irá realizar, deve se considerar que o servidor foi configurado para 10 segmentos de conexões TCP, logo sua janela inicial será de 14KB já com gzip e os headers.

## Paralelização de requisições

A paralelização de requisições consiste em colocar alguns recursos (normalmente importações de imagens) em um dominio diferente da página, dessa forma separando as responsabilidades no carregamento, que antes aceitaria 6 conexões TCP em paralelo pode aceitar praticamente 12 conexões em paralelo com uso de um dominio adicional.

<hr />

## Critical Rendering Path

Se refere a ordem de renderização de uma página, que por padrão realiza a chamada dos recursos externos ao index (scripts e css) na sequencia que foram importados, apenas após o carregamento desses recursos é que os elementos da tag `<body>` são chamados, podemos dizer que essas importações são bloqueantes do ponto de vista de renderização da página.
Uma maneira de melhor a experiencia do usuário para esse caso seria realizar a importação dos scripts ao final da tag `<body>`, após a construção dos elementos da DOM, dessa maneira evita que os scripts bloqueam a renderização das tags na *Render Tree*.

![Critical Rendering Path](https://res.infoq.com/presentations/critical-rendering-path/pt/slides/sl8.jpg)

## Async
Por padrão as importação dos scripts são carregados e executados em sequência, o que em determinados cenários pode trazer atrasos na execução da sequência de scripts. Uma alternativa é determinar que o script é não bloqueante (assíncrono), ou seja, não depende de outros scripts e após ser carregado já pode ser executado imadiatamente. Exemplo de sintaxe:
`<script async src="assets/js/menu.js"></script>`
