# Projeto do Curso de Otimização de Performance Web do Alura

## Comandos

* Gulp-cli instalação: `npm install -g gulp-cli`
* Minificar arquivos html, css e js: `gulp minify`

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
