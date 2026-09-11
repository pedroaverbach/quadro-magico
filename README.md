# ✍️ Quadro Mágico

Transforme uma imagem estilo quadro branco em um vídeo de **whiteboard animation** — direto no navegador, grátis, sem enviar nada para servidor nenhum (a imagem nunca sai do seu computador).

## Como usar

1. Abra o app e arraste uma imagem de traço sobre fundo claro (PNG ou JPG) — ou clique em "Testar com um desenho de exemplo".
2. Ajuste duração, formato (9:16 pra Reels é o padrão; também 16:9 e 1:1), ordem do desenho (de cima pra baixo, texto primeiro/último etc.) e a mão. No 9:16, os desenhos ficam automaticamente dentro da área que a interface do Instagram não cobre.
3. Clique no microfone de uma cena para abrir o **estúdio de narração** (veja abaixo) — opcional.
4. Veja a prévia e clique em **Gerar vídeo** — sai um MP4 pronto para WhatsApp, Instagram ou YouTube.

## Estúdio de narração

Cada cena tem um estúdio próprio, com teleprompter e gravação em trechos:

- escreva o texto da fala e ele sobe sozinho na tela enquanto você grava (velocidade e tamanho ajustáveis, com uma faixa marcando a linha de leitura);
- **pause e retome quantas vezes quiser** — cada pausa fecha um trecho, e no fim todos são emendados num áudio só;
- **errou no meio?** a onda do áudio aparece ao pausar: arraste o marcador até o último ponto bom, ouça pra conferir e corte — tudo depois do marcador é apagado, o teleprompter volta sozinho pra frase que você estava lendo naquele segundo, e é só gravar de novo dali;
- "apagar o último trecho" é o atalho pra desfazer só o pedaço desde a última vez que você apertou gravar;
- a narração já salva vira o ponto de partida quando você reabre o estúdio: dá pra cortar o fim ruim ou continuar gravando a partir dele;
- "ensaiar a leitura" roda o teleprompter sem gravar, para calibrar a velocidade;
- atalhos: espaço grava/pausa, Enter conclui, ↑ ↓ rolam o texto, Esc fecha;
- a cena dura exatamente o tempo da narração, e os textos escritos ficam salvos no navegador.

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
- a animação é desenhada em canvas e gravada com `MediaRecorder` (MP4 com fallback automático para WebM);
- a narração é gravada em trechos independentes, decodificados em `AudioBuffer` e concatenados — é isso que permite pausar e retomar em qualquer navegador; o corte fatia esse buffer, e um rastro de (segundo → posição do teleprompter) gravado durante a fala devolve o texto ao ponto certo.

## Rodando localmente

Qualquer servidor estático serve:

```bash
python3 -m http.server 8472
# abra http://localhost:8472
```

---

Feito com [Claude Code](https://claude.com/claude-code).
