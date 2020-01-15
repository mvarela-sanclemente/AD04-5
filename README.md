# Métodos de Querys en Hibernate

> As querys non son en SQL, senón en HQL inda que se parezan moito.

## Consulta de varios obxectos

Este código de exemplo imprime o nome tódolos xogadores gardados na base de datos. Utilizamos o método **list()** da clase **Query**.

```java
    //Collemos a sesión de Hibernate
    Session session = HibernateUtil.getSessionFactory().openSession();
    
    //Facemos unha consulta
    Query q1 = session.createQuery("SELECT x FROM Xogador x");
    List<Xogador> xogadores1 = q1.list();
    for(Xogador xogador:xogadores1){
        System.out.println(xogador.toString());
    }
```

## Consulta de certos parámatros de varios obxectos

Este código de exemplo imprime o nome e dorsal de tódolos xogadores gardados na base de datos. Utilizamos o método **list()** da clase **Query**.

```java
    //Collemos a sesión de Hibernate
    Session session = HibernateUtil.getSessionFactory().openSession();
    
    //Tamén podemos escoller recuperar só algún parámetro
    Query q2 = session.createQuery("SELECT x.nome,x.dorsal FROM Xogador x");
    List<Object[]> xogadores2 = q2.list();
    for(Object[] xogador:xogadores2){
        System.out.println(xogador[0] + " - Dorsal: " + xogador[1]);
    }
```

## Consulta de un único obxecto

Este código de exemplo imprime o nome do xogador que ten o dorsal 1 gardado na base de datos. Utilizamos o método **uniqueResult()** da clase **Query**.

```java
    //Collemos a sesión de Hibernate
    Session session = HibernateUtil.getSessionFactory().openSession();
    
    //Consulta con só un resultado
    Query q3 = session.createQuery("SELECT x FROM Xogador x WHERE dorsal=1");
    Xogador xogadorAux = (Xogador) q3.uniqueResult();
    System.out.println(xogadorAux.toString());
```

## Paxinación

Este código de exemplo mostra os xogadores da segunda páxina (as páxinas comezan a contar no índice 0, por iso indico na variable *paginaMostrar* é igual a 1) donde o tamaño da páxina e de un elemento. Utilizo os métodos **setMaxResults** **setFirstResult** da clase **Query**.

Ademais calculo o número de páxinas totais para poder mostrar tódolos xogadores.

```java
    //Collemos a sesión de Hibernate
    Session session = HibernateUtil.getSessionFactory().openSession();
    
    //Paginacion
    int tamPagina=1;
    //Primare pagína es la 0
    int paginaMostrar=1;
    Query q4 = session.createQuery("SELECT x FROM Xogador x ORDER BY dorsal");
    q4.setMaxResults(tamPagina);
    q4.setFirstResult(tamPagina * paginaMostrar);
    List<Xogador> xogadores4 = q4.list();
    for(Xogador xogador:xogadores4){
        System.out.println(xogador.toString());
    }
    
    //Numero de paginas
    Query q4_2 = session.createQuery("SELECT count(*) FROM Xogador x");
    long numObj = (Long) q4_2.uniqueResult();
    int numPaginas = (int) Math.ceil((double)numObj/(double)tamPagina);
    System.out.println("Numero de paginas: " + numPaginas);
```
