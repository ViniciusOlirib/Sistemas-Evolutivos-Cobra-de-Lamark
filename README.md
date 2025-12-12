# Sistemas-Evolutivos-Cobra-de-Lamark
Repositório referente ao projeto da disciplina Sistemas Evolutivos Aplicados à Robótica - SSC0713 ministrada pelo professor Eduardo do Valle Simões.

Este projeto implementa um agente autônomo capaz de aprender a jogar o clássico **Snake Game** utilizando uma abordagem híbrida de Inteligência Artificial.  
A solução combina **Reinforcement Learning (Deep Q-Learning)** para aprendizado intra-vida e **Algoritmos Genéticos (GA)** para evolução entre gerações.

---

## Sobre o Projeto

O objetivo principal do agente é coletar o máximo de maçãs possível sem colidir com as paredes ou com o próprio corpo.

Diferente de abordagens tradicionais que aplicam apenas uma técnica, este projeto explora a **sinergia entre aprendizado por reforço e evolução biológica simulada**.

---

## Principais Características

- **Deep Q-Learning (DQN)**: Treina a tomada de decisão imediata do agente com base em recompensas.
- **Algoritmo Genético (GA)**: Seleciona e cruza os melhores agentes entre gerações.
- **Processamento Paralelo**: Usa `torch.multiprocessing` para treinar vários agentes simultaneamente.
- **Alta Diversidade Genética**: Introdução de agentes “imigrantes” a cada geração para evitar mínimos locais.

---

## Arquitetura da Rede Neural (Linear_QNet)

A “mente” de cada agente é uma rede neural feedforward construída com PyTorch.

### **Entradas (Estado – 11 neurônios)**  
Valores binários representando o estado atual da cobra:

1. Perigo à frente  
2. Perigo à direita  
3. Perigo à esquerda  
4. Direção: Esquerda  
5. Direção: Direita  
6. Direção: Cima  
7. Direção: Baixo  
8. Comida à esquerda  
9. Comida à direita  
10. Comida acima  
11. Comida abaixo  

### **Camada Oculta**
- 256 neurônios (configurável)  
- Ativação: **ReLU**

### **Saídas (Ação – 3 neurônios)**  
O agente escolhe entre:

- `[1, 0, 0]` → Seguir reto  
- `[0, 1, 0]` → Virar à direita  
- `[0, 0, 1]` → Virar à esquerda  

---

## Metodologia Evolutiva (GA + RL)

O treinamento ocorre em **gerações de 20 agentes**.

### 1️**Treinamento (Deep Q-Learning)**
- Cada agente joga simultaneamente (multiprocessos).
- Recompensas:
  - Positiva ao comer.
  - Negativa ao colidir.
- Atualização dos pesos via **Backpropagation + Adam Optimizer**.

### **Seleção**
- Os agentes são ranqueados pelo *score*.
- Os **10 melhores** são selecionados como pais.

### **Reprodução (Crossover)**
- Descendentes gerados por média ponderada dos pesos:  
  `α·P1 + β·P2`, com α e β proporcionais ao score dos pais.

### **Mutação**
Dois tipos de ruído Gaussiano são aplicados:

- **Small Gaussian** → pequenas correções (exploração fina)  
- **Big Gaussian** → grandes alterações nos pesos (exploração ampla)

### **Imigração**
- **5 novos agentes aleatórios** adicionados a cada geração para manter diversidade genética.

---

## Estrutura dos Arquivos

