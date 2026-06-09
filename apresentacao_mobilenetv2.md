---
marp: true
title: Arquitetura Clássica — MobileNetV2 (Transfer Learning)
paginate: true
---

# 6. Arquitetura Clássica da Literatura
## MobileNetV2 + Transfer Learning

Classificação de resíduos (lixo) — 9 classes

> 🗣️ **Fala:** "Para essa etapa, em vez de criar mais uma rede do zero, aplicamos uma arquitetura consagrada na literatura: a **MobileNetV2**, usando a técnica de **transfer learning**."

---

# O que é uma "arquitetura clássica"?

- São redes neurais **famosas**, propostas em artigos científicos e validadas em competições (ex.: ImageNet).
- Exemplos: **LeNet, AlexNet, VGG, ResNet, DenseNet, EfficientNet, MobileNet**.
- Já vêm com uma **estrutura testada** e, muitas vezes, com **pesos pré-treinados** em milhões de imagens.

> 🗣️ **Fala:** "São arquiteturas que já provaram funcionar muito bem. Aproveitamos esse conhecimento pronto em vez de reinventar a roda."

---

# O que é a MobileNetV2?

- **Nome:** MobileNetV2
- **Ano:** 2018
- **Autores:** Sandler et al. — **Google**
- **Objetivo de projeto:** ser uma CNN **leve e eficiente**, capaz de rodar em **celulares e dispositivos com pouca memória**, sem perder muita precisão.

> 🗣️ **Fala:** "Ela foi criada pelo Google em 2018 pensando em rodar direto no celular — ou seja, é uma rede poderosa, mas econômica."

---

# Imagem da arquitetura

<!-- 🖼️ INSERIR AQUI o diagrama da MobileNetV2 (blocos de inverted residual).
     Sugestão: imagem do paper original ou um diagrama simplificado. -->

*(colar diagrama da MobileNetV2)*

> 🗣️ **Fala:** "Aqui vemos a estrutura em blocos. Vou explicar os dois diferenciais principais nos próximos slides."

---

# Diferencial 1 — Depthwise Separable Convolutions

A convolução normal faz tudo de uma vez (filtra espaço **e** combina canais). A MobileNetV2 **quebra isso em duas etapas**:

1. **Depthwise:** um filtro por canal (só a parte espacial).
2. **Pointwise (1×1):** combina os canais.

✅ Resultado: **muito menos cálculos e parâmetros** para um resultado parecido.

> 🗣️ **Fala:** "Em vez de uma operação pesada, ela faz duas leves. Isso reduz drasticamente o custo computacional."

---

# Diferencial 2 — Inverted Residuals + Linear Bottlenecks

- **Bottleneck:** a informação trafega "comprimida" (poucos canais) entre os blocos.
- **Inverted residual:** dentro do bloco, a rede **expande** os canais, processa, e **comprime** de novo.
- **Conexões residuais** (atalhos) ajudam o gradiente a fluir → treino mais estável em redes profundas.

> 🗣️ **Fala:** "Esses blocos preservam a informação importante usando pouquíssima memória — é o segredo da eficiência dela."

---

# O que é Transfer Learning?

- **Transferir conhecimento** de um modelo já treinado em um problema grande (ImageNet: **1,4 milhão de imagens, 1000 classes**) para o **nosso** problema.
- As primeiras camadas já sabem detectar **bordas, texturas, formas** — isso serve para **qualquer** imagem.
- Nós só **reaproveitamos** essa base e treinamos um **classificador novo** para as nossas 9 classes de lixo.

> 🗣️ **Fala:** "É como contratar alguém que já sabe enxergar e só ensinar a ela os nomes das nossas categorias de lixo."

---

# Por que usamos no nosso projeto?

1. **Temos poucos dados por classe** (~600 imagens/classe na subamostra). Treinar uma rede grande do zero levaria a **overfitting** — exatamente o que vimos na baseline.
2. **Os pesos do ImageNet generalizam melhor** com pouco dado.
3. **Mais rápido de treinar:** a base fica **congelada**, então treinamos só a "cabeça" do modelo.
4. **Eficiência:** MobileNetV2 é leve → cabe e roda fácil no Colab.

> 🗣️ **Fala:** "Nosso dataset é relativamente pequeno e variado. O transfer learning é a forma mais eficaz de conseguir boa acurácia nesse cenário."

---

# O que fizemos exatamente (deixando claro)

- Carregamos a **MobileNetV2 com pesos do ImageNet** (`weights="imagenet"`).
- **Removemos** o classificador original (`include_top=False`).
- **Congelamos a base** (`trainable = False`) → não treinamos os pesos pré-treinados.
- Adicionamos por cima:
  - `Rescaling` → converte os pixels de **[0,1] para [-1,1]** (formato que a MobileNetV2 espera);
  - `GlobalAveragePooling2D` + `Dropout` + `Dense(9, softmax)`.

> 🗣️ **Fala:** "Importante deixar claro: usamos transfer learning com a **base congelada**, não treinamos a MobileNetV2 do zero."

---

# Vantagens (resumo para a banca)

| Vantagem | Por quê |
|---|---|
| **Melhor acurácia com pouco dado** | aproveita o que o ImageNet já ensinou |
| **Treino rápido** | só a cabeça é treinada |
| **Menos overfitting** | base robusta + poucos parâmetros novos |
| **Leve / eficiente** | depthwise convs + bottlenecks |
| **Reprodutível** | pesos públicos e padronizados |

> 🗣️ **Fala:** "Em resumo: mais precisão, menos tempo e menos risco de overfitting do que a nossa CNN do zero."

---

# Resultado e comparação

- Comparamos **3 modelos** no **conjunto de teste**:
  - CNN baseline
  - CNN otimizada (Optuna)
  - **MobileNetV2 (transfer learning)**

> 📊 **Inserir aqui** o gráfico de comparação de acurácia no teste.

> 🗣️ **Fala:** "Como esperado, a MobileNetV2 teve o melhor desempenho, confirmando a vantagem do transfer learning." *(ajustar conforme o resultado real)*

---

# Limitações e possíveis melhorias

- A base ficou **congelada** — dá para fazer **fine-tuning** (descongelar as últimas camadas) para ganhar mais acurácia.
- Resolução de entrada reduzida (**96×96**) para economizar tempo; **224×224** (nativo do ImageNet) poderia melhorar.
- Dataset **desbalanceado** (algumas classes com menos imagens) afeta as classes minoritárias.

> 🗣️ **Fala:** "Há espaço para melhorar com fine-tuning e resolução maior, mas para o escopo do trabalho o resultado já é muito bom."

---

# Conclusão

- A **MobileNetV2** é uma arquitetura **leve, moderna e eficiente** (Google, 2018).
- O **transfer learning** permitiu obter **boa acurácia com pouco dado e pouco tempo de treino**.
- Foi a abordagem mais adequada ao nosso cenário (dataset pequeno e variado de resíduos).

> 🗣️ **Fala:** "Encerramos mostrando que reaproveitar uma arquitetura consagrada foi a escolha mais inteligente para o nosso problema."

---

# Obrigado!

**Perguntas?**

> 🗣️ Lembrete: todos os integrantes devem falar — dividam os slides entre a equipe.
