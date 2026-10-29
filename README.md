# 🍔 AI Menu Onboarding Copilot

Ferramenta de automação construída no **Make** para acelerar a entrada de novos restaurantes no catálogo de plataformas de delivery, utilizando **IA Multimodal (Vision)** para transcrever cardápios físicos diretamente para bancos de dados estruturados.

---

## 🎯 Problema de Negócio

No ecossistema de aquisição de parceiros (Restaurantes), o tempo de ativação (Time-to-Live) é uma métrica crucial. Atualmente, os executivos de vendas tiram fotos de cardápios impressos, e a equipe de *Menu Onboarding* precisa digitar manualmente centenas de itens, ingredientes e preços no sistema.
Isso gera:
* **Gargalo de Onboarding:** Filas de espera de até 5 dias para ativação do restaurante.
* **Erros de Digitação:** Preços errados que geram prejuízo ou cancelamentos.
* **Alto Custo Operacional:** Horas humanas gastas em tarefas repetitivas de *data entry*.

### 📊 Impacto Estimado

| Métrica | Processo Manual Tradicional | Com o AI Onboarding Copilot | Ganho Operacional |
| :--- | :--- | :--- | :--- |
| **Tempo de Processamento por Cardápio** | ~40 a 60 minutos | < 15 segundos (API Vision) | **99% de redução de tempo** |
| **Acurácia na Estruturação** | Sujeita a fadiga humana | Formato JSON estrito e tipado | **Redução de erros no BD** |
| **Time-to-Live (Ativação de Parceiro)** | 3 a 5 dias úteis | Mesma hora (Real-time) | **Aumento na taxa de conversão** |

---

## 🏗️ Arquitetura do Sistema
[Formulário Comercial (Typeform / Portal)]
└─ Envio da Imagem (JPG/PDF)
│
▼
[Webhook Inbound (Make)]
│
▼
[HTTP Request] ──► Faz o download temporário da imagem em Base64
│
▼
[LLM Vision Engine (GPT-4o / Gemini 1.5 Pro)]
├── Extração Textual Avançada (OCR)
├── Compreensão Semântica (Diferenciar preço de caloria)
└── Formatação Estrita via JSON Schema
│
▼
[Roteamento e Gravação]
├── [Iterator] ─────► Separa o array de itens do cardápio
├── [Google Sheets] ─► Salva na fila de Catálogo para revisão do N1
└── [Slack] ─────────► Notifica o Executivo de Vendas ("Cardápio Digitalizado!")

---

## 🚀 Como Executar o Projeto

### 1. Pré-requisitos
* Conta no [Make.com](https://www.make.com/).
* Chave de API da OpenAI (com saldo para o modelo GPT-4o) ou Google Gemini.
* Uma planilha no Google Sheets para receber os dados estruturados.

### 2. Configuração do Workflow
1. No Make, crie um novo cenário.
2. Na barra inferior, clique nos três pontos (`...`) e selecione **"Import Blueprint"**.
3. Carregue o arquivo `workflows/blueprint.json` deste repositório.
4. Conecte as suas credenciais (Google e OpenAI) nos módulos que apresentarem aviso de autenticação.
5. Copie o prompt contido na pasta `prompts/` e cole na configuração de sistema do módulo da LLM.

### 3. Teste Manual (Payload de Exemplo)

Você pode testar o Webhook de entrada enviando um POST via Postman ou cURL:

```json
{
  "restaurant_id": "REST-5092",
  "image_url": "[https://exemplo.com/fotos/cardapio-hamburgueria.jpg](https://exemplo.com/fotos/cardapio-hamburgueria.jpg)",
  "requested_by": "executivo.vendas@empresa.com"
}