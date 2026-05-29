# Projeto 4: Deep Convolutional Generative Adversarial Networks (DCGAN) e SN-GAN
**Autor:** João Gabriel Pereira de Pádua  
**Instituição:** Centro Universitário de Brasília (UniCEUB)  
**Avaliador:** Prof. George Hideyuki Kuroki Júnior  

---

## Objetivo do Projeto
Este repositório documenta a construção, treinamento e otimização de Redes Adversárias Generativas Profundas focadas na síntese de rostos de personagens de anime. 

Nesta segunda fase da pesquisa empírica, o projeto evoluiu da simples calibragem de hiperparâmetros (como *Label Smoothing* e *Instance Noise*) para intervenções arquiteturais diretas e testes de engenharia de dados. O objetivo principal foi atingir o **Equilíbrio de Nash** no jogo Minimax, evitando os clássicos problemas de *Mode Collapse* e *Vanishing Gradient* (Fuga de Gradiente) que ocorrem quando o Discriminador supera o Gerador prematuramente.

---

## Hipóteses Testadas e Resultados Obtidos

Durante esta etapa provisória, três grandes teses foram colocadas à prova para tentar estabilizar o treinamento:

1. **Curadoria de Dados e Redução de Domínio (A Tese dos Datasets Curados):** * **Hipótese:** Limitar o dataset a nichos específicos (ex: apenas personagens loiros, totalizando 700 imagens; ou cabelos pretos, 15.000 imagens) facilitaria o aprendizado do Gerador.
   * **Resultado:** Falha por *Data Starvation* (Inanição de Dados). Em volumes de dados tão baixos, o Discriminador decorou os pixels reais (*Overfitting* extremo) logo nas primeiras épocas, quebrando a homeostase e destruindo os gradientes do Gerador. A conclusão empírica foi o retorno mandatório ao dataset in-the-wild massivo (63.000 imagens).
2. **Self-Attention GAN (A Tese da Visão Global):**
   * **Hipótese:** Injetar módulos de Atenção Própria nas camadas finais para forçar a simetria anatômica global (ex: evitar olhos de cores diferentes).
   * **Resultado:** Desestabilização do treinamento e retorno da Fuga de Gradiente nesta resolução específica (64x64). 
3. **Spectral Normalization (A Tese Vencedora - SN-GAN):**
   * **Hipótese:** Substituir o `BatchNorm2d` do Discriminador pela técnica de Normalização Espectral, limitando matematicamente a constante de Lipschitz da rede para impedir que o "Policial" atinja 100% de confiança rápido demais.
   * **Resultado:** **Sucesso Absoluto.** O modelo atingiu o Equilíbrio de Nash perfeito (probabilidades estabilizadas na casa dos ~0.5 para ambas as redes). O Gerador foi capaz de aprender de forma constante durante 150 épocas sem sofrer colapso estético.

---

## Guia de Navegação do Repositório (2ª Entrega Provisória)

A estrutura de diretórios foi expandida para acomodar as novas frentes de pesquisa e preservar o histórico do projeto.

* **`/notebooks/`**
  * **`notebooks_de_testes/`**: Histórico da primeira fase (Testes de Label Smoothing, Fliping, etc).
  * **`notebooks_de_testes_cabelos_pretos/`**: Contém os cadernos utilizados para testar a hipótese de redução de domínio. Com cabelos pretos.
  * **`notebooks_de_testes_loiros/`**: Contém os cadernos utilizados para testar a hipótese de redução de domínio. Com cabelos loiros.
  * **`projeto-4-dcgan-v7-spectral-normalization-150-epochs.ipynb`** **[CÓDIGO ATUAL VENCEDOR]**: Implementação final desta entrega. Utiliza o dataset completo de 63.000 imagens e a arquitetura SN-GAN (Spectral Normalization) no Discriminador para estabilização forçada dos gradientes.

* **`/relatorios/`**
  * **`relatorios_antigos/`**: Armazena os relatórios da primeira entrega provisória para fins de documentação de progresso.
  * **`RELATORIO_PROVISORIO_V2.md`**: Documento atual detalhando a análise técnica dos novos logs, a matemática por trás da Normalização Espectral e as lições aprendidas com os testes de curadoria de dados.

---

## Status Atual e Próximos Passos
Esta é a segunda entrega provisória. O modelo atual (SN-GAN) provou ser a arquitetura mais resiliente, sustentando o treinamento saudável ao longo das 150 épocas. Os próximos passos antes da entrega final incluirão o refinamento estético destas imagens geradas sob a proteção da Normalização Espectral e a compilação das métricas finais de avaliação da disciplina.