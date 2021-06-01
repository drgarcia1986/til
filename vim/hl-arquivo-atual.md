# Ativar Highlight no arquivo atual
Em casos on o vim não detectar o tipo do arquivo (geralmente devido a extensão) é possível setar com syntax ele deve assumir setando a config `syntax`:

```vim
:set syntax=sql
```

Para desligar basta setar a config para `off`
```vim
:set syntax=off
```

## Referências
- [Forcing Syntax Coloring for files with odd extensions](https://vim.fandom.com/wiki/Forcing_Syntax_Coloring_for_files_with_odd_extensions)
