---
name: classroom-moodle-atividades
description: Varre o Google Classroom e o Moodle do aluno, estuda o material de cada matéria e resolve/entrega as atividades e avaliações pendentes do bimestre atual. Use quando o aluno pedir para ver pendências, fazer atividades, responder questionários ou marcar atividades como concluídas.
---

# Atividades do Classroom e do Moodle

Objetivo: encontrar o que está pendente no período letivo atual, estudar o conteúdo da matéria, resolver com base nesse conteúdo e entregar. Fale com o aluno no idioma dele, de forma curta e direta.

Esta skill precisa de um navegador que o Claude controle (Claude in Chrome ou o navegador interno do app desktop do Claude) com o aluno já logado na conta da instituição.

## Configuração (pergunte na primeira vez e guarde na memória)

- URL do Moodle da instituição (ex.: `https://moodle.suafaculdade.edu.br`).
- Qual conta Google é a institucional. No Classroom, a conta aparece no caminho `/u/N/`: `/u/0/` é a primeira conta logada no navegador, `/u/1/` a segunda, e assim por diante. Se a lista de turmas vier vazia, provavelmente é a conta errada.
- Período atual (ex.: 2º bimestre de 2026) e quais matérias estão ativas.
- Se alguma matéria usa outro lugar para o material (grupo de WhatsApp, site do professor), anote.

## Regras que não se quebram

1. **Estudar antes de responder.** Antes de responder qualquer questão ou escrever qualquer trabalho, leia TODO o material da unidade ou da matéria: material didático, slides, roteiro e enunciado. Cada resposta precisa apontar para um trecho desse material. Se houver vídeo que você não consegue assistir, diga isso.
2. **Só o período atual.** Ignore turmas e atividades de semestres ou bimestres anteriores. No relatório, liste em uma linha o que foi ignorado.
3. **Trabalho em grupo ou com apresentação: pergunte antes.** Alguém do grupo pode já ter entregado. Pergunte uma vez, com todas as dúvidas juntas: tema ou empresa, integrantes e situação. Nunca invente integrante, empresa ou dado.
4. **Se o aluno parecer confuso, pare.** Diante de "como assim", "pera" ou "não era isso", não confirme nem envie nada: explique em poucas linhas e pergunte.
5. **Senha nunca.** Se o Google ou o Moodle pedirem login ou confirmação de identidade, peça para o aluno entrar no navegador. Você não digita senha nem código.
6. **Antes de enviar algo irreversível** (finalizar questionário, entregar trabalho), confira tudo. Se o aluno pediu para você entregar, entregue e relate. Se não ficou claro, pergunte.

## Onde olhar

### Google Classroom

- Atribuído: `https://classroom.google.com/u/N/a/not-turned-in/all`
- Não entregue (atrasadas): `https://classroom.google.com/u/N/a/missing/all`. Clique em "Ver tudo" para listar todas.
- Para ver o enunciado e os anexos, abra a atividade e leia o texto da página.
- PDF anexado do Drive: abra no visualizador (`https://drive.google.com/file/d/<id>/view?authuser=N`), role até o fim com várias rolagens para todas as páginas carregarem e leia o texto da página. Conectores do Google Drive geralmente não enxergam a conta institucional.

### Moodle

- Painel → Linha do tempo: mude o filtro para "Todos" e depois para "Atrasado".
- Para listar cursos e conteúdo sem abrir página por página, execute JavaScript numa página do Moodle já aberta:
  - cursos: `core_course_get_enrolled_courses_by_timeline_classification`
  - conteúdo de um curso: `core_courseformat_get_state` com `courseid`
  - as duas via POST em `/lib/ajax/service.php?sesskey=` + `M.cfg.sesskey`, com corpo `[{index:0, methodname:..., args:{...}}]`
- **Texto dos PDFs dentro do próprio navegador**, sem precisar baixar:
  ```js
  const pdfjs = await import('https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.4.168/pdf.min.mjs');
  pdfjs.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.4.168/pdf.worker.min.mjs';
  const buf = await (await fetch('/mod/resource/view.php?id=<cmid>&redirect=1')).arrayBuffer();
  const doc = await pdfjs.getDocument({data: buf}).promise;
  let txt = '';
  for (let i = 1; i <= doc.numPages; i++) {
    const c = await (await doc.getPage(i)).getTextContent();
    txt += `\n[p${i}] ` + c.items.map(x => x.str).join(' ');
  }
  window.__mat = txt; txt.length
  ```
  Depois leia `window.__mat` em pedaços de cerca de 15 mil caracteres.

Dica: cada carregamento de página novo pode pedir aprovação do site. Sempre que der, use `fetch` e JavaScript dentro de uma página já aberta.

## Questionário do Moodle (avaliação objetiva)

1. Na página do questionário, confira antes de começar: tentativas permitidas, método de avaliação e limite de tempo.
2. Inicie pelo botão de tentativa e anote o id em `attempt.php?attempt=<id>`.
3. Leia TODAS as questões antes de marcar qualquer uma: `fetch('/mod/quiz/attempt.php?attempt=<id>&cmid=<cmid>&page=N')` para cada página, extraindo `.qtext` e as alternativas (`.answer > div` e o `value` de cada radio).
4. Para cada questão, escolha a alternativa e anote o trecho do material que a sustenta. Se alguma não estiver coberta pelo material, avise o aluno antes de enviar.
5. Salve página por página: pegue o `#responseform` de cada página, marque o radio, defina `nextpage` (próxima página, ou -1 na última) e envie com POST para a `action` do formulário.
6. Confira: o `summary.php` deve mostrar "Resposta salva" em todas, e o radio marcado em cada página deve ser o escolhido.
7. Finalize com o formulário que contém `finishattempt` no `summary.php`. Avise o aluno que a nota pode só aparecer depois do fechamento. Se houver outra tentativa e a nota vier baixa, revise apenas as erradas.

## Trabalhos escritos

- Leia o enunciado, o roteiro e o material das aulas da matéria (mesma regra de estudar antes).
- Use ABNT só quando a atividade pedir: A4, Arial ou Times 12, espaçamento 1,5, margens de 3 cm (superior e esquerda) e 2 cm (inferior e direita), capa, folha de rosto, sumário e referências. Quando não pedir, basta um título simples (pode ser o próprio enunciado) e o texto.
- **Escreva com a voz do aluno.** Peça um trabalho que ele já entregou e foi aprovado e use-o como amostra de estilo: pessoa verbal, tamanho de frase, vocabulário. Depois revise o texto tirando vícios típicos de IA sem mudar números, citações nem afirmações, como faz o [academic-humanizer](https://github.com/AIScientists-Dev/academic-humanizer). Não invente referências: cite só o que foi lido.
- Gere o arquivo em .docx ou PDF. Para anexar no Classroom, use "Adicionar ou criar → Arquivo". Se o seletor de arquivos não abrir pelo navegador controlado, peça para o aluno anexar e depois marque como entregue.

## Marcar como concluída ou entregar no Classroom

- Clique em "Marcar como concluída" (ou "Entregar", quando houver anexo) e confirme no diálogo.
- O layout da página muda quando os anexos carregam. Clique pelo elemento encontrado na página, não por uma coordenada antiga. Confirme lendo a mensagem "Sua atividade foi marcada como concluída".

## Relatório final

Seja curto. Para cada atividade, informe: matéria, nome, o que foi feito (resolvido, entregue ou marcado) e o que ficou pendente, com o motivo. Nos questionários, diga quando sai a nota. Guarde na memória o que o aluno explicar (grupos, temas, situação) para não perguntar de novo.
