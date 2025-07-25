🚀 Chains no LangChain
📌 O que são Chains?
Chains são fluxos de trabalho que conectam um ou mais componentes do LangChain — como LLMs (modelos de linguagem), prompts, memória, ferramentas etc. — para executar tarefas mais elaboradas do que uma simples chamada de modelo.

🧱 Principais Tipos de Chains
1. 🔗 LLMChain — Execução básica com prompt
Utiliza um prompt e um modelo de linguagem (LLM). Ideal para tarefas simples como perguntas e respostas, reformulações, análises, etc.

bach
Copiar
Editar
from langchain.chains import LLMChain
from langchain.prompts import PromptTemplate

# Define o template do prompt com uma variável chamada 'pais'
prompt = PromptTemplate.from_template("Qual é a capital de {pais}?")

# Cria a chain passando o modelo e o prompt
chain = LLMChain(llm=chat, prompt=prompt)

# Executa a chain com a variável preenchida
resposta = chain.run("Brasil")
print(resposta)
2. 🧠 ConversationChain — Memória de conversas
Permite interações com histórico, lembrando o que foi dito anteriormente. Ideal para chatbots ou assistentes.

bach
Copiar
Editar
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

# Cria a memória para armazenar o histórico da conversa
memory = ConversationBufferMemory()

# Cria a chain com o modelo e a memória
chain = ConversationChain(llm=chat, memory=memory)

# Envia uma mensagem para iniciar a conversa
resposta = chain.predict(input="Oi, tudo bem?")
print(resposta)
3. 🧩 SequentialChain — Execução em sequência
Executa várias chains ou funções em ordem, passando o resultado de uma como entrada para a próxima.

bach
Copiar
Editar
from langchain.chains import SequentialChain
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# Prompt 1: Gera um nome para um guerreiro medieval
prompt1 = PromptTemplate.from_template("Crie um nome para um guerreiro medieval.")
chain1 = LLMChain(llm=chat, prompt=prompt1, output_key="nome")

# Prompt 2: Cria uma história com base no nome gerado
prompt2 = PromptTemplate.from_template("Escreva uma história com o guerreiro chamado {nome}.")
chain2 = LLMChain(llm=chat, prompt=prompt2, output_key="historia")

# Cria a chain sequencial com entrada e saída definidas
chain = SequentialChain(
    chains=[chain1, chain2],
    input_variables=[],
    output_variables=["historia"],
    verbose=True
)

# Executa a chain
resultado = chain.run()
print(resultado)
4. 🔄 SimpleSequentialChain — Versão simplificada
É uma versão mais simples do SequentialChain, usada quando o output de uma chain deve ir diretamente como input da próxima (sem nomes de variáveis intermediárias).

bach
Copiar
Editar
from langchain.chains import SimpleSequentialChain
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# Prompt 1: Gera um título
prompt1 = PromptTemplate.from_template("Gere um título de artigo sobre tecnologia.")
chain1 = LLMChain(llm=chat, prompt=prompt1)

# Prompt 2: Gera o conteúdo com base no título
prompt2 = PromptTemplate.from_template("Escreva um parágrafo sobre: {input}")
chain2 = LLMChain(llm=chat, prompt=prompt2)

# Encadeia as duas chains
simple_chain = SimpleSequentialChain(chains=[chain1, chain2], verbose=True)

# Executa o fluxo completo
resultado = simple_chain.run("Inteligência Artificial")
print(resultado)
5. 🔍 TransformChain — Pré-processamento ou ajustes
Executa uma função customizada entre chains, útil para transformar entradas ou saídas. Exemplo básico com manipulação de string:

bach
Copiar
Editar
from langchain.chains import TransformChain

# Função que transforma o texto para maiúsculas
def transforma_upper(inputs: dict) -> dict:
    texto = inputs["texto"]
    return {"texto": texto.upper()}

# Define a chain de transformação
transform_chain = TransformChain(
    input_variables=["texto"],
    output_variables=["texto"],
    transform=transforma_upper
)

# Executa a transformação
print(transform_chain.run({"texto": "olá mundo"}))
✅ Conclusão
Chains são essenciais no LangChain para orquestrar interações complexas com modelos de linguagem. Combinando prompts, memória, funções personalizadas e modelos, você pode construir fluxos de trabalho poderosos para:

Chatbots com memória

Geração de conteúdo em múltiplas etapas

Agentes que tomam decisões

Automação de tarefas de NLP
