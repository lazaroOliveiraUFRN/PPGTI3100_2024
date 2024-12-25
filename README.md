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

1. Foi realizada a leitura da lista de produtos e selecionado uma amostra de 5000 produtos.

<div style="margin-left: 40px;">

[1_Seleção_de_Lista_Amostra_de_Produtos.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/439ee5eaed79a5fd34276b8e674b0eddc56367e8/1_Sele%C3%A7%C3%A3o_de_Lista_Amostra_de_Produtos.ipynb)

<div>

2. Para embedding foi utilizado o modelo [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). Este modelo tem 1-24 fatores ou componentes. Também foi realizado uma classificação dos elementos da descrição dos produtos usando o modelo [llama3.1](https://ollama.com/library/llama3.1) e exportadado em um array de json.

A execução em GPU demora 2 segundos por requisição. A execução em CPU demora mais de 3 minutos cada.

[2_Meta_Llama_3_70B.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/439ee5eaed79a5fd34276b8e674b0eddc56367e8/2_Meta_Llama_3_70B.ipynb)

3. A primeira tentativa de agrupamento foi usando kmeans e avaliando a silhouette. O valores de silhouette (0,025 .. -0,05) indicaram que os produtos parecidos não formam grupos pequenos e identificáveis facilmente usando esta técnica.

[3_Clusteriza%C3%A7%C3%A3o_dos_produtos_k_means.ipynb](https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/fa994209f1d46f8583e035677217886662dd9e6b/3_Clusteriza%C3%A7%C3%A3o_dos_produtos_k_means.ipynb)

4. ???


5. Nesta verificação foi testado a distância cosseno entre os produtos. Inicialmente não foi usando nenhuma técnica, apenas se quando a distância cosseno era próxima os produtos parecidos tinham alta similaridade. Após essa verificação foi tentando agrupar usando kmeans e a distância cosseno. Como resultado os produtos semelhantes foram agrupados. O uso de kmeans verificado com silhouette melhorou em termos de resultados mas não foi 

(https://github.com/lazaroOliveiraUFRN/PPGTI3100_2024/blob/423db2d8c490fb9cd9ad1c2c25d2b02ee2b1c672/5_Estrat%C3%A9gia_verificar_apenas_a_dist%C3%A2ncia_cosseno_kmeans.ipynb)


### Resultado

