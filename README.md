# Lab 7

#### Gestión de configuración global: diseñe un sistema que garantice un único objeto de configuración accesible globalmente sin conflictos de acceso.

```java

public class Config{

    private Config test;

    private Config(){

    }

    public Config getInstace(){
        if(test == null){
            test = new Config();
        }

        return test;
    }

}

```


#### Creación dinámica de objetos basada en la entrada del usuario: implemente un sistema para crear dinámicamente varios tipos de elementos de interfaz de usuario basados ​​en las acciones del usuario.

```java

public interface Product{

}

public class PorductA implements Product{
    
}

public class PorductB implements Product{
    
}

public class PorductC implements Product{
    
}



enum FactoryProduct{

    PRODUCT_A(new PorductA()),
    PRODUCT_B(new PorductB()),
    PRODUCT_C(new PorductC());

    public Product product;

    FactoryProduct(Product product){
        this.product = product;
    }

    public Product getProduct(){
         return product;
    }

    public Product getProduct(String prod){
        if(prod == "a"){
            return FactoryProduct.PRODUCT_A.getProduct();
        }else if(prod == "b"){
            return FactoryProduct.PRODUCT_B.getProduct();
        }else if(prod == "c"){
            return FactoryProduct.PRODUCT_C.getProduct();
        }else {
            throw new IllegalArgumentException("Product not found");
        }
    }
}

class Main{
    public static void main(String[] args) {
        Product product = FactoryProduct.PRODUCT_A.getProduct("a");
        System.out.println(product.getClass().getName());
    }
}
    


```

#### Notificación de cambio de estado en todos los componentes del sistema: asegúrese de que los componentes reciban notificaciones sobre los cambios en el estado de otras partes sin crear un acoplamiento estrecho.

``` java

class WhatsApp implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println("WhatsApp " + arg);
    }
}

class SMS implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println("SMS " + arg);
    }
}

class Email implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println("Email " + arg);
    }
}

class Push implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println("Push " + arg);
    }
}

class NotificationTest extends Observable {
    public void setMessage(String message) {
        setChanged();
        notifyObservers(message);
    }
}

class Main2{
    public static void main(String[] args) {
        NotificationTest notificationTest = new NotificationTest();
        notificationTest.addObserver(new WhatsApp());
        notificationTest.addObserver(new SMS());
        notificationTest.addObserver(new Email());
        notificationTest.addObserver(new Push());
        notificationTest.setMessage("Hello");
    }
}


```

#### Gestión eficiente de operaciones asincrónicas: administre múltiples operaciones asincrónicas, como llamadas API, que deben coordinarse sin bloquear el flujo de trabajo principal de la aplicación.


```java 


class Main3{
    public static Future<String> calculateAsync(int n, int x) {
        CompletableFuture<String> completableFuture = new CompletableFuture<>();
        new Thread(() -> {
            completableFuture.complete("Hello " + (x+n));
        }).start();
        return completableFuture;

    }


    public static void main(String[] args) {
        Future<String> future = calculateAsync(1000, 1);
        try {
            while (!future.isDone()) {
                System.out.println(future.get());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```