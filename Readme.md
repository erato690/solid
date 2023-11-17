# SOLID

## Single responsability principle
A classe teve ter apenas uma responsabilidade ou seja ela deve ter um unico objetivo.

Exemplo de uma classe:

```  java
public class Pedido {

    public void criar() {
        System.out.println("Criando pedido de serviço...");
    }


    public void calcularValor() {
        System.out.println("Calculando pedido de serviço...");
    }

    public void salvar() {
        System.out.println("Abre conexão com o banco para salvar pedido de serviço...");
    }

    public void update() {
        System.out.println("Abre conexão com o banco para atualizar a pedido de serviço...");
    }

}
```
A classe a cima faz coisas de uma ordem  e tambem  abre conexão com o banco e salva, nesse exemplo é possivel ver que essa classe possui mais de um objetivo. 


## Open close principle
Toda classe deve ser aberta para extenção e fechada para modificação.
Na classe abaixo tem um exemplo onde toda vez que precisarmos adicionar um novo tipo vamos precisar mexer nessa classe.


```  java
public class Video {

    private String tipo;

    public void calcularInterrese() {
        
        if("FILME".equals(this.tipo){
             System.out.println("implementacao para os tipo filme");
        }else if("NOVELA".equals(this.tipo)){
            System.out.println("implementacao para os tipo novela");
        }

    }

}
```


Na classe acima ela não esta de acordo com o principio de aberta para extenção e fechada para modificação, porque toda vez que for necessario que tiver um novo tipo vamos precisar
alterar esse metodo.Para resolver esse problema podemos criar novas extenções dessa classe como:

```  java
public abstract class Video {

    public abstract void  calcularInterrese();
}

public class Filme extends Video {

    @Override
    public  void  calcularInterrese(){
        System.out.println("implementacao para os tipo filme");
    }


}

public class Novela extends Video {

    @Override
    public  void  calcularInterrese(){
        System.out.println("implementacao para os tipo novela");
    }


}
```

No exemplo assim a classe esta fechada para modificação e aberta para extenção.

## Liskov substitution principle - LSP
Uma sub classe pode ser utilizada no lugar da classe pai.
Isso quer dizer que  quando criamos uma instancia poliformica de uma classe todas as subclasse podem ser atribuidas  a uma referencia de uma classe pai e não quebra seu codigo.
Se a subclasse não tiver suporte para ser poliformica esta ferindo esse principio.

Exemplo:

```  java
public class Pato {

       public  void  anomatopeia(){
        System.out.println("Sou um pato vivo quack quack quack");
    }

}

public class PatoBorracha extends Pato {

    @Override
    public  void  anomatopeia(){
        System.out.println("Sou um pato borracha fiu fiu");
    }


}

//No exemplo abaixo é possivel ver que a instancia poliformica não quebra  ao chamar o metodo.
public class Teste {

    
    public static void  main(String [] args) {
        
        Pato pato = new Pato();
        pato.anomatopeia();

        Pato patoBorracha = new PatoBorracha();
        patoBorracha.anomatopeia();


    }


}
```
No exemplo acima o classe patoBorracha é um pato e emite som igual a um pato.
Caso a subclasse seja do tipo  da pai mas não tenhas todas suas caracteristicas ela não pode herda da classe pai, por que esta ferindo esse principio.

## Interface segregation principle -ISP
Suas classe deve implementar interfaces que sobscrevem todos os metodos desta interface, caso ela não implemente é precio cria uma nova interface.

Problema:
```  java
public interface Pato {

    void  anomatopeia();

    void andar();
}

public class PatoVivo extends Pato {

    @Override
    public  void  anomatopeia(){
        System.out.println("Sou um pato vivo quack quack quack");
    }

     @Override
    public  void  andar(){
        System.out.println("Sou um pato vivo estou andando" );
    }


}

/*
 * Aqui tem um problema uma bato de borracha não anda,
 * para resolver esse problema vamos criar uma nova interface
 */ 
public class PatoBorracha extends Pato {

    @Override
    public  void  anomatopeia(){
        System.out.println("Sou um pato vivo fiu fiu");
    }


}


```

Solução:

```  java
public interface Pato {

    void  anomatopeia();

}


public interface Locomocao {
    void andar();
}  

public class PatoVivo implements Pato,Locomocao {

    @Override
    public  void  anomatopeia(){
        System.out.println("Sou um pato vivo quack quack quack");
    }

     @Override
    public  void  andar(){
        System.out.println("Sou um pato vivo estou andando" );
    }


}

/*
 *Como um pato de borracha não anda ele não vai implementar a interface locomoção.
 */ 
public class PatoBorracha implements Pato {

    @Override
    public  void  anomatopeia(){
        System.out.println("Sou um pato vivo fiu fiu");
    }

}

```

Na solução acima dividimos a interface pato em duas, assim estamos aplicando o principio de segragação de interface.

## Dependency Inversion Principle
Neste principio  não devemos depender de implementações mas sim de abstrações.
Sempre que possivel em nossas classes caso o atributo seja um objeto se possivel sempre utilizar interfaces  e não limplementações.


Problema:

```  java


public class Filme {

/**
 * Um filme pode ter outra categoria sem ser drama  e nesse exemplo estamos forçando um filme sempre ter a categoria drama
 */
    private CategoriaDrama categoriaDrama;
    
    ...


}

```

Nesse exemplo acima é visivel que filme depende de categoria drama para solucionar esse problema vamos inverter essa dependencia onde
CategoriaDrama dependa de um filme e não ao contrario.

Solução:


```  java
public interface Categoria {
}  

public class Drama implements Categoria {

}  

public class Filme {

    /**
     * Agora estamos dependencia de uma abstração e não uma implementação.
     */
    private Categoria categoria;
    
    ...


}

```