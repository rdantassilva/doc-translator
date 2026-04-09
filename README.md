# Doc Translator

Traduz documentos locais (DOCX, PDF, EPUB) **100% offline** usando [Argos Translate](https://github.com/argosopentech/argos-translate) — sem API, sem custo.

Funciona de forma semelhante ao Whisper offline: baixa o modelo de tradução neural uma única vez (~100 MB) e depois roda tudo localmente.

## Requisitos

- Python 3.9+

## Instalação

```bash
pip install -r requirements.txt
```

## Uso

```bash
# Traduzir DOCX de Inglês para Português (padrão)
python translate.py documento.docx

# Traduzir PDF de Inglês para Espanhol
python translate.py artigo.pdf --source en --target es

# Traduzir EPUB de Francês para Português Brasil
python translate.py livro.epub --source fr --target pb

# Definir caminho de saída personalizado
python translate.py report.docx --output relatorio_traduzido.docx
```

Na primeira execução para um par de idiomas, o modelo será baixado automaticamente. Depois disso, funciona totalmente offline.

### Gerenciar modelos

```bash
# Baixar modelo manualmente (ex: Inglês → Português Brasil)
python translate.py --setup en pb

# Baixar modelo manualmente (ex: Inglês → Português Portugal)
python translate.py --setup en pt

# Listar todos os idiomas e modelos disponíveis
python translate.py --list-langs
```

## Opções

| Opção | Padrão | Descrição |
|---|---|---|
| `input` | — | Caminho do documento (obrigatório) |
| `--source`, `-s` | `en` | Código do idioma de origem |
| `--target`, `-t` | `pb` | Código do idioma de destino |
| `--no-format` | off | Desabilita a formatação automática do DOCX |
| `--chunk-size` | `2000` | Caracteres máximos por bloco de tradução |
| `--output`, `-o` | auto | Caminho do arquivo de saída |
| `--setup FROM TO` | — | Baixar modelo de tradução |
| `--list-langs` | — | Listar idiomas disponíveis |
| `--verbose` | off | Ativa logs detalhados |

## Formatos suportados

| Entrada | Saída |
|---|---|
| `.docx` | `.docx` (preserva estilos básicos) |
| `.pdf` | `.docx` + `.pdf` (ambos gerados automaticamente) |
| `.epub` | `.epub` (preserva estrutura e metadados) |

## Idiomas comuns

| Código | Idioma |
|---|---|
| `pb` | Português (Brasil) |
| `pt` | Português (Portugal) |
| `en` | English |
| `es` | Spanish |
| `fr` | French |
| `de` | German |
| `it` | Italian |
| `ja` | Japanese |
| `zh` | Chinese |
| `ru` | Russian |
| `ar` | Arabic |

Use `--list-langs` para ver a lista completa com 35+ idiomas.

## Formatação do DOCX (apenas para PDF)

Ao traduzir PDFs, a formatação original se perde na extração do texto. Por isso, o DOCX de saída recebe formatação limpa automaticamente:

- **Fonte:** Calibri 11pt
- **Espaçamento:** 1.15 entre linhas, 6pt após parágrafo
- **Margens:** 2.54 cm
- Texto uniforme, sem negrito/itálico — foco na legibilidade

Para DOCX e EPUB, a formatação original é preservada.

Para desabilitar a formatação automática no PDF:

```bash
python translate.py artigo.pdf --no-format
```
