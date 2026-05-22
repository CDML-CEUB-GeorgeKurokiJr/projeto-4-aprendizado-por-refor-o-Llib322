# Projeto 4: Deep Convolutional Generative Adversarial Networks (DCGAN)
**Autor:** João Gabriel Pereira de Pádua  
**Instituição:** Centro Universitário de Brasília (UniCEUB)  
**Avaliador:** Prof. George Hideyuki Kuroki Júnior  

---

## Objetivo do Projeto
Este repositório documenta a construção, treinamento e otimização de uma Rede Adversária Generativa Profunda (DCGAN) focada na geração de rostos de personagens de anime (Anime Faces). 

O desafio central deste projeto não é apenas escrever a topologia da rede, mas manter a **Homeostase do Treinamento** no Jogo Minimax entre o Gerador (Falsificador) e o Discriminador (Perito). Ao longo da pesquisa empírica, lidamos com desafios matemáticos clássicos como *Mode Collapse* (Colapso de Modo) e Fuga de Gradiente, aplicando técnicas de engenharia como *Label Smoothing*, *Label Flipping* (Instance Noise) e Superparametrização do Gerador.

---

## Guia de Navegação do Repositório (Entrega Provisória)

A estrutura deste repositório foi organizada para separar os experimentos exploratórios do código otimizado.

* **`/notebooks/`**
  * **`notebooks_de_testes/`**: Contém o histórico de experimentação. Devido ao tamanho das imagens geradas em alta resolução (salvas nos outputs), os arquivos possuem entre 29MB e 51MB.
    * `projeto-4-dcgan_versao1.ipynb`: Primeiro protótipo da arquitetura.
    * `projeto-4-dcgan_versao1_100_epocas.ipynb`: Teste de resistência do Gerador.
    * `projeto-4-dcgan_versao2_350_epocas.ipynb`: Análise do erro de *Overtraining* e colapso pelo mau uso do Label Smoothing duplo.
    * `projeto-4-dcgan_versao3_75_epocas.ipynb`: Aplicação de *Label Flipping* (5% de entropia). Atingiu equilíbrio numérico, mas perdeu nitidez visual.
  * **`projeto-4-dcgan_versao4_150_epocas.ipynb`** **[CÓDIGO ATUAL]**: O Gerador "Peso-Pesado". A rede geradora teve seus mapas de características dobrados (128 features), conseguindo competir visualmente com o Discriminador e gerando resultados de alta fidelidade anatômica.

* **`/relatorios/`**
  * **`RELATORIO_PROVISORIO.md`**: Documento detalhando a análise de cada iteração, por que certos modelos colapsaram e as justificativas teóricas por trás das mudanças arquiteturais.

---

## Status Atual e Próximos Passos
Esta é uma entrega provisória. O experimento provou que um **Gerador Superparametrizado** enfrentando um Discriminador rigoroso gera os melhores resultados visuais. A versão final e definitiva (treinada por 280 épocas com *Data Augmentation* Simétrico) está atualmente em processamento e será anexada em breve para a conclusão da disciplina.
