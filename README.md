# NER-spaCy
Este é um exemplo de como usar SpaCy para realizar a extração de informações de contratos. Neste exemplo, vamos usar NER (Reconhecimento de Entidades Nomeadas) para identificar informações relevantes em contratos, como nomes de partes, datas, etc.

# Requisitos
Para usar este exemplo, você precisará ter os seguintes pacotes instalados:

spaCy
pt_core_news_lg
pandas
tqdm
Você também precisará de um conjunto de dados de treinamento etiquetado para treinar seu modelo NER e instalar o pacote de configuração de acordo com o modelo escolhido para o treinamento.

# Instalando os pacotes
Use os seguintes comandos para instalar os pacotes:
!python -m spacy download pt_core_news_lg
Os demais pacotes, como o spacy, pandas e tqdm já vem instalados por padrão no python, sendo assim basta importá-los. Caso não estejam instalados, basta dar um pip install com o nome do pacote.

# Carregando o modelo 
Neste trabalho, foi utilizado o modelo em português do spaCy, entretanto, o framework disponibiliza vários outros idiomas para serem treinados.
Para carregar o modelo, utilize o comando load:

nlp = pt_core_news_lg.load()

# Etiquetando os dados de treinamento
Para treinar, é necessário um conjunto de daos etiquetados. Esses dados, são armazenados em uma lista composta pelo texto a ser treinado, posição inicial do termo a ser reconecido, posição final do termo a ser reconhecido e por fim a entidade que ela de ser reconhecida.

[ train = [
          ("COMURG, denominado pregoeiro",{"entities":[(0,6,"Organização")]}),
          ("75.990.524/0001-76",{"entities":[(0,21,"CNPJ")]}),
          ("25/12/2022",{"entities":[(0,10,"Data")]}),
          
        ]
 ]
# Treinamento das novas entidades

Com os dados etiquetados, podemos passar essas informações para o treino. Para isso, passamos a lista com os dados rotulados para o código que irá transformar seus dados de treinamento em um arquivo .spacy que pode ser usado para treinar seu modelo NER.

# Carregando os arquivos de configuração

Com o modelo treinado, precisamos carregar os arquivos de configuração com as características empregadas no modelo treinado, tais como: língua escolhida para treinamento, uso de CPU ou GPU, somente NER ou mais coisas e assim por diante.
Nesse repositório, já se encontra os arquivos de configuração para a reconhecimento em Português, uso de CPU e usando somente NER. Caso deseje outras configurações, será necessário gerar outro arquivo através do site: https://spacy.io/usage/training#quickstart

Para carregar os arquivos, execute os comandos seguindo a ordem:
!python -m spacy init fill-config base_config.cfg config.cfg
!python -m spacy train config.cfg --output ./output --paths.train ./train.spacy --paths.dev ./train.spacy

# Extraindo textos de contratos PDF 

Como fonte de dados, utilizamos alguns contratos PDF para extrair as informações e carregar no modelo já treinado.
Para extrair informações de um PDF, você precisa instalar o PyPDF2 utilizando o seguinte comando:

!pip install PyPDF2

Feito isso, carregue o arquivo PDF e extraia as informações das páginas e as agrupe em uma única linha. Esse processo, é realizado no arquivo NER Contratos.

# Carregando o modelo treinado

Com o modelo treinado e o texto extraído do PDF, podemos carregar o melhor modelo e passar o texto para identificação das entidades.

Utilize o comando load passando como parâmetro ./output/model-best para pegar o melhor modelo treinado:

nlp = spacy.load(r"./output/model-best")

Feito isso, é só passar o texto extraído do PDF para a variável em que carregou o modelo treinado:

doc = nlp1(pdf_text) # Faz a leitura de amostras de texto retiradas do PDF

doc.ents

# Conclusão
Este é um exemplo básico de como usar SpaCy para realizar a extração de informações de contratos. Você pode ajustar o modelo NER e o código de acordo com suas necessidades específicas.
