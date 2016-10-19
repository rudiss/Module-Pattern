# Module-Pattern

O Module Pattern é muito utilizado porque ele permite organizar melhor o código, sem expor variáveis globais de modo promíscuo. Como ainda não temos uma sintaxe de módulos do próprio JavaScript, usamos os módulos para garantir que o escopo de variáveis seja fechado, além de simular a privacidade de atributos e funções.

Como o JavaScript ainda não possui módulos nativamente (embora isso provavelvemente entre na versão ECMAScript 6, também chamada de Harmony), este pattern é muito utilizado para simular módulos. Isso só é possível por causa do escopo local de variáveis de uma função, que permite isolar tudo o que é feito dentro da função.

##Anonymous Closures (Closures Anônimos)

Anonymous Closures são o construtor fundamental em Module Pattern que tornam tudo isso possível e são uma das melhores característica de JavaScript: é possível criar uma função anônima e executá-la imediatamente. Todo o código que é executado dentro da função esta no escopo do closure (algo como “fechamento”), o que proporciona privacidade e estado durante toda a vida da aplicação.

```javascript
(function () {
    // Todas variáveis e funções estão somente neste escopo
    // e ainda mantêm acesso a todos os globais
}());
```

Observe a ```()``` em torno da função anônima. Isto é requerido pela linguagem, uma vez que declarações que começam com a função de token são sempre considerados declarações de função. Incluir ```()``` cria uma expressão de função.

##Global Import

JavaScript tem um recurso conhecido como implied globals (globals implícitas). Sempre que um nome é usado, o interpretador volta pela cadeia de escopo em busca de uma declaração de variável ```(var)``` para esse nome. Se nada for encontrado, a variável é assumida como global; se é usado em um assignmente, o global é criado se não existir. Isso significa que o uso ou a criação de variáveis ​​globais em um closure anônimo é fácil. Infelizmente, isso leva a código difícil de gerir, já que não é óbvio (para humanos) que as variáveis ​​são globais em um determinado arquivo.


```javascript
(function ($, YAHOO) {
    // Agora se tem acesso às globais jQuery (como "$") e YAHOO nesse código
}(jQuery, YAHOO));
```

##Module Export

Às vezes, você não quer apenas usar globais, mas quer declará-los. É possível fazer isso facilmente ao exportá-los, usando o valor de retorno da função anônima. Se o fizer, completará o Padrão de Módulo básico. 
```javascript
var MODULE = (function () {
    var my = {},
             privateVariable = 1;

    function privateMethod() {
        // ...
    }

    my.moduleProperty = 1;
    my.moduleMethod = function () {
        // ...
    };

    return my;
}());
```


#Padrões Avançados de Module Pattern

Os Padrões de Module Pattern mostrados anteriormente são suficientes para muitos casos, mas é possível levar isso adiante e criar alguns construtors muito poderosos e extensíveis, continuando com o módulo ```MODULE```.

##Augmentation

Uma limitação do Module Pattern até agora é que o módulo inteiro deve estar em um arquivo. Quem já trabalhou em uma grande base de código entende o valor de divisão de código em vários arquivos. Felizmente, existe uma boa solução: Augmentation (Ampliação). Em primeiro lugar, é preciso importar o módulo; então, são adicionadas novas propriedades e; por fim, ele é exportado.
```javascript
var MODULE = (function (my) {
    my.anotherMethod = function () {
        // Método adicionado...
    };

    return my;
}(MODULE));
```
##Loose Augmentation

Enquanto o exemplo acima requer a criação inicial do módulo em primeiro lugar e a ampliação acontecer depois disso, isso nem sempre é necessário. Uma das melhores coisas que um aplicativo JavaScript pode fazer em relação a desempenho é carregar scripts de forma assíncrona. É possível criar módulos flexíveis de várias partes que podem se carregar em qualquer ordem com o Loose Augmentation (Ampliação Fraca). Cada arquivo deve ter a seguinte estrutura:
```javascript
var MODULE = (function (my) {
    // Adiciona recursos...

    return my;
}(MODULE || {}));
```
##Submódulo

Submódulo (Sub-module) é considerado módulo mais simples para pequenas funções
```javascript

3
4
5
6
MODULE.sub = (function () {
    var my = {};
    // ...
 
    return my;
}());
```
##Conclusão
Module Pattern é bom para performance, ele passa por minificação muito bem, o que faz com que o download seja mais rápido. Usar Loose Augmentation permite downloads paralelos, fáceis e sem bloqueios. O tempo de inicialização é, provavelmente, um pouco mais lento do que outros métodos, mas o custo-benefício vale a pena. Desempenho em tempo de execução não deve sofrer penalidades desde que globais sejam importados corretamente e, provavelmente, há aumento de velocidade em submódulos, encurtando a cadeia de referência com variáveis ​​locais.


Fonte: Learning JavaScript Design Patterns
https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript
