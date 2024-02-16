# Explore o Machine Learning Automatizado no Microsoft Azure
## ***Resumo do Projeto - Bootcamp Microsoft Azure AI Fundamentals - DIO***
Criação de um modelo preditivo em machine learling utilizando o Microsoft Azure detalhando o passo a passo feito para chegar ao resultado final.
Neste exercício, você usará o recurso de aprendizado de máquina automatizado no Azure Machine Learning para treinar e avaliar um modelo de aprendizado de máquina. Em seguida, você implantará e testará o modelo treinado.

[Link para o meu Modelo:]()  

## ***Passo 1 - Crie um espaço de trabalho do Azure Machine Learning***  

Para utilizar o Azure Machine Learning, é necessário aprovisionar um espaço de trabalho do Azure Machine Learning na sua subscrição do Azure. Depois, você poderá usar o estúdio Azure Machine Learning para trabalhar com os recursos do seu workspace.

> ! **Dica** : *se você já tiver um espaço de trabalho do Azure Machine Learning, poderá usá-lo e pular para a próxima tarefa.*

1. Entre no portal do Azure usando [https://portal.azure.com](https://portal.azure.com) usando suas credenciais da Microsoft. É essencial cadastrar um cartão de crédito para conseguir se registrar e utilizar a plataforma ( será feita uma cobrança de 1$ e será extornada apenas para fins de validação)

2. Selecione **+ Criar um recurso** , pesquise Machine Learning e crie um novo recurso **do Azure Machine Learning com as seguintes configurações:**
   
  + **Assinatura** : sua assinatura do Azure .
  + **Grupo de recursos** : Crie ou selecione um grupo de recursos .
  + **Nome** : Insira um nome exclusivo para seu espaço de trabalho .
  + **Região** : Selecione a região geográfica mais próxima .
  + **Conta de armazenamento** : observe a nova conta de armazenamento padrão que será criada para seu espaço de trabalho .
  + **Cofre de chaves** : Observe o novo cofre de chaves padrão que será criado para seu espaço de trabalho .
  + **Insights de aplicativo** : observe o novo recurso padrão de insights de aplicativo que será criado para seu espaço de trabalho .
  + **Registro de contêiner** : Nenhum ( um será criado automaticamente na primeira vez que você implantar um modelo em um contêiner ).

3. Selecione **Revisar + criar** e selecione **Criar** . Aguarde a criação do seu espaço de trabalho (pode demorar alguns minutos) e, em seguida, vá para o recurso implantado.

4. Selecione **Launch Studio** (ou abra uma nova guia do navegador e navegue até [https://ml.azure.com](https://ml.azure.com/) e entre no Azure Machine Learning Studio usando sua conta da Microsoft). Feche todas as mensagens exibidas.

5. No estúdio Azure Machine Learning, você deverá ver seu espaço de trabalho recém-criado. Caso contrário, selecione **Todos os espaços de trabalho** no menu à esquerda e selecione o espaço de trabalho que você acabou de criar.

## ***Passo 2 - Use aprendizado de máquina automatizado para treinar um modelo***

O aprendizado de máquina automatizado permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Neste exercício, você usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguel de bicicletas esperado em um determinado dia, com base em características sazonais e meteorológicas.

> ***Citação : Os dados usados ​​neste exercício são derivados da [Capital Bikeshare](https://capitalbikeshare.com/system-data) e são usados ​​de acordo com o [contrato de licença](https://ride.capitalbikeshare.com/data-license-agreement) de dados publicado .***

No [Azure Machine Learning Studio](https://ml.azure.com/) , veja a página **Automated ML (em Authoring)**.

Crie um novo trabalho de ML automatizado com as seguintes configurações, usando Next conforme necessário para avançar pela interface do usuário:

### ***Configurações básicas :***

+ **Nome do trabalho** : mslearn-bike-automl
+ **Novo nome do experimento** : mslearn-bike-rental
+ **Descrição** : Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
+ **Marcadores** : nenhum


### ***Tipo de tarefa e dados :***

+ **Selecione o tipo de tarefa** : Regressão
+ **Selecionar conjunto de dados** : crie um novo conjunto de dados com as seguintes configurações:

  + **Tipo de dados** :

    + **Nome** : aluguel de bicicletas
    + **Descrição** : dados históricos de aluguel de bicicletas
    + **Tipo** : Tabular
      
+ **Fonte de dados** :
    + Selecione Dos arquivos da web

+ **URL da Web** :
    
    + **URL da Web** : [https//aka.ms/bike-rentals]()
    + **Ignorar validação de dados** : não selecionar
 
+ **Configurações :**
    + **Formato de arquivo** : Delimitado
    + **Delimitador** : Vírgula
    + **Codificação** : UTF-8
    + **Cabeçalhos de coluna** : apenas o primeiro arquivo possui cabeçalhos
    + **Pular linhas** : Nenhum
    + **O conjunto de dados contém dados multilinhas** : não selecione

+ **Esquema** :

    + Incluir todas as colunas exceto **Caminho**
    + Revise os tipos detectados automaticamente
      
Selecione **Criar** . Após a criação do conjunto de dados, selecione o conjunto de dados **de aluguel de bicicletas** para continuar a enviar o trabalho de ML automatizado.


### ***Configurações de tarefa :***

  + **Tipo de tarefa** : Regressão
  + **Conjunto de dados** : aluguel de bicicletas
  + **Coluna de destino** : Aluguéis (inteiro)
  + **Configurações adicionais** :

    + **Métrica primária** : raiz do erro quadrático médio normalizado
    + **Explique o melhor modelo** : Não selecionado
    + **Usar todos os modelos suportados** : Desmarcado . Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
    + **Modelos permitidos** : Selecione apenas **RandomForest** e **LightGBM** — normalmente você gostaria de tentar o máximo possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.

+ **Limites** : expanda esta seção
  + **Máximo de testes** : 3
  + **Máximo de testes** simultâneos : 3
  + **Máximo de nós** : 3
  + **Limite de pontuação da métrica** : 0,085 (*para que, se um modelo atingir uma pontuação da métrica de erro quadrático médio normalizado de 0,085 ou menos, o trabalho termina.*)
  + **Tempo limite** : 15
  + **Tempo limite de iteração** : 15
  + **Habilitar rescisão antecipada** : selecionado

+ **Validação e teste :**
   + **Tipo de validação** : divisão de validação de trem
   + **Porcentagem de dados de validação** : 10
   + **Conjunto de dados de teste** : Nenhum

### ***Calcular :***

  + **Selecione o tipo de computação** : sem servidor 
  + **Tipo de máquina virtual** : CPU
  + **Camada de máquina virtual** : Dedicada
  + **Tamanho da máquina virtual** : Standard_DS3_V2*
  + **Número de instâncias** : 1

* *Se a sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.*

3. **Envie o trabalho de treinamento.** Ele inicia **automaticamente**.

4. **Espere o trabalho terminar.** Pode demorar um pouco – agora pode ser um bom momento para uma pausa para o café! ***(Forte e SEM açucar como um bom programador TRUE hehe)***

## ***Avalie o melhor modelo***

Quando o trabalho automatizado de aprendizado de máquina for concluído, você poderá revisar o melhor modelo treinado.

  1. Na guia **Visão geral** do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do modelo. Captura de tela do melhor resumo do modelo do trabalho automatizado de aprendizado de máquina com uma caixa ao redor do nome do algoritmo.
     ![print azure](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/use-automated-machine-learning/complete-run.png)

> *** ! Observação Você poderá ver uma mensagem com o status “Aviso: pontuação de saída especificada pelo usuário alcançada…”. Esta é uma mensagem esperada. Continue para a próxima etapa.***

Selecione o texto em *Nome do algoritmo* do melhor modelo para visualizar seus detalhes.

Selecione a guia **Métricas** e selecione os gráficos **residuais** e **predito_true** se eles ainda não estiverem selecionados.

Revise os gráficos que mostram o desempenho do modelo. O gráfico **de resíduos** mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. O gráfico **predito_true** compara os valores previstos com os valores verdadeiros.

## ***Implantar e testar o modelo***

1. Na guia **Modelo** do melhor modelo treinado pelo seu trabalho automatizado de machine learning, selecione **Implantar** e use a opção de **serviço Web** para implantar o modelo com as seguintes configurações:
   + **Nome** : prever-aluguéis
   + **Descrição** : Prever aluguel de bicicletas
   + **Tipo de computação** : Instância de Contêiner do Azure
   + **Habilitar autenticação** : selecionado

2. Aguarde o início da implantação – isso pode levar alguns segundos. O **status de implantação** do endpoint de **previsão de aluguel** será indicado na parte principal da página como *Running* .

3. Aguarde até que o **status da implantação** mude para Succeeded . Isso pode levar de 5 a 10 minutos.


## ***Testar o serviço implantado***
Agora você pode testar seu serviço implantado.

1. No estúdio Azure Machine Learning, no menu esquerdo, selecione Endpoints e abra o ponto final em tempo real de **previsão de alugueres**.

2. Na página do **endpoint** em tempo real de previsão de aluguel, visualize a guia Teste.

3. No painel **Dados de entrada para testar o endpoint** , substitua o modelo *JSON* pelos seguintes dados de entrada:
````
 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }
````
4. Clique no botão **Testar** .

5. Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada - semelhante a este:
```
 {
   "Results": [
     444.27799000000000
   ]
 }
```
O painel de teste pegou os dados de entrada e usou o modelo treinado para retornar o número previsto de aluguéis.

Vamos revisar o que você fez. Você usou um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número de alugueres de bicicletas esperados num determinado dia, com base em características sazonais e meteorológicas .

## ***Limpar***
O serviço web que você criou está hospedado em uma *instância de contêiner do Azure* . Se não pretender experimentá-lo ainda mais, deverá eliminar o ponto final para evitar acumular utilização desnecessária do Azure.

  1. [No estúdio Azure Machine Learning](https://ml.azure.com/) , na guia **Endpoints** , selecione o ponto de extremidade de **previsão de aluguel** . Em seguida, selecione **Excluir** e confirme que deseja excluir o endpoint.
    
     Excluir sua computação garante que sua assinatura não será cobrada por recursos de computação. No entanto, será cobrada uma pequena quantia pelo armazenamento de dados, desde que o espaço de trabalho do Azure Machine Learning exista na sua assinatura. Se tiver 
     terminado de explorar o Azure Machine Learning, poderá eliminar o espaço de trabalho Azure Machine Learning e os recursos associados.

Para excluir seu espaço de trabalho:

  1. No [portal Azure](https://portal.azure.com/) , na página **Grupos de recursos** , abra o grupo de recursos que especificou ao criar o seu espaço de trabalho Azure Machine Learning.
  2. Clique em **Excluir grupo de recursos** , digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir** .

