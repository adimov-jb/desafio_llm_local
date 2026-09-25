# Desafio LLM Local

Projeto em Python que usa um modelo de linguagem (LLM) rodando **localmente**, via [LM Studio](https://lmstudio.ai/), para **analisar resenhas de usuários do aplicativo ChatGPT**.

## Objetivo

Ler um arquivo com resenhas escritas em vários idiomas e, para cada uma, usar o LLM para:

1. **Identificar o usuário** que escreveu a resenha
2. **Traduzir** a resenha para o português
3. **Classificar** a resenha como `Positiva`, `Negativa` ou `Neutra`

No final, o script mostra quantas resenhas há de cada tipo e lista todas as resenhas já processadas.

O código usa a biblioteca `openai`, mas nada vai para a nuvem: as requisições vão para o servidor local do LM Studio, que imita a API da OpenAI.

## Como funciona

```
Resenhas_App_ChatGPT.txt ──► desafio.py ──► contato_com_LLM.py ──► LM Studio (gemma-3-1b)
                                 │                                      │
                                 ◄──────────── JSON por resenha ◄───────┘
                                 │
                                 ▼
                     contagem + textos unidos
```

1. `desafio.py` lê o arquivo `Resenhas_App_ChatGPT.txt`, uma resenha por linha.
2. Cada linha é enviada para a função `recebe_linha_e_retorna_json` (em `contato_com_LLM.py`), que pede ao LLM um JSON com as chaves:
   - `usuario`: nome de quem escreveu a resenha
   - `resenha_original`: texto no idioma original
   - `resenha_pt`: texto traduzido para o português
   - `avaliacao`: `Positiva`, `Negativa` ou `Neutra`
3. As respostas são convertidas em dicionários Python com `json.loads`.
4. A função `contador_e_juntador` conta as avaliações e junta todas as resenhas em um único texto, separadas por `#####`.

### Configuração do LLM

- **Servidor:** LM Studio, em `http://127.0.0.1:1234/v1`
- **Modelo:** `google/gemma-3-1b`
- **Temperatura:** `0.0`, para respostas mais consistentes

## Pré-requisitos

- [Python 3](https://www.python.org/downloads/)
- [LM Studio](https://lmstudio.ai/) instalado

## Como rodar

1. **Prepare o LM Studio**
   - Abra o LM Studio e baixe o modelo `google/gemma-3-1b`.
   - Na aba **Developer**, carregue o modelo e inicie o servidor local (porta `1234`).

2. **Crie e ative um ambiente virtual**

   ```powershell
   python -m venv desafio
   .\desafio\Scripts\Activate.ps1
   ```

   Se o PowerShell bloquear a ativação, rode uma vez:
   `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`

3. **Instale a dependência**

   ```powershell
   pip install openai
   ```

4. **Execute o script**

   ```powershell
   python desafio.py
   ```

### Exemplo de saída

```
Positivas: 12
Negativas: 10
Neutras: 2
{'usuario': '...', 'resenha_original': '...', 'resenha_pt': '...', 'avaliacao': 'Positiva'}#####{...}
```

## Arquivos

| Arquivo | Descrição |
|---|---|
| `desafio.py` | Lê as resenhas, envia cada uma ao LLM, conta as avaliações e mostra o resultado |
| `contato_com_LLM.py` | Conversa com o LM Studio e devolve o JSON de cada resenha |
| `Resenhas_App_ChatGPT.txt` | Resenhas do app ChatGPT, no formato `id$nome_do_usuario$texto_da_resenha` |

## Limitações conhecidas

- O `gemma-3-1b` é um modelo pequeno: às vezes ele traduz errado, classifica errado ou esquece alguma chave do JSON. Modelos maiores no LM Studio dão resultados melhores.
- Resenhas sem a chave `avaliacao` são contadas como `Neutras`.
