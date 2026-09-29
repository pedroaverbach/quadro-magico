# ✍️ Quadro Mágico

Transforme uma imagem estilo quadro branco em um vídeo de **whiteboard animation** — direto no navegador, grátis, sem enviar nada para servidor nenhum (a imagem nunca sai do seu computador).

## Como usar

1. Abra o app e arraste uma imagem de traço sobre fundo claro (PNG ou JPG) — ou clique em "Testar com um desenho de exemplo".
2. Ajuste duração, formato (9:16 pra Reels é o padrão; também 16:9 e 1:1), ordem do desenho (de cima pra baixo, texto primeiro/último etc.) e a mão. No 9:16, os desenhos ficam automaticamente dentro da área que a interface do Instagram não cobre.
3. Clique no microfone de uma cena para abrir o **estúdio de narração** (veja abaixo) — opcional. Dá pra gravar só a voz ou **voz + vídeo**, com o seu rosto numa bolinha no canto do vídeo.
4. Veja a prévia e clique em **Gerar vídeo** — sai um MP4 pronto para WhatsApp, Instagram ou YouTube.

## Estúdio de narração

Cada cena tem um estúdio próprio, com teleprompter e gravação em trechos:

- escreva o texto da fala e ele sobe sozinho na tela enquanto você grava (velocidade e tamanho ajustáveis, com uma faixa marcando a linha de leitura);
- **pause e retome quantas vezes quiser** — cada pausa fecha um trecho, e no fim todos são emendados num áudio só;
- **o texto destrava na pausa**: o teleprompter só fica travado enquanto o microfone está ligado, então dá pra corrigir uma frase que não ficou boa e seguir gravando (o botão "✎ editar o texto" faz o mesmo);
- **errou no meio?** a onda do áudio aparece ao pausar: arraste o marcador até o último ponto bom, ouça pra conferir e corte — tudo depois do marcador é apagado, o teleprompter volta sozinho pra frase que você estava lendo naquele segundo, e é só gravar de novo dali;
- "apagar o último trecho" é o atalho pra desfazer só o pedaço desde a última vez que você apertou gravar;
- a narração já salva vira o ponto de partida quando você reabre o estúdio: dá pra cortar o fim ruim ou continuar gravando a partir dele;
- "ensaiar a leitura" roda o teleprompter sem gravar, para calibrar a velocidade;
- "Concluir" salva a narração e volta pra tela principal; o ✕ também salva o que já foi gravado;
- atalhos: espaço grava/pausa, Enter conclui, ↑ ↓ rolam o texto, Esc fecha;
- a cena dura exatamente o tempo da narração, e os textos escritos ficam salvos no navegador.

## Narração com vídeo (o seu rosto no canto)

No topo do estúdio, troque **só voz** por **voz + vídeo**:

- a câmera liga na hora e você se vê numa bolinha no canto (espelhada, como no Zoom — no vídeo final você aparece como os outros te veem);
- todas as ferramentas da voz valem pro vídeo: teleprompter, pausar e retomar, cortar na onda, "apagar o último trecho", continuar depois de reabrir, e a cena durando o tempo exato da fala;
- ao arrastar o marcador na onda, a bolinha mostra a sua imagem naquele segundo; o ▶ toca voz e imagem juntas;
- no vídeo final o rosto aparece numa bolinha com aro branco: entra crescendo na primeira cena, se dissolve de uma cena pra outra quando as duas têm vídeo, sai nas cenas só com voz e sai de novo no zoom final, pra revelar a cartolina inteira;
- a bolinha fica no canto de cima, à direita; no passo 2 (Estilo) dá pra trocar o **canto** e o **tamanho** (P, M, G); no 9:16 ela fica dentro da área que a interface do Reels não cobre;
- se um desenho for ficar embaixo da bolinha, a câmera reenquadra a cena (pra cima, pra baixo ou pro lado, o que deixar o desenho maior);
- dá pra misturar: cada cena escolhe se é só voz ou voz + vídeo.

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
- a narração é gravada em trechos independentes, decodificados em `AudioBuffer` e concatenados — é isso que permite pausar e retomar em qualquer navegador; o corte fatia esse buffer, e um rastro de (segundo → posição do teleprompter) gravado durante a fala devolve o texto ao ponto certo;
- o vídeo da narração não usa um segundo gravador: cada quadro da câmera (`requestVideoFrameCallback`, com a hora de captura) vira um JPEG quadrado de 360 px com o segundo em que foi capturado, na mesma linha do tempo do áudio. Por isso cortar, emendar e continuar valem igual pros dois, e a boca bate com a voz (medido: ~20 ms no take e no arquivo final — o MP4 do Chrome começa o som ~44 ms atrasado por causa do compressor AAC, e o rosto espera esse tanto). Na hora de tocar, os quadros são decodificados um pouco à frente (`createImageBitmap`) e desenhados no canvas, dentro da bolinha.

## Rodando localmente

Qualquer servidor estático serve:

```bash
python3 -m http.server 8472
# abra http://localhost:8472
```

---

Feito com [Claude Code](https://claude.com/claude-code).
