# Métodos de identificação automática de dados de estimativa de precipitação de satélite incosistentes

#### Aluno: Hugo Bernardo Barros Torraca (https://github.com/link_do_github)
#### Orientadora: [Nome Sobrenome](https://github.com/link_do_github).
---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

---

### Resumo

A estimativa da precipitação observada em uma área ou bacia hidrográfica é um importante dado para diversos usos e aplicações. Contudo, a baixa densidade de postos pluviométricos em grande parte do Brasil dificulta a quantificação dessa variável. Para mitigar esse problema diversos usuários têm utilizado estimativas de satélite ou combinações entre precipitação de postos e satélites. Porém, essas estimativas possuem diversas imprecisões e muitas vezes superestimam o volume de chuva. Para facilitar a identificação de possíveis dados estimados espúrios esse trabalho propõe o uso de técnicas de classificação automática que indiquem dados suspeitos para posterior análise manual de um meteorologista. Os resultados indicam que os métodos conseguem sinalizar quase todos os dados espúrios com valores de Recall superiores a 0,95, no entanto eles também apresentam muitos falsos positivos com precisões entre 0,3 e 0,25.  

### 1. Introdução

A precipitação é uma as variáveis meteorológicas mais utilizadas para os mais diversos fins no Brasil e sua correta mensuração é fundamental. Para o setor elétrico a precipitação observada é um insumo fundamental para realizar diagnóstico das bacias hidrográficas e auxiliar na previsão de vazão dos pontos interesses.  Para garantir a qualidade desse dado os detentores de concessões de usinas hidrelétricas são obrigados a instalar estações pluviométricas ao longo da bacia hidrográfica, além disso outros órgãos também possuem postos nessas áreas o que contribuir para o adensamento da rede de pluviográfos. 
Contudo, a expansão do setor elétrico para a região Norte do País trouxe um desafio para a quantificação da chuva nessas bacias devido a diversos fatores dentre os quais podemos destacar:  o grande tamanho das bacias hidrográficas nessa região, grandes extensões da bacia serem pouco povoadas ou áreas de floresta que impedem a instalação e manutenção de estações e parte da área das bacias estar em outros Países.  Para mitigar essa baixa qualidade o setor elétrico tem mesclado os dados de postos com estimativas de precipitação por satélite que calculam a precipitação com base em parâmetros das nuvens que estão sobre a região.  Porém, a estimativa de satélite nessa região tem apresentado desvios, principalmente superestimando o volume precipitado em alguns momentos. A causa desse viés ainda não é clara e esse comportando não possui padrão definido o que torna difícil estimar uma equação de remoção de viés. 
Dessa forma, a equipe meteorológica do Operador Nacional do Sistema Elétrico (ONS) tem que analisar manualmente dados suspeitos para que possam corrigi-los. Essa atividade caso fosse realizada para todos os locais de interesse demoraria diversas horas e impactaria os demais processos da Organização. É necessário então a criação de uma ferramenta automática que faça uma triagem dos dados para que as equipes atuem apenas nos dados que possuam características suspeitas.  
Esse tipo de separação de dados pode se considerado um problema de classificação aonde os dados suspeitos são categorizados como um grupo e os dados não suspeitos em outro.  Para realizar isso existem diversos algoritmos com diferentes abordagens e esse trabalho tem como objetivo estudar diferentes métodos de classificação para descobrir quais métodos possuem melhor resultado para a essa aplicação.

### 2. Modelagem

2.1 Dados de entrada

Para a criação da massa de dados foi utilizada a precipitação diária de diferentes locais das regiões Norte, Nordeste e Centro-Oeste que tinham sido previamente consistidos para uma atividade anterior do ONS de calibração de modelos de previsão de vazão totalizando 69.914 registros. Foram considerados como dados inconsistentes os dados dessa base no quais a média da bacia dos dados combinados de satélite e postos era superior à média pura dos postos e que houve uma redução de ao menos 15 mm entre o dado original da combinação com o dado consistido pela a equipe.  Essa escolha de considerar apenas os dados onde houve redução foi feita pois a superestimativa da precipitação, além de ser mais comum, é também o que causa maior impacto nos processos do ONS, principalmente a previsão de vazão. Dessa forma, dos 69914 registros 662 foram classificados como inconsistentes e o restante como consistente.

Para a criação do dataset foram consideradas as seguintes colunas:
Estacao:	Estação do ano 
Valor:		precipitação estimada pela combinação de satélite e postos (mm/dia)
Valor _D-1:	precipitação consistida do dia anterior (mm/dia)
m_postos:	média apenas dos postos pluviométricos (mm/dia)
Area:		Área da bacia hidrográfica ( km²)
P100:		Maior precipitação já registrada na bacia naquela estação do ano (mm/dia)
P95: 		Precipitação superada apenas 5% das vezes na bacia naquela estação do ano (mm/dia)
P90:		Precipitação superada apenas 10% das vezes na bacia naquela estação do ano (mm/dia)

2.2 tratamento dos dados

O grande desbalanceamento dos dados pode causar problemas na divisão de treino e teste mesmo utilizando técnicas que mantenham a proporção de ambas as classes igual na calibração e teste. Para contornar isso cada modelo foi calibração e testado com 50 divisões (80-20) diferentes utilizando a função train_test_split do pacote sklearn cada um com uma semente própria, após isos cada conjunto de calibração passou de normatização utilizando a função  MinMaxScaler() do sklearn que normatiza cada coluna no range de 0 a 1. 
Além disso, foram realizados 2 testes, no primeiro foi utilizada a base toda para classificação já na segunda a base foi pre processada e todos dados no quais a coluna valor era menor 15 e/ou que a coluna valor era menor que a coluna m_postos foram retirados da massa de calibração e na base de testes foram já automaticamente classificados como não consistentes. 


2.3 Modelos


### 3. Resultados

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

### 4. Conclusões

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

---

Matrícula: 123.456.789

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
