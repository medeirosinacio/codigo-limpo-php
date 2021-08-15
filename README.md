# Código Limpo - PHP

Um guia não tão `quick reference` com exemplos retirados do próprio livro convertido em PHP.

![Clean Code](./imagens/cleancode.png)

## Índice

 1. [Introdução](#introdução)
 1. [Nomes Significativos](#capítulo-2--nomes-significativos)
 1. [Funções](#capítulo-3--funções)
 1. [Comentários](#capítulo-4--comentários)
 1. [Formatação](#capítulo-5--formatação)
 1. [Objetos e Estrutura de Dados](#capítulo-6--objetos-e-estrutura-de-dados)
 1. [Tratamento de Erro](#capítulo-7--tratamento-de-erro)
 1. [Limites](#capítulo-8--limites)
 1. [Teste de Unidade](#capítulo-9--teste-de-unidade)
 1. [Classes](#capítulo-10--classes)

## Introdução

 Esse repositorio se destina a um estudo pessoal do livro **Clean Code: A Handbook of Agile Software Craftsmanship**, convertendo seus exemplos e metodologia para o PHP, a fim de praticar e fixar os exercicios.

> Há duas vertentes para se obter habilidade profissional: conhecimento e trabalho.

## Capítulo 2 — Nomes Significativos

![Nomes Significativos](./imagens/1-nomes-significativos.jpg)

### Use Nomes que Revelem seu Propósito

O nome de uma variável, função ou classe deve responder a todas as grandes questões. ele deve lhe dizer porque existe, o que faz e como é usado.

```PHP

 // RUIM   
 public int $d; // tempo decorrido em dias

 // BOM
 public int $tempoPassadoEmDias;
 public int $diasDesdeCriado;
 public int $diasDesdeModificado;
 public int $tempoDeArquivoCriadoEmDias;

```

#### Evite Informações Erradas

O que essa função faz?

```PHP

public function busca()
{
    $lista = [];
    foreach ($this->todaLista as $x)
    {
        if($x[0] === 4){
            $lista[] = $x;
        }
    }
    return $lista;
}

```

* Que tipos de informação estão em `todaLista`?
* Qual a importância de um item na posição zero na `todaLista`?
* Qual importância do valor 4?
* Como eu usaria a lista retornada?

E agora?

```PHP

public function buscaCelulasInvalidas()
{
    $celulasInvalidas = [];
    foreach ($this->tabuleiro as $celula)
    {
        if($celula[self::STATUS] === self::INVALIDA){
            $celulasInvalidas[] = $celula;
        }
    }
    return $celulasInvalidas;
}

```

Note que a simplicidade do código não mudou, mas ele ficou mais explícito, descrevendo exatamente o que ele esta fazendo em cada etapa, não deixando índices ambíguos ou confusos.

Podemos ainda abstrair o condição `if`, criando uma função para revelar exatamente seu propósito.

```PHP

public function buscaCelulasInvalidas()
{
    $celulasInvalidas = [];
    foreach ($this->tabuleiro as $celula)
    {
        if($this->celulaInvalida($celula)){
            $celulasInvalidas[] = $celula;
        }
    }
    return $celulasInvalidas;
}

```

E se quiser simplificar mais, use uma função especifica para filtro ao invés de um foreach.

```PHP

public function buscaCelulasInvalidas()
{
    return array_filter($this->tabuleiro, function($celula){
        return $this->celulaInvalida($celula);
    });
}

```
