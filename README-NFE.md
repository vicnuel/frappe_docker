# Integração do Módulo NFe/NFCe no ERPNext com Docker

Este projeto integra o módulo `nfe_nfce_erpnext` ao ambiente ERPNext usando Docker.

## 📋 Pré-requisitos

- Docker instalado
- Docker Compose instalado
- Pelo menos 4GB de RAM disponível
- Porta 8082 disponível

## 🚀 Instalação Rápida

### Windows (PowerShell)
```powershell
.\install-nfe.ps1
```

### Linux/Mac (Bash)
```bash
chmod +x install-nfe.sh
./install-nfe.sh
```

### Instalação Manual

1. **Parar containers existentes** (se houver):
   ```bash
   docker-compose -f pwd.yml down
   ```

2. **Iniciar o ambiente**:
   ```bash
   docker-compose -f pwd.yml up -d
   ```

3. **Aguardar a instalação** (pode levar alguns minutos na primeira vez)

## 🔗 Acesso ao Sistema

- **URL**: http://localhost:8082
- **Usuário**: Administrator
- **Senha**: admin

## 📦 O que foi instalado

- ERPNext v15.75.1
- Módulo NFe/NFCe for ERPNext
- MariaDB 10.6
- Redis (cache e queue)
- Nginx (proxy reverso)

## 🛠️ Comandos Úteis

### Verificar status dos containers
```bash
docker-compose -f pwd.yml ps
```

### Acompanhar logs
```bash
docker-compose -f pwd.yml logs -f
```

### Acessar o container backend
```bash
docker-compose -f pwd.yml exec backend bash
```

### Executar comandos bench
```bash
docker-compose -f pwd.yml exec backend bench --version
docker-compose -f pwd.yml exec backend bench --site frontend console
```

### Parar todos os serviços
```bash
docker-compose -f pwd.yml down
```

### Parar e remover volumes (dados)
```bash
docker-compose -f pwd.yml down -v
```

## 🔧 Estrutura do Projeto

```
frappe_docker/
├── pwd.yml                 # Docker Compose principal
├── nfe_nfce_erpnext/      # Módulo NFe/NFCe
├── install-nfe.ps1        # Script de instalação (Windows)
├── install-nfe.sh         # Script de instalação (Linux/Mac)
└── README-NFE.md          # Este arquivo
```

## 📚 Funcionalidades do Módulo NFe/NFCe

O módulo `nfe_nfce_erpnext` adiciona:

- Emissão de Nota Fiscal Eletrônica (NFe)
- Emissão de Nota Fiscal de Consumidor Eletrônica (NFCe)
- Configurações fiscais brasileiras
- Integração com SEFAZ
- Customizações específicas para o Brasil

## 🐛 Solução de Problemas

### Container não inicia
```bash
# Verificar logs
docker-compose -f pwd.yml logs backend

# Verificar se as portas estão disponíveis
netstat -an | grep 8082
```

### Erro de permissão (Linux/Mac)
```bash
sudo chown -R $USER:$USER ./nfe_nfce_erpnext
```

### Reinstalar completamente
```bash
docker-compose -f pwd.yml down -v
docker system prune -f
docker-compose -f pwd.yml up -d
```

### Site não carrega
1. Aguarde alguns minutos após o primeiro `docker-compose up`
2. Verifique se todos os containers estão rodando: `docker-compose -f pwd.yml ps`
3. Verifique os logs: `docker-compose -f pwd.yml logs -f create-site`

## 📞 Suporte

- Para problemas com o módulo NFe/NFCe: [repositório original](https://github.com/shirkit/nfe_nfce_erpnext)
- Para problemas com Docker: [frappe_docker](https://github.com/frappe/frappe_docker)

## 📄 Licença

Este projeto mantém as licenças originais:
- ERPNext: GPL v3
- Módulo NFe/NFCe: MPL-2.0
