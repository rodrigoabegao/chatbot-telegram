🌦️ Bot de Clima no Telegram com n8n
Desafio – Fase 2

Este projeto implementa um bot de clima no Telegram utilizando n8n, integrando a API do OpenWeatherMap e o modelo Google Gemini para melhoria opcional da resposta final.

O workflow foi desenvolvido seguindo rigorosamente os requisitos da Fase 2 do desafio, incluindo fallback determinístico obrigatório para garantir funcionamento mesmo sem uso de IA.

🚀 Funcionalidades

✅ Recebe cidade via Telegram

✅ Normaliza entrada do usuário

✅ Trata acentuação automaticamente

✅ Consulta API OpenWeatherMap

✅ Gera resposta determinística

✅ Usa Google Gemini para melhorar a saída (opcional)

✅ Possui fallback completo sem IA

✅ Retorna sempre JSON estruturado

🧠 Arquitetura do Workflow
Telegram Trigger
   ↓
Tratamento da entrada
   ↓
Consulta OpenWeatherMap (com acento)
   ↓
Consulta OpenWeatherMap (sem acento - fallback)
   ↓
Code Node (mensagem determinística)
   ↓
Google Gemini (melhoria opcional)
   ↓
Fallback final
   ↓
Telegram Send Message
📌 Como Funciona
1️⃣ Entrada do Usuário

O usuário pode enviar:

São Paulo,SP
sao paulo sp
Vitória da Conquista
Salvador BA

O workflow:

Remove espaços extras

Remove apóstrofos

Ajusta capitalização

Detecta estado (UF)

Define país como BR por padrão

Monta query no formato:

Cidade,UF,BR
2️⃣ Consulta à API

Endpoint utilizado:

https://api.openweathermap.org/data/2.5/weather

Parâmetros:

units=metric

lang=pt_br

q=Cidade,UF,BR

A temperatura é retornada em graus Celsius.

3️⃣ Tratamento de Acentuação

Se a primeira tentativa falhar:

O workflow remove os acentos da cidade

Tenta novamente a consulta

Caso ainda falhe, retorna mensagem orientativa ao usuário

🤖 Uso do Google Gemini

O workflow inclui um nó Google Gemini Chat Model para melhorar a mensagem final.

Ele:

Reescreve a resposta de forma mais natural

Mantém os valores numéricos inalterados

Garante resposta em português

Retorna apenas JSON válido

Configuração utilizada:

Temperature: 0.1 (baixa, para respostas determinísticas)

Saída obrigatória em formato:

{"message":"texto","ok":true}

Sem emojis

Sem markdown

Sem texto adicional fora do JSON

🛡️ Fallback Obrigatório (Sem IA)

Caso o Google Gemini não esteja configurado:

O bot continua funcionando normalmente

Um Code Node determinístico gera a mensagem final

A avaliação pode ser realizada sem custos de IA

Exemplo de saída:

{
  "message": "A temperatura em Salvador é de 27°C.",
  "ok": true
}
🔑 Configuração de Credenciais (IMPORTANTE)
⚠️ Este repositório NÃO contém:

API Keys

Tokens do Telegram

Credenciais do Gemini

Todas as credenciais devem ser configuradas manualmente no n8n.

📌 1. Telegram

Acesse @BotFather no Telegram

Crie um novo bot

Copie o token gerado

No n8n:

Vá em Credentials

Adicione Telegram API

Insira o token

📌 2. OpenWeatherMap

Crie uma conta em:
https://openweathermap.org/

Gere sua API Key

No n8n:

Vá em Credentials

Adicione a credencial da API

Insira sua chave

📌 3. Google Gemini (Opcional)

Acesse o Google AI Studio

Gere sua API Key

No n8n:

Vá em Credentials

Adicione Google Gemini

Insira sua chave

⚠️ Caso não configure o Gemini, o workflow continuará funcionando usando o fallback determinístico.

🧪 Como Testar

Após configurar as credenciais:

Ative o workflow no n8n

Envie uma mensagem ao bot no Telegram:

São Paulo,SP

ou

Salvador BA

O bot retornará a temperatura atual da cidade.

📂 Estrutura do Repositório
/workflow-telegram-chatbot.json
/README.md
🔒 Segurança

Nenhuma chave está versionada

Nenhum token está exposto

Credenciais são gerenciadas pelo sistema seguro do n8n

O projeto segue boas práticas de segurança

📌 Requisitos do Desafio Atendidos

✔ Bot funcional no Telegram

✔ Integração com API externa

✔ Tratamento de entrada

✔ Tratamento de erro

✔ Uso do Google Gemini

✔ Temperatura baixa (0–0.2)

✔ Saída determinística em JSON

✔ Fallback obrigatório implementado

✔ Documentação completa

✔ Nenhuma chave exposta

👨‍💻 Autor

Rodrigo Garcia Abegão
Desafio Fase 2 — Bot de Clima com n8n
