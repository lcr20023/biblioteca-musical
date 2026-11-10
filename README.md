# 🎵 Biblioteca Musical

Player e biblioteca de músicas **offline**, em **um único arquivo HTML** (zero dependências, zero build, zero servidor).

Abra o `index.html` (ou acesse pelo GitHub Pages) e adicione suas músicas. Você escolhe onde a biblioteca vive:

- **Pasta do computador** (recomendado) — as músicas, playlists e favoritas ficam em arquivos reais numa pasta sua.
- **Armazenamento do navegador** — nada é gravado no disco, funciona em qualquer navegador.

![Prévia](preview.png)

## Os dois modos

| | 📁 Pasta do computador | 💾 Navegador |
|---|---|---|
| Onde ficam as músicas | nos arquivos da pasta escolhida | banco interno do navegador |
| Playlists e favoritas | arquivo `.biblioteca-musical.json` dentro da pasta | banco interno do navegador |
| Some ao limpar o navegador | **não** | sim |
| Leva a biblioteca para outro PC | **sim**, é só abrir a mesma pasta | não |
| Ocupa espaço do navegador | mínimo (metadados) | sim, cada arquivo é copiado |
| Requer | Chrome ou Edge no computador | qualquer navegador |

Dá para **migrar** nos dois sentidos: ao escolher uma pasta, o app oferece copiar para lá as músicas que já estavam no navegador (levando favoritas e playlists junto).

## Funcionalidades

- **Importação** por seletor de arquivos, por pasta inteira ou arrastando e soltando. No modo pasta, os arquivos são **copiados para dentro da pasta**; fora dele, para o banco do navegador.
- **Leitura real de tags**: título, artista, álbum, ano, gênero, número da faixa e **capa embutida**.
  - MP3: ID3v2.2 / v2.3 / v2.4 (incluindo acentuação UTF-8 e UTF-16)
  - FLAC e OGG: Vorbis Comments + bloco de imagem
  - Sem tags? Deduz `Artista - Música` a partir do nome do arquivo.
- **Player completo**: play/pause, anterior/próxima, barra de progresso, volume, aleatório, repetir tudo / repetir uma.
- **Controles do sistema**: Media Session API (tela de bloqueio e teclas de mídia).
- **Organização**: busca instantânea, ordenação, favoritas e playlists.
- **Ocultar sem perder**: tirar uma música da lista principal (ou apagar da tela) **não** a remove das playlists. Ela continua tocando de dentro da playlist e fica guardada na seção *Ocultas*, de onde volta quando você quiser. Apagar o arquivo de verdade é uma ação separada, com aviso.
- **Controle de capas**: colocar capa a partir de uma imagem sua, trocar, remover, ou voltar à capa que já vem dentro do arquivo. A capa escolhida é redimensionada e recortada para não pesar.
- **Apagar arquivo** direto do disco (com confirmação), no modo pasta.
- **Reler a pasta** para quando você adicionar ou remover músicas por fora.
- **Responsivo**: barra lateral no desktop, mini-player e menu lateral no celular.
- **Atalhos**: `espaço` play/pause, `Shift + ←/→` faixa anterior/próxima, `Esc` fecha janelas.

## Como usar

### 1. Pelo GitHub Pages (mais fácil)
Acesse **https://lcr20023.github.io/biblioteca-musical/**, clique em **Pasta** e escolha a pasta com suas músicas.

> O navegador pede a permissão de leitura/gravação da pasta uma vez. Em novas visitas ele pode pedir de novo; o app mostra um aviso com o botão **Reconectar**.

### 2. Direto do arquivo
Baixe o `index.html` e abra com dois cliques. Se o navegador bloquear o armazenamento interno, use a opção 1 ou um servidor local:

```bash
python3 -m http.server 8000
# abra http://localhost:8000
```

## Formatos suportados

`mp3` `m4a` `aac` `ogg` `oga` `opus` `wav` `flac` `webm` `wma` `aiff` `m4b`

A reprodução depende do suporte do navegador ao codec. A leitura de tags é feita internamente, sem bibliotecas.

## Detalhes técnicos

| Item | Escolha |
|---|---|
| Arquitetura | arquivo único (`index.html`), sem dependências |
| Pasta real | File System Access API (`showDirectoryPicker`), handle guardado no IndexedDB |
| Metadados da pasta | cache local (IndexedDB) validado por tamanho + data de modificação |
| Playlists/favoritas na pasta | `/.biblioteca-musical.json` (texto, versionável, portátil) — guarda também `hidden`, `covers` e `noCovers` |
| Capas personalizadas (pasta) | arquivos JPEG em `/.capas/`, referenciados pelo sidecar; viram data URL só ao renderizar |
| Modo navegador | IndexedDB — lojas `tracks`, `audio` e `playlists` |
| Memória | áudio carregado sob demanda via `Blob` + `URL.createObjectURL`, liberado ao trocar de faixa |
| Tags MP3 | parser próprio de ID3v2 (sincsafe, unsynchronisation, UTF-8/UTF-16/latin-1, APIC) |
| Tags FLAC/OGG | parser de metadados FLAC (Vorbis Comment + Picture) e OGG Vorbis |
| Duração | `loadedmetadata` do elemento de áudio, com gravação de volta no cache |
| Layout | CSS Grid puro, sem framework, breakpoints em 900px e 420px |

## Roadmap

- [ ] Letras (`.lrc`) sincronizadas, lendo o arquivo ao lado da música
- [ ] Equalizador e normalização de volume
- [ ] Edição de metadados direto no arquivo (título, artista, álbum)
- [ ] Backup `.zip` da biblioteca no modo navegador
- [ ] Fila de reprodução reordenável por arrastar

## Licença

Uso livre, pessoal ou comercial. Só não vendas como se fosse seu. 😄
