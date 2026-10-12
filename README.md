<p align="center">
  <img src="logo.png" alt="DivinuS Mod Restart" width="320">
</p>

<h1 align="center">DivinuS Mod Restart</h1>

<p align="center">
  Restart automático para servidores <b>DayZ</b> com aviso na tela, sirene e logout seguro, feito todo pelo próprio mod.<br>
  <a href="README.en.md">English version</a>
</p>

---

## Sobre

O **DivinuS Mod Restart** reinicia o servidor nos horários que você escolher e prepara os jogadores antes disso. Ele mostra um aviso no topo da tela e toca uma sirene. Perto da hora, faz o personagem sentar e sair pelo logout normal do DayZ. Assim o personagem é salvo antes do restart e ninguém perde item por rollback.

O mod não usa CFTools nem `messages.xml` e não precisa de programa extra. Toda a configuração fica em um arquivo: `profiles/restart/restart.json`.

## Recursos

- Restart em horários fixos (`04:00`, `08:00`...) ou por intervalo (a cada X minutos).
- Aviso centralizado numa caixa com borda. A contagem é em minutos e, no último minuto, em segundos, com o texto piscando.
- Sirene antes do restart, que pode repetir até a hora.
- Logout seguro: o personagem senta e passa pela tela de logout do próprio DayZ (a mesma do botão **Sair**). No fim, ele é salvo.
- Quem entrar no servidor ou cancelar o logout nesse período recebe o logout de novo.
- Na hora do restart, o servidor salva quem ainda estiver online antes de fechar.
- Dois modos de restart: `REINICIAR` (o jogo fecha e abre de novo) ou `DESLIGAR` (só fecha, para hospedagens que religam o servidor sozinhas).
- Textos, cores, tamanhos, posição, borda e fundo do aviso podem ser configurados.
- Uma opção liga e desliga o restart.
- Não depende de CF nem de nenhum outro mod.

## Como funciona (configuração padrão)

| Faltando | O que acontece |
|---|---|
| 5 min | Aparece o aviso `RESTART DO SERVIDOR EM 5 MIN` |
| 3 min | Toca a sirene |
| 2 min | O personagem senta e começa o logout do DayZ (15 s). No fim ele é salvo e desconectado |
| 1 min | A contagem passa para segundos e o texto pisca |
| 0:00 | O servidor salva quem sobrou e reinicia |

---

## Instalação

### Servidor

1. Copie a pasta `@DivinuS_Mod_Restart` para a pasta do servidor. Ela vem da Steam Workshop ou da página de Releases.
2. No parâmetro de inicialização do servidor, adicione o mod em `-mod=`, separado dos outros mods por `;`:
   ```
   -mod=@CF;@OutroMod;@DivinuS_Mod_Restart
   ```
   Use `-mod=` e **não** `-serverMod=`. Os jogadores precisam do mod para ver o aviso, ouvir a sirene e sentar.
3. Copie a chave `.bikey` de `@DivinuS_Mod_Restart/keys` para a pasta `keys` do servidor.
4. Ligue o servidor uma vez. O mod cria sozinho o arquivo `<pasta profile>/restart/restart.json`. A `<pasta profile>` é a pasta indicada em `-profiles=`.
5. Edite o arquivo como quiser e reinicie o servidor.

### Jogadores

Os jogadores baixam o mod pela Steam Workshop ou pelo launcher, como qualquer outro mod do servidor.

---

## Configuração

Arquivo: `<pasta profile>/restart/restart.json`

```json
{
    "RestartLigado": 1,
    "HorariosDeRestart": ["00:00", "04:00", "08:00", "12:00", "16:00", "20:00"],
    "IntervaloEmMinutos": 0,
    "ComoReiniciar": "REINICIAR",
    "MinimoMinutosLigado": 10,
    "AvisoFaltandoMinutos": 5,
    "SireneFaltandoMinutos": 3,
    "RepetirSirene": 0,
    "LogoutFaltandoMinutos": 2,
    "SentarAntesDoLogout": 1,
    "SegundosDoLogout": 15,
    "TextoMinutos": "RESTART DO SERVIDOR EM %1 MIN",
    "TextoSegundos": "RESTART DO SERVIDOR EM %1 SEG",
    "TextoLogout": "SALVANDO SEU PERSONAGEM PARA O RESTART",
    "TextoReiniciando": "REINICIANDO O SERVIDOR...",
    "CorDoTexto": [255, 40, 40],
    "TamanhoDoTexto": 28,
    "DistanciaDoTopo": 100,
    "PiscarNoUltimoMinuto": 1,
    "MostrarCaixa": 1,
    "LarguraDaCaixa": 620,
    "EspessuraDaBorda": 2,
    "CorDaBorda": [255, 40, 40],
    "OpacidadeDoFundo": 60
}
```

### Opções

**Ligar e desligar**

| Campo | Padrão | O que faz |
|---|---|---|
| `RestartLigado` | `1` | `1` = ligado. `0` = desligado (sem aviso e sem restart) |

**Quando reiniciar**

| Campo | Padrão | O que faz |
|---|---|---|
| `HorariosDeRestart` | `["00:00", "04:00", ...]` | Horários no relógio do PC do servidor, no formato `HH:MM` |
| `IntervaloEmMinutos` | `0` | Só vale se a lista de horários estiver vazia. Reinicia X minutos depois que o servidor liga (`0` = sem restart) |
| `MinimoMinutosLigado` | `10` | Se o servidor acabou de ligar, pula um horário que cairia antes desse tempo (0 a 120) |

**Como reiniciar**

| Campo | Padrão | O que faz |
|---|---|---|
| `ComoReiniciar` | `"REINICIAR"` | `"REINICIAR"` = o jogo fecha e abre de novo. `"DESLIGAR"` = só fecha. Se o modo escolhido não fechar o servidor, o mod tenta o outro depois de 60 segundos |

**Tempos (minutos antes do restart)**

| Campo | Padrão | O que faz |
|---|---|---|
| `AvisoFaltandoMinutos` | `5` | Quando o aviso aparece (1 a 60) |
| `SireneFaltandoMinutos` | `3` | Quando a sirene toca (`0` = sem sirene) |
| `RepetirSirene` | `0` | `1` = repete a sirene até o restart |
| `LogoutFaltandoMinutos` | `2` | Quando começa o logout (`0` = sem logout, máximo 30) |
| `SentarAntesDoLogout` | `1` | `1` = o personagem senta antes do logout |
| `SegundosDoLogout` | `15` | Duração da tela de logout do DayZ, em segundos (5 a 60) |

**Textos** (`%1` vira o número)

| Campo | Padrão | Quando aparece |
|---|---|---|
| `TextoMinutos` | `RESTART DO SERVIDOR EM %1 MIN` | Contagem em minutos |
| `TextoSegundos` | `RESTART DO SERVIDOR EM %1 SEG` | Último minuto |
| `TextoLogout` | `SALVANDO SEU PERSONAGEM PARA O RESTART` | Durante o logout |
| `TextoReiniciando` | `REINICIANDO O SERVIDOR...` | Na hora do restart |

**Visual**

| Campo | Padrão | O que faz |
|---|---|---|
| `CorDoTexto` | `[255, 40, 40]` | Cor do texto em R, G, B (0 a 255) |
| `TamanhoDoTexto` | `28` | Altura do texto em pixels (10 a 80) |
| `DistanciaDoTopo` | `100` | Distância do topo da tela em pixels |
| `PiscarNoUltimoMinuto` | `1` | `1` = o texto pisca no último minuto |
| `MostrarCaixa` | `1` | `1` = caixa escura com borda. `0` = só o texto |
| `LarguraDaCaixa` | `620` | Largura da caixa em pixels (100 a 1600) |
| `EspessuraDaBorda` | `2` | Espessura da borda em pixels (`0` = sem borda, máximo 10) |
| `CorDaBorda` | `[255, 40, 40]` | Cor da borda em R, G, B |
| `OpacidadeDoFundo` | `60` | `0` = fundo transparente. `100` = fundo preto |

### Regras importantes

- Edite o arquivo num editor como o Notepad++ e não apague aspas nem vírgulas.
- Os campos `LEIA_1` a `LEIA_5` são só explicação e podem ficar no arquivo.
- O arquivo é lido quando o servidor liga. Depois de mudar alguma coisa, reinicie o servidor.
- Se o arquivo tiver erro de digitação, o restart fica **desligado** e o log mostra `ERRO ao ler profiles/restart/restart.json`. Corrija e reinicie.
- Valores fora do limite são ajustados automaticamente para o mínimo ou o máximo permitido.

---

## Exemplos

**Restart a cada 3 horas, a partir da meia-noite**
```json
"HorariosDeRestart": ["00:00", "03:00", "06:00", "09:00", "12:00", "15:00", "18:00", "21:00"],
"IntervaloEmMinutos": 0,
```

**Restart a cada 6 horas, contando de quando o servidor liga**
```json
"HorariosDeRestart": [],
"IntervaloEmMinutos": 360,
```

**Hospedagem que religa o servidor sozinha**
```json
"ComoReiniciar": "DESLIGAR",
```

**Só o aviso, sem sirene e sem logout**
```json
"SireneFaltandoMinutos": 0,
"LogoutFaltandoMinutos": 0,
```

**Textos em inglês**
```json
"TextoMinutos": "SERVER RESTART IN %1 MIN",
"TextoSegundos": "SERVER RESTART IN %1 SEC",
"TextoLogout": "SAVING YOUR CHARACTER FOR THE RESTART",
"TextoReiniciando": "RESTARTING THE SERVER...",
```

**Aviso amarelo, sem caixa**
```json
"CorDoTexto": [255, 200, 0],
"MostrarCaixa": 0,
```

---

## Como testar

Para ver o ciclo completo em 10 minutos, deixe a lista de horários vazia e use o intervalo:

```json
"HorariosDeRestart": [],
"IntervaloEmMinutos": 10,
```

Reinicie o servidor e entre no jogo. Os tempos contam a partir de quando o servidor termina de carregar:

| Tempo | O que deve acontecer |
|---|---|
| 5 min | O aviso aparece |
| 7 min | A sirene toca |
| 8 min | O personagem senta e sai pela tela de logout |
| 10 min | O servidor reinicia |

Depois do teste, volte os horários e coloque `"IntervaloEmMinutos": 0`. Se esquecer, o servidor reinicia a cada 10 minutos.

## Logs

O mod escreve no script log do servidor (`script_*.log`, na pasta profile). Todas as linhas começam com `[DivinuS_Restart]`:

```
[DivinuS_Restart] profiles/restart/restart.json carregado.
[DivinuS_Restart] Restart LIGADO. Modo de reinicio: REINICIAR
[DivinuS_Restart] Proximo restart em 10 minutos.
[DivinuS_Restart] Logout para o restart: NomeDoJogador (15 s)
[DivinuS_Restart] HORA DO RESTART. Salvando jogadores e reiniciando em 5 segundos.
[DivinuS_Restart] Reiniciando o servidor (RequestRestart). Tentativa 1
```

---

## Usando em hospedagem

- Um mod não consegue ligar um servidor que está desligado. Se a sua hospedagem já religa o servidor quando ele fecha, use `"ComoReiniciar": "DESLIGAR"` e deixe o religamento automático do painel ligado.
- Desligue outros restarts agendados (CFTools, painel da hospedagem) para o servidor não reiniciar duas vezes.
- Os horários seguem o relógio da máquina do servidor. Confira o fuso horário da hospedagem.
- Antes de usar de verdade, faça o teste de 10 minutos descrito acima.

## Problemas comuns

| Problema | O que fazer |
|---|---|
| O aviso não aparece | Confira se o mod está em `-mod=` (não em `-serverMod=`) e se `RestartLigado` está `1`. Procure `ERRO` nas linhas `[DivinuS_Restart]` do log |
| O log mostra `ERRO ao ler profiles/restart/restart.json` | Falta ou sobra aspas ou vírgula no arquivo. Corrija e reinicie o servidor |
| O log mostra `Horario invalido ignorado` | Use o formato `HH:MM`, por exemplo `"04:00"` |
| O restart acontece no horário errado | O mod usa o relógio do PC do servidor. Confira o fuso horário da máquina ou da hospedagem |
| O servidor reinicia duas vezes | Há outro restart agendado (CFTools ou painel) ou o mod está carregado duas vezes. Deixe só um |
| O servidor fecha e não volta | Na hospedagem, use `DESLIGAR` e ative o religamento automático do painel |
| Mudei o arquivo e nada mudou | O arquivo só é lido quando o servidor liga. Reinicie o servidor |

---

## Para desenvolvedores

### Estrutura do repositório

```
DivinuS-Mod-Restart/
├── DivinuS_Mod_Restart/            código-fonte (vira o PBO)
│   ├── $PBOPREFIX$.txt
│   ├── config.cpp
│   ├── data/logo.paa
│   ├── gui/images/branco.paa
│   ├── gui/layouts/restart.layout
│   ├── sounds/sirene.ogg
│   └── Scripts/
│       ├── 3_Game/DivinuS_Restart/DVS_RestartClient.c
│       └── 5_Mission/DivinuS_Restart/
│           ├── DVS_RestartConfig.c
│           ├── MissionServer.c
│           └── MissionGameplay.c
├── @DivinuS_Mod_Restart/mod.cpp    nome, autor e logo no launcher
├── profiles/restart/restart.json   exemplo de configuração
└── docs/logo.png
```

### Como funciona por dentro

- **Servidor (`MissionServer.c`)**: lê o `restart.json`, calcula o próximo restart e envia o horário para os jogadores por RPC, a cada 30 segundos e quando alguém conecta. Na hora do logout, chama `OnClientDisconnectedEvent`, o mesmo caminho do botão **Sair** do DayZ. Na hora do restart, salva e desconecta quem sobrou e chama `RequestRestart` ou `RequestExit`.
- **Jogador (`MissionGameplay.c`)**: desenha o aviso, toca a sirene (SoundSet `DVS_RestartSiren_SoundSet`, sem posição 3D) e faz o personagem sentar (gesto `ID_EMOTE_SITA`).
- **Sincronização (`DVS_RestartClient.c`)**: recebe as configurações enviadas pelo servidor. O RPC usa o id `1146508114`.

### Compilar o PBO

1. Abra o **Addon Builder** (DayZ Tools).
2. Origem: a pasta `DivinuS_Mod_Restart`. Destino: `@DivinuS_Mod_Restart/addons`.
3. Em **Options**, confira se o prefixo é `DivinuS_Mod_Restart` e adicione `*.c;*.layout;*.ogg;*.paa` em **List of files to copy directly**.
4. Clique em **Pack**.

### Assinar

1. No **DS Utils** (DayZ Tools), use **DSCreateKey** para criar a sua chave. Isso gera um `.biprivatekey` e um `.bikey`.
2. Use **DSSignFile** para assinar o `DivinuS_Mod_Restart.pbo`. O `.bisign` aparece ao lado do PBO.
3. Coloque o `.bikey` em `@DivinuS_Mod_Restart/keys`.

> **Nunca** envie o arquivo `.biprivatekey` para o GitHub nem para ninguém. O `.gitignore` deste repositório já bloqueia esse arquivo.

---

## Créditos

**Autor:** DivinuS
