# Chains no LangChain

No LangChain, *chains* são fluxos de trabalho que conectam diferentes componentes, como modelos de linguagem (LLMs), agentes, ferramentas e outros recursos, para realizar tarefas complexas. A seguir, exploraremos os principais tipos de *chains*, com exemplos e explicações detalhadas.

---

## 1. Simple Chain

O *simple chain* é o tipo básico de cadeia, onde uma entrada é passada diretamente para um modelo de linguagem ou outra ferramenta, e o resultado é retornado.

### Exemplo:

```python
from langchain.chains import LLMChain
from langchain.prompts import PromptTemplate
from langchain.llms import OpenAI

# Criação do modelo de linguagem
llm = OpenAI(temperature=0.7)

# Definição do prompt
prompt = PromptTemplate(input_variables=["name"], template="Qual é o seu nome, {name}?")

# Criação do LLMChain
chain = LLMChain(llm=llm, prompt=prompt)

# Execução da cadeia com entrada
output = chain.run(name="Cícero")
print(output)  # Saída: Qual é o seu nome, Cícero?
#```bach

---

## 2. MapReduce Chain

O *MapReduce chain* é utilizado para processar dados em paralelo, realizando um mapeamento das entradas e depois uma redução para combinar os resultados.

### Exemplo:

```python
from langchain.chains import MapReduceChain
from langchain.llms import OpenAI

# Função de mapeamento e redução
def map_fn(text):
    return f"Processando: {text}"

def reduce_fn(results):
    return "\n".join(results)

# Criando a cadeia MapReduce
chain = MapReduceChain(map_fn=map_fn, reduce_fn=reduce_fn, llm=OpenAI(temperature=0.7))

# Entrada para a cadeia
output = chain.run(["Entrada 1", "Entrada 2", "Entrada 3"])
print(output)  # Processando: Entrada 1\nProcessando: Entrada 2\nProcessando: Entrada 3
#```bach

---

## 3. Agent Chain

O *Agent Chain* envolve agentes que podem tomar decisões dinâmicas com base nas entradas e interagir com ferramentas externas ou dados. Este é um tipo de cadeia flexível e adaptável.

### Exemplo:

```python
from langchain.agents import initialize_agent
from langchain.agents import Tool
from langchain.llms import OpenAI
from langchain.tools import DuckDuckGoSearchResults

# Definindo a ferramenta de busca
search = DuckDuckGoSearchResults()

# Definindo o agente
llm = OpenAI(temperature=0.7)
tools = [Tool(name="Pesquisa", func=search.run, description="Busca na web")]
agent = initialize_agent(tools, llm, agent_type="zero-shot-react-description", verbose=True)

# Executando o agente
output = agent.run("Qual é o melhor filme de 2025?")
print(output)
#```bach

---

## 4. LLMChain com Memory

A *memory* no LangChain permite que a cadeia mantenha o estado entre as execuções, o que é útil para interações contínuas, como em chatbots ou assistentes virtuais.

### Exemplo:

```python
from langchain.chains import LLMChain
from langchain.memory import ConversationBufferMemory
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate

# Criando o modelo de linguagem
llm = OpenAI(temperature=0.7)

# Definindo o prompt
prompt = PromptTemplate(input_variables=["message"], template="Qual é a sua opinião sobre {message}?")

# Criando a memória de conversação
memory = ConversationBufferMemory(memory_key="chat_history")

# Criando o LLMChain com memória
chain = LLMChain(llm=llm, prompt=prompt, memory=memory)

# Execução da cadeia com entradas e manutenção do estado
output1 = chain.run(message="tecnologia em IA")
output2 = chain.run(message="futuro da computação")
print(output1)
print(output2)
#```bach

---

## 5. SQL Chain

O *SQL Chain* é usado para consultar bancos de dados SQL diretamente a partir do modelo de linguagem, tornando mais fácil interagir com dados estruturados.

### Exemplo:

```python
from langchain.chains import SQLDatabaseChain
from langchain.llms import OpenAI
from langchain.sql_database import SQLDatabase
import sqlite3

# Conexão com banco de dados SQLite
connection = sqlite3.connect(":memory:")
cursor = connection.cursor()
cursor.execute("CREATE TABLE filmes (id INTEGER PRIMARY KEY, nome TEXT)")
cursor.execute("INSERT INTO filmes (nome) VALUES ('Inception')")
connection.commit()

# Criando o banco de dados e o modelo de linguagem
db = SQLDatabase(connection)
llm = OpenAI(temperature=0.7)

# Criando a cadeia SQLDatabaseChain
chain = SQLDatabaseChain(llm=llm, database=db)

# Executando uma consulta via cadeia
output = chain.run("Qual é o nome do filme com o id 1?")
print(output)  # Saída esperada: Inception
#```bach

---

## 6. Text Splitter Chain

O *Text Splitter Chain* é usado para dividir grandes textos em partes menores, o que é útil quando se lida com textos muito grandes que precisam ser processados em partes.

### Exemplo:

```python
from langchain.chains import TextSplitterChain
from langchain.llms import OpenAI
from langchain.text_splitter import RecursiveCharacterTextSplitter

# Criando o modelo de linguagem
llm = OpenAI(temperature=0.7)

# Criando o splitter de texto
text_splitter = RecursiveCharacterTextSplitter(chunk_size=100)

# Criando a cadeia de divisão de texto
chain = TextSplitterChain(llm=llm, text_splitter=text_splitter)

# Entrada para a cadeia
output = chain.run("Texto longo que precisa ser dividido em partes menores.")
print(output)  # Saída com as partes do texto divididas
#```bach

---

## 7. Summarization Chain

O *Summarization Chain* é utilizado para resumir textos longos, condensando as informações mais importantes em um resumo conciso.

### Exemplo:

```python
from langchain.chains import SummarizationChain
from langchain.llms import OpenAI

# Criando o modelo de linguagem
llm = OpenAI(temperature=0.7)

# Criando a cadeia de sumarização
chain = SummarizationChain(llm=llm)

# Entrada para a cadeia
output = chain.run("Texto longo que precisa ser resumido.")
print(output)  # Saída: Resumo do texto
#```bach

---

Esses são alguns dos tipos principais de *chains* no LangChain, com exemplos e explicações que demonstram sua aplicação prática. Esses fluxos de trabalho oferecem a flexibilidade para combinar diferentes ferramentas, modelos e interações, permitindo a criação de soluções mais sofisticadas e adaptáveis.
