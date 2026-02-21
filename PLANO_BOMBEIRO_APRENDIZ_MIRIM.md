# Plano Completo — Bombeiro Aprerndiz Mirim (FlutterFlow + Android + Families/LGPD)

## A) Sitemap completo (nomes exatos de páginas)

Crie no FlutterFlow **exatamente** estas páginas:

1. `SplashPage`
2. `HomePage`
3. `WebBoasVindasPage`
4. `WebCursosPage`
5. `WebGamesPage`
6. `QuizThemesPage`
7. `QuizLessonsPage`
8. `QuizQuestionPage`
9. `QuizSummaryPage`
10. `SimuladorListPage`
11. `SimuladorScenarioPage`
12. `SimuladorResultPage`
13. `ConquistasPage`
14. `CertificadoPage`
15. `ParentalGatePage`
16. `ResponsaveisPage`

Fluxo principal:
- `SplashPage` → `HomePage`
- `HomePage` → (Webs, Quiz, Simulador, Conquistas, Certificado, Responsáveis)
- `ResponsaveisPage` sempre via `ParentalGatePage`

---

## B) App State (variáveis globais)

No FlutterFlow: **App Settings > App State > Add Field**.

### 1) Conteúdo estático (JSON)
- `quizThemesJson` (JSON, default: `[]`)
- `simuladorScenariosJson` (JSON, default: `[]`)

### 2) Progresso local
- `lessonBestScores` (JSON, default: `{}`)
  - chave: `themeId_lessonId` (ex.: `T1_L1`)
  - valor: inteiro 0–100
- `lessonCompleted` (JSON, default: `{}`)
  - chave: `themeId_lessonId`
  - valor: boolean
- `totalPerguntasRespondidas` (int, default: `0`)
- `totalLicoesConcluidas` (int, default: `0`)
- `simuladorConcluidos` (JSON, default: `[]`)
  - array de `scenarioId`

### 3) Sessão de quiz (runtime)
- `selectedThemeId` (String, default: `""`)
- `selectedLessonId` (String, default: `""`)
- `currentQuestionIndex` (int, default: `0`)
- `currentQuizHits` (int, default: `0`)
- `currentQuizAnswers` (JSON, default: `[]`)

### 4) Conquistas / certificado
- `badgesUnlocked` (JSON, default: `[]`)
  - guardar IDs únicos (sem duplicar)
- `certificadoUnlocked` (bool, default: `false`)
- `certificadoDataISO` (String, default: `""`)

### 5) Segurança/UX
- `webBannerText` (String, default: `"Você está acessando uma página dentro do app. Recomendamos acompanhamento de um responsável."`)
- `antiTroteAccepted` (bool, default: `false`)

### Persistência
- Para todos os campos de progresso, marque **Persisted** no App State.
- Não usar login/firestore para progresso (somente local).

---

## C) Modelo de dados JSON pronto para colar

> Uso prático: coloque em um arquivo local e atribua ao App State no `SplashPage` (se vazio).

### C.1 Quiz (6 temas × 10 lições; 60 perguntas iniciais)

```json
{
  "themes": [
    {
      "id": "T1",
      "title": "Prevenção em Casa",
      "lessons": [
        {
          "id": "L1",
          "title": "Cozinha Segura",
          "questions": [
            {"id":"Q1","q":"O que fazer ao sair da cozinha?","options":["Deixar panela no fogo","Desligar o fogão","Cobrir tomadas"],"correctIndex":1,"explanation":"Desligar o fogão evita acidentes e incêndios."}
          ]
        },
        {"id":"L2","title":"Tomadas e Fios","questions":[{"id":"Q1","q":"Fio desencapado deve ser:","options":["Usado com cuidado","Ignorado","Consertado por adulto"],"correctIndex":2,"explanation":"Um adulto deve resolver para evitar choque e curto."}]},
        {"id":"L3","title":"Gás de Cozinha","questions":[{"id":"Q1","q":"Cheiro de gás em casa:","options":["Acender luz","Avisar adulto e ventilar","Usar fósforo"],"correctIndex":1,"explanation":"Ventilar e chamar responsável é o procedimento seguro."}]},
        {"id":"L4","title":"Velas e Incensos","questions":[{"id":"Q1","q":"Vela acesa deve ficar:","options":["Perto da cortina","Sem supervisão","Longe de materiais inflamáveis"],"correctIndex":2,"explanation":"Distância de materiais inflamáveis evita início de fogo."}]},
        {"id":"L5","title":"Plano de Saída","questions":[{"id":"Q1","q":"Em emergência, a família deve:","options":["Improvisar","Ter rota combinada","Esperar o fogo aumentar"],"correctIndex":1,"explanation":"Rotas planejadas facilitam saída rápida e segura."}]},
        {"id":"L6","title":"Objetos Quentes","questions":[{"id":"Q1","q":"Panela com cabo para fora é:","options":["Seguro","Perigoso","Normal"],"correctIndex":1,"explanation":"Alguém pode esbarrar e causar queimadura."}]},
        {"id":"L7","title":"Quarto Seguro","questions":[{"id":"Q1","q":"Carregador em uso contínuo:","options":["Pode aquecer e danificar","Sempre seguro","Não precisa observar"],"correctIndex":0,"explanation":"Superaquecimento pode causar risco elétrico."}]},
        {"id":"L8","title":"Brincadeiras Seguras","questions":[{"id":"Q1","q":"Brincar com fogo é:","options":["Permitido com amigos","Perigoso e proibido","Seguro no quintal"],"correctIndex":1,"explanation":"Fogo não é brinquedo e pode sair de controle."}]},
        {"id":"L9","title":"Sinais de Risco","questions":[{"id":"Q1","q":"Cheiro de queimado sem motivo:","options":["Ignorar","Avisar responsável","Abrir gás"],"correctIndex":1,"explanation":"Avisar rápido pode evitar problema maior."}]},
        {"id":"L10","title":"Revisão T1","questions":[{"id":"Q1","q":"Primeira atitude ao notar perigo:","options":["Filmar","Avisar adulto","Se esconder"],"correctIndex":1,"explanation":"Adulto responsável deve ser acionado imediatamente."}]}
      ]
    },
    {
      "id":"T2",
      "title":"Segurança na Escola",
      "lessons":[
        {"id":"L1","title":"Saídas de Emergência","questions":[{"id":"Q1","q":"Na escola, deve-se conhecer:","options":["Somente a cantina","Saídas de emergência","A sala do diretor"],"correctIndex":1,"explanation":"Saber as saídas acelera evacuação."}]},
        {"id":"L2","title":"Alarme de Incêndio","questions":[{"id":"Q1","q":"Ao ouvir alarme, você:","options":["Corre sozinho","Segue orientações dos adultos","Volta para pegar objetos"],"correctIndex":1,"explanation":"Seguir a equipe escolar mantém todos protegidos."}]},
        {"id":"L3","title":"Fila e Calma","questions":[{"id":"Q1","q":"Durante evacuação:","options":["Empurrar","Manter calma e fila","Gritar"],"correctIndex":1,"explanation":"Organização evita quedas e confusão."}]},
        {"id":"L4","title":"Laboratório","questions":[{"id":"Q1","q":"No laboratório, líquidos desconhecidos:","options":["Tocar","Cheirar de perto","Não manusear sem orientação"],"correctIndex":2,"explanation":"Somente com supervisão e orientação adequada."}]},
        {"id":"L5","title":"Pátio Seguro","questions":[{"id":"Q1","q":"Tomada danificada na escola:","options":["Brincar perto","Avisar professor","Cobrir com papel"],"correctIndex":1,"explanation":"Professor deve acionar manutenção."}]},
        {"id":"L6","title":"Quadra e Eventos","questions":[{"id":"Q1","q":"Em evento lotado, sua atitude:","options":["Empurrar para sair","Ficar com grupo/turma","Correr sem direção"],"correctIndex":1,"explanation":"Permanecer com o grupo reduz riscos."}]},
        {"id":"L7","title":"Mochila e Corredores","questions":[{"id":"Q1","q":"Mochila no corredor:","options":["Pode deixar","Atrapalha passagem","Serve de apoio"],"correctIndex":1,"explanation":"Corredores livres facilitam evacuação."}]},
        {"id":"L8","title":"Extintor (Noções)","questions":[{"id":"Q1","q":"Criança deve usar extintor sozinha?","options":["Sim","Não","Depende"],"correctIndex":1,"explanation":"Uso deve ser feito por adulto treinado."}]},
        {"id":"L9","title":"Ponto de Encontro","questions":[{"id":"Q1","q":"Após sair do prédio:","options":["Voltar correndo","Ir ao ponto de encontro","Esconder-se"],"correctIndex":1,"explanation":"Ponto de encontro ajuda a conferir todos."}]},
        {"id":"L10","title":"Revisão T2","questions":[{"id":"Q1","q":"Regra principal em emergência:","options":["Desobedecer orientações","Seguir orientações dos adultos","Ficar sozinho"],"correctIndex":1,"explanation":"Orientação adulta é essencial em segurança."}]}
      ]
    },
    {
      "id":"T3",
      "title":"Primeiros Cuidados",
      "lessons":[
        {"id":"L1","title":"Pedir Ajuda","questions":[{"id":"Q1","q":"Viu acidente leve, faz o quê?","options":["Ignora","Chama adulto","Posta em rede social"],"correctIndex":1,"explanation":"Chamar adulto é a atitude certa."}]},
        {"id":"L2","title":"Queimadura Leve","questions":[{"id":"Q1","q":"Queimadura leve: primeiro passo","options":["Gelo direto","Água corrente fresca","Pasta de dente"],"correctIndex":1,"explanation":"Água corrente ajuda a resfriar a área."}]},
        {"id":"L3","title":"Cortes","questions":[{"id":"Q1","q":"Em corte pequeno:","options":["Usar pano sujo","Lavar e avisar adulto","Ignorar sangramento"],"correctIndex":1,"explanation":"Higiene e adulto responsável são fundamentais."}]},
        {"id":"L4","title":"Desmaio","questions":[{"id":"Q1","q":"Se alguém desmaia:","options":["Dar comida","Chamar adulto/ajuda","Sacudir forte"],"correctIndex":1,"explanation":"Procure ajuda imediata de adulto."}]},
        {"id":"L5","title":"Telefone de Emergência","questions":[{"id":"Q1","q":"Ligações de emergência devem ser:","options":["Brincadeira","Somente reais","Feitas por curiosidade"],"correctIndex":1,"explanation":"Trote atrapalha quem precisa de socorro."}]},
        {"id":"L6","title":"Animais e Picadas","questions":[{"id":"Q1","q":"Picada com reação forte:","options":["Esconder","Avisar adulto rápido","Passar qualquer produto"],"correctIndex":1,"explanation":"Atenção rápida reduz complicações."}]},
        {"id":"L7","title":"Calor e Hidratação","questions":[{"id":"Q1","q":"No calor intenso:","options":["Não beber água","Hidratar e procurar sombra","Correr sem parar"],"correctIndex":1,"explanation":"Hidratação previne mal-estar."}]},
        {"id":"L8","title":"Queda","questions":[{"id":"Q1","q":"Após queda com dor forte:","options":["Levantar à força","Chamar adulto","Ignorar"],"correctIndex":1,"explanation":"Evite agravar lesão e peça ajuda."}]},
        {"id":"L9","title":"Kit Básico","questions":[{"id":"Q1","q":"Kit de primeiros cuidados deve ficar:","options":["Em local acessível ao adulto","Trancado sem acesso","No chão"],"correctIndex":0,"explanation":"Adultos devem acessar rapidamente quando necessário."}]},
        {"id":"L10","title":"Revisão T3","questions":[{"id":"Q1","q":"Regra de ouro:","options":["Resolver sozinho","Chamar adulto","Sair correndo"],"correctIndex":1,"explanation":"Nunca enfrentar emergência sem adulto."}]}
      ]
    },
    {
      "id":"T4",
      "title":"Segurança em Espaços Públicos",
      "lessons":[
        {"id":"L1","title":"Shopping e Cinema","questions":[{"id":"Q1","q":"Em local cheio, você deve:","options":["Ficar perto do responsável","Sair sozinho","Correr"],"correctIndex":0,"explanation":"Permanecer com responsável aumenta segurança."}]},
        {"id":"L2","title":"Praia e Parque","questions":[{"id":"Q1","q":"No sol forte:","options":["Sem água","Hidratar e proteger-se","Ignorar calor"],"correctIndex":1,"explanation":"Cuidados simples evitam riscos."}]},
        {"id":"L3","title":"Transporte Público","questions":[{"id":"Q1","q":"Dentro do ônibus:","options":["Brincar no corredor","Segurar-se e obedecer","Abrir porta"],"correctIndex":1,"explanation":"Comportamento seguro evita quedas."}]},
        {"id":"L4","title":"Eventos","questions":[{"id":"Q1","q":"Se perder do responsável:","options":["Sair procurando sozinho","Procurar equipe de apoio","Esconder-se"],"correctIndex":1,"explanation":"Equipe de apoio ajuda no reencontro seguro."}]},
        {"id":"L5","title":"Escadas Rolantes","questions":[{"id":"Q1","q":"Em escada rolante:","options":["Brincar sentando","Segurar corrimão","Pular degraus"],"correctIndex":1,"explanation":"Segurar corrimão reduz acidentes."}]},
        {"id":"L6","title":"Feiras e Mercados","questions":[{"id":"Q1","q":"Objeto quebrado no chão:","options":["Chutar","Avisar adulto","Ignorar"],"correctIndex":1,"explanation":"Avisar evita que alguém se machuque."}]},
        {"id":"L7","title":"Sinalização","questions":[{"id":"Q1","q":"Placa de emergência indica:","options":["Decoração","Rota e segurança","Brincadeira"],"correctIndex":1,"explanation":"Sinalização orienta em situações de risco."}]},
        {"id":"L8","title":"Rua Segura","questions":[{"id":"Q1","q":"Atravessar rua deve ser:","options":["Correndo sem olhar","Com adulto e atenção","Com celular na mão"],"correctIndex":1,"explanation":"Atenção e supervisão são essenciais."}]},
        {"id":"L9","title":"Chuva Forte","questions":[{"id":"Q1","q":"Durante alagamento:","options":["Brincar na água","Evitar áreas alagadas","Nadar"],"correctIndex":1,"explanation":"Água de alagamento oferece riscos sérios."}]},
        {"id":"L10","title":"Revisão T4","questions":[{"id":"Q1","q":"Em público, prioridade é:","options":["Aventura","Segurança","Velocidade"],"correctIndex":1,"explanation":"Priorizar segurança evita acidentes."}]}
      ]
    },
    {
      "id":"T5",
      "title":"Natureza e Defesa Civil",
      "lessons":[
        {"id":"L1","title":"Chuva e Raios","questions":[{"id":"Q1","q":"Com raios, você deve:","options":["Ficar em área aberta","Buscar abrigo seguro","Subir em árvore"],"correctIndex":1,"explanation":"Abrigo seguro protege contra descargas."}]},
        {"id":"L2","title":"Ventos Fortes","questions":[{"id":"Q1","q":"Telhas voando na rua:","options":["Filmar perto","Entrar em local seguro","Continuar brincando"],"correctIndex":1,"explanation":"Afaste-se de objetos que podem cair."}]},
        {"id":"L3","title":"Queimada","questions":[{"id":"Q1","q":"Fumaça intensa:","options":["Ir ver de perto","Avisar adulto e sair da área","Ficar parado"],"correctIndex":1,"explanation":"Distância e aviso rápido são fundamentais."}]},
        {"id":"L4","title":"Trilhas","questions":[{"id":"Q1","q":"Em trilha com família:","options":["Separar-se","Ficar no grupo","Correr sozinho"],"correctIndex":1,"explanation":"Manter-se no grupo evita perda."}]},
        {"id":"L5","title":"Animais Silvestres","questions":[{"id":"Q1","q":"Encontrou animal silvestre:","options":["Tocar","Observar de longe","Alimentar"],"correctIndex":1,"explanation":"Distância respeita o animal e protege você."}]},
        {"id":"L6","title":"Calor Extremo","questions":[{"id":"Q1","q":"Sinal de calor excessivo:","options":["Tontura","Energia extra","Frio"],"correctIndex":0,"explanation":"Tontura pode indicar desidratação."}]},
        {"id":"L7","title":"Comunidade Segura","questions":[{"id":"Q1","q":"Ao notar risco no bairro:","options":["Ignorar","Avisar adulto/autoridade","Compartilhar boato"],"correctIndex":1,"explanation":"Comunicação correta ajuda prevenção."}]},
        {"id":"L8","title":"Mochila de Emergência","questions":[{"id":"Q1","q":"Item importante:","options":["Lanterna","Fogos de artifício","Brinquedo quebrado"],"correctIndex":0,"explanation":"Lanterna é útil em falta de energia."}]},
        {"id":"L9","title":"Ponto Seguro","questions":[{"id":"Q1","q":"Família deve combinar:","options":["Ponto de encontro","Segredo","Nada"],"correctIndex":0,"explanation":"Ponto de encontro ajuda organização."}]},
        {"id":"L10","title":"Revisão T5","questions":[{"id":"Q1","q":"Defesa civil é sobre:","options":["Prevenção e proteção","Jogos","Competição"],"correctIndex":0,"explanation":"Objetivo é reduzir riscos e proteger pessoas."}]}
      ]
    },
    {
      "id":"T6",
      "title":"Missão Bombeiro Aprendiz",
      "lessons":[
        {"id":"L1","title":"Valores","questions":[{"id":"Q1","q":"Atitude de aprendiz:","options":["Responsabilidade","Descuido","Pressa"],"correctIndex":0,"explanation":"Responsabilidade é base da prevenção."}]},
        {"id":"L2","title":"Trabalho em Equipe","questions":[{"id":"Q1","q":"Em equipe, o ideal é:","options":["Cooperar","Competir sem regra","Ignorar colegas"],"correctIndex":0,"explanation":"Cooperação melhora resultados e segurança."}]},
        {"id":"L3","title":"Comunicação Clara","questions":[{"id":"Q1","q":"Em risco, informe:","options":["Com calma e clareza","Gritando sem foco","Com brincadeiras"],"correctIndex":0,"explanation":"Mensagem clara acelera ajuda."}]},
        {"id":"L4","title":"Observação de Risco","questions":[{"id":"Q1","q":"Ver risco pequeno significa:","options":["Agir cedo e avisar","Esperar piorar","Ignorar"],"correctIndex":0,"explanation":"Prevenção começa cedo."}]},
        {"id":"L5","title":"Autoproteção","questions":[{"id":"Q1","q":"Primeiro cuidado em emergência:","options":["Sua segurança","Objetos","Pontuação"],"correctIndex":0,"explanation":"Sem autoproteção, o risco aumenta."}]},
        {"id":"L6","title":"Revisão de Conhecimentos","questions":[{"id":"Q1","q":"Fogo é:","options":["Brinquedo","Perigo real","Decoração"],"correctIndex":1,"explanation":"Fogo exige respeito e prevenção."}]},
        {"id":"L7","title":"Mini Missão 1","questions":[{"id":"Q1","q":"Cheiro de gás + adulto ausente:","options":["Acender luz","Sair e buscar adulto","Ligar fogão"],"correctIndex":1,"explanation":"Sair do local e buscar adulto é seguro."}]},
        {"id":"L8","title":"Mini Missão 2","questions":[{"id":"Q1","q":"Alarme tocou:","options":["Voltar mochila","Seguir rota de saída","Esconder"],"correctIndex":1,"explanation":"Evacuação imediata e organizada."}]},
        {"id":"L9","title":"Mini Missão 3","questions":[{"id":"Q1","q":"Colega caiu e sente dor:","options":["Levantar à força","Chamar adulto","Dar corrida"],"correctIndex":1,"explanation":"Ajuda adulta evita piora da lesão."}]},
        {"id":"L10","title":"Desafio Final","questions":[{"id":"Q1","q":"Sua missão final é:","options":["Aplicar segurança com responsabilidade","Buscar risco","Ignorar regras"],"correctIndex":0,"explanation":"Parabéns por concluir a trilha final."}]}
      ]
    }
  ]
}
```

### C.2 Simulador (10 cenários)

```json
{
  "scenarios": [
    {"id":"S1","title":"Cheiro de gás na cozinha","context":"Você sente cheiro forte de gás.","options":["Acender a luz para ver melhor","Abrir janelas e chamar um adulto","Ligar o fogão para testar"],"correctIndex":1,"feedback":"Correto: ventilar e avisar um adulto é a ação segura."},
    {"id":"S2","title":"Panela no fogo sem adulto","context":"Você vê uma panela no fogo e não há adulto por perto.","options":["Tentar mexer na panela","Chamar um adulto imediatamente","Jogar água na panela"],"correctIndex":1,"feedback":"Correto: nunca manuseie sozinho, chame um adulto."},
    {"id":"S3","title":"Tomada soltando faísca","context":"Uma tomada fez barulho e soltou faísca.","options":["Encostar para verificar","Avisar responsável e se afastar","Cobrir com pano"],"correctIndex":1,"feedback":"Correto: afaste-se e chame um adulto."},
    {"id":"S4","title":"Alarme da escola tocou","context":"O alarme dispara durante a aula.","options":["Voltar para buscar material","Seguir a professora até a saída","Sair correndo sozinho"],"correctIndex":1,"feedback":"Correto: siga orientação do adulto responsável."},
    {"id":"S5","title":"Colega com queimadura leve","context":"Seu colega encostou em algo quente.","options":["Passar produto caseiro","Levar para água corrente e chamar adulto","Ignorar"],"correctIndex":1,"feedback":"Correto: água corrente e apoio adulto."},
    {"id":"S6","title":"Chuva com raios no parque","context":"Começou tempestade com trovões.","options":["Ficar sob árvore","Buscar abrigo seguro com responsável","Continuar brincando"],"correctIndex":1,"feedback":"Correto: procure abrigo seguro imediatamente."},
    {"id":"S7","title":"Fumaça em um corredor","context":"Você nota fumaça no corredor do prédio.","options":["Ir olhar de perto","Sair pela rota segura e avisar adulto","Se esconder no quarto"],"correctIndex":1,"feedback":"Correto: evacue com segurança e avise."},
    {"id":"S8","title":"Objeto quebrado no chão","context":"Há vidro quebrado no caminho.","options":["Pisar com cuidado","Afastar pessoas e avisar adulto","Brincar de chutar"],"correctIndex":1,"feedback":"Correto: prevenção evita novos acidentes."},
    {"id":"S9","title":"Perdeu responsável em evento","context":"Você não encontra seu responsável.","options":["Sair procurando sozinho","Pedir ajuda à equipe de apoio","Ir para rua"],"correctIndex":1,"feedback":"Correto: busque equipe oficial do local."},
    {"id":"S10","title":"Desafio final do simulador","context":"Você vê uma situação de risco e colegas nervosos.","options":["Incentivar correria","Manter calma, avisar adulto e seguir rota segura","Gravar vídeo"],"correctIndex":1,"feedback":"Correto: calma + orientação adulta salvam vidas."}
  ]
}
```

---

## D) Fluxo de Actions por página (FlutterFlow passo a passo)

## `SplashPage`
1. **On Page Load**:
   - If `quizThemesJson` vazio → `Set App State` com JSON do quiz.
   - If `simuladorScenariosJson` vazio → `Set App State` com JSON dos cenários.
2. Delay 1.2s.
3. Navigate → `HomePage` (replace).

## `HomePage`
Layout com botões grandes:
- Boas-vindas → `WebBoasVindasPage`
- Cursos → `WebCursosPage`
- Games → `WebGamesPage`
- Quiz → `QuizThemesPage`
- Simulador → `SimuladorListPage`
Atalhos menores:
- Conquistas → `ConquistasPage`
- Certificado → `CertificadoPage`
- Responsáveis → `ParentalGatePage`

## `WebBoasVindasPage`, `WebCursosPage`, `WebGamesPage`
1. AppBar com **Voltar** e botão **Fechar** (volta `HomePage`).
2. Widget WebView carregando URL fixa:
   - Boas-vindas: `https://sites.google.com/view/bemvindo-bpm/in%C3%ADcio`
   - Cursos: `https://cursotreinemaisead.com.br/produto/aprendiz-mirim/`
   - Games: `https://bombeiroaprendizmirim.com.br/games/bpm/`
3. Banner fixo no rodapé com `webBannerText`.
4. Plano de bloqueio externo (detalhe na seção F).

## `QuizThemesPage`
1. ListView dos temas de `quizThemesJson.themes`.
2. Ao tocar tema:
   - `Set App State selectedThemeId`.
   - Navigate → `QuizLessonsPage`.

## `QuizLessonsPage`
1. Filtrar tema por `selectedThemeId`.
2. Listar 10 lições.
3. Cada item mostra:
   - melhor score de `lessonBestScores[Tn_Lm]`
   - status concluído por `lessonCompleted[Tn_Lm]`
4. Ao tocar lição:
   - set `selectedLessonId`
   - reset runtime: `currentQuestionIndex=0`, `currentQuizHits=0`, `currentQuizAnswers=[]`
   - Navigate → `QuizQuestionPage`.

## `QuizQuestionPage`
1. Buscar pergunta atual por índice.
2. Botões de resposta (3 opções):
   - comparar índice escolhido com `correctIndex`
   - se correto: `currentQuizHits += 1`
   - sempre: incrementar `totalPerguntasRespondidas += 1`
   - mostrar bottom sheet com explicação curta (`explanation`)
3. Ao avançar:
   - se há próxima pergunta: `currentQuestionIndex += 1`
   - senão: Navigate → `QuizSummaryPage`

## `QuizSummaryPage`
1. Calcular:
   - `totalQuestoes = questions.length`
   - `score = round((currentQuizHits / totalQuestoes) * 100)`
   - `completed = score >= 70`
2. Persistir:
   - chave `k = selectedThemeId + '_' + selectedLessonId`
   - `lessonBestScores[k] = max(scoreAtual, scoreAnterior)`
   - se `completed` e `lessonCompleted[k] != true`:
     - `lessonCompleted[k] = true`
     - `totalLicoesConcluidas += 1`
3. Rodar action de badges (ver função lógica abaixo).
4. Certificado:
   - se `totalLicoesConcluidas >= 30` OU (`selectedThemeId == 'T6'` e `selectedLessonId == 'L10'` e `completed`)
     - `certificadoUnlocked = true`
     - se `certificadoDataISO` vazio: salvar data atual ISO.
5. Botões:
   - “Refazer lição” → `QuizQuestionPage` (reset runtime)
   - “Voltar às lições” → `QuizLessonsPage`

## `SimuladorListPage`
1. Listar `simuladorScenariosJson.scenarios`.
2. Mostrar concluído se `scenarioId` está em `simuladorConcluidos`.
3. Tap cenário:
   - guardar índice/id temporário (page state/local state)
   - Navigate `SimuladorScenarioPage`

## `SimuladorScenarioPage`
1. Exibir contexto + 3 opções.
2. Ao escolher:
   - calcula correto/errado
   - se `scenarioId` não em `simuladorConcluidos`: adicionar
3. Navigate → `SimuladorResultPage` com parâmetros (acerto + feedback).

## `SimuladorResultPage`
1. Mostrar feedback educativo e aviso “simulação educativa”.
2. Ao continuar:
   - atualizar badges por quantidade de cenários concluídos.
   - voltar para `SimuladorListPage`.

## `ConquistasPage`
1. Lista fixa de badges com ID + critério.
2. Card desbloqueado quando ID ∈ `badgesUnlocked`.
3. Nunca duplicar: sempre checar existência antes de adicionar.

## `CertificadoPage`
1. Se `certificadoUnlocked == false`: mostrar requisitos.
2. Se true: renderizar cartão com:
   - “Certificado de Participação – Bombeiro Aprerndiz Mirim”
   - data (`certificadoDataISO` formatada)
   - sem nome/idade.
3. Botão compartilhar:
   - alternativa simples: screenshot da área do certificado + share image.
   - alternativa robusta: gerar PDF local e compartilhar.

## `ParentalGatePage`
1. Gerar desafio aleatório simples (ex.: 7 + 5 = ?).
2. Input numérico + validar.
3. Se correto → `ResponsaveisPage`.
4. Se errado → mensagem “Somente responsável pode acessar esta área”.

## `ResponsaveisPage`
1. Texto de privacidade resumido:
   - sem coleta de dados
   - sem login
   - progresso local.
2. Mostrar links das WebViews (Boas-vindas / Cursos / Games).
3. Botão “Resetar progresso”:
   - confirmar em dialog
   - limpar states de progresso, badges e certificado.

### Lógica de badges (executar após resumo quiz e após simulador)
Sugestão IDs:
- `B1_PRIMEIRO_QUIZ` (primeira lição concluída)
- `B2_5_LICOES`
- `B3_10_LICOES`
- `B4_30_PERGUNTAS`
- `B5_60_PERGUNTAS`
- `B6_3_CENARIOS`
- `B7_10_CENARIOS`

Pseudofluxo:
- if `totalLicoesConcluidas >= 1` add B1
- if `>= 5` add B2
- if `>= 10` add B3
- if `totalPerguntasRespondidas >= 30` add B4
- if `>= 60` add B5
- if `simuladorConcluidos.length >= 3` add B6
- if `>= 10` add B7
Sempre com: “se não contém, adiciona”.

---

## E) HomePage — layout sugerido

Configuração prática:
1. `SafeArea` + `Column`.
2. Topo: logo + texto de boas-vindas.
3. Bloco principal (botões grandes 2 colunas):
   - Boas-vindas
   - Cursos
   - Games
   - Quiz
   - Simulador
4. Rodapé com atalhos menores:
   - Conquistas
   - Certificado
   - Responsáveis
5. Componentes visuais:
   - botões com altura mínima 64dp
   - contraste alto
   - ícones grandes
   - textos curtos para crianças.

Alternativas:
- **Simples:** `Wrap` com botões padrão do FlutterFlow.
- **Robusta:** criar `Reusable Component` de botão (ícone + título + subtítulo).

---

## F) WebView (topbar, banner fixo, bloqueio de navegação externa)

## Topbar
- AppBar padrão com:
  - seta voltar (`Navigate Back`)
  - botão fechar (`Navigate HomePage`, replace)

## Banner fixo
- Estrutura da página: `Stack`
  - camada 1: WebView (com padding inferior)
  - camada 2: `Align(bottomCenter)` com container fixo e texto:
  - “Você está acessando uma página dentro do app. Recomendamos acompanhamento de um responsável.”

## Bloqueio de links externos (especialmente Games)
- **Alternativa simples (sem custom code):**
  - abrir somente URL inicial fixa.
  - ocultar menus/botões externos no conteúdo da página web (ajuste no seu site).
  - no Games, não inserir links para fora do domínio.
- **Alternativa robusta (com Custom Code / pacote WebView):**
  - interceptar navegação (`navigationDelegate`).
  - permitir apenas host autorizado:
    - `bombeiroaprendizmirim.com.br` (games)
    - hosts específicos de boas-vindas/cursos quando necessário.
  - bloquear/ignorar qualquer URL externa.

---

## G) Textos prontos (copy)

## G.1 Boas-vindas interna (Home)
“Bem-vindo ao **Bombeiro Aprerndiz Mirim**!
Aqui você encontra conteúdos educativos, quiz, simulador e conquistas para aprender prevenção e segurança de forma divertida, sempre com acompanhamento de um responsável.”

## G.2 Banner WebView (fixo)
“Você está acessando uma página dentro do app. Recomendamos acompanhamento de um responsável.”

## G.3 Área do responsável
“Este aplicativo é voltado ao público infantil e foi projetado para respeitar privacidade.
- Não coletamos dados pessoais (sem nome, e-mail, telefone ou login).
- O progresso fica salvo apenas neste aparelho.
- Não exibimos anúncios, não usamos compras no app e não utilizamos rastreadores.”

## G.4 Mensagem anti-trote
“Emergência é assunto sério. Não faça trotes. Ligações falsas podem impedir o atendimento de quem realmente precisa.”

## G.5 Aviso educativo do simulador
“Simulação educativa: este conteúdo ensina boas práticas de prevenção. Em situações reais, procure imediatamente um adulto responsável e os serviços de emergência.”

---

## H) Checklist Google Play (Families + LGPD)

## H.1 Configurações a evitar
- Não integrar SDKs de anúncios.
- Não integrar analytics/tracking SDK.
- Não solicitar permissões sensíveis desnecessárias:
  - localização, contatos, câmera (se não usar), microfone, arquivos amplos.
- Não usar identificadores para perfilamento.

## H.2 Data Safety (cenário “sem coleta”)
Preencher com consistência técnica:
- Coleta de dados: **Não**.
- Compartilhamento de dados: **Não**.
- Criptografia em trânsito: aplicar para WebViews HTTPS (sim).
- Exclusão de dados: como não coleta dados pessoais no servidor, informar que não há conta/dados remotos.

## H.3 Families Policy (boas práticas)
- Declarar app como infantil (child-directed).
- Conteúdo apropriado para 6–15.
- Área do responsável protegida por Parental Gate.
- Linguagem educativa e não manipulativa.
- Sem ads, sem compras, sem link comercial agressivo.
- WebViews com aviso claro de acompanhamento do responsável.

## H.4 Ficha do app
- Descrição curta: foco educativo e prevenção.
- Capturas de tela reais do app (Home, Quiz, Simulador, Conquistas, Certificado).
- Política de privacidade simples e objetiva, refletindo “sem coleta”.
- Classificação indicativa coerente com conteúdo infantil educativo.

---

## I) Checklist de testes antes da publicação (Android)

## I.1 Navegação e estabilidade
- Abrir/fechar todas as páginas sem crash.
- Botão voltar do Android funcionando em cada fluxo.
- Deep flow completo: Tema → Lição → Perguntas → Resumo → salvar progresso.

## I.2 WebViews
- Boas-vindas abre URL correta.
- Cursos abre URL correta.
- Games abre URL do domínio correto.
- Banner fixo visível em todas as WebViews.
- Tentativas de saída para domínio externo (Games) bloqueadas (na versão robusta).

## I.3 Persistência local
- Fechar app e reabrir: progresso permanece.
- Melhor pontuação por lição preservada.
- Badges não duplicam.
- Certificado desbloqueia nos critérios corretos.
- Reset de progresso limpa estados e pede confirmação.

## I.4 Offline e conectividade
- Sem internet: WebViews mostram fallback amigável.
- Recursos nativos (quiz/simulador/conquistas já carregados) funcionam offline.

## I.5 Dispositivos e acessibilidade básica
- Testar telas pequenas (5"), médias e tablets.
- Contraste e tamanho de fonte legíveis.
- Alvos de toque grandes.
- Leitura de textos principais por leitor de tela (quando possível).

## I.6 Compliance final
- Confirmar ausência de SDKs proibidos.
- Confirmar ausência de coleta de PII.
- Confirmar permissões mínimas no `AndroidManifest`.
- Revisar textos de privacidade dentro do app e na Play Console para consistência.

---

## Implementação por etapas (ordem recomendada)

1. Criar páginas (sitemap A).
2. Criar App State (B) e marcar persistência.
3. Carregar JSON inicial no `SplashPage`.
4. Montar Home + navegação básica.
5. Implementar 3 WebViews com banner.
6. Implementar Quiz completo (tema > lição > pergunta > resumo).
7. Implementar Simulador + feedback.
8. Implementar Conquistas e regras anti-duplicação.
9. Implementar Certificado e compartilhamento por screenshot.
10. Implementar Parental Gate + Área do responsável + reset.
11. Rodar checklist de testes (I).
12. Preparar Play Console com foco Families/Data Safety.

Com esse plano você entrega a versão 1 com manutenção simples, totalmente local, sem monetização, e alinhada ao posicionamento infantil (Families) e privacidade (LGPD por minimização de dados).
