title: Vocês precisam conhecer as namedtuples
date: 2021-04-27 16:00
author: Rodrigo B. Medeiros
category: Articles

![image][main_image]

# __Vocês precisam conhecer as namedtuples__
  
Mesmo quem começou a desenvolver em python faz pouco tempo (me incluo nessa categoria), tem familiariade com algumas estruturas de dados que são muito presentes na linguagem, como as listas, os dicionários, os sets e as tuples. Diria que conhecendo bem essas estruturas, é possível resolver uma grande variedade de problemas, onde cada estrutura tem suas peculiaridades. Uma comparação muito comum é entre listas e tuples, onde a principal diferença é que as listas são mutáveis, ou seja permitem modificações após sua criação, e as tuples são imutáveis, ou seja não permitem modificações após sua criação. Com relação as similaridades, listas e tuples aceitam elementos de tipos diferentes em uma mesma instância, tem os elementos acessados via índice e são usadas para agrupar informação.
Passei algum tempo utilizando essas estruturas e elas sempre funcionaram muito bem para mim. Porém há alguns meses, fazendo alguns desafios de código pela plataforma [pybites][pybites_link] me deparei com algumas novas estruturas presentes no módulo __collections__, que são basicamente subclasses das estruturas básicas com algumas funcionalidades especificas adicionadas. Dentre elas, tem uma em especial que passei a utilizar em praticamente todos os meus códigos e é sobre ela que vou falar um pouco mais hoje. São as __namedtuples__.

Neste artigo vamos ver:

- O que são namedtuples.
- Como criar uma namedtuple.  
- Como acessar elementos de uma namedtuple.  
- Como converter uma namedtuple em um dicionário (e vice-versa).
- Como utilizar namedtuples para criar classes derivadas.


## __O que são namedtuples?__

Namedtuple é uma subclasse da classe tuple com a diferença básica de terem campos nomeados. Ou seja, além de podermos acessar os elementos através do índice e iterando entre eles, podemos acessá-los na forma de atributos nomeados o que torna o código muito mais limpo e legível. Vamos ver agora como criar e desenvolver com essa estrutura.

## __Criando uma namedtuple__

Para criar uma namedtuple primeiramente precisamos importa-la do módulo collections.

```python
from collection import namedtuple
```

Com a possibilidade de criar namedtuples podemos agora análisar as principais diferenças entre elas e as tuples, para isso vou utilizar como exemplo o cadastro de um cliente de um banco. Para definir o cliente, preciso do __nome completo__, __agencia__ e __conta__. Como esses dados são imutáveis, é natural pensarmos em armazena-los em uma tuple. Como será feito a seguir:

```python
nome = 'Rodrigo Bernardo Medeiros'
agencia = '8475'
conta = '09350-9'

# CRIANDO UMA TUPLE
# 1° Forma de definção de tuples
# - Chamando a key word tuple e passando uma coleção, uma lista por exemplo.
cliente = tuple([nome, agencia, conta])

# 2° Forma de definição de tuples
# - Simplesmente passando os valores entre parentesis separados por vírgula.
cliente = (nome, agencia, conta)

# CRIANDO UMA NAMEDTUPLE
# Definição de uma namedtuple
# 1° Criação de uma instância namedtuple
Cliente = namedtuple('Cliente', ['nome', 'agencia', 'conta'])
cliente = Cliente(nome, agencia, conta)
```

O processo de criação de uma namedtuple difere da criação de uma tuple com um passo adicional, primeiro criamos uma namedtuple com as características específicas do problema a ser resolvido, no caso uma namedtuple chamada __Cliente__, com os atributos __nome__, __agencia__ e __conta__. Em um segundo momento criamos uma instância da namedtuple Cliente onde de fato vamos definir cada um dos campos para cada cliente específico do banco. Com isso temos então um objeto imutável, derivado de uma tuple e com seus elementos sendo acessados através de atributos.

## __Acessando elementos de uma namedtuple__

A principal vantagem de uma namedtuple é poder acessar os elementos através de campos nomeados, o que torna o código muito mais limpo e legível e neste caso podemos acessar os elementos também através de índices como mostrado a seguir:

```python

# ACESSANDO ELEMENTOS VIA NOME DOS ATRIBUTOS
cliente.nome # > Retorna o nome Rodrigo Bernardo Medeiros
cliente.agencia # > Retorna a agencia 8475
cliente.conta # > Retorna a conta 09350-9

# ACESSANDO ELEMENTOS VIA ÍNDICES
cliente[0] # > Retorna o nome Rodrigo Bernardo Medeiros
cliente[1] # > Retorna a agencia 8475
cliente[2] # > Retorna a conta 09350-9

# ITERANDO SOBRE ATRIBUTOS
for att in cliente: 

    # PRIMEIRO LOOP RETORNA O NOME
    # SEGUNDO LOOP RETORNA A AGENCIA
    # TERCEIRO LOOP RETORNA A CONTA   
```

A estrutura das namedtuples são ideais para armazenar informação de modo organizado, tendo toda a estrutura de implementação das tuples com o fator adicional da nomeação dos campos. No exemplo acima, pensando no que está armazenado, ao fazer o acesso através dos índices, podemos pensar que cada índice seria um cliente, o que dificultaria o entendimento do código já que na verdade cada campo representa uma característica de um cliente.

## __Mais sobre namedtuples__

- __Converão namedtuple x dicionário.__

Podemos facilmente converter as namedtuples em dicionários através do código a seguir.

```python
cliente._asdict()

# Retorna um dicionário onde cada campo nomeado se transforma em uma chave
{
    'nome': 'Rodrigo Bernardo Medeiros'
    'agencia': '8475'
    'conta': '09350-9'
}
```

- __Conversão dicionário x namedtuple__

Assim como a conversão namedtuple em dicionário, podemos facilmente fazer o inverso.

```python
cliente_info = {
    'nome': 'Rodrigo Bernardo Medeiros'
    'agencia': '8475'
    'conta': '09350-9'
}

# Há o desempacotamento de informações associando os valores aos campos nomeados da namedtuple.
cliente = Cliente(**cliente_info)
cliente.nome # > Retorna o nome Rodrigo Bernardo Medeiros
cliente.agencia # > Retorna a agencia 8475
cliente.conta # > Retorna a conta 09350-9 
```

- __Obtendo o nome dos campos__

É possível obter o nome dos campos definidos na criação da namedtuple, essa informação pode ser utilizada para criar novas named tuples por exemplo.

```python
cliente._fields

# Retorna os campos listados em uma tuple. 
('nome', 'agencia', 'conta')
```

- __Criando classes derivadas de namedtuples__

Podemos criar classes que herdam os atributos de uma namedtuple, com isso podemos construir métodos em uma classe considerando os atrbutos nomeados na definição da namedtuple.

```python
Cliente = namedtuple('Cliente', ['nome', 'agencia', 'conta'])

class ClientInformationGenerator(Cliente):

    def mount_information(self):

        return self.nome + self.agencia + self.conta
```

Neste exemplo, a classe __ClienteInformationGenerator__ herda os atributos da namedtuple, com isso ao definir o método __mount_information__, posso acessar a informação através dos nomes definidos previamente.

## __Conclusão__

Vimos neste artigo todo o necessário para entender e começar a utilizar as namedtuples, e já digo que é um caminho sem volta, ao começar utilizar vocês vão identificar cada vez mais pontos em seus códigos onde uma namedtuple pode encaixar bem (acreditem, aconteceu comigo). Vale a pena também explorar a [documentação][namedtuples_link], por lá vocês podem encontrar mais informação e exemplos das namedtuples e das outras estruturas de dados contidas no módulo collection. No mais, foi um prazer escrever mais um post principalmente sobre esta estrutura de dados que tenho utilizado muito.

Obrigado pela leitura.

Um grande abraço e até a próxima.

__Rodrigo B. Medeiros__

[main_image]:images/PythonNamedTuple.jpeg
[pybites_link]:https://pybit.es/
[namedtuples_link]: https://docs.python.org/3/library/collections.html#namedtuple-factory-function-for-tuples-with-named-fields

img[alt="image"] { 
  max-width:  20px; 
  display: block;
}