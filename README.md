# ✍️ Quadro Mágico

Transforme uma imagem estilo quadro branco em um vídeo de **whiteboard animation** — direto no navegador, grátis, sem enviar nada para servidor nenhum (a imagem nunca sai do seu computador).

## Como usar

1. Abra o app e arraste uma imagem de traço sobre fundo claro (PNG ou JPG) — ou clique em "Testar com um desenho de exemplo".
2. Ajuste duração, formato (16:9, 9:16 ou 1:1), ordem do desenho (de cima pra baixo, texto primeiro/último etc.) e a mão.
3. Veja a prévia e clique em **Gerar vídeo** — sai um MP4 pronto para WhatsApp, Instagram ou YouTube.

## Mão personalizada

Você pode usar uma foto da sua própria mão segurando a caneta:

- recorte guiado do fundo (varinha por clique, borracha, limpador de pontinhos soltos e zoom 2×);
- marcação da ponta da caneta;
- o braço é prolongado automaticamente até a borda do vídeo, como uma filmagem real;
- a mão fica salva no navegador para os próximos vídeos.

## Como funciona por dentro

Tudo é JavaScript puro em um único `index.html`:

- os traços são detectados por limiar de cor e separados em componentes conectados;
- uma "caneta virtual" percorre cada traço pelo caminho natural (BFS duplo acha a ponta; um disco varre seguindo sempre o trecho não desenhado mais próximo);
- a animação é desenhada em canvas e gravada com `MediaRecorder` (MP4 com fallback automático para WebM).

## Rodando localmente

Qualquer servidor estático serve:

```bash
python3 -m http.server 8472
# abra http://localhost:8472
```

---

Feito com [Claude Code](https://claude.com/claude-code).
