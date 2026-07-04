# COMO_FUNCIONA.md — Documentação Técnica

Guia detalhado da estrutura da tradução para quem deseja contribuir, revisar termos ou adaptar para outros idiomas.

---

## 🧱 Estrutura do jogo

O *Warhammer 40,000: Legacy of Dorn — Herald of Oblivion* utiliza:

- **Engine:** Unity 4.7.0f1
- **Motor de livro-jogo:** Gamebook Adventures (Tin Man Games)
- **Sistema de localização:** nenhum — textos embutidos em TextAssets JSON

Os textos do jogo estão distribuídos em dois lugares:

```
Games Workshop - Herald of Oblivion_Data/
  resources.assets          ← textConfig, rules e outros configs
Documents/
  MainBook10_external.txt   ← conteúdo principal do livro-jogo (PRIORIDADE)
```

> **Importante:** O jogo verifica primeiro se existe `Documents/MainBook10_external.txt`. Se existir, carrega dele — ignorando o MainBook embutido no `resources.assets`. Isso foi verificado via `output_log.txt`:
> ```
> CANNOT FIND BOOK IN CACHE. LOADING NEW ONE. MainBook
> loading book from: .../Documents//MainBook10_external.txt
> ```

---

## 🗂️ Estrutura dos TextAssets

Cada TextAsset no `resources.assets` é um JSON com formato específico do motor Gamebook Adventures.

### MainBook / lore / rules

```json
{
  "editor": { "bookID": "...", "version": "800", ... },
  "engineConfig": { "chave": "valor", ... },
  "meta": [ { "gameID": "NomeItem", "name": "Nome Exibido", "type": "..." }, ... ],
  "section_0": [ { "type": "text", "value": "Texto narrativo..." }, ... ],
  "section_1": [ ... ],
  ...
}
```

Tipos de itens dentro das seções:
- `text` → texto narrativo exibido ao jogador
- `turnto` → link de navegação entre seções (campo `value.link` = label do botão)
- `combat` → gatilho de combate
- `pickup` / `drop` → coleta/descarte de item
- `image` → imagem exibida
- `config` → override de configuração

### textConfig

Arquivo plano de chave-valor:
```json
{
  "roll intro": "Determine your destiny",
  "combat win": "You deliver a devastating blow! <insert> is defeated!",
  ...
}
```

Tags dinâmicas preservadas: `<insert>`, `<name>`, `<base>`, `<total>`, `<damage>`, `<plural>`, `<rollValue>`, `<statValue>`, `<statName>`, `<weapon>`, `<enemy>`

---

## 🔧 Ferramentas utilizadas

- **AssetStudio GUI** — extração visual dos TextAssets de `resources.assets`
- **Asset Bundle Extractor (ABE)** — export em formato JSON Dump
- **UnityPy** — patch binário direto em `resources.assets`
- **Python 3.12** — scripts de extração, tradução e reinjeção
- **Claude (Sonnet)** — tradução manual seção por seção (27 lotes)

---

## 📐 Estrutura do patch

O `resources.assets` é um arquivo binário Unity (formato version=9, big-endian header, little-endian data).

Layout do header (bytes 0-19, big-endian):
```
uint32  metadata_size
uint32  file_size
uint32  version (= 9)
uint32  data_offset (= 4096)
uint32  flags
```

Tabela de objetos (dentro dos primeiros 4096 bytes, little-endian):
```
int32   path_id
uint32  data_offset_relative   (relativo ao data_offset do header)
uint32  data_size
int32   type_id
int16   class_id
int16   is_destroyed
```

Cada TextAsset serializado (little-endian, alinhado a 4 bytes):
```
int32   len(m_Name)
char[]  m_Name
pad4
int32   len(m_Script)
char[]  m_Script      ← aqui está o JSON traduzido
pad4
int32   len(m_PathName)
char[]  m_PathName
pad4
```

O patch cirúrgico:
1. Localiza cada TextAsset alvo pelo `path_id` na tabela de objetos
2. Substitui o bloco serializado pelo novo (com m_Script traduzido)
3. Atualiza `data_offset_relative` e `data_size` para todos os objetos subsequentes
4. Atualiza `file_size` no header

---

## 📊 Cobertura da tradução

| Arquivo | PathID | Unidades | Tipo |
|---|---|---|---|
| textConfig | 74 | 263 | chave-valor de UI |
| MainBook | 79 | 2.889 | narrativa + meta + engineConfig |
| rules | 82 | 167 | seções de regras + meta |

### MainBook detalhado

| Tipo | Quantidade |
|---|---|
| Blocos de texto narrativo | 1.800 |
| Links de navegação (labels de botão) | 1.086 |
| engineConfig (strings de UI do livro) | 246 |
| meta_name (nomes de itens/inimigos/stats) | 307 |

---

## 🌍 Glossário de termos mantidos em inglês

Termos canônicos 40K não traduzidos (convenção GW-BR):

**Facções/raças:** Imperial Fists, Space Marine, Adeptus Astartes, Orks, Tyranids, Necrons, Dark Eldar, Eldar, Mechanicus

**Unidades:** Terminator, Genestealer, Broodlord, Carnifex, Hive Tyrant, Lictor, Dreadnought, Immortal, Wraith, Canoptek Spyder

**Veículos/estruturas:** Stompa, Trukk, Warbike, Tombship, Raider, Voidraven

**Termos técnicos:** Cogitator, Narthecium, Teleportarium, Apothecarion, Warp, Empyrean, Geller field, Bolter, Chainsword, Las-weapon

**Nomenclatura em latim/High Gothic:** Adeptus, Astartes, Omnissiah, Primarch, Aquila, Ceramite, Adamantium, Promethium

---

## 📜 Scripts do projeto

- **extract.py** — extrai strings traduzíveis dos TextAssets
- **make_batches.py** — divide o MainBook em lotes de ~2.500 palavras
- **reinject.py** — injeta traduções de volta nos TextAssets (resources.assets)
- **build_batch_XX_pt.py** — scripts de tradução por lote (00-26)

---

## 📌 Histórico de versões

- **1.0.0 — Jul/2026**
  - Primeira versão pública
  - ~3.319 unidades traduzidas
  - Compatível com versão Steam atual (Unity 4.7.0f1)
