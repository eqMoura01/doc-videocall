### Documento de Engenharia de Software: Implementação de Sistema de Videoconferência e Transcrição

#### Issue: #9803

---

## 1. **Introdução**

Este documento visa detalhar os requisitos e a arquitetura de uma nova funcionalidade de videoconferência (denominada "Call") para o sistema atual, integrando transcrição de áudio para texto (STT - Speech-to-Text) e geração automática de laudos médicos com base no conteúdo das conferências. A implementação deve evitar o uso de APIs de terceiros para videoconferências e adotar soluções de transcrição e inteligência artificial para automação de laudos.

---

## 2. **Objetivos do Sistema**
1. **Criar um link para a Call**: O sistema deve permitir que o médico crie um link exclusivo para cada videoconferência (Call) que será compartilhado com o paciente.
2. **Enviar o link para o paciente**: O sistema deve possibilitar o envio do link via e-mail, SMS, WhatsApp ou outros meios de comunicação.
3. **Remover dependência de APIs de terceiros**: Atualmente, o sistema usa APIs como Zoom e Google Meet. O novo sistema deve implementar sua própria solução de videoconferência.
4. **Integrar STT**: Um sistema de transcrição automática deve ser incorporado para converter o áudio da conferência em texto.
5. **Geração de laudo**: A partir da transcrição da conferência, o sistema deve gerar laudos médicos de forma automática usando uma solução de IA.
6. **IA para laudos**: O sistema deve usar uma API de IA para a criação dos laudos médicos. As opções sugeridas incluem OpenAI (API pronta) ou Llama (modelo open-source).
7. **Armazenamento de gravações**: O áudio da Call deve ser persistido, mas a gravação de vídeo não é um requisito prioritário.

---

## 3. **Requisitos Funcionais**

### 3.1. Videoconferência (Call)
- **RF-01**: O sistema deve permitir que o médico inicie uma videoconferência.
- **RF-02**: O sistema deve gerar um link único para a Call.
- **RF-03**: O sistema deve possibilitar o envio do link por vários meios (e-mail, SMS, WhatsApp).
- **RF-04**: O sistema não deve depender de APIs externas para a realização da videoconferência.

### 3.2. Transcrição (STT)
- **RF-05**: O sistema deve transcrever automaticamente o áudio da Call em texto.
- **RF-06**: A transcrição deve ocorrer em tempo real e ser associada ao médico da Call.
- **RF-07**: O sistema deve persistir as transcrições no banco de dados.

### 3.3. Geração de Laudos Médicos
- **RF-08**: Após a Call, o sistema deve processar a transcrição e gerar um laudo médico.
- **RF-09**: A geração de laudo deve ser realizada através de uma IA integrada (OpenAI ou Llama).
- **RF-10**: O laudo gerado deve ser armazenado no sistema e associado ao paciente e à consulta.

### 3.4. Armazenamento de Gravações
- **RF-11**: O áudio da Call deve ser armazenado para consulta futura.
- **RF-12**: A persistência de vídeo da Call não é necessária.

---

## 4. **Requisitos Não Funcionais**

### 4.1. Performance
- O sistema deve garantir baixa latência para garantir uma boa qualidade na videoconferência e transcrição em tempo real.
  
### 4.2. Segurança
- O tráfego de vídeo, áudio e dados deve ser criptografado utilizando protocolos como TLS e SRTP.
- O acesso à videoconferência deve ser restrito aos participantes com links de acesso únicos e autenticação.

### 4.3. Escalabilidade
- A solução de videoconferência deve ser escalável para múltiplos médicos e pacientes ao mesmo tempo.
  
### 4.4. Custos
- A implementação de STT e IA deve levar em consideração os custos de uso de serviços pagos como OpenAI. A opção por modelos de IA gratuitos, como Llama, também deve ser considerada.

---

## 5. **Arquitetura do Sistema**

### 5.1. Componentes Principais
- **Frontend (Angular)**:
  - Interface do médico e paciente para iniciar e ingressar na Call.
  - Integração com o STT para exibir transcrições em tempo real.
  
- **Backend (Spring Boot)**:
  - Gerenciamento de videoconferências (geração de links e autenticação).
  - Integração com o STT para transcrever o áudio.
  - Integração com a API de IA para geração de laudos médicos.

### 5.2. Fluxo de Dados

1. **Médico Inicia a Call**: O médico inicia uma Call no frontend, e o backend gera um link único que é enviado ao paciente.
2. **Paciente Conecta-se à Call**: O paciente clica no link e ingressa na videoconferência.
3. **Transcrição em Tempo Real**: O áudio do médico é capturado no frontend, enviado ao backend e transcrito em tempo real usando STT.
4. **Geração de Laudo**: Após o fim da Call, o texto transcrito é enviado à API da IA (OpenAI ou Llama) para geração de um laudo médico.
5. **Persistência**: O laudo gerado e o áudio da Call são armazenados no sistema.

---

## 6. **Escolha da API de IA**

### 6.1. OpenAI (API Pronta)
- **Prós**: Solução madura, fácil de integrar, excelente qualidade de geração de texto.
- **Contras**: Custos contínuos de uso.

### 6.2. Llama (Meta) – Modelo Local
- **Prós**: Sem custos por uso (após a configuração inicial), maior controle sobre o processamento.
- **Contras**: Requer infraestrutura robusta, como GPU/NPU de alto desempenho. Maior esforço de estudo e configuração.

---

## 7. **Plano de Implementação**

### 7.1. Etapa 1: Configuração da Videoconferência
- **Tarefa 1**: Implementar geração de links para a Call.
- **Tarefa 2**: Criar interface de videoconferência com WebRTC.

### 7.2. Etapa 2: Integração do STT
- **Tarefa 1**: Integrar solução de STT no backend (usar API existente ou uma nova).
- **Tarefa 2**: Exibir transcrição em tempo real no frontend.

### 7.3. Etapa 3: Geração de Laudos
- **Tarefa 1**: Configurar integração com API de IA (OpenAI ou Llama).
- **Tarefa 2**: Implementar fluxo de geração de laudos a partir da transcrição.

---

## 8. **Riscos e Considerações**

- **Risco 1**: Custos crescentes com APIs de STT e IA (mitigado pela avaliação do uso do Llama).
- **Risco 2**: Escalabilidade da infraestrutura de videoconferência ponto a ponto com WebRTC.
- **Risco 3**: Latência na geração de laudos, dependendo do desempenho da IA utilizada.

---

## 9. **Conclusão**

Este documento define a abordagem técnica e funcional para a implementação da funcionalidade de videoconferência e transcrição, com geração de laudos médicos automatizada. O projeto segue o objetivo de independência de APIs externas para videoconferências e oferece alternativas para o uso de STT e IA, equilibrando custos e desempenho.

## 10. **Transcrição exemplo (Gerado por IA)**
[Transcrição de consulta médica](transcricao-consulta.md)

## 11. **Laudo gerado com base na transcrição (Gerado por IA)**
[Laudo](laudo-gerado-por-ia.md)

## 12. **Estudo sobre tecnologias**
[Material de apoio](estudos-necessarios.md)

## 13. **RoadMap Desenvolvimento**
[RoadMap](ordem-implementação.md)
