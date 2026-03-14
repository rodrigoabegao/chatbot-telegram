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
  
    
  ## 🔑 Passo 1: Criando as Credenciais no Painel do n8n
  
  Para cada serviço, siga este fluxo básico:
  
  1. No menu lateral esquerdo do n8n, clique em **Credentials**.
  2. Clique no botão **Add Credential** (canto superior direito).
  3. Pesquise pelo nome do serviço e siga as instruções abaixo para cada um:
  
  ### 1. Telegram API
  
  * **Nome do Serviço:** Procure por `Telegram`.
  * **O que inserir:** Cole o **Access Token** que você recebeu do [@BotFather](https://t.me/botfather).
  * **Nome da Credencial no n8n:** Recomenda-se salvar como `telegramApi` para manter a compatibilidade com o arquivo JSON importado.
  
  ### 2. OpenWeatherMap API
  
  * **Nome do Serviço:** Procure por `OpenWeatherMap`.
  * **O que inserir:** Cole a sua **API Key** gerada no portal do [OpenWeatherMap](https://home.openweathermap.org/api_keys).
  * **Nome da Credencial no n8n:** Salve como `openWeatherMapApi`.
  
  ### 3. Google Gemini (AI)
  
  * **Nome do Serviço:** Procure por `Google Gemini(PaLM) Api`.
  * **O que inserir:** Cole a **API Key** obtida no [Google AI Studio](https://aistudio.google.com/).
  * **Nome da Credencial no n8n:** Salve como `googlePalmApi`.
  
  ---
  
  ## 🔗 Passo 2: Vinculando as Credenciais aos Nós
  
  Após criar as chaves, você precisa "avisar" aos nós do workflow que elas devem ser usadas:
  
  1. **No nó do Telegram:** Abra o nó `Telegram Trigger1`, vá até o campo **Credential for Telegram API** e selecione a credencial `telegramApi` que você criou.
  2. **No nó de Clima:** Abra o nó `HTTP Request1`, clique em **Authentication** e selecione `Generic Credential Type`. No campo **Credential**, selecione `openWeatherMapApi`.
  3. **No nó de IA:** Abra o nó `Google Gemini Chat Model`, vá em **Credential for Google Gemini(PaLM) Api** e selecione `googlePalmApi`.
  
  ---
  
  ## 💡 Dica de Especialista: O Teste de Conexão
  
  Sempre que terminar de configurar uma credencial, clique no botão **Test Connection** dentro da janela de edição da credencial. Se aparecer um check verde, a comunicação entre o n8n e o servidor externo (Telegram/Google) está funcionando perfeitamente.
  
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
