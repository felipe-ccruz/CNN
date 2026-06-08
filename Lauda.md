# Implementação de uma CNN (Convolutional Neural Network) — Lauda

## Objetivo

Implementar uma Rede Neural Convolucional (CNN) em um dataset de classificação de imagens, aplicar ajuste de hiperparâmetros com **Optuna** e analisar experimentalmente o impacto das configurações testadas no desempenho do modelo. O trabalho enfatiza:

- A compreensão dos componentes de uma CNN;
- O controle do custo computacional em experimentos com imagens;
- A análise crítica dos resultados obtidos;
- A exploração de uma rede consagrada na literatura.

**Equipes:** 2 a 4 alunos.

---

## Metodologia

### 1. Escolha e Validação do Dataset

Acesse o [Kaggle](https://www.kaggle.com/), [UCI Machine Learning Repository](https://archive.ics.uci.edu/) ou outro repositório e escolha um dataset de classificação de imagens.

O dataset deve conter, preferencialmente:

- Pelo menos **1000 imagens** no total;
- Pelo menos **100 imagens por classe**.

> ⚠️ **Estão vedados** datasets já trabalhados em sala, como **MNIST**, **Fashion MNIST**, **CIFAR-10**, ou datasets excessivamente documentados.

📋 Antes de iniciar o pré-processamento, o dataset escolhido deve ser **validado pela professora**. Para isso, a equipe deve enviar ou mostrar em aula:

- Nome do dataset;
- Link para o dataset;
- Número total de imagens;
- Número de classes;
- Quantidade de imagens por classe.

> ✅ Somente após essa validação o grupo poderá prosseguir com as etapas seguintes.

---

### 2. Pré-processamento do Dataset

Realize o pré-processamento adequado das imagens.

A divisão deve seguir o método **holdout estratificado**, mantendo, sempre que possível, a proporção das classes nos conjuntos de treino, validação e teste.

> ⚠️ O conjunto de **teste** deve ser separado desde o início e utilizado **apenas na avaliação final**.

---

### 3. Construção da CNN Baseline

Implemente uma CNN inicial, chamada de **baseline**, contendo pelo menos:

- 1 camada convolucional;
- Função de ativação;
- 1 camada de pooling;
- 1 camada de normalização ou regularização;
- 1 camada densa na MLP;
- Camada de saída adequada ao número de classes.

Defina os hiperparâmetros (filtros, kernel, dropout, otimizador, learning rate, batch size, épocas, etc.) livremente.

Treine a CNN baseline e registre:

- Arquitetura utilizada;
- Parâmetros;
- Acurácia/loss de treino e validação;
- Curvas de treino e validação;
- Breve análise sobre overfitting ou underfitting.

---

### 4. Ajuste de Hiperparâmetros com Optuna

Realize o ajuste de hiperparâmetros da CNN usando **Optuna**, com o sampler **TPE**.

O tuning deverá ser feito com **orçamento fixo** (número limitado de trials).

**Configuração mínima:**

- No mínimo **15 trials**;
- Uso obrigatório de **early stopping**;
- Uso de uma **amostra estratificada** para tuning, quando o dataset for grande.

> 💡 Para controlar o custo computacional, o tuning poderá ser realizado em uma amostra estratificada do conjunto de treino/validação, com tamanho recomendado entre **1000 e 5000 imagens**.

Após o tuning, a melhor configuração encontrada deverá ser treinada novamente usando o **maior conjunto de treino viável**, mantendo o teste separado.

#### Espaço de Busca de Referência

| Hiperparâmetro | Valores / Intervalo Recomendado |
|---|---|
| Filtros na 1ª camada convolucional | 16 a 64 |
| Filtros na 2ª camada convolucional | 32 a 128 |
| Unidades na camada densa | 64 a 256 |
| Taxa de dropout | 0.1 a 0.5 |
| Learning rate | 0.0001 a 0.01 |
| Batch size | 32 ou 64 |
| Número máximo de épocas por trial | 10 a 15 |

#### Valores Fixos Recomendados (salvo justificativa)

| Item | Valor Recomendado |
|---|---|
| Tamanho do kernel | 3×3 |
| Função de ativação intermediária | ReLU |
| Otimizador | Adam |
| Função de perda | `sparse_categorical_crossentropy` ou `categorical_crossentropy`, conforme o formato dos rótulos |
| Métrica principal | Acurácia ou F1-score, se houver forte desbalanceamento |

> A equipe poderá ampliar ou restringir os intervalos sugeridos, **justificando a necessidade**.

---

### 5. Análise dos Trials e dos Resultados

Registre e analise os resultados obtidos pelo Optuna. Apresente pelo menos:

- Tabela com os trials executados;
- Melhor configuração encontrada;
- Comparação entre a CNN baseline e a CNN otimizada;
- Análise dos hiperparâmetros mais relevantes;
- Curvas de treino e validação do modelo final;
- Avaliação final no conjunto de teste;
- Matriz de confusão.

A análise deve discutir:

- Se o tuning melhorou o desempenho em relação à baseline;
- Se o resultado no teste foi compatível com o resultado da validação;
- Se houve sinais de overfitting ou underfitting;
- Se o melhor trial pareceu consistente ou isolado;
- Quais classes foram mais confundidas;
- Quais limitações o experimento apresentou.

---

### 6. Arquitetura Clássica da Literatura

Escolha e aplique uma **CNN famosa** (LeNet, AlexNet, VGG, ResNet, DenseNet, EfficientNet, etc.) no mesmo dataset.

Apresente:

- Nome e ano de publicação;
- Diagrama/imagem da arquitetura;
- Breve explicação do seu diferencial.

> ⚠️ **Atenção:** dependendo do tamanho do dataset e da complexidade da arquitetura escolhida, talvez seja necessário usar **transfer learning** (por exemplo, usar pesos pré-treinados e ajustar alguns layers) ao invés de treinar do zero. Fiquem à vontade para usar transfer learning se pertinente, mas **deixem claro o que foi feito**.

---

## Entregas

### 📓 Notebook Completo

Um notebook (de preferência desenvolvido no **Google Colab**) contendo todos os códigos desenvolvidos, desde eventuais etapas de pré-processamento dos dados, definição e treinamento dos modelos, busca de hiperparâmetros, até a avaliação e comparação de resultados.

O notebook deve:

- Conter textos explicativos em **Markdown** intercalados com o código (como um relatório escrito dentro do notebook);
- Incluir gráficos de treino/validação, tabelas comparativas de performance e comentários sobre os resultados;
- Estar **organizado, comentado e executável em ordem**.

> 💡 É preferível entregar o notebook no formato do Google Colab (link ou arquivo `.ipynb`). Verifiquem se **todas as células executam em ordem sem erros** e comentem qualquer dependência ou passo necessário para reproduzir os resultados.

### 🎤 Apresentação Oral de Resultados

Os resultados serão apresentados para a turma oralmente. A estrutura da apresentação deve seguir as mesmas etapas do trabalho, a partir do item **2 (Pré-processamento)**, considerando que o dataset já está validado.

Requisitos:

- Duração máxima de **12 minutos**;
- **Todos os membros** da equipe devem falar;
- A ausência na apresentação implica **nota zero individual**, mesmo com a entrega do código.

### 📊 Slides

Devem ser submetidos no Classroom juntamente com o notebook.

---

## ⚠️ Importante: Declaração de Uso de IA Generativa

Seguindo os princípios institucionais quanto à transparência no uso da IA Generativa e respeitando o regulamento para uso acadêmico de IA disponibilizado, o notebook deverá conter, de forma **clara e explícita**, uma **seção inicial** com:

- Os **nomes dos integrantes** da equipe;
- A **declaração de uso de IA Generativa** no trabalho (caso tenha sido utilizada), contendo no mínimo:
  - A ferramenta utilizada;
  - A finalidade do uso;
  - A extensão aproximada da contribuição da IA Generativa;
  - As medidas adotadas para validação do conteúdo.