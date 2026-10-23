# Dashboard — Kommo · Pré-qualificação

Painel dos **leads** do Kommo: quantos entraram, de onde vieram, em que fase estão, o que
responderam na triagem e quantos viraram reunião. Lê em tempo real a aba **Leads** da planilha
**Dashboards Kommo** (`1Pj8w7YofhBR2PixGFUET9AhG713ZUcbig3lu3rlnlQI`), preenchida pelo Apps
Script a partir da API do Kommo.

**Acesso geral.** Os três perfis da Central — Administração, Lideranças e Equipe — têm cofre
neste painel. Entra-se pela [Central de Dashboards](https://borgesmacedoadvocacia.github.io/).

## Como os dados chegam

```
Kommo (API v4) ──► Apps Script "Dashboard Meta Ads" (KommoLeads.gs, de hora em hora)
                                      │
                                      ▼
                      planilha Dashboards Kommo › aba Leads ──► este painel
```

O coletor vive no projeto do Meta Ads só por causa do token: `KOMMO_TOKEN` já está nas
Propriedades daquele script e um token só é menos coisa para vencer. Fora isso é independente —
outra planilha, outra aba, outro gatilho (`sincronizarKommoLeads`).

| Coluna da aba Leads | Conteúdo |
|---|---|
| ID, Lead | identificação no Kommo (o painel linka para o lead) |
| Criado em, Hora | quando o lead entrou — é por esta data que o painel filtra |
| Funil, Etapa | onde o lead está agora |
| Situação | Entrada · Em qualificação · Reunião marcada · Perdido, derivada do nome da etapa |
| Origem, Campanha, Produto jurídico | campos preenchidos pela automação do Kommo |
| Plano de saúde, Tipo do plano, Mensalidade, Tempo de plano | as quatro respostas da triagem |
| Responsável | quem atende |
| Reunião em | data da entrada na etapa **Enviado ao ADVBOX** |

## Regras de leitura

- **O funil `Processual` fica de fora**: lá são clientes, não leads.
- **Reunião marcada = entrada na etapa `Enviado ao ADVBOX`**, a mesma definição do painel de
  campanhas do Meta. Uma função só, usada pelos dois (`primeirasEntradasAdvbox_`), para não
  existirem duas contagens diferentes da mesma coisa.
- A reunião conta **no mês em que o lead entrou**, não no mês da reunião: é a conversão da safra
  que interessa aqui. (No painel do Meta é o contrário — lá a reunião conta no mês em que
  aconteceu, para comparar com o gasto daquele mês.)
- **Pré-qualificado** = respondeu ao menos uma das quatro perguntas da triagem. Quem entrou por
  fora do formulário (direct, grupo, lista) fica em *sem pré-qualificação* e é apontado em
  *Pontos de atenção*.
- As colunas são achadas **pelo nome do cabeçalho**, nunca pela posição: o coletor reescreve a
  aba e uma coluna nova no meio não pode trocar o significado das outras.

## Filtros

Mês de entrada (um ou vários), origem e funil. Todo cartão, linha de tabela e barra abre a lista
dos leads por trás do número, com link direto para o lead no Kommo.

## Estrutura

```
index.html    painel completo (cofres, leitura via Sheets API v4, gráficos Chart.js)
```

Publicação: GitHub Pages via Actions (`.github/workflows/pages.yml`), a cada push em `main`.
O coletor está em [dashboard-meta-ads](https://github.com/borgesmacedoadvocacia/dashboard-meta-ads)
(`apps-script/KommoLeads.gs`).
