# Formulário de Atendimento

Formulário web de agendamento com calendário. Cada dia aceita **apenas 1 atendimento**: depois que alguém reserva, o dia fica trancado (🔒) e as próximas pessoas precisam escolher outra data.

Os dados são gravados em uma planilha do Google Sheets por meio de um Google Apps Script. Não precisa de servidor nem de banco de dados.

## Funcionalidades

- Calendário mensal com navegação entre os meses
- Dias já reservados aparecem em rosa, com cadeado, e não podem ser selecionados
- Dias passados ficam desativados
- Bloqueio no servidor: se duas pessoas tentarem o mesmo dia ao mesmo tempo, só a primeira é registrada e a outra recebe um aviso
- Campos: nome e data de nascimento
- Layout responsivo (funciona no celular)
- Registro automático em planilha do Google Sheets

## Como funciona

```
┌──────────────┐   GET: lista de datas ocupadas    ┌─────────────────┐   lê/grava   ┌────────────────┐
│  index.html  │ ────────────────────────────────▶ │  Google Apps    │ ───────────▶ │ Google Sheets  │
│ (formulário) │ ◀──────────────────────────────── │  Script (/exec) │ ◀─────────── │  (planilha)    │
└──────────────┘   POST: nome, nascimento, data    └─────────────────┘              └────────────────┘
```

1. Ao abrir, a página consulta o script (`GET`) e recebe as datas já reservadas.
2. O calendário tranca esses dias.
3. Ao enviar, o script (`POST`) confere de novo se o dia está livre e só então grava na planilha.
4. Para liberar um dia, basta apagar a linha correspondente na planilha.

## Estrutura do projeto

```
form_de_atendimento/
├── index.html      # Formulário e calendário (HTML, CSS e JavaScript)
├── apps-script.gs  # Código do Google Apps Script (servidor)
└── README.md
```

## Instalação

### 1. Planilha

Crie uma planilha no Google Sheets. O script usa a aba **Página1** e cria o cabeçalho sozinho se ela estiver vazia. As colunas ficam assim:

| A | B | C | D |
|---|---|---|---|
| Registrado em | Nome | Data de Nascimento | Data Agendada |

As datas das colunas C e D são gravadas como texto, no formato `dd/mm/aaaa`.

### 2. Apps Script

1. Na planilha, abra **Extensões > Apps Script**.
2. Apague o conteúdo do editor e cole o código de `apps-script.gs`. Salve.
3. Clique em **Implantar > Nova implantação** e escolha o tipo **App da Web**.
4. Configure:
   - **Executar como:** Eu
   - **Quem tem acesso:** Qualquer pessoa
5. Clique em **Implantar**, autorize as permissões e copie a URL que termina em `/exec`.

> Sempre que alterar o script, use **Implantar > Gerenciar implantações > Editar > Nova versão**. Assim a URL continua a mesma.

### 3. Formulário

Abra o `index.html` e troque a URL na constante `SCRIPT_URL`:

### 4. Publicação com GitHub Pages

1. Envie os arquivos para o repositório.
2. Vá em **Settings > Pages**.
3. Em **Source**, escolha a branch `main` e a pasta `/ (root)`.
4. Salve. O link do formulário aparece em instantes, no formato  (https://luccasfsilva.github.io/form_de_atendimento/).

## Como usar

1. A pessoa informa o nome e a data de nascimento.
2. Escolhe um dia disponível no calendário.
3. Clica em **Agendar**.
4. O dia fica reservado e passa a aparecer trancado para todos.

### Liberar um dia

Apague a linha (ou a célula da coluna **Data Agendada**) na planilha e recarregue a página. Mantenha o formato `dd/mm/aaaa` ao editar manualmente.

## Personalização

| O que mudar | Onde |
|---|---|
| Cores | Variáveis e gradientes no bloco `<style>` do `index.html` |
| Título e textos | Cabeçalho e caixa de informativo no `index.html` |
| Campos do formulário | `index.html` (HTML e função de envio) e `doPost` em `apps-script.gs` |
| Nome da aba | Constante `ABA` em `apps-script.gs` |

## Tecnologias

- HTML, CSS e JavaScript (sem frameworks)
- Google Apps Script
- Google Sheets
- Font Awesome e Google Fonts

## Privacidade

O formulário coleta dados pessoais (nome e data de nascimento). Mantenha a planilha com acesso restrito, não a publique na web e informe aos participantes como os dados serão usados, conforme a LGPD.

## Licença

Defina aqui a licença do projeto (por exemplo, MIT).
