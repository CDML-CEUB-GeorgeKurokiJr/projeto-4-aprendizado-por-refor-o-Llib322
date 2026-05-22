# Projeto 4: Deep Convolutional Generative Adversarial Networks (DCGAN)
**Autor:** João Gabriel Pereira de Pádua  
**Instituição:** Centro Universitário de Brasília (UniCEUB)  
**Avaliador:** Prof. George Hideyuki Kuroki Júnior  

---

## Objetivo do Projeto
Este repositório documenta a construção, treinamento e otimização de uma Rede Adversária Generativa Profunda (DCGAN) focada na geração de rostos de personagens de anime (Anime Faces). 

O desafio central deste projeto não é apenas escrever a topologia da rede, mas manter a **Homeostase do Treinamento** no Jogo Minimax entre o Gerador (Falsificador) e o Discriminador (Perito). Ao longo da pesquisa empírica, lidamos com desafios matemáticos clássicos como *Mode Collapse* (Colapso de Modo) e Fuga de Gradiente, aplicando e contrastando técnicas de engenharia como *Label Smoothing*, *Label Flipping* (Instance Noise) e a Superparametrização do Gerador.

---

## Guia de Navegação do Repositório (Entrega Provisória)

A estrutura deste repositório foi organizada para separar os experimentos exploratórios do código otimizado.

* **`/notebooks/`**
  * **`notebooks_de_testes/`**: Contém o histórico de experimentação. Devido ao tamanho das imagens geradas em alta resolução (salvas nos outputs), os arquivos brutos desta pasta possuem entre 29MB e 51MB.
    * `projeto-4-dcgan_versao1.ipynb`: Primeiro protótipo da arquitetura.
    * `projeto-4-dcgan_versao1_100_epocas.ipynb`: Teste inicial de resistência do Gerador.
    * `projeto-4-dcgan_versao2_350_epocas.ipynb`: Análise do erro de *Overtraining* e colapso pelo mau uso do Label Smoothing duplo.
    * `projeto-4-dcgan_versao3_75_epocas.ipynb`: Aplicação de *Label Flipping* (5% de entropia). Atingiu equilíbrio numérico, mas resultou em perda de nitidez visual.
  
  * **`projeto-4-dcgan_versao4_150_epocas.ipynb`** **[CÓDIGO ATUAL]**: O Gerador "Peso-Pesado". A rede geradora teve seus mapas de características dobrados (128 features), conseguindo competir visualmente com um Discriminador rigoroso (1.0 e 0.0 absolutos) e gerando resultados de alta fidelidade anatômica.
    * **Aviso de Infraestrutura:** Os outputs de imagens deste caderno final foram intencionalmente purgados (Clear Outputs). A geração de 150 tensores de alta resolução inflou o JSON do arquivo para 51 MB, corrompendo a estrutura nativa de visualização nas IDEs web. Os logs de época e as amostras visuais foram documentados fisicamente no Relatório.

* **`/relatorios/`**
  * **`RELATORIO_PROVISORIO.md`**: Documento detalhando a análise de cada iteração, os motivos do colapso de certos modelos, as ilusões numéricas do treinamento adversarial e as justificativas teóricas por trás da superparametrização final.

---

## Status Atual e Próximos Passos
Esta é uma entrega provisória. O experimento provou que um **Gerador Superparametrizado** enfrentando um Discriminador estrito gera os melhores resultados visuais. A versão final e definitiva (treinada por 280 épocas com *Data Augmentation* Simétrico) está atualmente em processamento para explorar o limite computacional do hardware e será anexada em breve para a conclusão da disciplina.