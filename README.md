# infra-lab-playbook
Infrastructure  lab documentation, procedures and confis

## Sync com GIT
### 1. Cria o repositório no GitHub
Acessa github.com/new, preenche nome, descrição, marca Add README e cria.

### 2. Clona no computador
````bash
cd ~
git clone git@github.com:magnogl/nome-do-repositorio.git
````
### 3. Entra na pasta e trabalha
````bash
cd nome-do-repositorio
# cria arquivos, pastas, edita o que precisar
````
#### 4. Fluxo de commit sempre que quiser salvar
````bash
git add .                        # adiciona tudo que mudou
git status                       # confere o que vai ser enviado
git commit -m "mensagem curta"   # salva o ponto no histórico
git push                         # envia para o GitHub
````

O git add . adiciona tudo na pasta atual. Se quiser adicionar só uma pasta específica usa git add docs/ ou um arquivo específico git add docs/ssh/arquivo.md.
