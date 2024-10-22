https://github.com/Maaaikol/ej-caso-final-integrador.git

El código fuente del intérprete tiny-lisp se proporcionará al final. Sólo unas pocas docenas de líneas de C++ son necesarias para analizar y evaluar expresiones basadas en la sintaxis de LISP.
A lo largo de los siguientes trabajos prácticos, vamos a desglosar y familiarizarnos con la estructura del proyecto de C++ tiny-lisp antes de ponerlo en marcha de dos formas distintas. El primer método es una aplicación de consola capaz de cargar y ejecutar scripts. El segundo enfoque es el uso del intérprete tiny-lisp desde un editor de scripts con interfaz gráfica.

a. Anatomía de la Solución 
Después de descargar los archivos complementarios del libro, abrir en Visual Studio la solución tiny-lisp.sln que consta de tres proyectos:

El proyecto C++ tiny-lisp, que alberga el código del intérprete.
El proyecto C# tinylisplab, que contiene el código para el editor de código fuente tiny-lisp.
El proyecto C# WinFormsSyntaxHighlighter, que incluye extensiones para implementar la resaltación de sintaxis para el componente RichTextBox.
Vamos a concentrarnos en el proyecto tiny-lisp de C++ y a presentar las principales carpetas (también llamadas "filtros" en Visual Studio).

b. La Carpeta Config 
La mayor parte del código está organizado como clases de C++, descritas en archivos de encabezado .h y archivos fuente .cpp.

La carpeta Config alberga la clase Configuration, utilizada para habilitar módulos de extensión para el lenguaje tiny-lisp.

c. La Carpeta Framework 
La carpeta Framework incluye varias clases de utilidad para gestionar colores, archivos o la comunicación entre objetos.

d. La Carpeta Jsonlib 
El formato JSON (JavaScript Object Notation) se está convirtiendo en la elección principal sobre XML; la carpeta Jsonlib incluye una adaptación del código json11 para convertir objetos C++ en flujo JSON y viceversa.

e. La Carpeta Language 
Esta carpeta es el núcleo del intérprete tiny-lisp, contiene la mayoría de las clases C++ del proyecto. Los archivos principales son type.h, que contiene las clases Variant y Environment, así como Parser.h/Parser.cpp.

El analizador de expresiones (parser) de LISP, es el encargado de desglosar las cadenas de caracteres (strings) en instrucciones y descifrar su significado.

f. La Carpeta Modules 
La sintaxis del lenguaje LISP es bastante minimalista, cercana al cálculo lambda. Para adherirse a esta filosofía minimalista, las palabras clave especializadas en las aplicaciones del lenguaje, por ejemplo, gestión de bases de datos, gestión de archivos, modelado digital, procesamiento de textos, etc., no están destinadas a definirse todas en el mismo lugar, en el código central de la clase Parser.

La carpeta Modules se corresponde con una clase base Module, que sirve de base para la implementación de los dos primeros módulos MathModule y DBModule.

g. La Carpeta Scriptboxes 
La carpeta Scriptboxes proporciona entornos de ejecución específicos para el intérprete tiny-lisp. Esto permite tener un solo lenguaje, un único intérprete, pero con la capacidad de controlar las interacciones con el mundo exterior (con o sin entrada/salida, con gestión de memoria compartida, con soporte de red, etc.).

El resto de los ejercicios prácticos continúan de la misma manera con explicaciones detalladas sobre cómo utilizar el intérprete en modo interactivo, código del bucle principal, cómo utilizar el depurador y cómo usar el intérprete desde tinylisplab. También se incluyen imágenes para una mayor claridad visual.

Estos casos prácticos están diseñados para ayudar a los estudiantes a familiarizarse con el intérprete tiny-lisp y su funcionamiento en C++, además de cómo utilizar el depurador en Visual Studio y el editor de texto LISP integrado. Al final de estos ejercicios, los estudiantes tendrán un entendimiento sólido y práctico del intérprete tiny-lisp.

Código
Te proporcionaré una versión simplificada de un intérprete tiny-lisp como base para tus ejercicios. Tendrás que expandir y mejorar este intérprete en tus tareas prácticas. Para mantenerlo manejable, este ejemplo no implementará todas las características del lenguaje LISP, solo unas pocas fundamentales.


// tiny_lisp.cpp
#include <iostream>
#include <sstream>
#include <stack>
#include <vector>

typedef std::vector<std::string> Tokens;

Tokens tokenize(const std::string& input) {
    Tokens tokens;
    std::string token;
    std::istringstream stream(input);

    while (stream >> token) {
        tokens.push_back(token);
    }

    return tokens;
}

int evaluate(Tokens& tokens) {
    std::stack<int> stack;

    while (!tokens.empty()) {
        std::string token = tokens.back();
        tokens.pop_back();

        if (token == "+") {
            int a = stack.top();
            stack.pop();
            int b = stack.top();
            stack.pop();
            stack.push(a + b);
        } else if (token == "*") {
            int a = stack.top();
            stack.pop();
            int b = stack.top();
            stack.pop();
            stack.push(a * b);
        } else {
            stack.push(std::stoi(token));
        }
    }

    return stack.top();
}

int main() {
    std::string input;
    std::getline(std::cin, input);
    Tokens tokens = tokenize(input);
    std::cout << evaluate(tokens) << std::endl;
    return 0;
}
Este programa interpreta expresiones en notación polaca inversa (RPN), que es similar a LISP en que las operaciones vienen antes que sus operandos. Puedes entrar expresiones como 3 4 + y 3 4 + 2 * y devolverá el resultado de la evaluación.

Como ejercicio práctico, puedes intentar mejorar este programa agregando características como:

Soporte para más operaciones, como - y /.
Soporte para operaciones que toman más o menos de dos argumentos, como min y max.
Soporte para variables, permitiendo al usuario definir sus propios símbolos y asociarlos con valores.
Soporte para funciones, permitiendo al usuario definir sus propias funciones y usarlas en expresiones.
Mejorar la interfaz de usuario, permitiendo al usuario introducir múltiples expresiones, mostrándoles un prompt, etc.
