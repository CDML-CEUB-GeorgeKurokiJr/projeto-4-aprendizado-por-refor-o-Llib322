# Relatório Provisório de Pesquisa: DCGAN e o Equilíbrio Minimax
**Projeto 4 - Aprendizado Não Supervisionado e Redes Adversárias**

## 1. Introdução e Configuração do Espaço Latente
A presente pesquisa documenta o desenvolvimento empírico de uma Rede Adversária Generativa Profunda (DCGAN) projetada para sintetizar imagens a partir do dataset *Anime Faces* (63.632 imagens em 64x64 pixels, canais RGB). O desenvolvimento foi pautado pela observação contínua da Função de Perda (BCELoss) e sua tradução no aspecto qualitativo (nitidez anatômica e diversidade de geração).

O pipeline de ingestão foi rigorosamente configurado para mapear os tensores de imagem no espectro matemático de [-1.0, 1.0]. O objetivo central da pesquisa consistiu em testar limites arquiteturais para manter a homeostase no Jogo Minimax, evitando os dois problemas mais críticos em GANs: o esmagamento dos gradientes (Fuga de Gradiente) e a saturação de geração (*Mode Collapse*).

## 2. Metodologia e Histórico de Ablações

Para atingir o "Estado da Arte Local", a arquitetura passou por quatro iterações principais. Abaixo, detalhamos as motivações, execuções e resultados de cada fase.

### 2.1. Experimento 1: O Baseline Inicial (100 Épocas)
* **Motivação:** Estabelecer uma linha de base (*baseline*) utilizando a arquitetura clássica proposta por Radford et al., com otimizadores Adam estritos (beta1 = 0.5).
* **Execução:** O modelo rodou por 100 épocas sem intervenções ou ruídos artificiais.
* **Resultado Numérico:** O Discriminador dominou o jogo nas primeiras épocas, atingindo métricas de confiança superiores a **90%**, enquanto o Gerador oscilava em taxas de sucesso inferiores a **2%**.
* **Diagnóstico Visual:** Confirmou-se na prática o "Paradoxo da Loss" em redes generativas. Apesar de os logs indicarem um massacre matemático contra o Gerador, os micro-gradientes residuais fluíram. O modelo começou a formar estruturas faciais rudimentares coerentes, provando que a rede estava aprendendo, embora de forma sufocada.

### 2.2. Experimento 2: A Armadilha do Smoothing Duplo (350 Épocas)
* **Motivação:** Escalar o tempo de treinamento (350 épocas) e aplicar *Label Smoothing* (suavização de rótulos para **0.9**) a fim de "frear" a confiança excessiva do Discriminador.
* **Falha Estrutural:** A teoria foi aplicada incorretamente no turno do Gerador. O alvo do falsificador foi ajustado para **0.9** em vez da perfeição absoluta (**1.0**).
* **Diagnóstico Visual e Matemático:** A rede neural encontrou uma brecha exploratória na função matemática. O Gerador percebeu que matrizes distorcidas e hiper-saturadas causavam exatamente **10%** de confusão no Discriminador. Ocorreu um *Mode Collapse* violento, resultando na degeneração completa dos pixels. 

### 2.3. Experimento 3: Injeção de Entropia e a Perda de Sharpness (75 Épocas)
* **Motivação:** Corrigir a métrica do Gerador (alvo em **1.0**) e testar a técnica de *Instance Noise* (*Label Flipping*). Injetamos ruído estocástico, forçando o Discriminador a receber rótulos invertidos (mentiras) em **5%** das iterações, ancorando assim a perda matemática de forma artificial.
* **Resultado Numérico:** A técnica forçou um "Equilíbrio Tenso" perfeito. Os logs exibiram estabilidade inédita (`D(x): 0.765` vs `D(G(z)): 0.100`).
* **Diagnóstico Visual:** Fracasso prático. Ao tornar o Discriminador artificialmente tolerante, o Gerador perdeu o incentivo de refinar micro-características. As imagens geradas apresentaram texturas lavadas, como aquarelas, perdendo totalmente a nitidez de contorno (*sharpness*).

### 2.4. Experimento 4: Força Bruta e Superparametrização (150 Épocas)
* **Motivação:** Abandonar as "trapaças" lenientes. Restaurar o rigor implacável do Discriminador (rótulos absolutos de **1.0** e **0.0**), e resolver a assimetria da Batalha Minimax fortalecendo o Gerador.
* **Execução:** O número base de *Feature Maps* da rede geradora foi dobrado (de **64** para **128**), concedendo "músculos" computacionais para lidar com o escrutínio do Perito.
* **Diagnóstico Visual e Matemático:** O Discriminador manteve-se severo (`D(x): 0.842`), mas a superparametrização permitiu ao Gerador manter a resiliência (`D(G(z)): 0.076`). Visualmente, atingimos nosso melhor resultado provisório: reflexos ópticos nos olhos, mechas capilares definidas e proporções de crânio respeitadas. 

---

## 3. Desafios de Engenharia de Software e Infraestrutura

Como evidência do peso computacional processado na versão final de 150 épocas, a geração iterativa de tensores visuais ao final de cada laço inflou o arquivo de testes `.ipynb` para **51 MB** (devido à codificação das imagens em Base64 dentro do JSON). 

Isto deflagrou o limite de *parse* nas IDEs baseadas na web (Kaggle/Colab), disparando o erro `Unterminated string in JSON`. Em estrito alinhamento com as melhores práticas de versionamento (*Git*), os tensores visuais foram expurgados (limpos) do código-fonte armazenado neste repositório. A integridade estrutural do notebook foi preservada, delegando o armazenamento visual exclusivamente a imagens físicas isoladas (`.png`).

---

## 4. Tabela de Resumo Estratégico

| Experimento | Arquitetura G | Épocas | Estratégia Adicional | Resultado Visual |
| :--- | :--- | :--- | :--- | :--- |
| **V1. Baseline** | 64 Features | 100 | Nenhuma | Inicial, borrado, aprendizado lento. |
| **V2. Collapse** | 64 Features | 350 | Smoothing em G (0.9) | Colapso de Modo. Pixels estourados. |
| **V3. Flipping** | 64 Features | 75 | Label Flipping (5%) | Equilíbrio numérico, mas sem *sharpness*. |
| **V4. Força Bruta**| 128 Features | 150 | Nenhuma (Rigor Absoluto) | **Estado da Arte Local.** Alta nitidez. |

## 5. Conclusão Provisória e Próximos Passos
Restou empiricamente comprovado que a Assimetria de Tarefas em Redes Adversárias favorece o classificador. A melhor abordagem não consiste em "cegar" a rede discriminadora, mas sim fornecer capacidade cognitiva superior à rede geradora. O último teste da pesquisa consistirá em rodar a versão purificada por 280 épocas, incorporando espelhamento horizontal estocástico (`RandomHorizontalFlip`) no pipeline de dados para retardar a memorização do Discriminador e explorar o limite máximo do hardware na nuvem.
