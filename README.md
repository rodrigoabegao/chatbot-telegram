# 🌤️ Telegram WeatherBot: Orquestração com n8n, IA e APIs Geográficas

Este repositório contém o workflow para um bot do Telegram que fornece dados meteorológicos em tempo real. O sistema utiliza o **n8n** como orquestrador central, integra-se à **API do OpenWeatherMap** e aplica **Inteligência Artificial (Google Gemini)** para o refinamento de linguagem natural (NLG).

## 🏗️ Arquitetura do Sistema

O fluxo de dados foi projetado seguindo princípios de **Clean Code** e **resiliência**, dividido em quatro camadas modulares:

1. **Ingestão & Validação:** Recebimento de webhooks e verificação de integridade dos dados.
2. **Processamento Geográfico:** Normalização de strings e sanitização via JavaScript para evitar falhas em nomes com acentos ou caracteres especiais.
3. **Integração de Dados:** Consumo de serviços externos via REST API.
4. **Camada Cognitiva:** Processamento de dados brutos através de Large Language Models (LLM) para gerar respostas humanizadas.

---

## 🔑 Configuração de Credenciais e Variáveis

Para que o workflow opere corretamente, as seguintes credenciais devem ser configuradas no seu ambiente n8n:

| Credencial | Provedor | Função no Fluxo |
| --- | --- | --- |
| `telegramApi` | [Telegram BotFather](https://t.me/botfather) | Permite o recebimento de triggers e o envio de mensagens. |
| `openWeatherMapApi` | [OpenWeatherMap](https://openweathermap.org/) | Fornece dados técnicos como temperatura, umidade e condições climáticas. |
| `googlePalmApi` | [Google AI Studio](https://aistudio.google.com/) | Utilizada pelo modelo Gemini para naturalização da resposta final. |

---

## 📥 Guia de Instalação (Passo a Passo)

1. **Download do Projeto:** Baixe o arquivo `workflow-telegram-chatbot (1).json`.
2. **Importação no n8n:**
* Acesse sua instância do n8n.
* No painel lateral, vá em **Workflows** > **Add Workflow**.
* No menu de opções (três pontos no canto superior direito), selecione **Import from File**.
* Selecione o arquivo JSON baixado.


3. **Vinculação de Credenciais:**
* Abra os nós **Telegram Trigger1**, **HTTP Request1** e **Google Gemini Chat Model**.
* Em cada um, selecione a credencial correspondente criada previamente no seu painel de `Credentials`.


4. **Ativação:**
* Clique no botão **Save** e, em seguida, alterne a chave para **Active** no canto superior direito para registrar o webhook no Telegram.



---

## 🛡️ Protocolo de Robustez (Senior Features)

### 1. Pre-Flight Check (Validação Inicial)

O nó `Code in JavaScript2` atua como um firewall de inicialização. Ele valida se a mensagem recebida possui texto e se o `chatId` está presente antes de processar qualquer lógica, economizando recursos de processamento e tokens de API.

### 2. Normalização de Strings (`Code in JavaScript1`)

Utilizamos lógica avançada de **Regex** para tratar entradas variadas (ex: "São Paulo, SP", "curitiba br"). O script remove espaços extras, trata apóstrofos e aplica `encodeURIComponent` para garantir que a requisição HTTP não quebre com caracteres especiais.

### 3. Tratamento de Erros e Fallback

O workflow possui um nó condicional (`If`) que verifica se a cidade foi encontrada pela API. Caso ocorra um erro (Cidade não encontrada), o fluxo é desviado para uma mensagem de erro educativa preparada pelo nó `Code in JavaScript`, que sugere ao usuário a correção da grafia.

---

## ⚙️ Especificações Técnicas

* **Versão n8n:** 1.9+.
* **Modelo de IA:** Gemini 1.5 Flash (configurado para baixa latência).
* **Formato de Resposta:** JSON Estrito (garante que a saída do LLM seja sempre interpretável pelo nó de envio).

---

**Documentação gerada por Rodrigo Garcia Abegão.**
