---
theme: apple-basic
title: Workorders
mdc: true
duration: 120min
drawings:
  persist: false
layout: intro-image-right
image: '/public/imgs/dummies.png'
transition: slide-left
addons:
  - slidev-addon-tldraw
---

# Workorders
## 17/12/2025

---

# Âmbitos

- ID4.1 - ORAC
- ID4.3 - ORAP
- ID 5 e 6 - HPs 
- ID 1 e 2 - Estado Construido, Drop Patchcord, atualização estados de portos de PDOs

---
layout: two-cols
---

# Tipos

* <span v-mark.circle.orange="1">IncreaseNetwork</span>
* DensifyFTTHAccess
* ChangeNetworkReorganize
* DeployFTTHNetwork
* DeployFTTHNetworkExtra
* DeployDarkFiberNetwork
* DeployFTTHNetworkAdapt

::right::
<div v-click="2">

# Estados

* <span v-mark.green="3">Registado</span>
* Construido
* Aprovado
* Reprocessar_Ficheiro
* Pendente
* Nao_Aprovado
* Concluido
* <span v-mark.red="4">Fechado</span>
* <span v-mark.red="4">Cancelado</span>

</div>

---
layout: two-cols-header
---

# Operações 

::left::

<div class="pr-3">

## Criação

```xml
<ns2:item>
  <ns2:value ns1:type="xsd:string">123456</ns2:value>
  <ns2:characteristic>WO_ID</ns2:characteristic>
</ns2:item>
<ns2:item>
  <ns2:value ns1:type="xsd:string">IncreaseNetwork</ns2:value>
  <ns2:characteristic>TIPO_WO</ns2:characteristic> 
</ns2:item>
<ns2:item>
  <ns2:value ns1:type="xsd:string">34AV01GP030</ns2:value>
  <ns2:characteristic>CODIGO_ZONA</ns2:characteristic>
</ns2:item>                 
<ns2:item>
  <ns2:value ns1:type="xsd:string">Desc</ns2:value>
  <ns2:characteristic>DESCRICAO_PROJETO</ns2:characteristic>
</ns2:item>
<ns2:item>
  <ns2:value ns1:type="xsd:string">APF</ns2:value>
  <ns2:characteristic>PROPRIETARIO_REDE</ns2:characteristic>
</ns2:item>
```

</div>

::right::

<div v-click class="pl-3">

## Atualização de Estados

```xml
<ns2:value ns1:type="xsd:string">123456</ns2:value>
<ns2:characteristic>WO_ID</ns2:characteristic>
...
<ns2:value ns1:type="xsd:string">Aprovado</ns2:value>
<ns2:characteristic>ESTADO</ns2:characteristic>
...
<ns2:value ns1:type="xsd:string">01/10/2020 01:00:00</ns2:value>
<ns2:characteristic>DATA_ESTADO</ns2:characteristic>
...
<ns2:value ns1:type="xsd:string">NPU_STRING</ns2:value>
<ns2:characteristic>NPU</ns2:characteristic>
...
<ns2:value ns1:type="xsd:string">Inutil</ns2:value>
<ns2:characteristic>OBSERVACOES</ns2:characteristic>
...   
<ns2:value ns1:type="xsd:string">WO_12378.csv</ns2:value>
<ns2:characteristic>NOME_FICHEIRO</ns2:characteristic>
...
```
</div>

---
layout: two-cols
---

# Regras Estados

- Impossível transitar dos estados terminais
- Só é permitida uma passagem pelo estado Concluido
- WO a Concluido só pode transitar para Reprocessar_Ficheiro ou Fechado
- Obrigatório passar por Concluido para chegar a Fechado
- Impossível passar para Fechado se existirem jobs de geração ORAC/P agendados

::right::

<div v-click>

# Processamentos Associados

- Aprovado - Geração ficheiros ORAC/P
- Reprocessar_Ficheiro - Geração ficheiros ORAC/P
- Construido - Atualização de estados de portos de PDOs
- Concluido - Geração ficheiros ORAC/P
- Cancelado - Cancela jobs de geração ORAC/P pendentes

</div>

---
layout: image
image: /public/imgs/diagram_Estados_invis.png
backgroundSize: contain
---

# <span class="text-black">Diagrama Estados</span>

--- 
layout: section
---

# ORAC/P
## ID4.1 e ID4.3

--- 

# O que é?

- Outros operadores precisam de passar cabo por infraestruturas PT Comunicações
- Sistema de "aluguer"
- Cenários diferentes são taxados de formas distintas com base em regras definidas
- O foco são troços de cabos
- ORAC - Subterrâneo 
- ORAP - Aéreo

--- 
layout: two-cols-header
---

# Tipos de PIs

::left::

## ORAC

- CO - PI tipo Central
- CV - PI tipo Câmara
- RAG 
- RAE
- RAT
- RAP

::right::

<div v-click>

## ORAP

- PO - Poste PT Comunicações
- PP - Poste outro proprietário
- EP - Pose com transição para Edifício
- TPP - Poste com Tubo de subida PT Comunicações
- TPB - Poste com Tubo de subida FastFiber

</div>

---
layout: two-cols-header
---

# Estrutura do ficheiro

::left::

- <span v-mark.green="1">workOrderID</span>
- codigoCAOP
- localidade
- <span v-mark.green="1">tipoPI_A/B</span>
- ACL_A/B
- <span v-mark.green="1">referenciaPI_A/B</span>
- PePartilhadoA/B
- tipoPL_A/B
- tipoWSMPL_A/B
- referenciaPL_A/B
- fabricantePL_A/B

::right::

- modeloPL_A/B
- existePL_A/B
- folga_A/B
- quadricula_A/B
- referenciaCabo
- tipoCabo
- comprimentoTracado
- tipoConduta
- quadriculas_intermedias
- seccao_rede

---

# Conceitos

- Âmbito / Projeto Misto:
    - 1 - ORAC
    - 2 - ORAP
    - 3 - Misto
    - 0 - Sem dados
- Domínio - Associado ao nome ficheiro enviado no pedido de atualização de estado WO
    - Se contiver "P_" -> ORAP
    - Caso contrário ORAC
- NPU - código de identificação único por request, usado em gerações de ficheiro originadas através de alteração de estado para ir na notificação a identificar a "origem" do ficheiro enviado. É null para outras ações que despoletam geração de ficheiro.

--- 
layout: two-cols-header
---

# Geração ficheiro

::left::
## Ações
- Imediato - Alteração de Estado WO
- Geração 5am (desde que já tenha passado por um Estado que gera ficheiro e não esteja em Concluido):
    - Adicionar / Remover cabos
    - Reassociar Cabo / Reassociar PI
    - Editar tipo cabo
    - Alteração ao Nome do Cabo (Fonte, nº do cabo, ER final)
    - Alterar Modelo / Fabricante de um PL
    - Alteração de WO para PLs ou cabos

::right::

<div v-click>

## Regras

- Estado Aprovado / Geração 5am - gera com base no âmbito
- Estado Concluido - Verifica que o âmbito foi alterado anteriormente e gera com base no(s) âmbitos perdidos + âmbito atual
- Estado Reprocessar_Ficheiro - gera com base no domínio (nome ficheiro)
- Alguns estados geram ficheiro vazio (apenas cabeçalho)
</div>

---
layout: image
image: /public/imgs/gerarFicheiroWO.jpg
backgroundSize: contain
---

# <span class="text-black">Diagrama Geração</span>

--- 

# Notificação

- As notificações contêm informação:
    - nome do ficheiro
    - WO
    - nome ficheiro plantas
    - record_count - nº de linhas do ficheiro sem contar com cabeçalho
    - projeto misto - sim ou não
    - status - apenas se for estado concluido
- Só são enviadas se o ficheiro existir (não for null)

---

# Pedido de atualização ORAC/P

- Serve para preenchimento da tab ORAC/P

```xml
<ns10:item ns1:type="ns10:CaboOrax">
  <ns10:referenciaCabo>referenciaCabo</ns10:referenciaCabo>
  <ns10:refPedido>refPedido</ns10:refPedido>
  <ns10:refOracInt>refOracInt</ns10:refOracInt>
  <ns10:estadoPedido>estadoPedido</ns10:estadoPedido>                       
  <ns10:dataRecepcao>02/10/2020 10:10:00</ns10:dataRecepcao>
  <ns10:dataEstado>02/10/2020 10:13:00</ns10:dataEstado>
  <ns10:tipoPedido>AV</ns10:tipoPedido>
</ns10:item>
```

---
layout: section
---

# Notas para DEVs

---

# Tabelas de suporte

<table class="w-full border border-gray-900 text-left">
    <thead>
        <tr class="bg-gray-300">
            <th class="p-2 border">Tabela</th>
            <th class="p-2 border">Finalidade</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="p-2 border">GIR_TP_WO</td>
            <td class="p-2 border">Cadastro de WOs</td>
        </tr>
        <tr>
            <td class="p-2 border">GIR_TP_WO_HIST</td>
            <td class="p-2 border">Histórico de transições de estado de WO</td>
        </tr>
        <tr>
            <td class="p-2 border">GIR_TP_WO_STATE</td>
            <td class="p-2 border">Catálogo de Estados WO</td>
        </tr>
        <tr>
            <td class="p-2 border">GIR_TP_WO_TYPE</td>
            <td class="p-2 border">Catálogo de Tipos WO</td>
        </tr>
        <tr>
            <td class="p-2 border">WO_PROCESS_ORAC_AUX</td>
            <td class="p-2 border">Tabela de controlo de geração de ficheiro para ORAC</td>
        </tr>
        <tr>
            <td class="p-2 border">WO_PROCESS_ORAP_AUX</td>
            <td class="p-2 border">Tabela de controlo de geração de ficheiro para ORAP</td>
        </tr>
        <tr>
            <td class="p-2 border">NS_SCH_USER_JOB</td>
            <td class="p-2 border">Tabelas de jobs agendados</td>
        </tr>
  </tbody>
</table>

---

# Métodos BD

<table class="w-full border border-gray-900 text-left">
    <thead>
        <tr class="bg-gray-300">
            <th class="p-2 border">Método</th>
            <th class="p-2 border">Finalidade</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="p-2 border">PCK_WO_T.createWO</td>
            <td class="p-2 border">Criar WO</td>
        </tr>
        <tr>
            <td class="p-2 border">PCK_WO_T.updateStateWO</td>
            <td class="p-2 border">Atualizar Estado WO</td>
        </tr>
        <tr>
            <td class="p-2 border">PCK_WO_T.getWOinfoOraC</td>
            <td class="p-2 border">Método para gerar ficheiros ORAC</td>
        </tr>
        <tr>
            <td class="p-2 border">PCK_WO_T.getWOinfoOraP</td>
            <td class="p-2 border">Método para gerar ficheiros ORAP</td>
        </tr>
        <tr>
            <td class="p-2 border">PCK_WO_T.get_ambitoWO</td>
            <td class="p-2 border">Calcular ambito</td>
        </tr>
        <tr>
            <td class="p-2 border">PCK_WO_T.get_DominioWO</td>
            <td class="p-2 border">Calcular dominio</td>
        </tr>
        <tr>
            <td class="p-2 border">PCK_WO_T.call_ns_sch_user_job</td>
            <td class="p-2 border">Criar job para geração ficheiro</td>
        </tr>
  </tbody>
</table>

---

# Metodos JAVA

<table class="w-full border border-gray-900 text-left">
    <thead>
        <tr class="bg-gray-300">
            <th class="p-2 border">Método</th>
            <th class="p-2 border">Finalidade</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="p-2 border">WO_PROJECTControllerEJB</td>
            <td class="p-2 border">Classe responsável</td>
        </tr>
        <tr>
            <td class="p-2 border">WO_PROJECTControllerEJB.generateDataFileAndNotify</td>
            <td class="p-2 border">Metodo principal chamado no job de geração</td>
        </tr>
        <tr>
            <td class="p-2 border">WO_PROJECTControllerEJB.generateFile</td>
            <td class="p-2 border">Contem as regras de que estados geram ficheiros vazios</td>
        </tr>
        <tr>
            <td class="p-2 border">WO_PROJECTControllerEJB.generateAndUploadFiles</td>
            <td class="p-2 border">Gera o ficheiro ORAC/P e Plantas</td>
        </tr>
        <tr>
            <td class="p-2 border">WO_PROJECTControllerEJB.sendNotification</td>
            <td class="p-2 border">Método responsável pelo envio da notificação</td>
        </tr>
  </tbody>
</table>

---

# Notas finais

- Job tem 3 tentativas se der algum erro durante a execução do WO_PROJECTControllerEJB.generateDataFileAndNotify. A tentativa atual é guardada numa flag na bd e é resetada quando a geração corre sem problemas
- Um job de geração é constituido por 2 ou 3 params dependendo do que origina a geração:
    - Ações que despoletam o job automaticamente criam com 2 params: NPU e WO_ID -> NPU é null neste caso
    - UpdateStateWO cria o job com 3 params: NPU, WO_ID, TESTING -> Testing é enviado apenas em casos de DEBUG, este param serve para não ser feito o upload do ficheiro gerado para o ETF nem para enviar notifs

