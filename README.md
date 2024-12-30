# PPGTI3100 - TÓPICOS AVANÇADOS EM INTELIGÊNCIA COMPUTACIONAL 1
## [UFRN](https://www.ufrn.br/) - [IMD](https://www.metropoledigital.ufrn.br/portal/) - [PPGTI](https://sigaa.ufrn.br/sigaa/public/programa/apresentacao.jsf?lc=pt_BR&id=7872)
## Programa de Mestrado em Inteligência Computacional
## Professor: [ELIAS JACOB DE MENEZES NETO](http://www.docente.ufrn.br/elias.jacob)
## Aluno: Lázaro Raimundo de Oliveira


# Verificação de estratégias de identificação produtos semelhantes usando Embeddings 

### Objetivo

Tentar identificar estratégias que permitam apontar produtos semelhantes numa base de preço 

### Motivação

Bases de preço ou banco de preços possuem produtos que são iguais ou muito semelhantes com descrições diferentes. Isso dificulta o agrupamento e, consequentemente, a comparação de preços de produtos. Esse problema tem sido trabalhado por vários autores. Nesses notebooks propõem-se avaliar estratégias baseadas em modelos LLM's estado da arte. 

### Passos e técnicas testadas

### Leitura de Produtos

Foi realizada a leitura da lista de produtos e selecionada uma amostra de 5000 itens.

[1_Seleção_de_Lista_Amostra_de_Produtos.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/439ee5eaed79a5fd34276b8e674b0eddc56367e8/1_Sele%C3%A7%C3%A3o_de_Lista_Amostra_de_Produtos.ipynb)

### Embedding e classificação das partes dos nomes dos produtos

Para embedding foi utilizado o modelo [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). Esse modelo tem 1024 fatores ou componentes. Também foi realizada uma classificação dos elementos da descrição dos produtos usando o modelo [llama3.1](https://ollama.com/library/llama3.1) e exportado em um array de json.

Para classificação das partes da descrição do produto. a execução em GPU demorou 2 segundos por requisição. A execução em CPU demorou mais de 3 minutos cada.

Para obtenção dos embeddings, a execução total em GPU demorou menos de 5 minutos. 

[2_Meta_Llama_3_70B.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/439ee5eaed79a5fd34276b8e674b0eddc56367e8/2_Meta_Llama_3_70B.ipynb)


### Tentativa de clusterização usando kmeans e métrica silhouette

A primeira tentativa de agrupamento foi usando kmeans e avaliando a silhouette. Os valores de silhouette (0,025 .. -0,05) indicaram que os produtos parecidos não formam grupos pequenos e identificáveis facilmente.

[3_Clusteriza%C3%A7%C3%A3o_dos_produtos_k_means.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/fa994209f1d46f8583e035677217886662dd9e6b/3_Clusteriza%C3%A7%C3%A3o_dos_produtos_k_means.ipynb)

### Clusterização Usando Agglomerative

Nessa tentantiva foi usado Cluster aglomerativo (Agglomerative Cluster) com a métrica silhouette. Para medida de distância foi considerada a métrica cosseno. O melhor valor de silhouette foi 0,10, indicando que o processo não parece adequado. Também foi construída uma leitura 3D dos 3 maiores componentes que correspondiam a aproximadamente 15% da variabilidade, tendo como resultado visual uma nuvem bem compacta com aparente visualização de 2 grupos.

[4_Clusteriza%C3%A7%C3%A3o_dos_produtos_AgglomerativeClustering](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/448212fdc8f6a7c7a8a093b619d2251002740307/4_Clusteriza%C3%A7%C3%A3o_dos_produtos_AgglomerativeClustering.ipynb)

### Verificação direta da distância cosseno

Nessa verificação foi testada diretamente a distância cosseno entre os produtos e selecionados os mais próximos. Inicialmente não foi usada nenhuma técnica de clusterização, apenas a distância cosseno, tendo critério de similaridade se o valor estava acima de 0,8. Pela verificação manual em lista de produtos, foi possível averiguar que os produtos possuem grande similaridade, mas um valor maior que 0,8 podia ser necessário.
  
Também foi realizado um teste com kmeans usando dessimilaridade, o que resultou em um aumento da qualidade da métrica silhouette quando considerados 2 grandes grupos indo esse valor para 0,79.

[5_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_kmeans](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/423db2d8c490fb9cd9ad1c2c25d2b02ee2b1c672/5_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_kmeans.ipynb)

### Clusterização usando distância cosseno e Spectral

Nessa verificação foi usado Spectral clusters com distância cosseno avaliado pela silhouette. O melhor valor de silhouette foi 0,15 com dois 2 grupos.

[6_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_spectral](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/55491825f30e0b38384e4a1684f3c1208a26025e/6_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_spectral.ipynb)

### Clusterização usando agrupamento hierárquico, similaridade cosseno e threshold

Nessa verificação foi escolhido o método agrupamento hierárquico devido a ele subagrupar os itens. A similaridade cosseno mostrou-se melhor nos resultados anteriores e foi escolhida como medida de distância.
Como a silhouette não se mostrou apropriada para encontrar os clusters buscados, optou-se por uma estratégia que fosse dos itens para os grupos, sendo escolhido o valor arbitrário de distância de ligação(distance_threshold).

Nessa tentantiva com a distância de ligação de 1,5 foi possível identificar prováveis grupos de produtos que são iguais ou semelhantes. Observou-se também que esse valor pode ser melhorado.

[7_Estrat%C3%A9gia_Agrupamento_hier%C3%A1rquico_usando_cosine_similarity](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/ab1193fa94f2e7a52141e2ff1cc6c28b4b591bc9/7_Estrat%C3%A9gia_Agrupamento_hier%C3%A1rquico_usando_cosine_similarity.ipynb)

## Resultado

Para a identificação de produtos semelhantes, a seleção de técnicas que mostrou-se mais promissora usando embedding de 1024 com o modelo mxbai-embed-large fatores, foi Agrupamento hierárquico, com similaridade cosseno e definindo uma distância de agrupamento baixa.
