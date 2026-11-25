# Leitor de Texto (OCR) — notas para quem mexe no código

Dono: Igor (engenheiro civil, não programador). Responder sempre em português, com link direto e nome exato do arquivo em cada passo. Só publicar quando ele disser "pode publicar".

## Origem
Criado em 25/11/2026 a partir do Imagens GoogleEarth (repositório `totalcivil-site`, `googleearth/index.html`, versão GE-20). Por pedido do dono, cada programa novo fica em **repositório próprio** e **não aparece na página principal** do site; este é **grátis para quem tem cadastro** no Total Civil.

## Estrutura do `index.html`
1. Tela de login (`#porta` + `<script type="module">`): Firebase 12.19.0 (gstatic), e-mail/senha, "Esqueci minha senha", link para criar conta no site. Entra qualquer conta; bloqueia só `usuarios/{uid}.recusado == true`. O github.io não enxerga a sessão do totalcivil.com.br, por isso o login é próprio.
2. MOTOR (1º `<script>` comum): COPIADO SEM ALTERAÇÃO do `googleearth/index.html` (coordenadas/endereço/data, `abrirImagem`, `lerExif`, bloco GE-01/03, leitor `ge16`, `__ge16Frouxa`, `lerDeNovo` do GE-20). Única diferença: `criarLeitor` usa `tesseract/` deste repositório (fora de http(s) vai direto à CDN). Para melhorar o OCR, corrigir no Imagens GoogleEarth e copiar de novo.
3. Aplicativo (2º `<script>`): texto completo (PSM 3; foto → PSM 11 com linhas de confiança ≥ 70), opção das 3 fontes de localização, `consolidar` (linhas da tarja lidas 2+ vezes), "[reconhecido pelo programa]", caixa editável, TXT/CSV, pergunta de georreferenciar (Google Earth Web e KMZ até 50 pontos).

## Regras aprendidas
- Coordenada a > 15 km da região de referência é SUSPEITA: a releitura ampliada da linha leu "22,5316S" como 22,9316 em várias leituras (fotos de vistoria de 29/10/2026). Força a 2ª leitura; se nada cair na região, procura linha por linha no texto lido; se ainda assim nada, mostra "suspeita — confira na foto", não entra no KMZ e o Google Earth Web usa o endereço.
- Endereço só entra se plausível (sem texto embaralhado, sem rua terminando em "da/de/do").
- Ver também a skill `ocr-fotos-tarja`.

## Testes (25/11/2026, Tesseract.js real no Chromium, 2 núcleos; Firebase simulado)
Login: senha errada, conta recusada, conta normal, sair. Documento gerado 100% em 1 s; 8 fotos de vistoria em 55 s (6 com coordenada certa, 2 suspeitas avisadas, endereços certos); 2 fotos Timemark ~27 s cada; aberto por file:// com o leitor da internet. NÃO testado: Firebase real neste domínio, celular, HEIC, PDF.
