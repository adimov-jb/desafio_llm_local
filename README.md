# Desafio LLM Local

Projeto em Python que conversa com um modelo de linguagem (LLM) rodando **localmente** na sua máquina, usando o [LM Studio](https://lmstudio.ai/) como servidor.

O script `desafio.py` envia uma pergunta ao modelo e mostra a resposta no terminal. Ele usa a biblioteca `openai`, mas nada vai para a nuvem: as requisições são feitas para o servidor local do LM Studio, que imita a API da OpenAI.

## Como funciona

- **Servidor:** LM Studio, em `http://127.0.0.1:1234/v1`
- **Modelo:** `google/gemma-3-1b`
- **Personalidade:** um prompt de sistema faz o assistente responder de forma sarcástica
- **Pergunta de exemplo:** "O que é a IA Generativa?"
- **Temperatura:** `1.0`, para respostas mais criativas

## Pré-requisitos

- [Python 3](https://www.python.org/downloads/)
- [LM Studio](https://lmstudio.ai/) instalado

## Como rodar

1. **Prepare o LM Studio**
   - Abra o LM Studio e baixe o modelo `google/gemma-3-1b`.
   - Na aba **Developer**, carregue o modelo e inicie o servidor local (porta `1234`).

2. **Crie e ative um ambiente virtual** (opcional, mas recomendado)

   ```powershell
   python -m venv desafio
   .\desafio\Scripts\Activate.ps1
   ```

3. **Instale a dependência**

   ```powershell
   pip install openai
   ```

4. **Execute o script**

   ```powershell
   python desafio.py
   ```

## Personalizando

Em `desafio.py`, você pode trocar:

- `model`: qualquer modelo que estiver carregado no LM Studio
- a mensagem `system`: muda a personalidade do assistente
- a mensagem `user`: muda a pergunta
- `temperature`: valores menores dão respostas mais previsíveis; valores maiores, mais variadas

## Arquivos

| Arquivo | Descrição |
|---|---|
| `desafio.py` | Script que envia a pergunta ao LLM local |
| `Resenhas_App_ChatGPT.txt` | Resenhas de usuários do app ChatGPT (formato `id$nome$texto`), usadas como dados do desafio |
