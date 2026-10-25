# Contador de itens em esteira por subtração de background

Avaliação I — Visão Computacional (prof. Lúcio Campos). **Deadline: 25/10/2026.**
Entrega: link do notebook no Colab (conclusões escritas no próprio notebook, sem relatório Word).

## Objetivo

Contar itens que atravessam uma linha virtual numa esteira, usando subtração de background (MOG2 ou KNN) + morfologia + contornos, **em dois vídeos** (comparativo), e comparar com a contagem manual.

## Critérios de aceitação

1. Os dois vídeos processados pelo **mesmo pipeline**, com parâmetros por vídeo explícitos e justificados.
2. Etapa 1 completa: leitura de frames → subtração de background → limiarização → morfologia → contornos filtrados por área → contagem por cruzamento de linha virtual (sem contar o mesmo objeto duas vezes).
3. Etapa 2 completa, por vídeo: contagem manual (ground truth), contagem automática, erro absoluto `|auto − manual|` e erro percentual `|auto − manual| / manual · 100`.
4. Análise de falhas com **evidência visual** (frame/máscara) para: sombras, objetos encostados, variação de iluminação.
5. Conclusão comparando os dois vídeos, escrita no notebook.
6. Notebook executa de ponta a ponta no Colab (sem `cv2.imshow`; usar `matplotlib` ou `cv2_imshow` de `google.colab.patches`).

## Estrutura

```
contador-esteira/
  data/raw/         vídeos originais + contagem_manual.csv (imutáveis)
  data/processed/   máscaras/frames intermediários
  results/          figuras, vídeos anotados, tabela de erros
  src/              (vazio por ora: as funções ficam no próprio notebook para rodar sozinho no Colab)
  notebook/         notebook do Colab (entregável)
```

## Vídeos (`data/raw/`)

| Arquivo | Resolução | FPS | Frames | Duração | Observações |
|---------|-----------|-----|--------|---------|-------------|
| `01.mp4` | 352×288 | 25 | 2129 | 85,2 s | CCTV noturna; timestamp (topo-esq.) e "Camera 01" (base-dir.) mudam e viram foreground; itens esparsos sobre superfície texturizada |
| `test_video.mp4` | 1920×1080 | 30 | 720 | 24,0 s | Maçãs em esteira, perspectiva diagonal; muitos itens, encostados; esteiras ao fundo também em movimento; marca "Veo" no canto inferior direito |

## Pipeline (por frame)

`frame → cinza/blur → apply(MOG2|KNN) → threshold (remove sombra) → abertura → fechamento/dilatação → findContours → filtro por área → centroide → cruzou a linha?`

## Plano de trabalho

| # | Etapa | Validação |
|---|-------|-----------|
| 0 | Colocar os 2 vídeos em `data/raw/`; contar manualmente cada um (2 passadas) e anotar em `contagem_manual.csv` | contagem manual reprodutível |
| 1 | Ler vídeo, mostrar frame e metadados (fps, resolução, nº de frames) | frame exibido |
| 2 | Subtração de background; comparar MOG2 vs KNN numa máscara | máscara com itens visíveis |
| 3 | Threshold + morfologia; ajustar kernel | ruído removido, itens inteiros |
| 4 | Contornos + filtro de área | só itens, não ruído |
| 5 | Linha virtual + contagem (rastreio por centroide) | contagem em ~10 s de vídeo confere |
| 6 | Rodar nos dois vídeos, tabela de erros | erro abs./perc. calculados |
| 7 | Análise de falhas + conclusão | frames-evidência no notebook |

## Status

- **Notebook (`notebook/contador_esteira.ipynb`):** cobre todas as etapas do plano (leitura, MOG2/KNN, limiar + morfologia, contornos com filtro de área, contagem por linha virtual), mais conferência visual, sensibilidade a parâmetros e imagem espaço-tempo da linha.
- **Feito no notebook:** contagem manual (13 leitões e 40 maçãs, vídeo inteiro), erro absoluto e percentual (seção 13), análise de falhas (seções 14 a 16) e conclusão com referências (seção 17).
- **Falta:** compartilhar o link do Colab com o professor e conferir a política da disciplina sobre ajuda de IA no texto de análise.
- **Viés:** a calibração (linha, zona, áreas, ajuste do subtrator) usou os mesmos dois vídeos da avaliação, então o erro da etapa 6 é otimista. Dizer isso na conclusão.

## Commits sugeridos

- `feat: estrutura inicial do projeto contador-esteira`
- `feat: leitura de vídeo e subtração de background`
- `feat: contagem por linha virtual`
- `feat: avaliação com erro absoluto e percentual`
