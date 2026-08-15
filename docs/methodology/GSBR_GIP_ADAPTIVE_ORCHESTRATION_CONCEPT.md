# GSBR ↔ Core 3.0 ↔ GIP — Conceito de Orquestração Adaptativa

## Status

**Documento conceitual / futuro desenvolvimento.**

Este documento registra decisões de arquitetura discutidas durante a preparação do ensaio `GSBR-E6H-01`. Ele **não altera o foco atual**, que permanece sendo a execução, auditoria e consolidação do teste de endurance de 6 horas do Grom Synthetic BR.

A implementação desta camada administrativa/orquestradora deverá ocorrer somente após o fechamento técnico da rodada E6H-01 e do primeiro benchmark controlado do Core 3.0.

---

## 1. Contexto

O Grom Synthetic BR (GSBR) não deve ser tratado apenas como um gerador massivo de placas. Seu valor principal é produzir famílias controladas de amostras, nas quais uma placa-base canônica (L0) pode originar versões com adversidades isoladas, progressivas e combinadas, mantendo rastreabilidade integral.

O Core 3.0, por sua vez, deve avaliar essas amostras e produzir evidência estruturada sobre acertos, erros, confiança, pares de caracteres confundidos, estágios de falha e fronteiras de robustez.

O GIP deverá atuar futuramente como coordenador/orquestrador entre os sistemas independentes, usando os resultados do Core para decidir quais novas amostras o GSBR deve produzir, em que quantidade e com quais adversidades.

O princípio é:

`GSBR gera -> Core avalia -> GIP diagnostica -> GIP emite nova ordem -> GSBR gera de forma dirigida -> Core reavalia`

---

## 2. Separação de responsabilidades

### 2.1 Grom Synthetic BR

Responsável por:

- geração de placas-base canônicas;
- geração de adversidades controladas;
- combinações manuais de adversidades;
- combinações aleatórias controladas;
- execução de ordens de produção;
- manifesto, seed, versões, parâmetros e hashes;
- checkpoints, telemetria e auditoria;
- estados de governança do dataset.

O GSBR **não decide sozinho o que deve treinar o Core**.

### 2.2 Core 3.0

Responsável por:

- executar leitura/OCR sobre os lotes disponibilizados;
- registrar resultado por `sample_code`;
- registrar confiança, exact match, erros por caractere e estágio de falha;
- produzir métricas por adversidade e nível;
- identificar a fronteira de falha (`failure boundary`);
- exportar resultados estruturados para análise externa.

O Core **não deve comandar diretamente a geração de dados**.

### 2.3 GIP

Responsável futuramente por:

- coordenar campanhas GSBR ↔ Core;
- receber resultados do Core;
- manter um registro estruturado de fraquezas;
- priorizar carências reais observadas;
- determinar quantidade, tipo e dificuldade das novas amostras;
- emitir ordens de produção ao GSBR;
- coordenar futuras coortes de treinamento controlado;
- preservar trilha de auditoria das decisões.

O GIP deverá atuar como **orquestrador**, sem retirar a independência operacional do GSBR ou do Core.

---

## 3. Controle administrativo de produção do GSBR

O GSBR deverá possuir uma camada administrativa própria, separada do motor de geração.

Essa camada deverá permitir definir explicitamente uma campanha ou Ordem de Produção Sintética (OPS).

### 3.1 Quantidade por padrão

Exemplo inicial:

- `50` placas Mercosul (`MER`);
- `50` placas Legacy (`LEG`);
- total: `100` placas-base L0.

A quantidade deverá ser configurável independentemente para cada padrão.

### 3.2 Adversidades selecionáveis

Para cada campanha, o operador poderá escolher quais famílias serão aplicadas, por exemplo:

- `RA` — chuva / superfície molhada;
- `BL` — baixa luminosidade;
- `EL` — excesso de luminosidade;
- `RF` — reflexo / glare;
- `MB` — motion blur;
- `AN` — ângulo / perspectiva;
- `SJ` — sujeira / poeira;
- `OC` — oclusão parcial;
- `RZ` — redução de resolução;
- `CP` — compressão.

### 3.3 Níveis

Cada adversidade poderá ser habilitada em níveis específicos:

- `L0` — placa-base canônica, representada somente pelo código-base;
- `L1` — leve;
- `L2` — moderado;
- `L3` — severo;
- `L4` — stress / extremo.

Exemplo:

- RA: níveis 1, 2, 3 e 4;
- BL: níveis 1, 2 e 3;
- MB: níveis 2 e 3.

### 3.4 Combinações manuais

O operador poderá definir combinações explícitas, por exemplo:

- `RA2MB2`;
- `BL2MB2`;
- `RF2AN2`;
- `BL3MB2`.

O sistema não deverá executar produto cartesiano indiscriminado por padrão.

### 3.5 Combinações aleatórias controladas

Também deverá existir um modo de combinação aleatória, permitindo ao GSBR selecionar livremente adversidades dentro de limites administrativos previamente definidos.

A liberdade deverá ser governada por parâmetros como:

- famílias permitidas;
- níveis permitidos;
- quantidade máxima de adversidades por amostra;
- quantidade máxima de amostras aleatórias por campanha;
- seed de seleção;
- limites de combinações incompatíveis;
- distribuição esperada por família/nível.

Exemplo:

- adversidades permitidas: RA, BL, MB, AN;
- níveis permitidos: 1 a 3;
- máximo de 2 adversidades por amostra;
- 200 combinações aleatórias;
- seleção reprodutível por seed.

Aleatório não significa não auditável.

---

## 4. Padrão de identificação

O padrão já definido permanece:

`[LEG|MER][ID de 5 dígitos][ADV][nível]...`

Exemplos:

- `LEG00001` — Legacy limpa / L0;
- `MER00001` — Mercosul limpa / L0;
- `LEG00001RA1` — chuva leve;
- `LEG00001BL2MB2` — baixa luminosidade moderada + motion blur moderado;
- `MER00125RA3AN2` — chuva severa + perspectiva moderada.

A placa limpa não recebe sufixo `L0`; o próprio código-base representa o controle canônico.

---

## 5. Ordem de Produção Sintética — OPS

Deverá ser criada futuramente uma entidade administrativa própria, por exemplo:

`OPS-GSBR-00001`

Ela deverá conter, no mínimo:

- identificador da campanha;
- objetivo;
- quantidade MER;
- quantidade LEG;
- adversidades habilitadas;
- níveis habilitados por adversidade;
- combinações manuais;
- configuração de combinações aleatórias;
- seed(s);
- limites de produção;
- perfil de governança;
- estado da ordem.

A OPS deverá ser validada antes da geração.

---

## 6. Campanha, Ordem e Run

Os seguintes conceitos deverão permanecer separados:

### Campanha

Representa o experimento ou objetivo maior.

Exemplo:

`GSBR-E6H-01`

### Ordem de Produção

Representa exatamente o que foi solicitado ao GSBR.

Exemplo:

`OPS-GSBR-00001`

### Run

Representa uma execução concreta da ordem.

Exemplo:

`RUN-20260815-020300`

Uma mesma OPS pode possuir mais de um run em caso de retomada, repetição experimental ou falha operacional, sem perder a rastreabilidade histórica.

---

## 7. Estados administrativos

A execução da ordem poderá usar estados como:

- `DRAFT`;
- `VALIDATED`;
- `READY`;
- `RUNNING`;
- `PAUSED`;
- `COMPLETED`;
- `FAILED`;
- `AUDITED`.

Esses estados não devem ser confundidos com a governança do dataset.

Governança do dataset:

- `GENERATED_EXPERIMENTAL`;
- `VALIDATED_EXPERIMENTAL`;
- `APPROVED_FOR_CONTROLLED_TRAINING`.

A última transição nunca deverá ocorrer automaticamente apenas porque a geração foi concluída.

---

## 8. Resultados esperados do Core para o GIP

O Core deverá futuramente fornecer ao GIP, por amostra, ao menos:

- `sample_code`;
- `base_code`;
- `ground_truth`;
- texto OCR bruto;
- texto OCR final;
- confiança;
- `exact_match`;
- erros por caractere;
- posição do erro;
- pares confundidos;
- adversidade(s);
- nível(is);
- estágio de falha;
- comportamento de abstention/inconclusive;
- versão do Core/modelo;
- identificador do run de avaliação.

Exemplo conceitual:

```text
sample_code: MER00012BL2MB2
ground_truth: ABC1D23
ocr_final: ABC1023
exact_match: false
confidence: 0.61
failure_stage: OCR
confusion: D->0
```

---

## 9. Weakness Registry do GIP

O GIP deverá manter um registro estruturado das carências observadas pelo Core.

Esse registro deverá permitir agregação por:

- padrão MER/LEG;
- adversidade;
- nível;
- combinação;
- caractere;
- posição;
- par de confusão;
- estágio de falha;
- confiança;
- frequência;
- severidade;
- prioridade de correção.

Exemplos de conclusões possíveis:

- `MB3` derruba significativamente a taxa de exact match;
- `RF2` aumenta confusão O/0;
- `BL2MB2` afeta Mercosul mais que Legacy;
- determinada posição da placa apresenta maior taxa de erro sob perspectiva;
- casos L1/L2 já estão dominados e não justificam aumento indiscriminado de volume.

---

## 10. Planejamento adaptativo pelo GIP

A partir do Weakness Registry, o GIP poderá futuramente emitir novas OPS direcionadas.

Exemplo:

- produzir mais 120 amostras `MB3`;
- produzir 100 amostras `BL2MB2` somente para Mercosul;
- produzir 80 amostras `RF2` com maior presença de pares O/0;
- produzir nova bateria L2/L3 para uma classe de glyphs em que o Core apresentou regressão.

O princípio é **Weakness-Driven Adaptive Training**:

> gerar progressivamente mais material onde existe fraqueza medida, em vez de simplesmente aumentar volume em condições já dominadas.

---

## 11. Treinamento controlado

O GIP poderá coordenar futuramente coortes de treinamento, mas a sequência deverá ser obrigatória:

`GSBR gera -> auditoria -> Core avalia baseline -> GIP diagnostica -> GIP seleciona cohort -> aprovação explícita -> treinamento controlado -> reavaliação`

Nunca:

`GSBR gera -> treinamento automático imediato`

O Golden Set e demais conjuntos independentes de avaliação devem permanecer isolados.

---

## 12. Fluxo conceitual final

```text
                         +--------------------------+
                         |           GIP            |
                         | Orquestrador Adaptativo  |
                         +------------+-------------+
                                      |
                                      | OPS
                                      v
+-----------------------+     +-------+---------+
|         GSBR          |     |     Core 3.0    |
| Produção Sintética    |     | OCR / Avaliação |
+-----------+-----------+     +--------+--------+
            |                          ^
            | dataset auditado         |
            +--------------------------+
                       resultados
```

Ciclo operacional futuro:

`Generate -> Audit -> Evaluate -> Diagnose -> Prioritize -> Targeted Generate -> Controlled Train -> Re-evaluate`

---

## 13. Estado atual da infraestrutura E6H-01

Na data de registro desta decisão, a infraestrutura do ensaio de 6 horas já foi implementada e validada com o seguinte retorno:

### Pacote endurance

`tools/grom_synthetic_br_v2/endurance/`

- `sample_code.py` — padrão `[LEG|MER]xxxxx[ADV][1-4]`, ordem canônica RA..CP, níveis 1-4;
- `adversity_codes.py`;
- `checkpoint.py` — persistência atômica + resume;
- `telemetry.py`;
- `runner.py` — fases P0-P4, orçamento oficial de 21.600 segundos;
- `audit.py`;
- `governance.py`.

### CLIs e configuração

- `tools/run_endurance_6h.py`;
- `tools/audit_endurance_run.py`;
- `configs/endurance_6h_v1.yaml`.

### Documentação

- `docs/methodology/SAMPLE_CODE_STANDARD.md`;
- `docs/methodology/ENDURANCE_6H_PROTOCOL.md`.

### Testes

- `tests/test_sample_code_standard.py`;
- `tests/test_endurance_6h_runner.py`;
- `tests/test_endurance_6h_audit.py`.

### Validação reportada

- suíte completa: **298 passed**;
- **34 testes novos**;
- dry-run real com config oficial e seed 42: **102 amostras**;
- falhas: **0**;
- auditoria: `PASS`;
- `samples=102`;
- `files=102`;
- `issues=0`;
- resume sem duplicação de `sample_code`;
- auditoria detecta tamper SHA-256, órfãos, arquivo ausente e duplicados;
- `GENERATED_EXPERIMENTAL -> VALIDATED_EXPERIMENTAL` somente com `--apply-validated`;
- `APPROVED_FOR_CONTROLLED_TRAINING` nunca é atribuído automaticamente;
- nenhum treinamento foi iniciado;
- Golden Set permaneceu intocado.

### Correções realizadas durante a implementação

- import relativo em `sample_code.py`;
- `transformations_mismatch` no audit, relacionado à chave `operations`;
- inicialização de estado no resume;
- suporte ao smoke com `phase_duration_seconds`.

### Comando oficial preparado

```bash
python -m tools.run_endurance_6h --config configs/endurance_6h_v1.yaml --run-id GSBR-E6H-01
```

Este comando deve iniciar somente a campanha E6H-01 já validada. A arquitetura de orquestração descrita neste documento permanece registrada para implementação posterior.

---

## 14. Prioridade imediata

A sequência imediata permanece:

1. executar `GSBR-E6H-01`;
2. auditar e consolidar o resultado das 6 horas;
3. congelar o dataset experimental correspondente;
4. executar o primeiro benchmark T0 do Core 3.0 sobre material apropriado e isolado;
5. medir failure boundaries;
6. somente então retomar a implementação do controle administrativo GSBR e do orquestrador adaptativo no GIP.

A existência deste documento não autoriza antecipar essas etapas.
