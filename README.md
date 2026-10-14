# Sistema de Monitoria de Alunos

**integrantes:**
 - Herbert Aciolly Leão de Jesus (Implementação de tecnologias externas - Maven e Json)
 - Murilo Fagundes Rodrigues (Criação das classes principais do sistema)

## Descrição do Problema
O projeto Sistema de Monitoria de Alunos resolve um problema clássico de gestão acadêmica: a desorganização, a falta de controle e a ineficiência na marcação e acompanhamento de sessões de atendimento entre alunos e monitores numa instituição de ensino. Sem um sistema automatizado, gerir monitorias de forma manual costuma gerar diversos entraves operacionais.

---

## Funcionalidades

- **Gestão de Agendamentos e Limites**
  - **Solicitação de Atendimento:** Permite registar novos pedidos de monitoria para os alunos, associando data, hora e um tópico de ajuda específico.
  - **Controle de Limites:** Bloqueia automaticamente novos pedidos caso o aluno já tenha atingido o limite máximo de 2 agendamentos ativos.
  - **Validação de Agenda do Monitor:** Verifica o calendário do monitor antes de confirmar um atendimento, impedindo que ele fique sobrecarregado com horários cruzados.
  - **Gestão de Status:** Permite atualizar os pedidos de pendentes para confirmados, preparando-os para a execução.
- **Fila de Prioridade Acadêmica**
  - **Organização Justa:** Classifica os atendimentos de forma inteligente, dando prioridade automática aos alunos que apresentam maior dificuldade escolar (médias entre 0.0 e 6.9).
- **Validação de Início:** Garante que uma sessão de monitoria só possa começar se o agendamento correspondente estiver com o status "Confirmado".
- **Relatório Obrigatório:** Exige o preenchimento de um registo detalhado ao encerrar o atendimento (contendo a descrição, o estado de resolução da dúvida e recomendações de estudo).
- **Persistência de Dados em JSON**
  - **Salvamento Automático:** Grava e lê o estado de toda a aplicação (alunos, agendamentos e dados) utilizando a biblioteca Jackson num arquivo local (sistema_monitoria.json), garantindo que nada se perde ao fechar o programa.
- **Menu Interativo via Terminal (CLI)**
  - **Navegação Prática:** Uma interface em linha de comandos completa que centraliza todas as ações num menu estruturado para testar o fluxo real do sistema.

---

## Principais Classes

- App (Main)
- Agendamento
- Aluno
- Disciplina
- GerenciadorFila
- Monitor
- PersistenciaJson
- RelatorioSessao
- SessaoMonitoria
- TopicoAjuda

---

## Regras de Negócio

### 1. Prevenção de Conflitos de Horário
- **O que faz:** O sistema valida automaticamente a agenda do monitor antes de confirmar qualquer atendimento.
- **Objetivo:** Impede que um monitor seja alocado para duas ou mais sessões exatamente no mesmo horário, evitando sobreposições.

### 2. Fila de Prioridade Acadêmica
- **O que faz:** Organiza a ordem de atendimento de forma inteligente com base no desempenho escolar do aluno.
- **Objetivo:** Garante prioridade automática na fila para alunos que apresentam maior dificuldade (com notas ou médias situadas entre 0.0 e 6.9).

### 3. Validação de Status Obrigatório
- **O que faz:** Estabelece uma regra rígida para o fluxo de atendimento: uma sessão de monitoria só pode ser iniciada se o agendamento estiver com o status "Confirmado".
- **Objetivo:** Evita que sessões pendentes ou canceladas avancem para a fase de execução sem a devida validação prévia.

### 4. Limite de Agendamentos Ativos
- **O que faz:** Controla a quantidade de pedidos que cada aluno pode manter em aberto em simultâneo, limitando-os a um máximo de 2 agendamentos ativos (sejam pendentes ou confirmados).
- **Objetivo:** Evita o monopólio de vagas e garante que outros alunos também tenham oportunidade de acesso à monitoria.

### 5. Relatório de Sessão Obrigatório
- **O que faz:** Exige o preenchimento e registo obrigatório de um relatório logo após o término de qualquer atendimento.
- **Objetivo:** O relatório deve conter a descrição detalhada do que foi feito, se a dúvida foi resolvida (true/false) e as recomendações de estudo passadas ao aluno, garantindo a rastreabilidade pedagógica.

---

## Tecnologias Utilizadas

- **Gestão de Dependências e Construção**
  - **Apache Maven:** A ferramenta responsável pela gestão do projeto através do ficheiro pom.xml, controlando a compilação, o ciclo de vida e a integração das bibliotecas externas.
- **Persistência de Dados**
  - **Jackson (Jackson Databind):** A biblioteca utilizada na classe PersistenciaJson.java para serializar e desserializar os objetos do sistema em formato JSON, permitindo guardar e recuperar o estado dos dados num ficheiro local (sistema_monitoria.json).
- **Testes Unitários**
  - **JUnit 5:** A framework moderna de testes utilizada para criar a suíte de testes unitários (AlunoLimiteTest, GerenciadorFilaTest, etc.), garantindo que todas as regras de negócio funcionam de forma rigorosa e automatizada.

---

## Instruções para Configuração

### 1. Pré-requisitos do Sistema
Certificas-te de que tens as seguintes ferramentas instaladas no teu computador:
- **JDK (Java Development Kit):** Versão 11 ou superior recomendada.
- **Apache Maven:** Para a gestão do ciclo de vida do projeto e dependências.
- **Visual Studio Code:** Com o Extension Pack for Java instalado para o suporte a projetos Java.

### 2. Estrutura de Pastas do Projeto
O projeto deve seguir rigorosamente o padrão de diretórios do Maven para evitar erros de classpath:
- `src/main/java/`: Onde ficam todas as classes de negócio principais (App.java, Aluno.java, Monitor.java, Agendamento.java, SessaoMonitoria.java, RelatorioSessao.java, TopicoAjuda.java, Disciplina.java, GerenciadorFila.java e PersistenciaJson.java).
- `src/test/java/`: Reservado exclusivamente para os testes unitários (*Test.java).

### 3. Configuração das Dependências (pom.xml)
No ficheiro pom.xml do teu projeto, deves garantir a inclusão da biblioteca Jackson (para a serialização/desserialização em JSON) e do JUnit 5 (para os testes):
- **Jackson Databind:** Responsável por converter os objetos do sistema no ficheiro sistema_monitoria.json.
- **JUnit 5:** Framework para validar as regras de negócio de forma automatizada.

### 4. Como Compilar e Executar a Aplicação
1 - Abre o terminal na pasta raiz do projeto (MonitoriaAlunos).

2 - Executa o comando para limpar e compilar o código: 
   **mvn clean compile**

3 - Para validar se todas as regras de negócio passam nos testes unitários, execute:
   **mvn test**

## Exemplos de Utilização

- **1º Cenário:** O monitor X tem uma sessão confirmada para terça-feira às 14:00. Um aluno faz um pedido de agendamento exatamente para esse mesmo horário. O monitor escolhe a Opção 2 (Confirmar Agendamento) para validar o pedido. 
  - **O que o sistema faz:** O sistema deteta o choque de horários na agenda do monitor e rejeita a confirmação, impedindo que o monitor fique sobrecarregado com dois atendimentos em simultâneo. 
  - **Objetivo:** Mantém a consistência da agenda do monitor livre de sobreposições.

- **2º Cenário:** O agendamento foi devidamente confirmado. O monitor escolhe a Opção 3 (Iniciar Sessão de Monitoria) e, após o atendimento terminar, avança para a Opção 4 (Finalizar Sessão com Relatório). 
  - **O que o sistema faz:** Na Opção 3, verifica se o status está estritamente como "Confirmado" antes de abrir a sessão. Na Opção 4, exige obrigatoriamente o preenchimento de dados de fecho: uma descrição do atendimento, se a dúvida foi resolvida (true ou false) e as recomendações de estudo. 
  - **Objetivo:** Garante rastreabilidade pedagógica e assegura que nenhuma sessão termina sem o devido registo de acompanhamento.

## Uso de IA

| Ferramenta | Objetivo | Resumo do uso | Revisão pelo Dev |
| :--- | :--- | :--- | :--- |
| Gemini 3 Nível MAX de Raciocínio | Código Base, Regras de Negócio e Documentação | Auxiliou na estruturação do código base em Java, implementação e correção das regras de negócio, resolução de erros do Maven/classpath, suporte na persistência JSON e suporte na estética da estrutura do README.md. | Validação dos testes unitários e manuais, execução do projeto via CLI, revisão de erros e adaptação do código aos requisitos do projeto. |
