# Grom Synthetic BR

Framework auditável para geração sintética de placas brasileiras, pesquisa em OCR, visão computacional e experimentação reprodutível.

> Status: **Pesquisa / Experimental**. O projeto não é homologado e não substitui validação humana ou registros oficiais.

[English](README.md)

## Missão

O Grom Synthetic BR foi desenvolvido para suprir uma carência prática e acadêmica identificada durante pesquisas em OCR e visão computacional aplicadas ao contexto brasileiro. Existe amplo material internacional sobre dados sintéticos, ANPR/ALPR e reconhecimento óptico de caracteres, porém encontramos pouco conteúdo aberto, auditável e reutilizável com foco específico em placas brasileiras e em suas particularidades normativas e geométricas.

Grande parte da metodologia precisou ser desenvolvida praticamente do zero. Por isso, o projeto busca não apenas atender às necessidades do próprio laboratório, mas também deixar uma contribuição pública para pesquisadores, estudantes e desenvolvedores que enfrentem o mesmo problema.

## Propósito central: máxima fidelidade primeiro, adversidades controladas depois

O Grom Synthetic BR **não é apenas um gerador de placas visualmente corretas segundo o contexto normativo brasileiro**.

Seu propósito principal é produzir inicialmente uma representação canônica da placa com a maior fidelidade técnica e documental possível e, a partir dessa referência limpa, gerar cenários progressivamente mais difíceis para treinamento e avaliação de robustez de OCR.

O fluxo conceitual pretendido é:

`placa canônica de alta fidelidade -> adversidades visuais controladas -> amostra sintética rotulada -> treino / avaliação de robustez do OCR`

O projeto foi concebido para simular, de forma parametrizada e reproduzível, condições adversas como:

- excesso ou insuficiência de luminosidade;
- reflexos, brilho intenso, sombras e exposição irregular;
- chuva e efeitos de superfície molhada;
- neblina, névoa e perda atmosférica de contraste;
- sujeira, barro, poeira e contaminação parcial;
- envelhecimento, degradação e desgaste do material;
- caracteres desbotados ou parcialmente danificados;
- oclusão parcial;
- blur e perda de detalhe decorrente da captura;
- variações de perspectiva e ângulo de visão;
- combinações tipográficas difíceis;
- anomalias sintéticas controladas que representem degradação ou possíveis padrões de adulteração, **exclusivamente para pesquisa defensiva de robustez de OCR**.

Essas transformações não devem ser ruído arbitrário aplicado sobre a imagem. Cada adversidade deverá possuir parâmetros, versão e metadados suficientes para permitir que o experimento seja reproduzido e que a origem exata de cada amostra seja rastreada.

O objetivo é deliberadamente exigente: um OCR não deve aprender somente a partir de placas limpas e ideais. O sistema deve ser exposto a exemplos progressivamente mais difíceis, porém controlados e auditáveis, de forma que sua robustez possa ser medida objetivamente.

O projeto separa, portanto, duas camadas fundamentais:

1. **Camada de fidelidade canônica** — produzir a melhor representação defensável da placa brasileira a partir das evidências documentadas.
2. **Camada de adversidade / robustez** — aplicar degradações controladas somente depois da geração canônica, sem alterar silenciosamente o ground truth.

Uma degradação nunca deve redefinir a identidade da placa. A placa limpa de origem, seu texto, os parâmetros de geração e todas as transformações aplicadas devem permanecer rastreáveis.

## Princípios

- reprodutibilidade;
- máxima fidelidade canônica antes de qualquer degradação;
- simulação controlada e rastreável de adversidades para robustez de OCR;
- rastreabilidade de decisões;
- separação entre fato normativo, referência técnica e decisão de engenharia;
- revisão humana antes de promover novas baselines;
- ausência de deformações silenciosas apenas para melhorar métricas;
- controle rigoroso de licenças e proveniência;
- separação entre dados de treino e conjuntos independentes de avaliação;
- preservação de casos difíceis como sentinelas de pesquisa.

## Contribuição à comunidade

O objetivo é oferecer uma base técnica documentada para geração sintética brasileira, testes de OCR, análise de glyphs, estudos de layout, casos de stress reproduzíveis, auditoria normativa, simulação controlada de condições adversas e experimentação determinística.

A contribuição pretendida não é apenas um gerador. É uma metodologia aberta para que pesquisadores possam construir amostras brasileiras limpas e, depois, degradá-las de forma conhecida, parametrizada e reproduzível para estudos de robustez.

Pesquisadores e desenvolvedores são incentivados a reproduzir os experimentos, citar o projeto, relatar limitações, documentar modificações e contribuir com melhorias verificáveis.

## Contexto normativo

O projeto utiliza referências oficiais brasileiras, incluindo a Resolução CONTRAN nº 969/2022 e seus anexos. Uma referência normativa é tratada como evidência; inferências de engenharia continuam sendo identificadas como inferências.

Classificações usadas no projeto incluem:

- `NORMATIVE_FACT`;
- `DOCUMENTED_REFERENCE`;
- `MEASURED_FROM_NORMATIVE_FIGURE`;
- `DERIVED_VALUE`;
- `ENGINEERING_INFERENCE`;
- `UNKNOWN`.

Ausência de dado não autoriza inventar um dado.

## Sentinelas de pesquisa

- `ABC1D23` — controle histórico;
- `IJI1I11` — glyphs estreitos;
- `BOS5S68` — controle intermediário;
- `ZGQ2G62` — stress espacial;
- `MWQ8O01` — glyphs largos;
- `QMW0O80` — largura e confusão O/0.

Essas strings são controles sintéticos e não representam, por si só, placas reais observadas.

## Ecossistema relacionado

O Grom Synthetic BR é independente, mas poderá fornecer dados e metodologia para outros projetos Grom, incluindo:

- Grom OCR Core 3.0;
- Grom Vehicle Vision (GVV);
- GIP, como ambiente integrador de sistemas independentes.

Links públicos serão adicionados quando cada projeto estiver pronto para publicação.

## Licenciamento

O projeto adota licenciamento em camadas:

- código-fonte: Apache License 2.0;
- documentação original: CC BY 4.0, salvo indicação diferente;
- fontes, modelos, datasets e ativos de terceiros: conforme suas próprias licenças;
- datasets gerados: conforme política específica e proveniência dos insumos.

Um ativo usado localmente em laboratório não é automaticamente redistribuível.

## Citação e reciprocidade

Se este trabalho contribuir para artigo, TCC, dissertação, tese, benchmark, dataset, software ou relatório técnico, cite o projeto e informe a versão ou commit utilizado.

A licença não impõe obrigação adicional de publicar toda obra derivada. Ainda assim, quem se beneficiar do projeto é fortemente incentivado a devolver correções, melhorias, documentação e resultados reproduzíveis à comunidade.

## Segurança, privacidade e uso responsável

Não envie ao repositório dados pessoais, credenciais, chaves privadas, material sigiloso ou datasets de circulação restrita.

Cenários sintéticos de degradação ou adulteração são destinados exclusivamente ao fortalecimento defensivo de OCR e à avaliação de robustez. O projeto não pretende orientar métodos físicos de evasão ou derrota de sistemas de identificação.

Veja `SECURITY.md` e `CONTRIBUTING.md`.

## Roadmap

Entre as próximas etapas estão:

1. consolidar o repositório público e a auditoria de licenças;
2. concluir a revisão de ocupação espacial e layout;
3. estabilizar a geração canônica antes de escalar o dataset;
4. implementar um motor versionado e parametrizado de adversidades;
5. modelar progressivamente luminosidade, chuva, neblina, sujeira, degradação, desgaste, blur, perspectiva e demais condições de robustez;
6. registrar em metadados toda transformação aplicada;
7. criar releases versionadas de datasets sintéticos;
8. validar os ganhos em OCR sem contaminar conjuntos independentes de avaliação.

## Autor

**Josuel Grom**

Origem e direção de pesquisa: Grom Synthetic BR / Grom OCR Training Lab.
