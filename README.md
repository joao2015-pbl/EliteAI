# EliteAI

EliteAI — uma IA brasileira desenvolvida por João (joao2015-pbl).

Descrição
---------
EliteAI é uma aplicação web em Python (Flask) com interface em HTML que serve como front-end para um agente conversacional. O repositório contém código do servidor (`app.py`), templates HTML em `templates/`, um arquivo de memória (`memoria.json`), e configurações úteis como `requirements.txt`, `Procfile` e `buildozer.spec`.

Aviso de segurança
------------------
Há chaves de API comprometidas no código (`app.py`) como exemplos — NÃO deixe chaves reais no repositório público. Use variáveis de ambiente para configurar credenciais (explico abaixo). Se já houver chaves sensíveis aqui, substitua-as e gere novas chaves nos serviços respectivos.

O que já existe neste repositório (principais arquivos)
- app.py — servidor Flask / lógica do bot (ponto de entrada)
- requirements.txt — dependências: Flask, requests, gunicorn
- Procfile — comando para deploy em PaaS (Heroku)
- templates/ — pasta com arquivos HTML (ex.: `index.html` usado por `app.py`)
- memoria.py / memoria.json — utilitários / dados de memória
- buildozer.spec — configuração para empacotamento Android (opcional)
- LICENSE — arquivo de licença

Variáveis de ambiente importantes
- GROQ_API_KEY — chave para a API do provedor de LLM utilizado por `app.py` (recomendado configurar)
- NEWS_API_KEY — chave da NewsAPI (se usar pesquisas de notícias)
- WEATHER_API_KEY — chave para OpenWeatherMap
- PORT — porta de execução (opcional)

Instalação e execução (várias formas)
-----------------------------------
Abaixo estão instruções “reais” cobrindo desenvolvimento local, produção com Gunicorn, deploy via Heroku, execução com Docker e empacotamento Android com Buildozer.

1) Preparar (comum a quase todos os métodos)
- Clone o repositório:
  git clone https://github.com/joao2015-pbl/EliteAI.git
  cd EliteAI
- Abra o arquivo `app.py` e verifique se o ponto de entrada e portas estão conforme deseja.

2) Instalação rápida com Python + virtualenv (modo desenvolvedor)
- Recomendado para desenvolvimento local.

Linux/macOS:
- python3 -m venv venv
- source venv/bin/activate
- pip install --upgrade pip
- pip install -r requirements.txt

Windows (PowerShell):
- python -m venv venv
- venv\Scripts\Activate.ps1
- python -m pip install --upgrade pip
- pip install -r requirements.txt

Configurar variáveis de ambiente (exemplo Linux/macOS):
- export GROQ_API_KEY="sua_chave"
- export NEWS_API_KEY="sua_chave"
- export WEATHER_API_KEY="sua_chave"

Executar em modo desenvolvimento:
- python app.py
- Acesse: http://localhost:5002 (o `app.py` por padrão roda em 0.0.0.0:5002)

3) Executar em produção com Gunicorn
- Instale as dependências (depois de ativar venv): pip install -r requirements.txt
- Execute:
  gunicorn -w 4 -b 0.0.0.0:8000 app:app
- Usar um proxy reverso (Nginx) é recomendado em produção.

4) Usar Docker (recomendado para portabilidade)
- Crie um arquivo `Dockerfile` (exemplo):

  FROM python:3.11-slim
  WORKDIR /app
  COPY requirements.txt /app/
  RUN pip install --no-cache-dir -r requirements.txt
  COPY . /app
  ENV PORT=5002
  EXPOSE 5002
  CMD ["gunicorn", "-w", "2", "-b", "0.0.0.0:5002", "app:app"]

- Build e run:
  docker build -t eliteai:latest .
  docker run -e GROQ_API_KEY="sua_chave" -p 5002:5002 eliteai:latest

5) Deploy no Heroku (usando Procfile já presente)
- Requisitos: ter Heroku CLI instalado e estar logado (heroku login)
- Passos mínimos:
  heroku create nome-do-app
  git push heroku main
  heroku config:set GROQ_API_KEY="sua_chave" NEWS_API_KEY="sua_chave" WEATHER_API_KEY="sua_chave"
  heroku ps:scale web=1
- O `Procfile` do repositório indica o comando de start; ajuste conforme seu entrypoint (ex.: `web: gunicorn app:app`).

6) Empacotar para Android usando Buildozer (opcional — há `buildozer.spec`)
- Requisitos: Linux (ou WSL) + Buildozer + SDK/NDK do Android.
- Instalar buildozer e dependências (exemplo Debian/Ubuntu):
  sudo apt update && sudo apt install -y python3-pip python3-venv build-essential
  python3 -m pip install --user buildozer
  # instalar dependências do Android (SDK/NDK) conforme documentação Buildozer

- Preparar e gerar apk:
  buildozer android debug

Consulte a documentação oficial do Buildozer para passos completos — este processo exige configuração adicional.

Configurações e pontos importantes do `app.py`
- O app consulta várias APIs externas (Groq, NewsAPI, OpenWeather, CoinGecko, DuckDuckGo, Reddit, NASA). Certifique-se de definir as chaves necessárias como variáveis de ambiente.
- `memoria.json` é usado para armazenar o histórico local; não é obrigatório, mas ajuda a manter contexto entre reinícios.
- Ajuste `PERSONALIDADE` em `app.py` com cuidado — atualmente contém instruções muito permissivas. Considere revisar restrições de segurança e conformidade.

Boas práticas
- Nunca commit chaves em texto puro. Use `.env` local (e adicione `.env` ao .gitignore) ou serviços de secrets do provedor de nuvem.
- Adicione um `Procfile` correto para o provedor de PaaS que usar.
- Use um arquivo `.gitignore` para excluir `venv/`, `memoria.json` e outros artefatos locais.

Como contribuir
- Abra issues para bugs/enhancements.
- Fork → branch de feature → Pull Request com descrição clara.

Testes
- Não há testes automatizados no repositório atualmente. Recomenda-se adicionar `pytest` e fornecer comandos em `requirements-dev.txt`.

Licença
- Há um arquivo LICENSE no repositório — verifique qual licença está aplicada e mantenha conforme necessário.

Dúvidas ou mais ajustes
- Posso atualizar o README com exemplos de `Dockerfile`, `Procfile` e `.gitignore` no repositório se quiser.
- Posso também remover chaves sensíveis do código e sugerir um `.env.example` com variáveis necessárias.

---

Se quiser, eu já atualizo o README.md com este conteúdo no repositório agora. Confirma para eu commitar?