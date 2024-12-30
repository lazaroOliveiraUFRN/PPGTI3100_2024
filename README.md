# PPGTI3100 - TÓPICOS AVANÇADOS EM INTELIGÊNCIA COMPUTACIONAL 1
## [UFRN](https://www.ufrn.br/) - [IMD](https://www.metropoledigital.ufrn.br/portal/) - [PPGTI](https://sigaa.ufrn.br/sigaa/public/programa/apresentacao.jsf?lc=pt_BR&id=7872)
## Programa de Mestrado em Inteligência Computacional
## Professor: [ELIAS JACOB DE MENEZES NETO](http://www.docente.ufrn.br/elias.jacob)
## Aluno: Lázaro Raimundo de Oliveira


# Verificação de estratégias de identificação produtos semelhantes usando Embeddings 

### Objetivo

Tentar identificar estratégias que permitam identificar produtos semelhantes numa base de preço 

### Motivação

Bases de preço ou banco de preços possuem produtos que são igual ou muito semelhantes com descrições diferentes. Isso dificulta o agrupamento e consequentemente a comparação de preços de produtos. Este problema já tem sido trabalhando por vários autores. Nesse notebooks propõem-se avaliar estratégias baseadas em modelos LLM estado da arte. 

### Passos e técnicas testadas

### Leitura de Produtos

Foi realizada a leitura da lista de produtos e selecionado uma amostra de 5000 produtos.

[1_Seleção_de_Lista_Amostra_de_Produtos.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/439ee5eaed79a5fd34276b8e674b0eddc56367e8/1_Sele%C3%A7%C3%A3o_de_Lista_Amostra_de_Produtos.ipynb)

### Embedding e classificação das partes dos nomes dos produtos

Para embedding foi utilizado o modelo [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). Este modelo tem 1-24 fatores ou componentes. Também foi realizado uma classificação dos elementos da descrição dos produtos usando o modelo [llama3.1](https://ollama.com/library/llama3.1) e exportadado em um array de json.

Para classificação das partes da descrição do produto. a execução em GPU demora 2 segundos por requisição. A execução em CPU demora mais de 3 minutos cada.

Para obtenção dos embeddings a execução total em GPU demorou menos de 5 minutos. 

[2_Meta_Llama_3_70B.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/439ee5eaed79a5fd34276b8e674b0eddc56367e8/2_Meta_Llama_3_70B.ipynb)


### Tentativa de clusterização usando kmeans e métrica silhouette

A primeira tentativa de agrupamento foi usando kmeans e avaliando a silhouette. O valores de silhouette (0,025 .. -0,05) indicaram que os produtos parecidos não formam grupos pequenos e identificáveis facilmente usando esta técnica essa métrica de avaliação.

[3_Clusteriza%C3%A7%C3%A3o_dos_produtos_k_means.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/fa994209f1d46f8583e035677217886662dd9e6b/3_Clusteriza%C3%A7%C3%A3o_dos_produtos_k_means.ipynb)

### Clusterização Usando Agglomerative

Nesta tentantiva foi usando Cluster aglomerativo (Agglomerative Cluster) com a métrica silhouette. Para distância foi considerada a métrica cosseno. O melhor valor de silhouette foi 0,10 indicando que o processo não parece adequado. Também foi construído uma leitura 3D dos 3 maiores componentes que correspondiam a aproximadamente 15% da variabilidade, tendo como resultado visual uma nuvem bem compacta com aparente 2 grupos.

[4_Clusteriza%C3%A7%C3%A3o_dos_produtos_AgglomerativeClustering](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/448212fdc8f6a7c7a8a093b619d2251002740307/4_Clusteriza%C3%A7%C3%A3o_dos_produtos_AgglomerativeClustering.ipynb)

### Verificação direta da distância cosseno

Nesta verificação foi testado a distância cosseno entre os produtos e selecionado os mais próximos. Inicialmente não foi usando nenhuma técnica, apenas se quando a distância cosseno era próxima os produtos parecidos tinham proximidade acima de 0,8. Pela verificação em lista de produtos foi possível verificar que os produtos possuem grande similaridade, mas um valor maior que 0,8 pode ser necessário.
  
Também foi realizado um teste com kmeans usando dessimilaridade, o que resultou num aumento da qualidade do da métrica silhouette quando considerado 2 grandes grupos. O valor subiu para 0,79.

[5_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_kmeans](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/423db2d8c490fb9cd9ad1c2c25d2b02ee2b1c672/5_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_kmeans.ipynb)

### Clusterização usando distância cosseno e Spectral

Nesta verificação foi usado Spectral clusters com distância cosseno avaliado pela silhouette. O melhor valor foi 0,15 com dois 2 grupos.

[6_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_spectral](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/55491825f30e0b38384e4a1684f3c1208a26025e/6_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_spectral.ipynb)

### 

### Resultado

