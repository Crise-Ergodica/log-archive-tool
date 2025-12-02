# 📦 Log Archive Tool

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Shell Script](https://img.shields.io/badge/Shell-Bash-green.svg)](https://www.gnu.org/software/bash/)
[![Platform](https://img.shields.io/badge/Platform-Linux-orange.svg)](https://www.kernel.org/)

Ferramenta CLI profissional para arquivamento automático de logs em formato tar.gz com timestamp. Ideal para manter o sistema organizado, removendo logs antigos enquanto preserva-os compactados para referência futura.

> 🗺️ **Projeto baseado em**: [roadmap.sh - Log Archive Tool](https://roadmap.sh/projects/log-archive)

## ✨ Características

### 🎯 Funcionalidades Principais
- **Compressão eficiente**: Arquivos tar.gz com compressão gzip
- **Timestamp automático**: Nomes de arquivo no formato `logs_archive_YYYYMMDD_HHMMSS.tar.gz`
- **Log de histórico**: Registro completo de todas as operações em `archive_history.log`
- **Interface colorida**: Output visual com códigos de cor para melhor legibilidade
- **Validação robusta**: Verificação de diretórios e permissões

### 🚀 Funcionalidades Avançadas
- **Dry-run mode**: Simulação sem modificar arquivos
- **Filtros de exclusão**: Padrões glob para ignorar arquivos específicos
- **Filtro por idade**: Arquiva apenas logs mais antigos que N dias
- **Opção de preservação**: Mantém arquivos originais após arquivamento
- **Modo verbose**: Saída detalhada para debugging
- **Estatísticas de compressão**: Relatório de tamanho e taxa de compressão

## 💻 Instalação

### Método 1: Clone do Repositório

```bash
# Clone o repositório
git clone https://github.com/Crise-Ergodica/log-archive-tool.git
cd log-archive-tool

# Torne o script executável
chmod +x log-archive

# (Opcional) Instale globalmente
sudo cp log-archive /usr/local/bin/
```

### Método 2: Download Direto

```bash
# Download do script
wget https://raw.githubusercontent.com/Crise-Ergodica/log-archive-tool/main/log-archive

# Torne executável
chmod +x log-archive

# (Opcional) Mova para PATH
sudo mv log-archive /usr/local/bin/
```

## 🚀 Uso

### Sintaxe Básica

```bash
log-archive <log-directory> [options]
```

### Exemplos Práticos

#### 1. Arquivamento Básico

```bash
# Arquiva logs de /var/log
./log-archive /var/log
```

#### 2. Simulação (Dry Run)

```bash
# Veja o que seria arquivado sem modificar nada
./log-archive /var/log --dry-run
```

#### 3. Diretório de Saída Customizado

```bash
# Salva arquivos em diretório específico
./log-archive /var/log --output /backup/logs
```

#### 4. Manter Arquivos Originais

```bash
# Cria arquivo sem deletar originais
./log-archive /var/log --keep
```

#### 5. Filtro por Idade

```bash
# Arquiva apenas logs com mais de 7 dias
./log-archive /var/log --age 7
```

#### 6. Excluir Padrões

```bash
# Exclui arquivos .tmp e .swp
./log-archive /var/log --exclude '*.tmp' --exclude '*.swp'
```

#### 7. Modo Verbose

```bash
# Saída detalhada para debugging
./log-archive /var/log --verbose
```

#### 8. Combinação de Opções

```bash
# Arquiva logs antigos com verbose e preserva originais
./log-archive /var/log --age 7 --keep --verbose --exclude '*.gz'
```

## 📊 Exemplo de Output

```
========================================
  Log Archive Tool v1.0.0
========================================

INFO: Source directory: /var/log
INFO: Archive directory: /home/user/log_archives

INFO: Creating archive: logs_archive_20251202_102345.tar.gz
INFO: Found 47 file(s) to archive
INFO: Total size: 125 MB
SUCCESS: Archive created: /home/user/log_archives/logs_archive_20251202_102345.tar.gz
INFO: Archive size: 15 MB
INFO: Compression ratio: 88%
INFO: Removing original log files...
SUCCESS: Original files removed

SUCCESS: Archiving completed successfully!
INFO: Archive history: /home/user/log_archives/archive_history.log
```

## 🛠️ Opções Completas

| Opção | Descrição |
|--------|-------------|
| `-o, --output DIR` | Diretório de saída para arquivos (padrão: ~/log_archives) |
| `-k, --keep` | Mantém arquivos originais após arquivamento |
| `-d, --dry-run` | Simula operação sem modificar arquivos |
| `-v, --verbose` | Habilita saída detalhada |
| `-e, --exclude PATTERN` | Exclui arquivos que correspondem ao padrão |
| `-a, --age DAYS` | Arquiva apenas logs mais antigos que DAYS dias |
| `-h, --help` | Exibe mensagem de ajuda |
| `--version` | Exibe informações de versão |

## 📁 Estrutura de Arquivos

```
~/log_archives/
├── logs_archive_20251202_102345.tar.gz
├── logs_archive_20251201_153020.tar.gz
├── logs_archive_20251130_091530.tar.gz
└── archive_history.log
```

### Formato do archive_history.log

```
[2025-12-02 10:23:45] Archive created: logs_archive_20251202_102345.tar.gz | Files: 47 | Original: 125 MB | Compressed: 15 MB | Ratio: 88%
[2025-12-01 15:30:20] Archive created: logs_archive_20251201_153020.tar.gz | Files: 52 | Original: 143 MB | Compressed: 18 MB | Ratio: 87%
```

## ⏰ Automatização com Cron

### Arquivamento Diário

```bash
# Editar crontab
crontab -e

# Adicionar linha para executar todo dia às 2h da manhã
0 2 * * * /usr/local/bin/log-archive /var/log --age 7 >> /var/log/log-archive.log 2>&1
```

### Arquivamento Semanal

```bash
# Todo domingo às 3h da manhã
0 3 * * 0 /usr/local/bin/log-archive /var/log --age 30 --output /backup/weekly
```

### Arquivamento Mensal

```bash
# Primeiro dia do mês às 4h da manhã
0 4 1 * * /usr/local/bin/log-archive /var/log --age 90 --output /backup/monthly
```

## 🔧 Requisitos

- **Sistema Operacional**: Linux/Unix
- **Shell**: Bash 4.0 ou superior
- **Ferramentas**: `tar`, `gzip`, `find`, `stat`, `date`
- **Permissões**: Leitura no diretório de logs, escrita no diretório de saída

### Verificação de Dependências

```bash
# Verificar se todas as ferramentas estão disponíveis
for cmd in tar gzip find stat date; do
    command -v $cmd >/dev/null 2>&1 || echo "$cmd não encontrado"
done
```

## 🔒 Permissões

### Para /var/log (requer root)

```bash
# Executar com sudo
sudo ./log-archive /var/log
```

### Para diretórios de usuário

```bash
# Não requer privilégios especiais
./log-archive ~/logs
```

## 🐛 Troubleshooting

### Erro: "Permission denied"

**Causa**: Falta de permissões no diretório de logs

**Solução**:
```bash
sudo ./log-archive /var/log
```

### Erro: "Directory does not exist"

**Causa**: Caminho inválido

**Solução**: Verificar se o diretório existe:
```bash
ls -ld /caminho/para/logs
```

### Nenhum arquivo arquivado

**Causa**: Filtros muito restritivos ou diretório vazio

**Solução**: Use `--dry-run --verbose` para ver o que está sendo filtrado:
```bash
./log-archive /var/log --dry-run --verbose
```

## 🎓 Aprendizado

Este projeto ajuda a praticar:
- Manipulação de arquivos e diretórios em Bash
- Uso do comando `tar` para compressão e arquivamento
- Argumentos de linha de comando e parsing
- Tratamento de erros robusto com `set -euo pipefail`
- Formatação de output colorido
- Automação com cron jobs
- Logging e auditoria de operações

## 🚀 Funcionalidades Futuras

- [ ] Notificações por email sobre arquivamentos
- [ ] Upload automático para cloud storage (S3, Google Drive)
- [ ] Rotação automática de arquivos antigos
- [ ] Suporte a múltiplos diretórios de origem
- [ ] Compressão customizável (bzip2, xz, zstd)
- [ ] Verificação de integridade com checksums
- [ ] Dashboard web para visualização de histórico
- [ ] Integração com ferramentas de monitoring (Prometheus, Grafana)

## 📝 Licença

MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👤 Autor

**Aurora Ergodica**
- GitHub: [@Crise-Ergodica](https://github.com/Crise-Ergodica)
- Email: gdcm10@gmail.com

## 🔗 Links Úteis

- [GNU Tar Manual](https://www.gnu.org/software/tar/manual/tar.html)
- [Bash Scripting Guide](https://www.gnu.org/software/bash/manual/)
- [Cron Job Tutorial](https://crontab.guru/)
- [roadmap.sh Project](https://roadmap.sh/projects/log-archive)

---

<div align="center">

*"God's in His heaven, all's right with the world!"*

Feito com ❤️ por [Aurora Ergodica](https://github.com/Crise-Ergodica)

</div>
