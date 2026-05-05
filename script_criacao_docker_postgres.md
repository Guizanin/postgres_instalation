# 🤓 Comando para criar uma pasta no usuário para guardar os containers criados

## 🔁 Altere as linhas com os valores que desejar
| -e POSTGRES_PASSWORD=senhaSegura
| -e POSTGRES_USER=usuario

```bash
mkdir -p ~/docker/postgres_data
docker run -d \
  --name meu_postgres \
  -e POSTGRES_PASSWORD=senhaSegura \
  -e POSTGRES_USER=usuario \
  -e POSTGRES_DB=meubanco \
  -v ~/docker/postgres_data:/var/lib/postgresql \
  -p 5432:5432 \
  postgres:18

echo "alias pgstart='docker start meu_postgres'" >> ~/.bashrc
echo "alias pgstop='docker stop meu_postgres'" >> ~/.bashrc
echo "alias pgcli=\"docker exec -it meu_postgres psql -U usuario -d meubanco\"" >> ~/.bashrc

source ~/.bashrc
```
⚠️ Se estiver utilizando o docker em máquina pessoal e seu usuário NÃO estiver adicionado ao grupo docker, você precisará utilizar _sudo_ no início do comando docker
⚠️ Caso queira adicionar seu usuário ao grupo docker, segue abaixo comandos para fazer isso.
⚠️ Os comandos _echo_ acima são para atualizar seu bashrc(caso utilize o bash como terminal) com alias para ficar mais fácil controlar o container(start, stop, psql)

## 📣 Adicionar usuário a lista docker
```bash
getent group docker || sudo groupadd docker && sudo usermod -aG docker $USER
newgrp docker
docker ps
```
| Se não ocorreu nenhum erro, seu usuário: $USER, foi adicionado com sucesso
