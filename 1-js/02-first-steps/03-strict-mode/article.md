# O modo moderno, "use strict"

Por um longo tempo, o JavaScript evoluiu sem problemas de compatibilidade. Novos recursos foram adicionados ao idioma enquanto as funcionalidades antigas não foram alteradas.

Isso teve o benefício de nunca quebrar o código existente. Mas a desvantagem foi que qualquer erro ou decisão imperfeita feita pelos criadores do JavaScript acabou ficando presa na linguagem para sempre.

Este foi o caso até 2009, quando ECMAScript 5 (ES5) apareceu. Adicionou novos recursos à linguagem e modificou alguns dos existentes. Para manter o código antigo funcionando, a maioria das modificações está desativada por padrão. Você precisa habilitá-los explicitamente com uma diretiva especial: `" use strict "`.

## "use strict"

A diretiva parece uma string `"use strict"` ou `'use strict'`. Quando ele está localizado no topo de um script, todo o script funciona da maneira "moderna".

For example:

```js
"use strict";

// esse código funciona de forma moderna
...
```

<<<<<<< HEAD
Nós vamos aprender sobre funções, uma forma de agupar comandos, em breve.

=======
Quite soon we're going to learn functions (a way to group commands), so let's note in advance that `"use strict"` can be put at the beginning of a function. Doing that enables strict mode in that function only. But usually people use it for the whole script.
>>>>>>> cdf382de4cf3ed39ca70cb7df60c4c4886f2d22e

Vamos apenas observar que "use strict" pode ser colocado no início da maioria dos tipos de funções em vez do script inteiro. Fazer isso habilita o modo estrito apenas nessa função. Mas geralmente, as pessoas usam no script inteiro.

O modo estrito não está ativado aqui:

```js no-strict
alert("algum código");
// "use strict" abaixo é ignorado - deve estar no topo

"use strict";

// modo estrito não está ativado
```

Apenas comentários devem aparecer acima de `"use strict"`.

## Console do navegador

<<<<<<< HEAD
Para o futuro, quando você usar o console do navegador para testar funcionalidades, observe que ele não "usa strict" por padrão.

As vezes, quando usar `use strict` faz alguma diferença, você terá resultados incorretos.
=======
Once we enter strict mode, there's no going back.
```

## Browser console

When you use a [developer console](info:devtools) to run code, please note that it doesn't `use strict` by default.

Sometimes, when `use strict` makes a difference, you'll get incorrect results.

So, how to actually `use strict` in the console?

First, you can try to press `key:Shift+Enter` to input multiple lines, and put `use strict` on top, like this:

```js
'use strict'; <Shift+Enter for a newline>
//  ...your code
<Enter to run>
```
>>>>>>> cdf382de4cf3ed39ca70cb7df60c4c4886f2d22e

Mesmo se pressionarmos `key: Shift + Enter` para inserir várias linhas e colocar` use strict` no topo, isso não funcionará. Isso é por causa de como o console executa o código internamente.

<<<<<<< HEAD
A maneira confiável de garantir `use strict` seria inserir o código no console da seguinte forma:
=======
If it doesn't, e.g. in an old browser, there's an ugly, but reliable way to ensure `use strict`. Put it inside this kind of wrapper:
>>>>>>> cdf382de4cf3ed39ca70cb7df60c4c4886f2d22e

```js
(function() {
  'use strict';

<<<<<<< HEAD
  // ...seu código...
})()
```

## Sempre "use strict"

Ainda precisamos cobrir as diferenças entre o modo estrito e o modo "padrão".

Nos próximos capítulos, assim como nós vamos aprender novas funcinalidades, vamos aprender as diferenças entre o modo estrito e os modos padrões. Felizmente, não há muitos e eles realmente tornam nossas vidas melhores.

Por agora, de um momento geral, basta saber sobre isso:

1. A diretiva `" use strict "` alterna o mecanismo para o modo "moderno", alterando o comportamento de alguns recursos internos. Vamos ver os detalhes mais tarde no tutorial.
2. O modo estrito é ativado colocando `" use strict "` no topo de um script ou função. Vários recursos de idioma, como "classes" e "módulos", ativam o modo estrito automaticamente.
3. Modo estrito é suportado por todos os navegadores modernos.
4. Recomendamos sempre iniciar scripts com `" use strict "`. Todos os exemplos neste tutorial assumem o modo estrito, a menos que (muito raramente) seja especificado de outra forma.
=======
  // ...your code here...
})()
```

## Should we "use strict"?

The question may sound obvious, but it's not so.

One could recommend to start scripts with `"use strict"`... But you know what's cool?

Modern JavaScript supports "classes" and "modules" - advanced language structures (we'll surely get to them), that enable `use strict` automatically. So we don't need to add the `"use strict"` directive, if we use them.

**So, for now `"use strict";` is a welcome guest at the top of your scripts. Later, when your code is all in classes and modules, you may omit it.**

As of now, we've got to know about `use strict` in general.

In the next chapters, as we learn language features, we'll see the differences between the strict and old modes. Luckily, there aren't many and they actually make our lives better.

All examples in this tutorial assume strict mode unless (very rarely) specified otherwise.
>>>>>>> cdf382de4cf3ed39ca70cb7df60c4c4886f2d22e
