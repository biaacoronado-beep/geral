# Regras de análise — B.I. 009 (separação por responsabilidade)

Ao receber uma base "BI009" (aba única com colunas `Unidade, Arm, Status, ..., Codigo,
Qtd.Pen, Qtd.Receb., Qtd.Insp., Qtd.Disp.Prod, Est.Disp., ...`), consolidar por **Código ×
Status** e separar a quantidade nos grupos abaixo. Medida base = `Qtd.Pen`
(Qualidade usa `Qtd.Insp.`).

## Significado dos status
- **EST.SC** = Estoque **Santa Catarina** — ainda **não foi transferido**. É **Logística**.
- **PEND.TRANSF** = pendente de transferência. É **Logística**.
- **TRAN** = material em trânsito.
- **INSP / PROD / RETR / NCONF** = etapas produtivas/inspeção.

## Regras de classificação (usar sempre)
- **QUALIDADE** = Σ `Qtd.Insp.` apenas dos status **INSP, PROD, RETR, NCONF**
  (material parado em inspeção). **NÃO** incluir a inspeção do EST.SC.
- **LOGÍSTICA** = todo o resto sob responsabilidade da logística, **somando**:
  - **EST.SC inteiro** (inclusive a parcela em inspeção);
  - a parte **não inspecionada** de PROD/INSP/RETR/NCONF (`Qtd.Pen − Qtd.Insp.`);
  - **PEND.TRANSF** (pendente de transferência) — **somar dentro da Logística**.
- **TRANS** = Σ `Qtd.Pen` do status **TRAN** (manter como coluna própria).

Observação: `TRAN` e `PEND.TRANSF` carregam volume físico em `Est.Disp.`; a medida
padrão para os totais é `Qtd.Pen` (convenção do BI original). Se pedirem "estoque
físico em trânsito", usar `Est.Disp.`.

## Entregável
Gerar `.xlsx` com aba **Consolidado** (blocos por status Pen/Receb/Insp/Disp.Prod +
colunas de separação, com linha Total Geral congelada) e aba **Resumo** (totais e %).
Depois **commitar e dar push** no branch de trabalho.
