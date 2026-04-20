# Doc Translator

Traduz documentos locais (DOCX, PDF, EPUB) **100% offline** usando [Argos Translate](https://github.com/argosopentech/argos-translate) — sem API, sem custo.

Funciona de forma semelhante ao Whisper offline: baixa o modelo de tradução neural uma única vez (~100 MB) e depois roda tudo localmente.

## Requisitos

- Python 3.9+

## Instalação

```bash
git clone https://github.com/rdantassilva/doc-translator.git
cd doc-translator

python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
```

> No Windows, use `venv\Scripts\activate` no lugar de `source venv/bin/activate`.

## Uso

```bash
# Traduzir DOCX de Inglês para Português (padrão)
python translate.py documento.docx
# Gera em output/: pb - documento.docx + pb - documento.html

# Traduzir PDF de Inglês para Espanhol
python translate.py artigo.pdf --source en --target es
# Gera em output/: es - artigo.docx + es - artigo.pdf + es - artigo.html

# Traduzir EPUB de Francês para Português Brasil
python translate.py livro.epub --source fr --target pb

# Renomear apenas o arquivo de saída principal (em output/)
python translate.py report.docx --output relatorio_traduzido.docx

# Definir pasta e nome customizados para o arquivo principal
python translate.py artigo.pdf --output revisoes/es/artigo_traduzido.docx
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
| `--output`, `-o` | auto | Caminho do arquivo principal de saída |
| `--setup FROM TO` | — | Baixar modelo de tradução |
| `--list-langs` | — | Listar idiomas disponíveis |
| `--verbose` | off | Ativa logs detalhados |

## Formatos suportados

| Entrada | Saída |
|---|---|
| `.docx` | `.docx` + `.html` (DOCX preserva estilos básicos) |
| `.pdf` | `.docx` + `.pdf` + `.html` (gerados automaticamente) |
| `.epub` | `.epub` (preserva estrutura e metadados) |

### Observações sobre `--output`

- O `--output` renomeia apenas o **primeiro arquivo** gerado pelo fluxo.
- Sem `--output`, os arquivos são salvos em `output/` automaticamente.
- Com `--output` sem pasta (ex.: `relatorio.docx`), o arquivo principal vai para `output/relatorio.docx`.
- Com `--output` com pasta (ex.: `revisoes/relatorio.docx`), todos os arquivos vão para essa pasta.
- Para entrada `.docx`, o primeiro arquivo é o `.docx`.
- Para entrada `.pdf`, o primeiro arquivo é o `.docx` (PDF e HTML mantêm nome automático).
- Por segurança, a pasta de destino deve estar dentro do diretório atual do projeto.

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
