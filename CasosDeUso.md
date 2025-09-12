# Lista Completa de Casos de Uso do Sistema UE

## **📊 Casos de Uso**


```mermaid
---
config:
  theme: neutral
---
graph LR
    %% Atores
    %% 1. Defina uma classe CSS para a forma oval
    

    E["<div style="font-size:120px">𖨆</div><br><div style="font-size:30px">Eleitor</div>"]
    A["<div style="font-size:120px">𖨆</div><br><div style="font-size:30px">Admin</div>"]
    
    UC1(("Votar"))
    UC2(("Realizar Cadastro"))
    UC3((Gerenciar Candidatos))
    UC4((Gerenciar Eleitores))
    UC5((Gerenciar Eleição))
    UC6((Monitorar Votação))
    UC7((Apurar Resultados))
    UC8((Validar<br/>Cadastro Eleitor))

    
    %% Relacionamentos - Eleitor
    E --> UC1
    E --> UC2
    
    %% Relacionamentos - Administrador
    A --> UC3
    A --> UC4
    A --> UC5
    A --> UC6
    A --> UC7
    A --> UC8
    UC1 -.->|" << include >> "| UC8
    
    %% Estilos
    classDef ator fill:#ffffff,stroke:#ffffff,stroke-width:2px,color:#000
    classDef casoUso fill:#fff2cc,stroke:#d6b656,stroke-width:1px,color:#000,width:90px
    classDef ovalNode fill:#fff,stroke:#000,border-radius:50%
    
    class E,A ator
    class UC1,UC2,UC3,UC4,UC5,UC6,UC7,UC8 ovalNode
    
```

---

# Especificação Detalhada dos Casos de Uso (UML)

## **UC1 - Realizar Votação**

| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Eleitor |
| **Objetivo** | Permitir que o eleitor vote nos candidatos de sua escolha |
| **Pré-condições** | • UEv configurada e operacional<br/>• Eleitor habilitado para votar<br/>• Eleição em andamento |
| **Pós-condições** | • Voto registrado no sistema<br/>• Eleitor marcado como votante<br/>• Log de auditoria gerado |
| **Fluxo Principal** | **1.** Eleitor inicia sessão na UEv<br/> **2.** Sistema solicita identificação do eleitor<br/>**3.** Eleitor informa número de inscrição<br/>**4.** Sistema valida elegibilidade do eleitor<br/>**5.** Sistema apresenta interface de votação<br/>**6.** Para cada cargo disponível:<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.1** Sistema exibe candidatos do cargo<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.2** Eleitor seleciona candidato, vota em branco ou vota nulo<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.3** Sistema confirma seleção<br/>**7.** Sistema apresenta resumo dos votos<br/>**8.** Eleitor confirma votação final<br/>**9.** Sistema registra votos<br/>**10.** Sistema exibe comprovante de votação<br/>**11.** Caso de uso encerra |
| **Fluxos Alternativos** | **FA1 - Eleitor inválido (passo 4):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**4.1** Sistema informa eleitor não habilitado<br/>&nbsp;&nbsp;&nbsp;&nbsp;**4.2** Sistema retorna ao passo 1<br/><br/>**FA2 - Correção de voto (passo 6.2):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.2.1** Eleitor altera seleção anterior<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.2.2** Sistema atualiza seleção
| **Fluxos de Exceção** | **FE1 - Eleitor já votou:**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sistema informa que eleitor já exerceu o direito de voto
| **Regras de Negócio** | • RN1: Eleitor só pode votar uma vez<br/>• RN2: Voto em branco é válido<br/>• RN3: Número inválido gera voto nulo<br/>• RN4: Todos os cargos devem ser votados |

---

## **UC2 - Realizar Cadastro**
| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Eleitor |
| **Objetivo** | Se cadastrar para participar da votação |
| **Pré-condições** | Usuario não cadastrado ainda |
| **Pós-condições** | É enviado para um administrador aceitar o cadastro |
| **Fluxo Principal** | **1.** Eleitor entra com solicitação inicial do cadastro entrando com seus dados pessoais<br/>**2.** É criado um registro provisório do eleitor <br/>**3.** Sistema é notificado de um nova solicitação de cadastro<br/>**4.** Sistema realiza avaliação da solicitação de cadastro<br/>**5.** Sistema aprova cadastro<br/>**6.** Novo eleitor é registrado na UEv de destino<br/>**7.** Eleitor é informado da aprovação |
| **Fluxos Alternativos** | **FA1 - UEg rejeita solicitação de cadastro (passo 5):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**5** Sistema rejeita solicitação de cadastro<br/>&nbsp;&nbsp;&nbsp;&nbsp;**5.1** Usuário é informado do motivo da rejeição<br/>&nbsp;&nbsp;&nbsp;&nbsp;**5.2** Caso de uso encerra |
| **Regras de Negócio** | • RN5: Máximo 8 cargos por eleição<br/>• RN6: Data de início deve ser futura<br/>• RN7: Data fim deve ser posterior ao início |

---
## **UC3 - Configurar Eleição**

| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Administrador |
| **Objetivo** | Configurar parâmetros gerais da eleição |
| **Pré-condições** | • Sistema UEg operacional<br/>• Administrador autenticado<br/>• Não há eleição em andamento |
| **Pós-condições** | • Eleição configurada no sistema<br/>• Parâmetros salvos no banco de dados |
| **Fluxo Principal** | **1.** Administrador acessa módulo de configuração<br/>**2.** Sistema apresenta formulário de eleição<br/>**3.** Administrador informa dados básicos:<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Nome da eleição<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Data e horário de início/fim<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Tipo de eleição<br/>**4.** Administrador define cargos em disputa (até 8) <br>&nbsp;&nbsp;&nbsp;&nbsp;• **4.1** Define nome do cargo<br/>&nbsp;&nbsp;&nbsp;&nbsp;• **4.2** Define número de vagas<br/>&nbsp;&nbsp;&nbsp;&nbsp;• **4.3** Define regras específicas<br/>**5.** Administrador confirma configuração<br/>**6.** Sistema valida dados informados<br/>**7.** Sistema salva configuração<br/>**8.** UEg cria "pacote" com os dados da eleição<br/>**9.** UEg distribui dados para as UEv<br/>**10.** UEg confirma criação da eleição|
| **Fluxos de Exceção** | **FE1 - Configuração ou Dados Inválidos (passo 6):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.1** Sistema exibe erros de validação (ex: "Quantidade de cargos excedida (Máx: 8))<br/>&nbsp;&nbsp;&nbsp;&nbsp;**6.2** Retorna ao passo 3 |
| **Regras de Negócio** | • RN5: Máximo 8 cargos por eleição<br/>• RN6: Data de início deve ser futura<br/>• RN7: Data fim deve ser posterior ao início |

---

## **UC4 - Gerenciar Candidatos**

| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Administrador |
| **Objetivo** | Cadastrar, alterar, deletar e consultar candidatos da eleição |
| **Pré-condições** | • Eleição configurada<br/>• Administrador autenticado |
| **Pós-condições** | • Candidatos cadastrados/atualizados<br/>• Dados sincronizados com todas as UEv |
| **Fluxo Principal** | **1.** Administrador acessa gestão de candidatos<br/>**2.** Sistema exibe lista de candidatos cadastrados<br/>**3.** Administrador seleciona "Novo Candidato"<br/>**4.** Sistema apresenta formulário de cadastro<br/>**5.** Administrador informa dados obrigatórios:<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Cargo pretendido<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Nome completo<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Nome de urna (apelido)<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Número do candidato<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Fotografia<br/>**6.** Administrador confirma entrada de dados<br/>**7.** Sistema valida dados informados<br/>**8.** Sistema salva candidato<br/>**9.** Sistema distribui dados para todas as UEv |
| **Fluxos Alternativos** | **FA1 - Alterar candidato (passo 3):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Administrador seleciona candidato existente<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Sistema carrega dados para edição<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Continua no passo 5<br/><br/>**FA2 - Excluir candidato (passo 3):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Administrador seleciona "Excluir"<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Sistema solicita confirmação<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Sistema remove candidato |
| **Fluxos de Exceção** | **FE1 - Dados inválidos ou já registrados:**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sistema informa os dados inválidos e solicita novos dados |
| **Regras de Negócio** | • RN8: Número do candidato deve ser único por cargo<br/>• RN9: Foto é obrigatória<br/>• RN10: Nome de urna limitado a 30 caracteres |

---

## **UC5 - Gerenciar Eleitores**

| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Administrador |
| **Objetivo** | Cadastrar, alterar e consultar eleitores aptos a votar |
| **Pré-condições** | • Sistema UEg operacional<br/>• Administrador autenticado |
| **Pós-condições** | • Base de eleitores atualizada<br/>• Dados distribuídos para as UEv |
| **Fluxo Principal** | **1.** Administrador acessa gestão de eleitores<br/>**2.** Sistema exibe lista de eleitores<br/>**3.** Administrador seleciona "Cadastrar Eleitor"<br/>**4.** Sistema apresenta formulário<br/>**5.** Administrador informa dados obrigatórios:<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Número do documento<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Nome completo<br/>&nbsp;&nbsp;&nbsp;&nbsp;• UEv designada<br/>**6.** Administrador informa foto (opcional)<br/>**7.** Administrador confirma cadastro<br/>**8.** Sistema valida elegibilidade<br/>**9.** Sistema salva eleitor<br/>**10.** Sistema distribui dados para UEv designada |
| **Fluxos Alternativos** | **FA1 - Importar lista de eleitores (passo 3):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Administrador seleciona "Importar"<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Sistema solicita arquivo CSV/XML<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Sistema processa lote de eleitores<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.4** Sistema exibe relatório de importação<br/><br/> **FA2 - Editar informações de eleitor (passo 3)**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Administrador seleciona eleitor que deseja editar<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Sistema retorna informações do eleitor<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Administrador entra com as novas informações desejadas</br>&nbsp;&nbsp;&nbsp;&nbsp;**3.4** Administrador confirma edições</br>&nbsp;&nbsp;&nbsp;&nbsp;**3.5** Sistema salva novas informações<br/><br/> **FA3 - Excluir um eleitor (passo3)**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Administrador seleciona eleitor que deseja excluir<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Sistema retorna informações do eleitor</br>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Administrador seleciona "Excluir Eleitor"<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.4** Administrador confirma a ação<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.5** Sistema registra alterações |
| **Fluxos de Exceção** | **FE1 - Eleitor já cadastrado:**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sistema pergunta se deseja atualizar dados |
| **Regras de Negócio** | • RN11: Número do documento deve ser único<br/>• RN12: Eleitor deve ter UEv designada<br/>• RN13: Foto é opcional |

---

## **UC6 - Apurar Resultados**

| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Administrador |
| **Objetivo** | Consolidar votos de todas as UEv e calcular resultados |
| **Pré-condições** | • Votação encerrada<br/>• UEv transmitiram resultados |
| **Pós-condições** | • Resultados consolidados<br/>• Vencedores identificados por cargo |
| **Fluxo Principal** | **1.** Administrador inicia processo de apuração<br/>**2.** Sistema verifica se todas as UEv transmitiram<br/>**3.** Sistema consolida votos por cargo:<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Soma votos de cada candidato<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Contabiliza votos brancos e nulos<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Calcula percentuais<br/>**4.** Sistema identifica vencedores por cargo<br/>**5.** Sistema gera relatório de apuração<br/>**6.** Administrador valida resultados<br/>**7.** Sistema finaliza apuração<br/>**8.** Sistema disponibiliza resultados oficiais |
| **Fluxos Alternativos** | **FA1 - UEv pendente (passo 2):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**2.1** Sistema lista UEv que não transmitiram<br/>&nbsp;&nbsp;&nbsp;&nbsp;**2.2** Administrador decide se prossegue<br/>&nbsp;&nbsp;&nbsp;&nbsp;**2.3** Se sim, continua no passo 3 |
| **Fluxos de Exceção** | **FE1 - Inconsistência nos dados:**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sistema reporta divergências para análise |
| **Regras de Negócio** | • RN19: Todos os votos devem ser contabilizados<br/>• RN20: Empates devem ser sinalizados |

---

## **UC7 - Exportar Relatórios Relatórios**

| **Campo** | **Descrição** |
|-----------|---------------|
| **Ator Principal** | Administrador |
| **Objetivo** | Produzir relatórios diversos sobre a eleição |
| **Pré-condições** | • Dados disponíveis no sistema<br/>• Administrador autenticado |
| **Pós-condições** | • Relatórios gerados<br/>• Arquivos disponibilizados |
| **Fluxo Principal** | **1.** Administrador acessa módulo de relatórios<br/>**2.** Sistema apresenta tipos de relatório:<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Resultados por UEv<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Resultados consolidados<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Comparecimento eleitoral<br/>&nbsp;&nbsp;&nbsp;&nbsp;• Relatório de auditoria<br/>**3.** Administrador seleciona tipo de relatório<br/>**4.** Sistema solicita parâmetros específicos<br/>**5.** Administrador define filtros e formato<br/>**6.** Sistema processa dados solicitados<br/>**7.** Sistema gera relatório (PDF/Excel)<br/>**8.** Sistema disponibiliza arquivo para download |
| **Fluxos Alternativos** | **FA1 - Relatório personalizado (passo 3):**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.1** Administrador escolhe "Personalizado"<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.2** Sistema permite seleção de campos<br/>&nbsp;&nbsp;&nbsp;&nbsp;**3.3** Continua no passo 5 |
| **Fluxos de Exceção** | **FE1 - Erro na geração:**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Sistema reporta problema e sugere parâmetros alternativos |
| **Regras de Negócio** | • RN21: Relatórios devem incluir data/hora de geração<br/>• RN22: Formatos: PDF para oficiais, Excel para análises |

---
