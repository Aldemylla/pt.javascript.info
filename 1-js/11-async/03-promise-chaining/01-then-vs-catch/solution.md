<<<<<<< HEAD
A resposta breve é: **não, eles não são iguais**:
=======
The short answer is: **no, they are not equal**:
>>>>>>> e074a5f825a3d10b0c1e5e82561162f75516d7e3

A diferença é que se um erro ocorrer em `f1` ele será tratado pelo `.catch` neste caso:

```js run
promise
  .then(f1)
  .catch(f2);
```

...Mas não neste:

```js run
promise
  .then(f1, f2);
```

Isso é devido ao erro ser propagado pela cadeia, e no segundo código não há cadeia após `f1`.

<<<<<<< HEAD
Em outras palavras, `.then` passa resultados/erros para o próximo `.then/catch`. Então, no primeiro exemplo, há um `catch` em seguida, e no segundo exemplo não há, então o erro não é tratado. 
=======
In other words, `.then` passes results/errors to the next `.then/catch`. So in the first example, there's a `catch` below, and in the second one there isn't, so the error is unhandled.
>>>>>>> e074a5f825a3d10b0c1e5e82561162f75516d7e3
