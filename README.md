# Warhammer 40,000: Legacy of Dorn — Herald of Oblivion — Tradução PT-BR

> Tradução não-oficial completa do jogo para o Português do Brasil.

---

## 📋 Sobre o projeto

Este projeto assim como sua documentação foram feitos inteiramente com o uso da plataforma CLAUDE, sendo apenas alguns poucos termos corrigidos manualmente.

Este projeto traduz **~4.000 unidades de texto** do jogo Warhammer 40,000: Legacy of Dorn — Herald of Oblivion para o Português do Brasil, cobrindo todo o texto visível do jogo:

- Narrativa completa do livro-jogo (502 seções, ~57.500 palavras)
- Interface (menus, HUD, botões, combate)
- Sistema de combate completo (gwcombat, rolagens, stats)
- Nomes de itens, armas, armaduras e inimigos
- Conquistas e descrições
- Regras e tutoriais
- Créditos e textos de configuração

**Versão do jogo testada:** Steam (versão 4.7.0f1 Unity)  
**Plataforma:** PC (Steam)

---

## ✅ O que foi traduzido

| Arquivo | Unidades | Descrição |
|---|---|---|
| MainBook10_external.txt | 2.889 | Narrativa principal (502 seções do livro-jogo) |
| textConfig | 263 | Interface, combate, menus, rolagens |
| rules | 167 | Regras, tutoriais, atributos |
| **Total** | **~3.319** | |

### Termos mantidos em inglês (intencionalmente)

Termos canônicos do universo Warhammer 40K foram preservados no idioma original:
`Imperial Fists`, `Space Marine`, `Terminator`, `Genestealer`, `Necron`, `Tyranid`, `Ork`, `Dark Eldar`, `Dreadnought`, `Stompa`, `Trukk`, `Warbike`, `Cogitator`, `Narthecium`, `Teleportarium`, `Warp`, `Empyrean`, `Geller field`, `Bolter`, `Chainsword`, `Mechanicus`, `Archon`, `Haemonculus`, `Lictor`, `Carnifex`, `Hive Tyrant`, `Broodlord`, `Tombship`, `Wraithbone`

---

## Como foi traduzido?

Para saber mais detalhes leia `COMO_FUNCIONA.md`

Com ajuda do CLAUDE foi verificado que os textos do jogo estavam armazenados em dois lugares:
- `resources.assets` — assets Unity (textConfig, rules)
- `Documents/MainBook10_external.txt` — conteúdo principal do livro-jogo

O CLAUDE extraiu todos os TextAssets via AssetStudio/Asset Bundle Extractor, analisou a estrutura JSON proprietária do motor Gamebook Adventures (Tin Man Games), dividiu o MainBook em 27 lotes de ~2.500 palavras e traduziu cada um mantendo intactas todas as tags dinâmicas (`<insert>`, `<name>`, `<damage>`, links de navegação entre seções).

A reinjeção foi feita via patch binário direto no `resources.assets` usando Python + UnityPy, sem dependência de ferramentas externas pagas.

---

## 🛠️ Como instalar

### Passo a passo

**1. Faça backup dos arquivos originais**

```
...\Warhammer 40,000 Herald of Oblivion\Games Workshop - Herald of Oblivion_Data\resources.assets
...\Warhammer 40,000 Herald of Oblivion\Documents\MainBook10_external.txt
```

**2. Baixe os arquivos de tradução**

Baixe o arquivo da seção [Releases](https://github.com/crispinhatt/40kHeraldBR/releases) deste repositório.

**3. Substitua os arquivos**

- Copie `resources.assets` para:
  ```
  ...\Warhammer 40,000 Herald of Oblivion\Games Workshop - Herald of Oblivion_Data\
  ```

- Copie `MainBook10_external.txt` para:
  ```
  ...\Warhammer 40,000 Herald of Oblivion\Documents\
  ```

**4. Jogue!**

Abra o jogo diretamente pelo `.exe` ou pelo Steam. Os textos aparecerão em **Português do Brasil**.

> ⚠️ **Não é necessário nenhuma ferramenta externa** — apenas substituir os arquivos.

---

## 🔄 Como desinstalar / restaurar

Para voltar ao inglês original:

1. Feche o jogo
2. Copie os arquivos originais de volta para suas respectivas pastas
3. Confirme a substituição

---

## ⚠️ Avisos

- Esta é uma tradução **não-oficial** — não tem relação com a Tin Man Games ou Games Workshop
- Testada na versão Steam atual — pode ser necessário reaplicar se o jogo for atualizado
- O motor do jogo (Gamebook Adventures) carrega o conteúdo principal de um arquivo externo em `Documents/`, que tem prioridade sobre o `resources.assets`; ambos os arquivos devem ser substituídos
- Labels fixos da UI de combate (ATTACK!, ADVANCE, WITHDRAW, FOCUS) são hardcoded na DLL e não são traduzíveis sem decompilação

---

## 🐛 Reportar erros

Encontrou um erro de tradução, texto cortado ou algo que não faz sentido em PT-BR? Abra uma [Issue](https://github.com/crispinhatt/40kHeraldBR/issues) com:

- Print da tela com o erro
- Contexto (qual seção do livro, menu ou tela de combate)
- Sugestão de tradução correta (opcional)

---

## 🔧 Para desenvolvedores

Quer contribuir com melhorias ou entender a estrutura técnica? Veja o arquivo `COMO_FUNCIONA.md`.

---

## 📜 Licença

Este projeto está licenciado sob a MIT License.

Você pode usar, modificar, distribuir e até utilizar comercialmente este projeto livremente.

Warhammer 40,000: Legacy of Dorn — Herald of Oblivion © Tin Man Games / Games Workshop.  
Este projeto é uma tradução não-oficial e não possui vínculo com os detentores da propriedade intelectual.
