# Dynamic Groups vs User Groups na OCI: Entendendo com uma metáfora simples

Cloud computing trouxe muitas vantagens, mas também trouxe um desafio importante: controle de acesso.

Quem pode acessar recursos?
Quem pode criar serviços?
E mais importante: quem ou o que está executando ações dentro da cloud?

Na Oracle Cloud Infrastructure (OCI) existem dois conceitos fundamentais para resolver isso:

User Groups
Dynamic Groups
Embora pareçam semelhantes, eles resolvem problemas diferentes.

Para entender isso de forma simples, vamos usar uma metáfora.

---

## Imagine uma empresa
Pense na OCI como um prédio corporativo.

Nesse prédio existem dois tipos de “atores”:

Funcionários
Máquinas e sistemas automáticos
Os funcionários entram no prédio usando crachás pessoais.

Já as máquinas — como servidores ou robôs automatizados — também precisam realizar tarefas dentro da empresa, mas não são pessoas.

Esse é exatamente o cenário que acontece na OCI.

---

## Users e Groups: organizando pessoas
Na Oracle Cloud Infrastructure, um User representa uma pessoa que pode acessar o ambiente.

Exemplos:

Engenheiros DevOps
Desenvolvedores
Administradores cloud
Mas gerenciar permissões de forma nominal seria muito trabalhoso.

Por isso usamos Groups.

Um Group funciona como um departamento da empresa.

Por exemplo:

Press enter or click to view image in full size

Em vez de dar permissões para cada pessoa individualmente, damos permissões para o grupo.

Exemplo de policy:

Allow group Developers to manage instance-family in compartment Dev
Na prática isso significa:

“Todos os membros do grupo Developers podem gerenciar instâncias naquele compartment.”

---

## O Problema: recursos também precisam de permissões
Agora imagine outro cenário.

Uma aplicação rodando em uma Compute Instance precisa acessar o Object Storage para salvar arquivos.

Mas existe um problema:

Essa aplicação não é um usuário.

Ela é um recurso da cloud.

Então como dar permissão para ela acessar outro serviço?

É aqui que entram os Dynamic Groups.

---

## Dynamic Groups: permissões para recursos
Na Oracle Cloud Infrastructure, um Dynamic Group permite que recursos da própria cloud recebam permissões.

Exemplos de recursos que podem usar Dynamic Groups:

Compute Instances
OKE Nodes
Functions
Voltando à metáfora da empresa:

Se cada máquina ou robô tivesse um crachá físico com senha, alguém poderia pegar esse crachá e acessar áreas restritas.

Download the Medium app
Agora imagine um modelo mais seguro.

A segurança do prédio cria uma regra:

“Toda máquina que estiver na sala de servidores pode acessar o arquivo central.”

Ou seja, a autorização acontece com base em regras, não em credenciais individuais armazenadas em cada máquina.

É exatamente esse tipo de lógica que os Dynamic Groups ajudam a implementar na cloud.

---

## Como Dynamic Groups funcionam
Dynamic Groups usam regras baseadas em atributos para identificar recursos.

Por exemplo:

All {instance.compartment.id = 'ocid1.compartment.oc1...'}
Essa regra significa:

“Todas as instâncias dentro desse compartment fazem parte do Dynamic Group.”

Depois disso, criamos uma policy:

Allow dynamic-group AppServers to read buckets in compartment Data
Agora qualquer instância que satisfaça a regra pode acessar o Object Storage, sem a necessidade de credenciais.

---

## Exemplo real de uso
Imagine uma arquitetura comum na Oracle Cloud Infrastructure:

Compute Instance executando uma aplicação
Object Storage armazenando arquivos
Fluxo:

1️⃣ Criamos um Dynamic Group para as instâncias da aplicação

All {instance.compartment.id = '<compartment_id>'}
2️⃣ Criamos uma policy

Allow dynamic-group AppServers to manage objects in compartment Storage
Agora a aplicação pode armazenar arquivos automaticamente.

Sem usuários.
Sem chaves expostas.
Sem credenciais hardcoded.

---

## Quando usar cada um
Use Groups quando:

Pessoas precisam acessar a OCI
Login no console
Uso de CLI ou API
Use Dynamic Groups quando:

Recursos precisam acessar outros serviços
Automação
Integração entre serviços

---

## Regra mental para nunca esquecer
Se você lembrar apenas de uma coisa deste artigo, lembre disso:

Users entram na cloud.

Resources trabalham dentro da cloud.

Por isso:

Users → Groups
Resources → Dynamic Groups
Este artigo faz parte da série “Cloud explicada com metáforas do mundo real”, onde conceitos complexos de cloud são simplificados usando exemplos do dia a dia.

---

## Referências
Se você quiser se aprofundar nos conceitos apresentados neste artigo, estes materiais da documentação oficial da Oracle Cloud Infrastructure são um ótimo ponto de partida:

Dynamic Groups — documentação oficial da Oracle
https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managingdynamicgroups.htm
IAM Policies Overview
https://docs.oracle.com/en-us/iaas/Content/Identity/policieshow/Policy_Basics.htm
Managing Groups
https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managinggroups.htm

### Esses materiais explicam em mais detalhes como funcionam grupos, políticas e identidade de recursos na OCI.

