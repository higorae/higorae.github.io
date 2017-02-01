---
layout: post
title: Java Assertion
date:   2017-01-31 22:00:00
categories: posts
---

## Introdução

Introduzido no java 1.4, java assertion é uma forma de testar uma determinada premissa do seu programa, sem a necessidade de se fazer o uso de condicionais pra situações que nunca acontecerão. Por exemplo, para garantir que o número informado em seu método seja maior que zero, você pode afirmar que ele nunca será negativo.

## Sintaxe de uso

Existem duas formas de se utilizar o java asserts. Essas formas são:

    assert Expressão01; // Ou assert (Expressão01);
    assert Expressão01: Expressão02 // Ou  assert (Expressão01): Expressão02;

<br/>
Em ambas as formas é necessário informar uma expressão do tipo boolean (semelhante como é feito ao utilizar um if ou while), que, quando a assertion for avaliado pelo sistema e seu valor for `false` é lançado um `AssertionError`.

A diferença entre as duas formas de uso é que na segunda forma adiciona-se uma segunda expressão separada por dois pontos. O valor presente nessa segunda expressão pode ser qualquer coisa que resulte em um valor. Ou seja, por ser qualquer objeto ou tipo prmitivo.
 
Essa segunda expressão é usada para gerar uma string que mostrará um pouco mais de informação no stacktrace caso o `AssertionError` seja lançado.

#### Exemplo

Primeira forma:


    private void calcular(int x){
      assert(x < 0); // Caso o valor seja negativo é lançado um `AssertionError`.
    }


Segunda forma:


    private void calcular(int x){
      assert(x > 0) : "O valor informado ("+ x +") é menor que zero"; 
      // Caso o valor seja negativo é lançado um `AssertionError` com a informação 
      // adicional passada na segunda expressão.
    }


## Boas práticas no uso do java assertion

Segundo o livro OCA/OCP Java SE 7 da Kyte Sierra e Bert Bates existem 5 situações que devem ser consideradas ao se utilizar assertion. Veja, abaixo, um resumo de cada das situações levantados no livro.

**Não usar para validar argumentos de um public method**

Por não existir um controle sobre quem/como ira chamar esse método. Métodos publicos devem garantir que qualquer restrição quanto ao uso seu uso devam ser aplicado pelo próprio método, através do lançamento de IlligalArgumentException, por exemplo. Coisa que um assertion não garante, pois ele não é ativado por padrão.

**Usar para validar argumentos de um private method**

Diferente dos métodos públicos, há um controle sobre quem ira chamar um método privado, sendo assim, a lógica utilizado pelo método que chamará meu método privado deve estar supostamente correta.

**Não use assertions para validar argumentos de linhas de comando**

Esse item segue exatamente a mesma ideia expressa no item **Não usar para validar argumentos de um public method**. Caso seja necessário validar os argumentos passados utilize exception para evitar que valores invalidos sejam utilizados pelo método.


**Use assertions, mesmo em métodos públicos, para verificar em casos que você sabe que nunca devem acontecer**

Um exemplo muito bom do porquê se usar assertion para esses casos pode ser encontrado no site com a [documentação da oracle](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html) sobre o assunto.

Basicamente o que se fala nele é que você deve usar uma assertion sempre que você escreveria um comentário que afirme uma invariação. Outro exemplo seria o uso em switch para situações em que não se utiliza o default.

**Não use as expressões da assertion que possam causar efeitos colaterais**

Exemplo de mau uso retirado do livro da kate sierra e Bert Bates:

    public void doStuff(){
      assert(modificarValor())
    }
    
    public boolean modifyThings(){
        x += 1;
        return true;
    } 

Como não há uma garantia que a assertion vai estar sempre ativa. O seu código pode possuir comportamentos diferentes do esperado caso ela não esteja funcionando.

## Conclusão

O uso da Assertion visa agilizar o desenvolvimento e garantir que certas premissas do sistema sejam atendidas durante o seu desenvolvimento ou testes.