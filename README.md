# Leitor de Texto (OCR) — Total Civil

Programa grátis para quem tem cadastro no [Total Civil](https://totalcivil.com.br/).
Endereço: **https://igororcobras-hash.github.io/leitor-ocr/**

- Lê **todo o texto** de imagens (documentos fotografados, prints, placas, fotos de campo) e entrega em caixa de texto **editável** (copiar, `.txt`, CSV).
- Em fotos com tarja (Timemark, GPS Map Camera, carimbo de vistoria), reconhece **coordenada, endereço e data** por três fontes: o arquivo (nome e propriedades/GPS), a tarja (1ª leitura, com votação) e a 2ª leitura (recortes extras e graus com símbolos perdidos).
- Funciona em **todo o Brasil**: região pela coordenada de referência + raio limite, pela cidade ou automática (GPS das fotos ou endereço lido na tarja).
- Pergunta se quer **georreferenciar no Google Earth** (abre no Google Earth Web ou gera KMZ com a foto e o texto).
- Tudo é lido no computador de quem usa; as imagens não são enviadas a nenhum servidor.

## Arquivos
- `index.html` — o programa (arquivo único). Login próprio com a conta do Total Civil (Firebase `calculadora-dnit`).
- `tesseract/` — cópia do Tesseract.js 5.1.1, núcleo WebAssembly e português (`4.0.0_best_int`). Se não carregar, o programa usa a CDN jsdelivr.
- `.nojekyll` — GitHub Pages serve os arquivos como estão.

## Publicação (GitHub Pages)
Settings › Pages › *Deploy from a branch* › `main` / `(root)`.
O domínio `igororcobras-hash.github.io` precisa estar em Firebase › Authentication › Configurações › **Domínios autorizados**.
