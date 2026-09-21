# StatLab — Gerador de Avaliações de Estatística com IA

O **StatLab** é uma aplicação web (Django) que gera **provas e gabaritos de Estatística em LaTeX** com apoio de IA generativa. O professor monta a avaliação escolhendo tópicos, formato das questões e o contexto do curso. O sistema sorteia moldes de questões cadastrados e pede à IA que produza enunciados com valores novos. Devolve, separados, o código LaTeX da **prova** e do **gabarito com resolução passo a passo**.

## Problema que resolve

Preparar várias versões de uma avaliação exige reescrever enunciados, trocar valores e refazer a resolução de cada versão à mão. O StatLab automatiza esse trabalho mantendo fixa a estrutura matemática de cada questão e adaptando o contexto ao público (Ensino Fundamental, Médio ou Superior).

## Como funciona

1. **Banco de esqueletos:** cada questão é guardada como um "esqueleto", com tópico, subtópico, enunciado-base com variáveis (X, Y, Z) e instruções de sorteio e resolução.
2. **Alimentação do banco:** os esqueletos são cadastrados ou importados pelo painel administrativo, ou extraídos automaticamente de uma lista de exercícios em PDF (a IA lê o PDF e o converte em moldes genéricos).
3. **Montagem da prova:** na página inicial o professor escolhe o curso (nível e contexto) e adiciona blocos de questão, definindo tópico, quantidade de subitens e formato (aberta ou múltipla escolha).
4. **Geração:** para cada bloco, o sistema sorteia um esqueleto do tópico, monta o prompt e o envia a um modelo DeepSeek via NVIDIA NIM.
5. **Saída:** a resposta é dividida em prova e gabarito, ambos em LaTeX.

## Para quem é

Professores de Estatística que precisam produzir avaliações e variações de questões com rapidez, sem abrir mão do rigor matemático.

## Tecnologias

Python 3.12 · Django 6 · SQLite · API compatível com OpenAI (NVIDIA NIM / DeepSeek) · PyPDF2 · django-import-export · Gunicorn + WhiteNoise · Docker (com TeX Live)

## Estrutura do repositório

- `statlab/`: configuração do projeto Django (settings, urls, wsgi/asgi)
- `gerador/`: app principal (modelos `Curso`, `Topico` e `EsqueletoQuestao`, views de geração e de treinamento, templates)
- `Dockerfile` e `requirements.txt`: empacotamento e deploy

## ⚙️ Primeira Instalação (Setup em um PC novo)

Se você for rodar este projeto pela primeira vez ou configurá-lo em um computador novo, siga este passo a passo do zero:

1. **Clone o repositório e entre na pasta:**
```bash
git clone https://github.com/ProdutosEduacionaisStatlab/produtoes_educacionais.git
cd produtoes_educacionais
```

2. **Crie o ambiente virtual:**

```bash
python -m venv .venv
```

3. **Ative o ambiente virtual:**

```bash
.venv\Scripts\activate
```

*(O terminal deve exibir um `(.venv)` verde no começo da linha).*

4. **Instale as bibliotecas obrigatórias:**

```bash
pip install -r requirements.txt
```

5. **Configure a Chave Secreta da API:**

* Acesse <https://build.nvidia.com> e faça login (ou crie uma conta gratuita).
* No menu do seu perfil, vá em **Settings → API Keys** e clique em **Generate API Key** (ou **Create key**).
* Copie a chave gerada (começa com `nvapi-`) — ela só é exibida uma vez, então salve antes de fechar a tela.
* Crie um arquivo chamado `.env` na raiz do projeto (na mesma pasta do `manage.py`).
* Cole a sua chave da NVIDIA dentro dele assim:

```text
NVIDIA_API_KEY=nvapi-sua-chave-aqui
```

6. **Prepare o Banco de Dados inicial e crie o Usuário Admin:**

```bash
python manage.py migrate
python manage.py createsuperuser
```

---

## Como Rodar o Projeto no Dia a Dia

Se o projeto já está configurado e você quer apenas ligar o sistema para programar ou testar:

1. **Ative o Ambiente Virtual:**

```bash
.venv\Scripts\activate
```

2. **Ligue o Servidor:**

```bash
python manage.py runserver
```

3. **Acesse o Sistema no Navegador (localmente):**

* **Página Inicial (Gerador):** http://127.0.0.1:8000/
* **Painel do Professor (Admin):** http://127.0.0.1:8000/admin/
---

## 🛠️ Comandos Úteis (Manutenção)

* **Atualizar o Banco de Dados (Use sempre que modificar o arquivo `models.py`):**

```bash
python manage.py makemigrations
python manage.py migrate
```

* **Criar um usuário administrador extra:**

```bash
python manage.py createsuperuser
```
