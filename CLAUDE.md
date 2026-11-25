# Leitor de Texto (OCR) — notas para quem mexe no código

Dono: Igor (engenheiro civil, não programador). Responder sempre em português, com link direto e nome exato do arquivo em cada passo. Só publicar quando ele disser "pode publicar".

## Origem
Criado em 25/11/2026 a partir do Imagens GoogleEarth (repositório `totalcivil-site`, `googleearth/index.html`, versão GE-20). Por pedido do dono, cada programa novo fica em **repositório próprio** e **não aparece na página principal** do site; este é **grátis para quem tem cadastro** no Total Civil.

## Estrutura do `index.html`
1. Tela de login (`#porta` + `<script type="module">`): Firebase 12.19.0 (gstatic), e-mail/senha, "Esqueci minha senha", link para criar conta no site. Entra qualquer conta; bloqueia só `usuarios/{uid}.recusado == true`. O github.io não enxerga a sessão do totalcivil.com.br, por isso o login é próprio.
2. MOTOR (1º `<script>` comum): COPIADO SEM ALTERAÇÃO do `googleearth/index.html` (coordenadas/endereço/data, `abrirImagem`, `lerExif`, bloco GE-01/03, leitor `ge16`, `__ge16Frouxa`, `lerDeNovo` do GE-20). Única diferença: `criarLeitor` usa `tesseract/` deste repositório (fora de http(s) vai direto à CDN). Para melhorar o OCR, corrigir no Imagens GoogleEarth e copiar de novo.
3. Aplicativo (2º `<script>`): texto completo (PSM 3; foto → PSM 11 com linhas de confiança ≥ 70), opção das 3 fontes de localização, `consolidar` (linhas da tarja lidas 2+ vezes), "[reconhecido pelo programa]", caixa editável, TXT/CSV, pergunta de georreferenciar (Google Earth Web e KMZ até 50 pontos).

## Regras aprendidas
- TODO O BRASIL (OCR-02, 25/11/2026): nada fixo em Rio das Ostras. Região das fotos, nesta prioridade: (1) **coordenada de referência** digitada + **raio limite** (km, padrão 30); (2) **cidade** digitada (localizada no OpenStreetMap; se falhar, Photon); (3) automática: GPS/propriedades/nome do arquivo das fotos; senão o **endereço lido na tarja** localizado no mapa (`regiaoPeloEndereco`). Coordenada lida pelo OCR NUNCA define a região: nas fotos de vistoria a 1ª e a 2ª leitura erraram o mesmo número (5→9) em algumas fotos, e a maioria das fotos tinha o erro.
- Coordenada fora do raio = SUSPEITA (não entra no KMZ; o Google Earth Web usa o endereço). Se nenhuma cair no raio, procura linha por linha no texto lido (`avaliarRegiao`). No fim do lote tudo é reavaliado com a região final (texto editável só é refeito se o usuário não mexeu).
- Sem região fixa (coordenada/cidade), a 2ª leitura roda sempre, para conferir; aviso quando a tarja foi lida de formas diferentes (2+ leituras de cada versão).
- Cidade lida na tarja (`cidadeDoTexto`: “Cidade - UF”, “Cidade/UF”, “Cidade, UF”, 27 siglas) entra na busca do endereço no Google Earth. `REF_PADRAO` do motor = região atual ou centro do Brasil (âncora da leitura tolerante de graus e do hemisfério).
- Endereço só entra se plausível (sem texto embaralhado, sem rua terminando em "da/de/do").
- Ver também a skill `ocr-fotos-tarja`.

## Testes OCR-02 (todo o Brasil): foto com tarja de Campinas (desenhada) sem referência → região pelo endereço, coordenada aceita, cidade “Campinas - SP”; com referência de Rio das Ostras e raio 30 → suspeita (526 km); 8 fotos de vistoria sem referência → região pelo endereço, 6 certas + 2 suspeitas; com coordenada de referência → idem. Mapas simulados (Nominatim real não testado).

## Testes (25/11/2026, Tesseract.js real no Chromium, 2 núcleos; Firebase simulado)
Login: senha errada, conta recusada, conta normal, sair. Documento gerado 100% em 1 s; 8 fotos de vistoria em 55 s (6 com coordenada certa, 2 suspeitas avisadas, endereços certos); 2 fotos Timemark ~27 s cada; aberto por file:// com o leitor da internet. NÃO testado: Firebase real neste domínio, celular, HEIC, PDF.
