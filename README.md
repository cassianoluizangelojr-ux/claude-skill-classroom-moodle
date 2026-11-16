# Skill do Claude: atividades do Classroom e do Moodle

Skill para o [Claude](https://claude.ai) varrer o **Google Classroom** e o **Moodle**, estudar o material de cada matéria e resolver/entregar as atividades e avaliações pendentes do período atual.

## O que ela faz

- Lista o que está **atribuído** e **não entregue** no Classroom e o que está na linha do tempo do Moodle.
- **Estuda antes de responder:** lê material didático, slides, roteiro e enunciado (inclusive os PDFs do Moodle, extraídos dentro do navegador) e baseia cada resposta nesse conteúdo.
- Responde **questionários do Moodle**: lê todas as questões, salva página por página, confere e finaliza.
- Escreve **trabalhos** em ABNT quando a atividade pede, com a voz do aluno (usa um trabalho já aprovado como amostra de estilo).
- **Marca como concluída** ou entrega no Classroom.
- Pergunta antes de mexer em trabalho em grupo e só considera o período atual.

## Requisitos

| Recurso | Plano necessário |
|---|---|
| Skills (instalar e usar) | Free, Pro, Max, Team e Enterprise, com "Execução de código e criação de arquivos" ativada em Configurações → Capacidades |
| Navegador controlado pelo Claude ([Claude in Chrome](https://support.claude.com/en/articles/12012173-get-started-with-claude-in-chrome) ou o navegador do app desktop) | Planos pagos (Pro, Max, Team, Enterprise) |

**No plano Free** a skill instala, mas o Claude não consegue abrir o Classroom e o Moodle sozinho. Dá para usar no modo manual: você cola o enunciado e os materiais na conversa e ela resolve seguindo as mesmas regras.

Antes de usar, faça login no navegador com a **conta da instituição** (Google e Moodle). O Claude não digita senhas.

## Instalação

### Opção 1: copiar e colar no Claude (mais fácil)

Abra uma conversa nova no Claude (no app desktop ou no Cowork) e cole:

```
Instale pra mim a skill "classroom-moodle-atividades" deste repositório:
https://github.com/cassianoluizangelojr-ux/claude-skill-classroom-moodle
O arquivo da skill está em classroom-moodle-atividades/SKILL.md.
Se você conseguir salvar skills direto, salve. Se não conseguir, me gere um .zip
com a pasta classroom-moodle-atividades (com o SKILL.md dentro) e me diga onde enviar.
```

### Opção 2: enviar o .zip em Personalizar → Skills

1. Baixe o repositório (**Code → Download ZIP**), descompacte e compacte de novo **só a pasta `classroom-moodle-atividades`** (o .zip precisa ter essa pasta com o `SKILL.md` dentro).
2. No Claude, vá em **Personalizar → Skills**, clique em **+** → **Criar skill** → **Enviar uma skill**.
3. Escolha o .zip e deixe a skill ativada.

### Opção 3: Claude Code

```bash
git clone https://github.com/cassianoluizangelojr-ux/claude-skill-classroom-moodle.git
cp -r claude-skill-classroom-moodle/classroom-moodle-atividades ~/.claude/skills/
```

## Como usar

Exemplos de pedidos:

- "Vê o que tenho pendente no Classroom e no Moodle."
- "Faz a avaliação da unidade 4 de PEX, estudando o material antes."
- "Marca como concluída a atividade de Marketing, o grupo já entregou."

Na primeira vez, o Claude pergunta a URL do Moodle da sua instituição, qual conta Google é a institucional e quais matérias estão no período atual.

## Uso responsável

Confira a política da sua instituição sobre uso de IA em atividades e avaliações. A skill foi feita para se basear no material da disciplina e na sua própria voz, não para inventar fontes. Revise o que for entregue no seu nome.

## Limitações

- Não assiste videoaulas; avisa quando uma resposta dependeria de vídeo.
- Não faz login nem passa por verificação em duas etapas.
- O anexo de arquivos pelo seletor do navegador pode exigir que você clique.
- Testada no Moodle 4.x e no Google Classroom em português.

## Referências que inspiraram

- [blader/humanizer](https://github.com/blader/humanizer) e [academic-humanizer](https://github.com/AIScientists-Dev/academic-humanizer): escrita sem vícios de IA.
- [classroom-cli](https://github.com/MrGolddev/classroom-cli): alternativa via API oficial do Classroom (exige credencial do Google Cloud).
- [abnTeX2](https://www.ctan.org/pkg/abntex2): referência de formatação ABNT.
