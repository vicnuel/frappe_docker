# Como adicionar o módulo nfe_nfce_erpnext em um projeto ERPNext em produção (Docker Compose)

Este guia mostra como instalar o módulo `nfe_nfce_erpnext` em um ambiente ERPNext já em produção usando Docker Compose.

---

Para automatizar o processo, basta subir o ambiente com:

docker compose -f pwd.base.yml up -d
Após o site estar criado, rode: docker compose -f pwd.base.yml -f pwd.app.yml up -d
Instale o app no site com o comando bench.

## 1. Copie o app para o servidor

No servidor de produção, coloque a pasta `nfe_nfce_erpnext` dentro do diretório onde está seu `docker-compose.yml` (ou equivalente, como `pwd.yml`).

Exemplo de estrutura:

```
frappe_docker/
  nfe_nfce_erpnext/
  pwd.yml
  ...
```

## 2. Monte o app nos volumes dos serviços

No arquivo `pwd.yml`, adicione (ou confirme) a linha abaixo em todos os serviços que usam apps:

```yaml
- ./nfe_nfce_erpnext:/home/frappe/frappe-bench/apps/nfe_nfce_erpnext
```

## 3. Atualize o arquivo `sites/apps.txt`

Dentro do container backend, adicione o app ao arquivo `sites/apps.txt`:

```sh
docker compose exec backend bash
cd /home/frappe/frappe-bench/sites
ls ../apps > apps.txt
exit
```

## 4. Instale as dependências do app (se houver)

Se o app tiver um `requirements.txt`, instale:

```sh
docker compose exec backend bash
pip install -r /home/frappe/frappe-bench/apps/nfe_nfce_erpnext/requirements.txt
exit
```

## 5. Instale o app no site ERPNext

Execute:

```sh
docker compose exec backend bash
bench --site frontend install-app nfe_nfce_erpnext
exit
```

Substitua `nome-do-seu-site` pelo nome do seu site (ex: `meusite.com`).

## 6. Aplique as migrações

```sh
docker compose exec backend bash
bench --site nome-do-seu-site migrate
exit
```

## 7. Reinicie os containers

```sh
docker compose down
# (aguarde alguns segundos)
docker compose up -d
```

## 8. Verifique no ERPNext

Acesse o ERPNext, vá em "Configurações" > "Aplicativos" e confira se o módulo aparece.

---

Pronto! O módulo `nfe_nfce_erpnext` estará disponível no seu ambiente ERPNext em produção via Docker Compose.
