# cleaner-project

Nesse projeto experimental sobre agentes inteligentes é usada a plataforma NetLogo para simular uma versão do mundo aspirador de pó para dois tipos de agentes:

* Agente reativo simples
* Agente baseado em modelo

## Agente reativo simples

Possui sensores para verificar a existência de sujeira no local atual e nos locais adjacentes (cima, baixo, direita, esquerda) e se move nessas mesmas direções.

### Ações

O agente possui 3 ações principais que são realizadas nessa sequência repetidas vezes até que o mundo esteja limpo:

#### look-around

* Busca por sujeira no **patch** atual, caso haja ele chama a ação **clean-up**. 
* Busca por sujeira nos **patchs** adjacentes e define seu **heading** a partir da primeira sujeira encontrada ao redor. Se ele não encontrar nenhuma sujeira, seu **heading** será o mesmo de antes da ação ser chamada.

#### clean-up

* Define o **patch** atual como limpo.
* Retira a sujeira do **patch** atual.

#### move

* Anda 1 na direção do seu **heading** (caso não seja uma parede).
* Define seu próximo **heading** de forma aleatória.

## Agente baseado em modelo

Possui os mesmo sensores e anda nas mesmas direções que o **agente reativo simples**, porém, diferente do agente anterior, esse agente possui memórias para guardar os locais onde ele idenficou sujeira mas não limpou (**_where-dirt_**) e para guardar os locais que ele passou perto mas não visitou  (**_non-visited_**). Além disso, ele também possui um método para identificar sua localização atual (**_current_**) baseado no local onde ele começou, que é definido como ponto [0, 0] para ele.

### Ações

O agente possui 3 ações principais que são realizadas nessa sequência repetidas vezes até que o mundo esteja limpo: 

#### look-around

* Define o **patch** atual como visitado e retira **_current_** da memória **_non-visited_**.
* Busca por sujeira no **patch** atual, caso haja ele chama a ação **clean-up** e retira **_current_** da memória **_where-dirt_**.
* Busca por sujeira nos **patchs** adjacentes e define seu **heading** a partir da primeira sujeira encontrada ao redor. 
* Se ele não encontrar nenhuma sujeira ao redor, ele irá buscar se há algo na memória **_where-dirt_** e definir seu **heading** para a primeira posição da memória:
* Caso são haja nada em **_where-dirt_**, ele fará o mesmo para **_non-visited_**.
* Se as duas memórias estiverem vazias, seu **heading** será o mesmo de antes da ação ser chamada.

#### clean-up

* Define o **patch** atual como limpo.
* Retira a sujeira do **patch** atual.

#### move

* Anda 1 na direção do seu **heading** (caso não seja uma parede).
* Atualiza o **_current_** através da função **update-coo**.
* Define seu próximo **heading** de forma aleatória.

### Reporters

Esse agente também possui **reporters** para auxiliar no cálculo da sua posição no mundo.

#### direction-by-memory

* Recebe uma **string** com o tipo da memória e retorna a direção para qual o agente deve ir para chegar ao objetivo.

#### update-coo

* Retorna uma **list** contendo a coordenada do **patch** a frente baseado em seu **_current_**.

#### smaller-distance

* É usada como reporter do **sort-by** para reorganizar as memórias pela coordenada mais próxima na frente.

## Como usar

As interfaces de entrada dos dois agentes são idênticas, contendo as mesmas variáveis globais que serão definidas pelo usuário.

![image](https://user-images.githubusercontent.com/59544679/140555092-bc1dd7c3-799b-4a32-a2b3-724d694d1aca.png)

### Obrigatórias

#### px py

* Elas defirãoo tamanho do mundo, no lado x (**px**) e no lado y (**py**)

#### psize

* É o tamanho que você deseja para o **patch**.

#### number-of-dirt

* Será a quantidade de sujeiras que aparecerão de forma aleatória no mapa.

  **(Caso a variável _dirt-local_ seja definida o número de sujeiras será definido por ela e _number-of-dirt_ não é necessária)**

### Opcionais

#### dirt-local

* Deve ser uma lista contendo a posição de cada sujeira desejada.
  **Ex: [ [-2 4] [3 1] [0 -4] [ 1 2] [3 3] ]**
  
#### cleaner-local

* Define o local inicial do aspirador. Caso não seja definida, a posição do aspirador será aleatória.

## Em caso dúvidas

Entrar em contato: jamillep94@gmail.com
