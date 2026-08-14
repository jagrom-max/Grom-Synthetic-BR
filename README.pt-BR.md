# Grom Synthetic BR

Framework auditável para geração sintética de placas brasileiras, pesquisa em OCR, visão computacional e experimentação reprodutível.

> Status: **Pesquisa / Experimental**. O projeto não é homologado e não substitui validação humana ou registros oficiais.

[English](README.md)

## Missão

O Grom Synthetic BR foi desenvolvido para suprir uma carência prática e acadêmica identificada durante pesquisas em OCR e visão computacional aplicadas ao contexto brasileiro. Existe amplo material internacional sobre dados sintéticos, ANPR/ALPR e reconhecimento óptico de caracteres, porém encontramos pouco conteúdo aberto, auditável e reutilizável com foco específico em placas brasileiras e em suas particularidades normativas e geométricas.

Grande parte da metodologia precisou ser desenvolvida praticamente do zero. Por isso, o projeto busca não apenas atender às necessidades do próprio laboratório, mas também deixar uma contribuição pública para pesquisadores, estudantes e desenvolvedores que enfrentem o mesmo problema.

## Princípios

- reprodutibilidade;
- rastreabilidade de decisões;
- separação entre fato normativo, referência técnica e decisão de engenharia;
- revisão humana antes de promover novas baselines;
- ausência de deformações silenciosas apenas para melhorar métricas;
- controle rigoroso de licenças e proveniência;
- separação entre dados de treino e conjuntos independentes de avaliação;
- preservação de casos difíceis como sentinelas de pesquisa.

## Contribuição à comunidade

O objetivo é oferecer uma base técnica documentada para geração sintética brasileira, testes de OCR, análise de glyphs, estudos de layout, casos de stress reproduzíveis, auditoria normativa e experimentação determinística.

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

## Segurança e privacidade

Não envie ao repositório dados pessoais, credenciais, chaves privadas, material sigiloso ou datasets de circulação restrita.

Veja `SECURITY.md` e `CONTRIBUTING.md`.

## Autor

**Josuel Grom**

Origem e direção de pesquisa: Grom Synthetic BR / Grom OCR Training Lab.
