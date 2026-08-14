# EliteAI

EliteAI — uma IA brasileira desenvolvida por João (joao2015-pbl).  
Aplicação web em Flask com interface HTML que serve como front-end para um agente conversacional.

---

Sumário
- Descrição
- Avisos de segurança importantes
- O que existe no repositório
- Variáveis de ambiente (necessárias)
- Instalação e execução
  - Desenvolvimento (local)
  - Produção com Gunicorn
  - Docker
  - Deploy no Heroku
  - Empacotamento Android (Buildozer)
  - Systemd (exemplo de serviço)
- Endpoints e exemplos de uso (curl)
- Arquivos importantes e comportamento
- Boas práticas de segurança
- Troubleshooting
- Contribuições e licença
- Contato

---

Descrição
EliteAI é um chatbot/assistente construído em Python usando Flask, com templates HTML para interface. O servidor (ponto de entrada) é `app.py`, que consulta várias APIs externas para enriquecer respostas (clima, notícias, cripto etc.) e mantém um histórico local em `memoria.json`.

Avisos de segurança importantes
- Há chaves de API presentes no código como exemplos. NÃO deixe chaves reais em repositórios públicos. Revogue qualquer chave que já tenha sido comprometida.
- O arquivo `app.py` contém um texto de "PERSONALIDADE" com instruções que podem ser perigosas ou antiéticas. Revise e restrinja conforme necessário para conformidade com leis e políticas.
- Nunca compartilhe tokens ou credenciais em commits públicos.

O que existe no repositório
- `app.py` — servidor Flask (ponto de entrada)
- `requirements.txt` — dependências (flask, requests, gunicorn)
- `templates/` — arquivos HTML (ex.: `index.html`)
- `memoria.py`, `memoria.json` — utilitários / armazenamento de histórico
- `Procfile` — comando para PaaS (Heroku)
- `buildozer.spec` — configuração para empacotamento Android (opcional)
- `LICENSE` — licença do projeto

Variáveis de ambiente importantes
- `GROQ_API_KEY` — chave para provedor LLM (usado por `app.py`)
- `NEWS_API_KEY` — NewsAPI
- `WEATHER_API_KEY` — OpenWeatherMap
- `PORT` — porta (opcional; padrão 5002 no `app.py`)

Exemplo de `.env.example`
GROQ_API_KEY=your_groq_api_key_here  
NEWS_API_KEY=your_newsapi_key_here  
WEATHER_API_KEY=your_openweathermap_key_here  
PORT=5002

---

Instalação e execução (várias opções)

Pré-requisitos
- Python 3.8+ (recomendado 3.10/3.11)
- git
- Docker (se for usar Docker)
- Heroku CLI (se for usar Heroku)
- Para Buildozer: Linux (ou WSL), SDK/NDK Android, Buildozer instalado

1) Preparar (comum)
```bash
git clone https://github.com/joao2015-pbl/EliteAI.git
cd EliteAI