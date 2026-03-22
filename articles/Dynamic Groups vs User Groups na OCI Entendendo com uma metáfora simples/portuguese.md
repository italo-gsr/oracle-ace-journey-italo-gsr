# Dynamic Groups vs User Groups na OCI: Entendendo com uma Metáfora Simples

Cloud computing trouxe muitas vantagens, mas também trouxe um desafio importante: **controle de acesso**.

* Quem pode acessar recursos?
* Quem pode criar serviços?
* E mais importante: quem ou o que está executando ações dentro da cloud?

Na Oracle Cloud Infrastructure (OCI), existem dois conceitos fundamentais para resolver isso:

* **User Groups**
* **Dynamic Groups**

Embora pareçam semelhantes, eles resolvem problemas diferentes.

Para entender isso de forma simples, vamos usar uma metáfora.

---

## 🏢 Imagine uma Empresa

Pense na OCI como um prédio corporativo.

Dentro desse prédio existem dois tipos de “atores”:

* 👨‍💼 Funcionários
* 🤖 Máquinas e sistemas automatizados

Os funcionários entram no prédio usando crachás pessoais.

Já as máquinas — como servidores ou sistemas automatizados — também precisam realizar tarefas dentro da empresa, mas não são pessoas.

Esse é exatamente o cenário que acontece na OCI.

---

## 👥 Users e Groups: Organizando Pessoas

Na Oracle Cloud Infrastructure, um **User** representa uma pessoa que pode acessar o ambiente.

### Exemplos:

* Engenheiros DevOps
* Desenvolvedores
* Administradores de cloud

Gerenciar permissões individualmente para cada usuário seria ineficiente.

Por isso usamos **Groups**.

Um Group funciona como um departamento dentro da empresa.

Em vez de atribuir permissões para cada pessoa individualmente, atribuímos permissões para o grupo.

### Exemplo de policy:

```text id="7zv1di"
Allow group Developers to manage instance-family in compartment Dev
```

👉 Na prática:
“Todos os membros do grupo Developers podem gerenciar instâncias naquele compartment.”

---

## ⚠️ O Problema: Recursos Também Precisam de Permissões

Agora imagine outro cenário.

Uma aplicação rodando em uma **Compute Instance** precisa acessar o **Object Storage** para armazenar arquivos.

Mas existe um problema:

> Essa aplicação não é um usuário.
> Ela é um recurso da cloud.

Então como dar permissão para ela acessar outro serviço?

É aqui que entram os **Dynamic Groups**.

---

## ⚙️ Dynamic Groups: Permissões para Recursos

Na Oracle Cloud Infrastructure, um **Dynamic Group** permite que recursos da própria cloud recebam permissões.

### Exemplos de recursos:

* Compute Instances
* OKE Nodes
* Functions

### Voltando à metáfora:

Se cada máquina tivesse um crachá com credenciais, alguém poderia roubá-lo e acessar áreas restritas.

Agora imagine um modelo mais seguro:

> “Qualquer máquina que esteja na sala de servidores pode acessar o armazenamento central.”

A autorização passa a ser baseada em **regras**, não em credenciais armazenadas.

👉 É exatamente isso que os Dynamic Groups fazem.

---

## 🧩 Como os Dynamic Groups Funcionam

Dynamic Groups utilizam **regras baseadas em atributos** para identificar recursos.

### Exemplo de regra:

```text id="0l8s2p"
All {instance.compartment.id = 'ocid1.compartment.oc1...'}
```

👉 Significado:
“Todas as instâncias dentro desse compartment fazem parte do Dynamic Group.”

### Depois criamos uma policy:

```text id="s9w1mj"
Allow dynamic-group AppServers to read buckets in compartment Data
```

Agora qualquer instância que satisfaça a regra pode acessar o Object Storage — **sem necessidade de credenciais**.

---

## 🏗️ Exemplo Real

### Arquitetura:

* Compute Instance executando uma aplicação
* Object Storage armazenando arquivos

### Fluxo:

**1️⃣ Criar um Dynamic Group**

```text id="7xy68u"
All {instance.compartment.id = '<compartment_id>'}
```

**2️⃣ Criar uma policy**

```text id="3f02j1"
Allow dynamic-group AppServers to manage objects in compartment Storage
```

### Resultado:

* ✅ Sem usuários
* ✅ Sem chaves expostas
* ✅ Sem credenciais hardcoded

---

## 📌 Quando Usar Cada Um

### Use **Groups** quando:

* Pessoas precisam acessar a OCI
* Login no console
* Uso de CLI ou API

### Use **Dynamic Groups** quando:

* Recursos precisam acessar outros serviços
* Automação
* Integração entre serviços

---

## 🧠 Regra para Nunca Esquecer

Se você lembrar apenas de uma coisa:

> **Users entram na cloud**
> **Resources trabalham dentro da cloud**

Portanto:

* 👤 Users → Groups
* ⚙️ Resources → Dynamic Groups

---

## 📚 Referências

* Dynamic Groups (Documentação Oracle)
  https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managingdynamicgroups.htm

* IAM Policies Overview
  https://docs.oracle.com/en-us/iaas/Content/Identity/policieshow/Policy_Basics.htm

* Managing Groups
  https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managinggroups.htm

---

⭐ *Este artigo faz parte da série “Cloud explicada com metáforas do mundo real”.*
