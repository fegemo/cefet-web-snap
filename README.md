# PhotoSnap

Uma ferramenta para marcar suas fotos! Nesta atividade vamos praticar:

1. diferentes eventos (`mouseover`, `mouseout`, `mousemove`, `input`);
1. colocar e remover classes;
1. definir o conteúdo de um elemento;
1. exercitar _template strings_;
1. recuperar e definir atributos de dados;
1. interagir com vários tipos de campos de dados e de escolha;
1. uso da propriedade CSS `filter`;
1. uso da `FileReader` API trocar a foto (desafio opcional).

![Tela da página PhotnoSnap mostrando um cabeçalho com a logomarca, uma foto grande com duas regiões marcadas nela e vários campos de controle dessas marcações ao lado direito](https://fegemo.github.io/cefet-web/images/photosnap.jpg)


## Atividade

Crie uma página interativa que permite o usuário configurar 2 marcações em uma foto e que mostram um balãozinho com algumas informações quando se passa o mouse sobre elas.

Além da funcionalidade de visualizar as informações das marcações no balão, também deve ser possível definir diferentes propriedades das marcações (repare a seção de controles à direita da imagem), como sua posição, tamanho, formato, título, conteúdo e cor do texto no balãozinho.

Nesta atividade, serão sugeridas formas de implementação, mas fique à vontade implementar a mesma funcionalidade de outra forma.

<details>
  <summary>A estrutura da página...</summary>

  A estrutura HTML do trecho relevante da página para a atividade é o seguinte:

  ```pug
  body
    //...
    main
      //...
      div.linha
        div.foto-anotada
          img
          div.marcacao
          div.marcacao
        div#balaozinho

        section.controles
          //...
          label
            input
          label
            input
          //...
  ```

  Nessa estrutura, há uma `<div class="foto-anotada">` que possui a foto e duas `<div class="marcacao">` representando as regiões onde podemos passar o mouse em cima para obter mais informações. Essas são apresentadas dentro do `<div id="balaozinho">`. E os controles dentro de `<section class="controles">` permitem configurar as informações dessas marcações, dentre outras coisas.
</details>


### Exercício 0: Ocultar/mostrar marcações

Para esquentar: no arquivo `script/controles-marcacao.js` faça com que, ao marcar ou desmarcar o campo `ocultar marcações`, as duas `.marcacao` desapareçam (e não respondam a eventos de mouse).

<details>
  <summary>Sugestão de implementação...</summary>

  Use a classe `marcacoes-ocultas` em um ancestral das marcações (eg, o `body`) e crie uma regra CSS que as deixa invisíveis (selecione as `.marcacoes` que tenham como ancestral alguém com a classe `marcacoes-ocultas`).

  Se reparar o código HTML, verá que o `<input type="checkbox">` inclusive possui como `value` o nome dessa classe que você pode criar:
  ```html
  <label>ocultar marcações: 
    <input type="checkbox" id="visibilidade-das-marcacoes" value="marcacoes-ocultas">
  </label>
  ```

  Veja o FAQ para se lembrar de [como adicionar ou remover classes](#como-adicionar-ou-remover-classes) de um elemento HTML. Também há informação sobre [como alterar a visibilidade de elementos](#como-alterar-a-visibilidade-de-elementos) e [como saber se um checkbox está marcado](#como-saber-se-um-checkbox-esta-marcado).

  Nessa linha, uma estratégia é adicionar um evento de `input` ou `change` no checkbox e, em sua _callback_, inserir ou retirar a classe que é `value` do checkbox (pra não ter que digitar de novo, basta pegar seu `value`). Para saber se deve colocar ou retirar, pode-se verificar se o checkbox está marcado ou não, e chamar a `el.classList.toggle()` de acordo.
</details>


### Exercício 1: Balãozinho com informações da marcação

No arquivo `script/balaozinho.js`, este exercício pede que você configure o `#balaozinho` para que ele:
1. Apareça quando o mouse está sobre uma `.marcacao`
   - Ele deve ter o seguinte conteúdo:
     ```html
     <div id="balaozinho">
       <h2>...título...</h2>
       <p>...conteúdo...</p>
     </div>
     ```
   - O título e conteúdo devem ser preenchidos como os atributos `data-titulo` e `data-conteudo` da `.marcacao`.
     - OBSERVAÇÃO: é uma boa oportunidade para usar _template strings_ ao definir o conteúdo do balão (veja [slides sobre _template strings_][slides-template-strings])
   - A cor do texto do balãozinho deve ser definida como o valor do atributo `data-cor` da `.marcacao`.
     - Veja no FAQ [como alterar o estilo de um elemento](#como-alterar-o-estilo-de-um-elemento) HTML via JavaScript
1. Desapareça quando o mouse sair da `.marcacao`
1. Se movimente com sua posição sendo a mesma do mouse enquanto ele estiver em cima da `.marcacao`

Lembre-se dos [eventos de mouse](#eventos-de-mouse) no FAQ.

<details>
  <summary>Sugestão de implementação</summary>

  - No arquivo `script/balaozinho.js`, crie código para:
    1. Recuperar o `#balaozinho`
    1. Recuperar todas as `.marcacao`
    1. Para cada uma:
       1. Registrar função para evento de "mouse entrou"
          1. Determinar qual das marcações foi alvo do evento
          1. Definir seu conteúdo (FAQ: [como definir o conteúdo de um elemento](#como-definir-o-conteúdo-de-um-elemento)) como os atributos de dados referentes ao título e conteúdo da marcação alvo do evento
          1. Definir a `color` do balãozinho com o valor do `data-cor` da marcação
       1. Registrar função para evento de "mouse saiu"
          1. Simplesmente remover todos os filhos do `#balaozinho`
             - No FAQ tem [como limpar um elemento](#como-limpar-um-elemento)
       1. Registrar função para evento de "mouse movimentou"
          1. Definir as propriedades `left`, `top` do `#balaozinho` de acordo com a posição do mouse
             - O FAQ mostra [como pegar a posição do mouse](#como-pegar-a-posicao-do-mouse)
             - Não se esqueça que `e.pageX` e `e.pageY` são números sem unidade de medida e que você deve concatená-los com `px` quando for definir os valores de `left` e `top`
</details>

<details>
  <summary>Quais estilos do balãozinho...</summary>

  O `#balaozinho` é uma `<div>` posicionada de forma absoluta (para podermos definir exatos (x,y)), e outras propriedades para deixá-lo bonitinho:

  ```css
  #balaozinho {
    position: absolute;
    border: 1px solid silver;
    background-color: rgba(255, 255, 255, .9);
    font-size: 10px;
    padding: 0.5em 1em;
    border-radius: 4px;
    box-shadow: 5px 5px 8px rgba(0, 0, 0, .3);
    pointer-events: none; /* ¹ */
  }

  #balaozinho:empty {  /* ² */
    display: none;
  }
  ```

  Dois detalhes importantes:
  - ¹: queremos que o balão não responda a eventos de mouse para que, quando ele aparecer sob o ponteiro do mouse, a `.marcacao` não lance imediatamente o evento `mouseout` (ponteiro saiu da marcação e entrou no balão), acarretando no `#balaozinho` ficar piscando (comente essa linha e repare).
  - ²: quando o `#balaozinho` estiver com seu conteúdo vazio (ie, `innerHTML === ''`), ele ficará oculto.
</details>

### Megaexercício 2: Selecionar marcação e preencher controles

Em `script/controles-marcacao.js`, torne possível que o usuário selecione¹ uma `.marcacao` clicando nela. A marcação selecionada deve ter a classe `selecionada` adicionada a ela (aí fica com a borda amarelinha).

Uma vez selecionada, preencha os campos de `section.controles` com os respectivos valores:
1. x, y, largura e altura: vêm do `style` da marcação, das propriedades `left, top, width, height`
   - Observação: os valores recuperados do `style` do elemento possuem valor e unidade de medida (eg, '200px'). Contudo, ao definir o `value` dos `input[type="number"]`, deve-se passar apenas o número. Veja [como extrair apenas o número de uma string](#como-extrair-apenas-o-número-de-uma-string) no FAQ.
1. título, conteúdo e cor: vêm dos atributos de dados da `.marcacao`
1. formato da marcação: deve-se verificar as classes que a `.marcacao` possui:
   - Se ela tiver `.formato-oval`, deve-se "checkar" o `input[type="radio"]` cujo `value` é `formato-oval`
   - Senão, checkar o que tem `value="formato-retangular"`

¹Selecionar uma marcação: quando alguma `.marcacao` sofrer um `click`, remova a classe `selecionada` de quem é a marcação atual e insira essa mesma classe naquela que foi alvo do `click`.

<details>
  <summary>Sugestão de implementação...</summary>

  Uma ideia é criar uma função `atualizaControles(marcacaoEl)` que deve ser chamada sempre que uma `.marcacao` é selecionada. Essa função vai preencher os valores de todos os campos informados.

  Dentro dela, para cada controle que deve ser preenchido de acordo com a `.marcacao` selecionada:
  1. Pegar uma referência ao campo (`input`)
  1. Definir seu valor (`value`) para o respectivo atributo ou propriedade da `.marcacao`

  Detalhes importantes:
  - Campos de posição e tamanho: use `parseInt(el.value)`, com `el.style.left|top|width|height` sendo algo como `200px`, para extrair apenas a parte numérica antes de definir o `value` do `input`
  - Campos de conteúdo e cor: pegue o valor dos atributos de dados e use-os diretamente para definir o `value` do `input`
  - Botão de rádio do formato:
    1. Crie uma variável `formato` que conterá a string `formato-oval` ou `formato-retangular` verificando se a `marcacaoEl.classList.contains('formato-oval')` ou não
    1. Selecione o `input` cujo `[value="${formato}"]` e faça-o ser `checked`
</details>


### Megaexercício 3: Controles alteram marcação

De forma análoga ao exercício anterior, no mesmo arquivo `script/controles-marcacao.js`, agora você deve permitir o usuário alterar os controles e isso refletir no estado da `.marcacao.selecionada`.

Os campos que devem alterar o estado da `.marcacao.selecionada` são os mesmos do exercício anterior. Observe que:
1. Campos x, y, largura e altura: devem alterar o `style` da marcação
   - Lembre-se de concatenar o valor numérico do campo com a unidade de medida `px`
1. Campos título, conteúdo e cor: definem atributos de dados da marcação
1. Botão de rádio de formato: remove as classes de formato e insere novamente apenas aquela cujo botão de rádio está "checkado"


<details>
  <summary>Sugestão de implementação...</summary>

  Você pode registrar eventos/_callbacks_ específicos para cada campo, ou ser um pouquinho preguiçoso (tudo bem, tá liberado) e criar apenas uma `atualizaMarcacao(marcacaoEl)` monstrona que será registrada como _callback_ para todos os `input`s e definirá todo o estado da marcação (mesmo que só 1 campo tenha sido alterado).

  Para pegar todos os campos que devem ter o evento do tipo `input` registrado para essa função, você pode usar o `querySelectorAll` com um seletor que pegue "todos os `input` exceto aqueles com atributo `type="checkbox"`, e também os `textarea` (que é só 1). Lembre-se da aula sobre seletores, se quiser fazer um único monstrão desses.
</details>


### Exercício 4: Filtros na foto

Codifique no arquivo `script/controles-foto-anotada.js`.
Para exercitar a propriedade CSS `filter`, permita que o usuário escolha um filtro do `select#filtro-da-foto`. 

Quando o usuário escolher um filtro, simplesmente altere a propriedade `filter` da `.foto-anotada > img` para o valor da opção escolhida. O FAQ mostra [como pegar o valor da opção escolhida em um campo `select`](#como-pegar-o-valor-do-select).

<details>
  <summary>Código HTML pertinente...</summary>

  Repare a estrutura do código HTML do `select`, que nos permite pegar o `value` da `option` escolhida e simplesmente definir a propriedade `style` da `.foto-anotada > img`:
  ```html
  <select id="filtro-da-foto">
    <option value="none">nenhum</option>
    <option value="sepia(1)">tom de cor sépia</option>
    <option value="invert(1)">cores invertidas</option>
    <option value="grayscale(1)">escala de cinza</option>
    <option value="contrast(8)">super contraste</option>
    <option value="blur(3px)">borragem</option>
    <option value="hue-rotate(300deg)">rotação de tonalidade</option>
  </select>
  ```
</details>


### Desafio 1: Trocar a foto por outro arquivo
Há um `<input type="file">` que permite ao usuário escolher um arquivo
de seu computador. Faça com que o usuário possa escolher uma imagem para substituir a foto.


Veja o artigo a seguir e tente identificar um código
nele que faz o que você precisa: deixa usuário escolher um arquivo e
o coloca como uma imagem no lugar da foto dos pokémons.

Referência: https://www.html5rocks.com/en/tutorials/file/dndfiles/


## FAQ

Veja algumas perguntas frequentes ao realizar esta atividade e suas respostas.

### Como adicionar ou remover classes

Veja os [slides sobre alteração de classes][slides-classes]. Em suma:
```js
el.classList.add('nome-da-classe');     // coloca
el.classList.remove('nome-da-classe');  // retira
el.classList.toggle('nome-da-classe');  // coloca ou retira
```

O método `classList.toggle()` retorna um booleano indicando se colocou (`true`) ou se retirou (`false`) a classe do elemento. Por exemplo:
```js
const colocou = el.classList.toggle('nome-da-classe');
if (colocou) {
  console.log('.nome-da-classe foi INSERIDA no elemento');
} else {
  console.log('.nome-da-classe foi REMOVIDA no elemento');
}
```

Além disso, ele aceita um segundo parâmetro, booleano, que o força a inserir (se `true`) ou a retirar (se `false`). Por exemplo:
```js
const inserir = true;
el.classList.toggle('nome-da-classe', inserir);
// vai sempre inserir, mesmo que a classe já esteja lá
```


### Como alterar a visibilidade de elementos

Veja os [slides de alteração de visibilidade][slides-visibilidade]. Dependendo da forma usada, será possível ou não interagir com os elementos (por meio de eventos) mesmo com eles invisíveis. Use a forma mais adequada considerando que o enunciado pede para que não seja possível interagir com as marcações.


### Como saber se um checkbox está marcado

Veja os [slides sobre como interagir com checkbox via JavaScript][slides-interacao-campos-de-dados]. Tendo uma referência a um `<input type="checkbox">`, você pode acessar a propriedade `checked`, assim:

```js
const el = document....;
console.log(el.checked);  // true, se estiver marcado
```

A mesma lógica vale para saber se um certo botão de rádio (ie, `<input type="radio">`) está selecionado.

E, se quiser programaticamente marcar ou desmarcar um checkbox ou radio, vocÊ pode alterar o valor da propriedade `checked` para `true`/`false`, assim:

```js
const el = document....;
el.checked = true; // marcou
```


### Como definir o conteúdo de um elemento

Veja os [slides sobre como definir o conteúdo de um elemento][slides-conteudo] usando a propriedade `innerHTML`.


### Eventos de mouse

Veja os [slides sobre eventos de mouse][slides-eventos-mouse].


### Como alterar o estilo de um elemento

Veja os [slides sobre alteração de estilos via JavaScript][slides-alterar-estilos]. Em suma, usamos a propriedade `style` de um elemento HTML e podemos recuperar ou definir o valor de cada propriedade CSS. Lembre-se que os nomes das propriedades CSS, em JavaScript, devem ser "camelCased". Por exemplo:

```js
const el = document....;
el.style.color = 'white';
el.style.backgroundColor = 'black';
el.style.left = '10px';
el.style.position = 'absolute';
```


### Como limpar um elemento

Se quiser remover todo o conteúdo de um elemento, você pode definir seu `innerHTML` como uma string vazia:

```js
el.innerHTML = '';
```


### Como pegar a posição do mouse

Veja os [slides sobre como pegar a posição do mouse][slides-posicao-mouse] durante um evento de `mousemove`.


### Como extrair apenas o número de uma string

Para extrar apenas os números de um string que começa com números, mas tem algum texto depois, você pode usar:

```js
const numeroInteiro = parseInt(texto);
const numeroReal = parseFloat(texto);
```

Neste exemplo, se `texto === '235.2px'`, o `parseInt` retorna `235` e o `parseFloat` `235.2`. Lembre-se, contudo, que seja inteiro ou real, em JavaScript o único tipo de dados numérico se chama `Number` e é de ponto flutuante com precisão de 64 bits (equivalente ao `double` de C, Java).


### Como pegar o valor do select

Veja os [slides sobre como pegar o `value` da `option` selecionada em um `select`][slides-select]




[slides-classes]: https://fegemo.github.io/cefet-web/classes/js2/#colocando-removendo-classes
[slides-visibilidade]: https://fegemo.github.io/cefet-web/classes/css3/#visibilidade-de-elementos
[slides-interacao-campos-de-dados]: https://fegemo.github.io/cefet-web/classes/js4/#interagindo-js-campos-de-escolha
[slides-eventos-mouse]: https://fegemo.github.io/cefet-web/classes/js4/#eventos-de-mouse-movimento
[slides-alterar-estilos]: https://fegemo.github.io/cefet-web/classes/js4/#alterando-o-estilo-de-elementos
[slides-conteudo]: https://fegemo.github.io/cefet-web/classes/js2/#alterando-o-conteudo
[slides-posicao-mouse]: https://fegemo.github.io/cefet-web/classes/js4/#posicao-mouse
[slides-select]: https://fegemo.github.io/cefet-web/classes/js4/#interagindo-js-campos-de-escolha
[slides-template-strings]: https://fegemo.github.io/cefet-web/classes/js3/#template-strings
