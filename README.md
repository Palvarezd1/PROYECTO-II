# Universidad Mariano Galvez de Guatemala
### Ingenieria en Sistemas
### Seccion "C"
---
> PROYECTO II
### **INSTRUCCIONES**
```
Esta basado en el manejo de archivos, los lenguajes a utilizar son C++ (el incluir C afecta la
calificación) y Python, es decir dos aplicaciones que hacen lo mismo. Cada aplicación debe contener lo
siguiente.

Creación del archivo
Lectura del archivo
Manipulación del archivo

Se debe crear al menos una estructura al que se llamará producto, donde contenga:

nombre (alfanumérico)
codigo (alfanumérico)
precio (real)
proveedor (alfanumérico)
existencia (numérico)
estado (Caracter) donde A = Aprobado y R = reprobado
descuento (real)
 

Al iniciar la aplicación debe mostrar un menú que contenga las siguientes opciones

Agregar producto
Buscar a un producto
Modificar los datos de un producto
Cada opción del menú debe tener las siguientes limitantes

Agregar producto: el código de producto no debe repetirse
Buscar producto: se debe buscar por el código, o por el nombre. Para el segundo caso debe se
debe realizar la función contiene, por lo que si el usuario ingresa solo una palabra mostrará
todos los productos que contengan dicha palabra en su nombre
Modificar datos: el código no puede modificarse, los otros atributos si

Al cerrar la aplicación y volver a iniciarlo, si el archivo ya fue creado solo debe leerla y cargar la
información que contenga. Cree un repositorio Git y suba los archivos (.cpp y .py), recuerde que su proyecto debe contener documentación interna y externa escrita en formato Markdown.
```
---
___

> ## **PYTHON** 
###### **CODIGO**: 
:computer:
```python
# Aplicacion de manejo de archivos y Estructura
# funcion para guardar 
def guardarDatos(productos):
    with open("productos.txt", "w") as archivo:
        for producto in productos:
            archivo.write(f"{producto['nombre']} {producto['codigo']} {producto['precio']} {producto['proveedor']} {producto['existencia']} {producto['estado']} {producto['descuento']}\n")

# funcion para cargar datos
def cargarDatos():
    try:
        with open("productos.txt", "r") as archivo:
            productos = []
            for linea in archivo:
                datos = linea.strip().split()
                producto = {
                    'nombre': datos[0],
                    'codigo': datos[1],
                    'precio': float(datos[2]),
                    'proveedor': datos[3],
                    'existencia': int(datos[4]),
                    'estado': datos[5],
                    'descuento': float(datos[6])
                }
                productos.append(producto)
            return productos
    except FileNotFoundError:
        print("El archivo de datos no existe o no se puede abrir.")
        return []

# funcion para agregar un producto
def agregarProducto(productos):
    producto = {}
    producto['nombre'] = input("Nombre: ")
    producto['codigo'] = input("Código: ")

    for p in productos:
        if p['codigo'] == producto['codigo']:
            print("El código de producto ya existe. No se puede agregar.")
            return

    producto['precio'] = float(input("Precio: "))
    producto['proveedor'] = input("Proveedor: ")
    producto['existencia'] = int(input("Existencia: "))
    producto['estado'] = input("Estado (A = Aprobado, R = Reprobado): ")
    producto['descuento'] = float(input("Descuento: "))

    productos.append(producto)
    guardarDatos(productos)
    print("Producto agregado con éxito.")

# funcion para buscar un producto
def buscarProducto(productos):
    criterio = input("Ingrese el criterio de búsqueda (código o nombre): ")
    for producto in productos:
        if producto['codigo'] == criterio or criterio in producto['nombre']:
            print("Nombre:", producto['nombre'])
            print("Código:", producto['codigo'])
            print("Precio:", producto['precio'])
            print("Proveedor:", producto['proveedor'])
            print("Existencia:", producto['existencia'])
            print("Estado:", producto['estado'])
            print("Descuento:", producto['descuento'])
            print("***************************")

# funcion para modificar el producto
def modificarProducto(productos):
    codigo = input("Ingrese el código del producto que desea modificar: ")
    for producto in productos:
        if producto['codigo'] == codigo:
            producto['precio'] = float(input("Nuevo precio: "))
            producto['proveedor'] = input("Nuevo proveedor: ")
            producto['existencia'] = int(input("Nueva existencia: "))
            producto['estado'] = input("Nuevo estado (A = Aprobado, R = Reprobado): ")
            producto['descuento'] = float(input("Nuevo descuento: "))
            guardarDatos(productos)
            print("Producto modificado con éxito.")
            return

    print("No se encontró ningún producto con el código proporcionado.")

# funcion para iniciar la aplicacion y que nos despliegue el menu
def main():
    productos = cargarDatos()

    while True:
        print("Menú:")
        print("1. Agregar producto")
        print("2. Buscar producto")
        print("3. Modificar datos de un producto")
        print("4. Salir")
        opcion = input("Seleccione una opción: ")

        if opcion == "1":
            agregarProducto(productos)
        elif opcion == "2":
            buscarProducto(productos)
        elif opcion == "3":
            modificarProducto(productos)
        elif opcion == "4":
            return
        else:
            print("Opción no válida.")

if __name__ == "__main__":
    main()
```
> ## **C++**
###### **CODIGO**: 
:computer:
```c++
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

struct Producto {
    string nombre; 
    string codigo; 
    float precio; 
    string proveedor; 
    int existencia; 
    char estado; 
    float descuento; 
};

void agregarProducto(ofstream& archivo, Producto& producto) {
    
    archivo << producto.nombre << endl;
    archivo << producto.codigo << endl;
    archivo << producto.precio << endl;
    archivo << producto.proveedor << endl;
    archivo << producto.existencia << endl;
    archivo << producto.estado << endl;
    archivo << producto.descuento << endl;
}

void buscarProducto(ifstream& archivo, string& codigo) {
 
    Producto producto;
    while (getline(archivo, producto.nombre)) {
        getline(archivo, producto.codigo);
        archivo >> producto.precio >> ws;
        getline(archivo, producto.proveedor);
        archivo >> producto.existencia >> ws;
        archivo >> producto.estado >> ws;
        archivo >> producto.descuento >> ws;

        if (producto.codigo == codigo) {
         
            cout << "Nombre: " << producto.nombre << endl;
            cout << "Código: " << producto.codigo << endl;
            cout << "Precio: " << producto.precio << endl;
            cout << "Proveedor: " << producto.proveedor << endl;
            cout << "Existencia: " << producto.existencia << endl;
            cout << "Estado: " << producto.estado << endl;
            cout << "Descuento: " << producto.descuento << endl;

            return;
        }
    }

    cout << "No se encontro el producto con codigo " + codigo + "." << endl;
}

void modificarProducto(ifstream& archivo, ofstream& temporal, string& codigo) {
    
    Producto producto;
    while (getline(archivo, producto.nombre)) {
        getline(archivo, producto.codigo);
        archivo >> producto.precio >> ws;
        getline(archivo, producto.proveedor);
        archivo >> producto.existencia >> ws;
        archivo >> producto.estado >> ws;
        archivo >> producto.descuento >> ws;

        if (producto.codigo == codigo) {
            
            cout << "Ingrese el nuevo nombre del producto: ";
            getline(cin, producto.nombre);

            cout << "Ingrese el nuevo precio del producto: ";
            cin >> producto.precio;

            cout << "Ingrese el nuevo proveedor del producto: ";
            getline(cin, producto.proveedor);

            cout << "Ingrese la nueva existencia del producto: ";
            cin >> producto.existencia;

            cout << "Ingrese el nuevo estado del producto (A = Aprobado, R = Reprobado): ";
            cin >> producto.estado;

            cout << "Ingrese el nuevo descuento del producto: ";
            cin >> producto.descuento;

           
            agregarProducto(temporal, producto);
        } else {
           
            agregarProducto(temporal, producto);
        }
    }

    
    cout << "No se encontro el producto con codigo " + codigo + "."<<endl;

}

int main() {
    ofstream archivo("ManejoDeArchivosInternos.txt");
    
    Producto p1 = {"Producto 1", "001", 10.0f, "Proveedor 1", 100, 'A', 0.0f};
    Producto p2 = {"Producto 2", "002", 20.0f, "Proveedor 2", 200, 'R', 0.1f};
    
    agregarProducto(archivo, p1);
    agregarProducto(archivo, p2);

    archivo.close();

    ifstream lectura("ManejoDeArchivosInternos.txt");

    string codigo = "";
    
    buscarProducto(lectura,codigo);

    lectura.close();

    ifstream lectura2("ManejoDeArchivosInternos.txt");
    
    ofstream temporal("temporal.txt");

    
     modificarProducto(lectura2,temporal,codigo);

     lectura2.close();
     temporal.close();

     remove("ManejoDeArchivosInternos.txt");
     rename("temporal.txt","ManejoDeArchivosInternos.txt");

     return 0;

}

```
---
___