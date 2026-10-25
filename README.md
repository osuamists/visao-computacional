# Visão Computacional (UEMA)

Espaço de estudo e projetos da disciplina de Visão Computacional (prof. Lúcio Campos).

## Avaliação I: contador de itens em esteira

Contador simples de itens em esteira por subtração de background (MOG2 e KNN), aplicado a dois vídeos: leitões em uma rampa (`01.mp4`) e maçãs em uma esteira (`test_video.mp4`).

- Notebook no Colab, com as saídas da execução: https://colab.research.google.com/drive/1UvtyzEZwVZW3MtoTaAsbVcrpKjdbPhF4
- Notebook neste repositório: [projetos/contador-esteira/notebook/contador_esteira.ipynb](projetos/contador-esteira/notebook/contador_esteira.ipynb)
- Objetivo, plano e status: [projetos/contador-esteira/README.md](projetos/contador-esteira/README.md)

Contagem automática contra a contagem manual (13 leitões e 40 maçãs):

| Vídeo | Subtrator | Automática | Erro absoluto | Erro percentual |
|---|---|---|---|---|
| `01.mp4` | MOG2 | 10 | 3 | 23,1% |
| `01.mp4` | KNN | 11 | 2 | 15,4% |
| `test_video.mp4` | MOG2 | 59 | 19 | 47,5% |
| `test_video.mp4` | KNN | 43 | 3 | 7,5% |

A calibração (linha, zona, áreas e ajuste do subtrator) usou os mesmos dois vídeos, então os erros são otimistas. A análise de falhas (sombras, objetos encostados e iluminação) está nas seções 14 a 17 do notebook.

## Estrutura

```
projetos/contador-esteira/   notebook, README e pastas de dados e resultados
notebooks/                   experimentos exploratórios
referencias/                 artigos e capítulos de livros
```

Os vídeos, as aulas e os códigos da disciplina não estão versionados: vêm dos repositórios do professor (github.com/LucioCampos/VisaoComputacional e github.com/LucioCampos/Codigos).
