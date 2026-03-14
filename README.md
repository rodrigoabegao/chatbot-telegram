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

## 📥 Guia de Instalação e Importação (Passo a Passo)

Para garantir que o projeto seja replicado sem erros, siga rigorosamente estas etapas:

1. **Download do Arquivo:** Obtenha o arquivo `workflow-telegram-chatbot (1).json`.
2. **Acesso ao n8n:** Abra sua instância do n8n (Desktop, Cloud ou Self-hosted).
3. **Importação:**
* No menu lateral esquerdo, clique em **Workflows**.
* Clique no botão **Add Workflow** (ou abra um novo).
* No menu de três pontos (`...`) no canto superior direito, selecione **Import from File**.
* Selecione o arquivo `.json` baixado.


4. **Configuração de Credenciais:** Os nós importados aparecerão com ícones de alerta. Você deve clicar neles e vincular suas chaves (veja a seção abaixo).
5. **Ativação:** Salve o workflow (`Ctrl+S`) e alterne a chave para **Active**. Isso registrará o webhook automaticamente no Telegram.

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
