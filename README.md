# Banco de Dados Vetorial com FAISS e RetrievalQA

Este projeto traz um script em python que cria um banco de dados com FAISS e um pipeline de perguntas e respostas com RetrievalQA. O seu uso permite uma busca eficiente de informações por similaridade semântica com base em um texto fornecido.

Para usa-lo, deve-se carregar um arquivo com informações e posteriormente utilizar o modelo para responder perguntas com base no texto fornecido.

## Instalando o langchain_community

Se ainda não tiver instalado, instale o langchain_community através do pip install.

## Importação de arquivos

Neste exemplo usamos um artigo sobre Inteligência Artificial na Educação. Contudo, é possível carregar qualquer arquivo pdf sobre qualquer assunto. O código mostra uma forma de carregar arquivos PDF pelo langchain_community, mas é possível adicionar outros formatos de arquivos, substituindo o PyPDFLoader. 

Veja a documentação oficial:
https://reference.langchain.com/python/langchain-community/document_loaders.

## Importando um arquivo PDF

Importe o carregador de documento que deseja utilizar. Neste caso, utilizou-se o PyPDFLoader para carregar um arquivo no formato PDF.

O Arquivo "ArtigoIAEducacao.pdf" é meramente ilustrativo e pode ser substituído.

## Criando os chunks

Em seguida, o texto é dividido em chunks. Neste exemplo, criamos chunks de até 1000 caracteres e overlap de 50. Esses parâmetros podem ser ajustados para melhorar o retorno do modelo. Chunks maiores podem estender o resultado da query e menores podem trazer respostas insuficientes. Assim, é interessante realizar testes e escolher o tamanho mais adequado.

## Instalação do FAISS
Para criar o banco de dados vetorial com FAISS faz-se necessário instala-lo. Existem duas versões: CPU ou GPU. Instale GPU se houver um disponível. Se não, instale CPU.

## Importação do modelo para criação dos embeddings e FAISS

Importe o OllamaEmbeddings e o FAISS, como mostra o código. Neste exemplo utilizamos o modelo mxbai-embed-large para gerar os embeddings. Você deve ter o modelo previamente instalado na máquina. A página desse modelo pode ser vista em https://ollama.com/library/mxbai-embed-large e o comando para instalá-lo é ollama pull mxbai-embed-large.

A criação do banco de dados vetorial pode demorar alguns minutos a depender do tamanho do texto e da capacidade da máquina.

## Busca por chunks similares (opcional)

No código foi inserido um trecho que faz uma busca manual do chunk mais similar a uma query fornecida. Serve para verificar se o código está funcionando bem e se é capaz de trazer uma boa resposta. Por meio deste, é possível ajustar os tamanhos dos chunks para ter respostas mais adequadas. Muitas vezes o primeiro chunk não é o melhor para responder a query. Dessa forma, é possível verificar o segundo chunk mais próximo ajustando o docs[0] para docs[1] e assim sucessivamente.

## Criando o RetrievalQA

Para criar o RetrievalQA você deve importar o OllamaLLM e o RetrievalQA como mostra o código. O OllamaLLM carrega o modelo que será utilizado para responder as query inseridas. 

Neste exemplo utilizamos o llama3.2:latest, mas outros modelos podem ser utilizados. O modelo a ser utilizado deve estar instalado na máquina. A página do modelo usado neste exemplo é https://ollama.com/library/llama3.2. A sua instalação se dá pelo comando ollama run llama3.2. 

## Usando o modelo

O ultimo script usa o RetrievalQA criado para responder a query_retrieval. É possível substituir o questionamento de exemplo por outros. 

## Exemplos de perguntas feitas ao RetrievalQA e suas respostas

A seguir é possível verificar cinco exemplos de perguntas e respostas extraídas do texto através do modelo:

##### 1º EXEMPLO: QA Response: {'query': 'O que é inteligência Artificial?',

'result': 'A pergunta mais elemental!\n\nInteligência Artificial (IA) é um campo da informática e da ciência da computação que visa criar sistemas capazes de realizar tarefas que normalmente requerem inteligência humana, como aprendizado, raciocínio, resolução de problemas, entre outras.\n\nEm outras palavras, a Inteligência Artificial busca programar máquinas para que elas possam pensar, aprender e agir de forma mais inteligente do que as máquinas tradicionais. Isso inclui a capacidade de processar grandes quantidades de dados, aprender com eles e se adaptar às novas situações.\n\nMas é importante notar que, até o momento, a Inteligência Artificial ainda não alcançou um verdadeiro "inteligente" ou "super-ínteligente", como alguns cientistas e filósofos imaginam. A IA é uma ferramenta poderosa, mas ela tem limitações e é frequentemente usada para realizar tarefas específicas, como processamento de linguagem natural, reconhecimento de imagem, ou análise de dados.\n\nE agora, sobre o ChatGPT...'}

##### 2º EXEMPLO:  QA Response: {'query': 'Qual é o tema do artigo?',

'result': 'O tema do artigo é a Inteligência Artificial com foco no ChatGPT na Educação.'}

##### 3º EXEMPLO: QA Response: {'query': 'De que forma o artigo menciona que chatgpt pode ser usado na educação?', 

'result': 'O artigo menciona várias formas pelas quais o ChatGPT pode ser usado na educação, incluindo:\n\n* Gerar conteúdos criativos, como histórias, poemas e canções personalizadas;\n* Apoiar o planejamento de aulas e criar recursos didáticos, como unidades didáticas, rubricas e questionários;\n* Responder a perguntas e manter diálogos de maneira convincente;\n* Criar discursos e gerar textos coerentes em diversos idiomas;\n* Avaliar a compreensão dos alunos com avaliações personalizadas.\nAlém disso, o artigo também destaca as potencialidades do ChatGPT para causar uma disrupção na educação, incluindo:\n\n* Fornecer oportunidades de aprimoramento de experiências de aprendizado e ensino;\n* Ajudar indivíduos em todos os níveis de Educação.\nNo entanto, o artigo também menciona limitações do ChatGPT para uso na educação, como:\n\n* Falta de raciocínio lógico;\n* Precisão;\n* Desatualização.\n\nÉ importante notar que o artigo não fornece uma resposta definitiva sobre a forma como o ChatGPT deve ser usado na educação, mas sim apresenta uma análise crítica das possibilidades e limitações do modelo.'}

##### 4º EXEMPLO: QA Response: {'query': 'De acordo com artigo, quais as limitações do uso do chatgpt na educação?',

'result': 'De acordo com os artigos mencionados, algumas das limitações do uso do ChatGPT na Educação incluem:\n\n1. Falha de raciocínio lógico\n2. Respostas imprecisas\n3. Enviesamento (ou seja, a tendência a fornecer respostas que são mais favoráveis à sua própria opinião ou interesses do que objetivas e imparciais)\n4. Estímulo ao plágio (ou seja, a tendência de os alunos a copiar textos gerados pelo ChatGPT em vez de desenvolver suas próprias ideias e habilidades)\n5. Inibição da criatividade dos alunos\n\nÉ importante notar que essas limitações foram identificadas como potenciais desafios para o uso eficaz do ChatGPT na Educação, e que é necessário investir em estratégias pedagógicas cuidadosas para superá-las.'}

##### 5ª EXEMPLO: QA Response: {'query': 'Quais aspectos éticos o artigo menciona sobre uso do chatgpt na educação?', 

'result': 'O artigo menciona que as principais questões éticas relacionadas ao uso do ChatGPT na Educação incluem:\n\n*   Estímulo ao plágio\n*   Inibição da criatividade dos alunos\n\nEssas questões éticas são preocupações latentes que exigem estratégias pedagógicas cuidadosas para garantir uma integração segura e eficaz do ChatGPT na prática educacional.'}


